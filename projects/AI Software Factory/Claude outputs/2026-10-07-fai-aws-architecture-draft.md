# Fai architecture on AWS: Q4 draft

Draft for Chuck's review. Not a decision record. Facts about AWS come from the AgentCore harness GA post and AgentCore developer docs. Anything I could not confirm is marked **Unverified**. Items marked **Proposed** are my suggestions, not decisions you have made.

## Summary

Fai runs on AWS by default and moves off AWS only when a tripwire (listed below) trips. The design has six parts:

1. **Agent definitions** are versioned files in git. They say what each agent is: model policy, instructions, skills, tools, limits, owner, triggers.
2. **Temporal** owns every run's lifecycle: intake, retries, waits for humans, and the run record. It sends a slim run request that points at an agent definition.
3. **AgentCore Runtime** hosts coding agents in a container you control (OpenCode or pi as the harness).
4. **AgentCore harness** hosts config-defined agents (meeting synthesis, agent builder, project sync, staleness check).
5. An **AI gateway** sits in front of models. Bedrock is the first provider behind it.
6. A thin **run contract** keeps hosts swappable. Every run, whatever hosts it, takes the same request and returns the same result.

The factory builds its own agents. An agent builder agent, itself defined in the same format, opens PRs to the agent registry. A pipeline validates, evals, and deploys merged definitions. Humans merge everything. Gitar and Sonar stay as the PR gates. Knowledge lives in git (the companyOS repos and ai-toolbox), not in the platform.

## Design decisions this draft assumes

| Decision | Choice | Source |
|---|---|---|
| Cloud | AWS until it stops working | Chuck |
| Hosting split | AgentCore harness for config agents, AgentCore Runtime with a bring-your-own harness for coding agents | Chuck |
| Durable orchestration | Temporal | Chuck |
| Model access | AI gateway, Bedrock first behind it | Chuck |
| Q4 sandbox scope | Factory repos plus one non-fn-core repo | Chuck |
| Merge authority | Humans merge. Gitar and Sonar gate. | Q4 OKR, Fieldworks brainstorm |
| Skills location | ai-toolbox and the FN marketplace, not the OS repos | Chuck, Cory DM |
| Agent storage | Definitions in a git registry, deployed by a pipeline | Proposed |
| Agent creation | Agent builder agent opens PRs to the registry. It cannot merge or widen its own permissions. | Proposed |

## System diagram

```mermaid
flowchart LR
  subgraph Triggers
    L[Linear: eligible issue]
    S[Schedules and events: transcript drop, repo change]
    H[Humans: Slack, CLI, command center later]
  end

  subgraph Control["Control plane"]
    IN[API Gateway + Lambda: dedupe, auth, intake]
    T[Temporal: run workflows, retries, human waits]
    LEDGER[(Run ledger)]
    AIDX[(Agent index: id, version, host ref, owner)]
    AIGW[AI gateway: model routing, keys, budgets]
    MCPGW[AgentCore Gateway: MCP tools, Cedar policy]
    ID[AgentCore Identity + GitHub App tokens]
  end

  subgraph Registry["Agent registry and build"]
    AB[Agent builder agent]
    AGR[Agent registry repo: definitions, instructions, evals]
    CI[Harness CI: schema check, policy check, evals, image build]
    DEP[Deploy: harness versions and endpoints, Runtime images]
  end

  subgraph Exec["Execution plane (run contract)"]
    RT[AgentCore Runtime: OpenCode or pi in container]
    HN[AgentCore harness: config agents]
  end

  subgraph Know["Knowledge (git is source of truth)"]
    OS[companyOS repos: FNOS, Eng Senior Leadership OS]
    TB[ai-toolbox and FN marketplace skills]
    REPO[Target repos + S3 snapshots]
  end

  subgraph Gates["Delivery gates"]
    GH[GitHub PR]
    GI[Gitar]
    SQ[Sonar]
    HUM[Human reviewer and merge]
  end

  OBS[CloudWatch / OTel traces to Datadog]

  L --> IN
  S --> IN
  H --> IN
  IN --> T
  T -->|resolve agent_id| AIDX
  T --> RT
  T --> HN
  T --> LEDGER
  RT --> AIGW
  HN --> AIGW
  RT --> MCPGW
  HN --> MCPGW
  RT --> ID
  RT --> REPO
  RT --> TB
  HN --> OS
  HN --> TB
  RT --> GH
  AB -->|PR| AGR
  AGR --> GH
  GH --> GI
  GH --> SQ
  GI --> HUM
  SQ --> HUM
  HUM -->|merge| AGR
  AGR --> CI
  CI --> DEP
  DEP --> RT
  DEP --> HN
  DEP --> AIDX
  HUM --> T
  RT --> OBS
  HN --> OBS
  AIGW --> LEDGER
```

## Agent definitions and the run contract

Two things are kept apart. The **agent definition** is static and versioned. The **run request** is per invocation and thin.

### Agent definition

A definition holds everything that is true of the agent every time it runs. It lives in git and changes through PRs.

```yaml
# agents/meeting-synthesis/agent.yaml  (illustrative values)
agent_id: meeting-synthesis
owner: team:eng-leadership          # a named human or team, required
description: Turn a meeting transcript into a sourced record in the Engineering Senior Leadership OS.

host:
  type: harness                     # harness | runtime
  # runtime agents also set: base_image, harness (opencode | pi)

model_policy:
  default: bedrock/claude-sonnet    # resolved through the AI gateway
  allowed: [bedrock/claude-sonnet]

instructions: ./instructions.md

skills:
  - source: ai-toolbox
    name: meeting-synthesis
    ref: v0.0.0                     # pinned version

tools:
  tier: 1                           # 1 read-only, 2 write or sensitive, 3 new MCP servers
  targets: [gateway/google-drive-read, gateway/linear-read]

repos:
  - name: os-eng-leadership
    access: pr-only                 # branch and PR, never merge

limits:
  max_wall_minutes: 20
  max_cost_usd: 2.00
  max_tool_calls: 100

triggers:
  - type: event
    name: transcript-dropped

evals: ./evals/                     # cases that gate every change to this definition
```

Rules:

- The definition format mirrors the harness config (model, instructions, tools, skills, memory, limits) so one format covers both host types.
- Every agent has a named owner.
- Raising `tools.tier` or adding a tool target needs approval from the tool approval group (Q11 and Q12 of the Fieldworks brainstorm: a dedicated group that likely includes security).
- Changes to instructions, skills, or limits need the owner's review and passing evals.

### Run request

Temporal builds the request from the definition and the trigger. It carries only what varies:

- `run_id`
- `agent_id` and `agent_version`, resolved from the agent index and pinned for the run
- `trigger` (Linear issue ID, schedule, event, human)
- `inputs` and target `repos` with refs
- `overrides` (optional)

Overrides can only narrow what the definition allows. A run can lower the spend cap. It cannot add a tool, raise the tool tier, or swap skills.

### Run result

- `status` (succeeded, failed, blocked, needs_human) and `failure_reason`
- `outputs` (PR URL, files written, links to source records)
- `trace_id`, `duration`, `tokens`, `cost`
- `session_ref` so a human can resume where the agent stopped

Every run records its `agent_version` in the ledger, so any PR can be traced to the exact instructions, skills, and limits that produced it. Every run emits the same trace and cost fields, which is what lets the Q4 report show a numerator, denominator, and baseline for each KR.

## How agents are stored

**Proposed.** Git is the source of truth for agents, the same way it is for knowledge and skills.

### Registry repo

One repo (name to be chosen, for example `fai-agents`) with one directory per agent:

```text
agents/
  meeting-synthesis/
    agent.yaml
    instructions.md
    evals/
  agent-builder/
    ...
templates/
  harness-agent/        # starting point for config agents
  runtime-coding-agent/ # starting point for OpenCode or pi agents
schema/
  agent.schema.json
CODEOWNERS
```

`CODEOWNERS` maps each agent directory to its owner. Changes to `tools` fields also require the tool approval group as a required reviewer. The repo is covered by Gitar and Sonar like any other.

Skills do not live here. Definitions reference skills in ai-toolbox or the FN marketplace by pinned version.

### CI on every PR (Harness)

1. Validate `agent.yaml` against the schema. Fail if there is no owner or the tool tier exceeds what the owner may request.
2. Policy check: model names are in the gateway allowlist, tool targets exist in the AgentCore Gateway catalog, repo access is `pr-only`.
3. Run the agent's evals in a sandbox. MOI-105 and MOI-145 are the eval path for ai-toolbox, and the same engine gates definitions.
4. For Runtime agents, build the container image and push it to ECR. Most Runtime agents share one base image with OpenCode or pi installed. The image stays under the 2 GB limit.

### Deploy on merge

- **Harness agents:** call `CreateHarness` or `UpdateHarness`. Each update creates an immutable version. Point the `STAGING` endpoint at the new version, then promote to `PROD` after a smoke run. Rollback is pointing the endpoint at an earlier version. AgentCore allows 10 endpoints per agent, 1,000 versions per agent, and 1,000 agents per account by default.
- **Runtime agents:** deploy the new image and update the Runtime version and endpoint the same way.
- **Triggers:** register the definition's triggers (Linear label, schedule, event) with the intake Lambda and Temporal schedules. Removing a trigger from a definition removes it on deploy.
- **Agent index:** the pipeline writes one row per deployed agent: `agent_id`, `agent_version`, `host` and its ARN, `owner`, `status`. Temporal resolves `agent_id` through this index and never reads git at run time.

**Unverified:** whether AgentCore resources have first-class infrastructure-as-code support (CloudFormation or OpenTofu) for harnesses and endpoints. The deploy step can call the API or the `agentcore` CLI either way. Choose IaC later based on what exists.

## How the factory builds agents

**Proposed.** The Q4 milestone "agents can be defined by config and deployed through the Factory" is delivered by an **agent builder agent** that uses the same path as every other change.

### The flow

1. A human asks for an agent: a Linear issue with an `agent-request` label, a Slack message, or a CLI call. The request names the purpose, the owner, the inputs, and the outputs.
2. The intake Lambda starts a Temporal workflow. It runs the agent builder agent like any other agent, with an ordinary run request.
3. The builder:
   - Picks a template from `templates/` based on the purpose (config agent or coding agent).
   - Drafts `agent.yaml`, `instructions.md`, and starter `evals/`.
   - Reuses existing skills from ai-toolbox where they fit. If a needed skill does not exist, it files a ticket instead of writing a one-off copy in the registry.
   - Opens a PR to the registry repo.
4. CI validates, runs evals, and builds. Gitar and Sonar review the PR.
5. The named owner reviews. The tool approval group reviews if the definition uses tier 2 or tier 3 tools. A human merges.
6. The deploy pipeline publishes the agent to `STAGING` and updates the agent index.
7. The owner runs a smoke test, then promotes to `PROD`. The agent appears in the index and, later, on the command center map.

### Guardrails on the builder

- It has PR access to the registry only. It cannot merge, approve, or edit `CODEOWNERS` or the schema.
- It cannot create an agent with a higher tool tier than the requester could approve. The tier is a request that a human approves.
- It cannot edit its own definition. Changes to `agent-builder` need the Fai team as reviewers like any other tier 3 change.
- It sets `owner` to the requester, so no agent exists without a named human.

### Bootstrapping

The first two definitions are hand-written: `agent-builder` and `meeting-synthesis`. The meeting synthesis agent is the first deployed through the pipeline, which proves the path. After that, new agents come from the builder. The scaffolder, project sync, and staleness check in the Q4 milestone become the first agents the builder creates, and they maintain FNOS and the Engineering Senior Leadership OS.

### Scope for Q4 and after

Q4 covers platform-owned agents only. Anyone on the team can request an agent, and the Fai team owns the review. User-deployed agents (levels 1 and 2 in the Fieldworks vision) arrive in a later phase. The same flow supports them: a non-engineer files the request, the builder drafts it, and only open-tier tools are allowed without approval.

## Components

### Intake and triggers

Linear webhooks, schedule events, and GitHub events land on API Gateway and a Lambda that authenticates, deduplicates, and starts a Temporal workflow. The Lambda holds no business logic. Only issues with the agreed eligibility label start coding runs.

### Orchestration: Temporal

One workflow per run. The workflow:

1. Resolves the agent definition version from the agent index and creates the run record in the ledger (this is "Factory first substantive work" for KR1).
2. Calls the execution host through an activity.
3. Waits for PR events, Gitar and Sonar results, and human review as signals or timers. These waits can last days, which is why the durable layer sits outside AgentCore sessions (8 hour maximum, 15 minute idle timeout by default).
4. Records merge, rejection, or failure.

Activities that call external systems use idempotency keys, because Temporal retries them.

AgentCore can host Temporal workers (serverless workers invoked on demand). Temporal describes that integration as **prerelease**. Plan to run workers on ECS or EKS if the AgentCore path is not stable enough, since workers are plain processes and not part of the run contract.

**Open choice:** Temporal Cloud or self-hosted. Cloud adds a vendor to the security review but removes an operations burden for a team of four.

### Execution plane

**Coding agents: AgentCore Runtime, bring-your-own harness.** Package OpenCode or pi in an ARM64 container, push it to ECR, and deploy to Runtime. You own the agent loop. This is the only route to a non-Strands harness, because the managed harness is Strands-based and the Claude Agent SDK export is listed as coming soon. At start, the entrypoint reads the agent definition for its version, configures the harness (model through the gateway, skills, MCP config), and runs the task.

Facts that shape the sandbox:

- Each session runs in its own Firecracker microVM.
- Per-session hardware is capped at 2 vCPU and 8 GB. Container images are capped at 2 GB. Neither limit is adjustable.
- Maximum session lifetime is 8 hours (adjustable). Idle timeout is 15 minutes (adjustable). Synchronous requests time out at 15 minutes. Asynchronous jobs can run up to 8 hours.
- Managed session storage is 1 GB per session. Larger working sets need EFS or S3 Files, and both require VPC mode.
- Defaults per account: 5,000 active sessions in us-east-1 and us-west-2, 25 new sessions per second.

Third-party proof that coding CLIs run on Runtime: a Classmethod writeup ran Claude Code and Codex CLI through a Runtime execution role against Bedrock, but only with a hello-world prompt. **Unverified:** OpenCode or pi on Runtime doing a real coding task with MCP, git, and tests. That is the first spike.

**Config agents: AgentCore harness.** `CreateHarness` and `InvokeHarness` define and run an agent from configuration: model, instructions, tools, skills, memory, limits. Each update creates an immutable version, and named endpoints (for example `STAGING`, `PROD`) pin versions. Skills load from git or S3. The agent definition maps almost one to one onto the harness config. There is no harness fee. You pay for Runtime compute ($0.0895 per vCPU-hour and $0.00945 per GB-hour of active use), Gateway, Memory, CloudWatch, and model inference.

**Workspace bootstrap.** Each repo has a snapshot in S3, refreshed on PR merge or on a cron. A new session restores the snapshot and runs a `git fetch` to pick up the delta, so start time stays short. With 1 GB of managed session storage, larger repos need an EFS or S3 Files mount or a shallow clone. The harness `InvokeAgentRuntimeCommand` API can run this setup step without spending model tokens.

### Model access: AI gateway

Agents never call model providers directly. They call the AI gateway, which holds provider keys, enforces per-run budgets, and records tokens and cost against `run_id`. Bedrock is the first provider and supports Claude and OpenAI GPT-5.x in-boundary.

AgentCore Gateway does not do this job. It brokers tools (OpenAPI, Smithy, Lambda, MCP), not model calls.

The harness accepts a `liteLlm` provider type, and OpenCode and pi can point at an OpenAI-compatible endpoint. **Unverified:** that the harness can target a custom gateway base URL. **Open choice:** buy or compose the gateway (LiteLLM-class, a vendor, or an AWS-native pattern).

### Tool access: AgentCore Gateway

MCP servers (Linear, GitHub, Sonar, and later Datadog) register as Gateway targets. Cedar policies gate calls by caller, condition, and arguments. Tools are tiered:

- Tier 1: read-only, open to any agent.
- Tier 2: write or sensitive data, granted per agent with a named approver.
- Tier 3: new MCP servers, reviewed before entering the catalog.

The tier in an agent definition is the request. The Gateway policy is what enforces it, so editing a definition cannot grant a tool by itself.

### Identity

Each agent is its own principal with a named owner. AgentCore Identity holds workload identities and the token vault. For GitHub, mint a short-lived installation token per run from a GitHub App, scoped to the target repo, so the agent can push a branch and open a PR but cannot merge. Branch protection and CODEOWNERS enforce the human merge. Okta remains the human identity provider.

**Unverified:** whether tokens placed in the sandbox are visible to the agent's shell. Assume they are, and keep their scope minimal.

### Knowledge layer

Git is the source of truth. The companyOS repos (FNOS and the Engineering Senior Leadership OS) have a root `AGENTS.md` and an `AGENTS.md` in each part intended to be worked on in isolation. Skills live in ai-toolbox and the FN marketplace and are pulled into runs by version. They do not live in the OS repos or the agent registry.

Agents write to the OS repos through PRs, and each OS has a named human owner who reviews. Goals and decisions carry a current and an archive area. Archived content is skipped by default, and anything not marked current is a hint, not a source of truth.

Review-relevant skills are also committed in each repo, because vendor-hosted review reads repo content only.

### Delivery gates

The factory opens a PR on a branch. Gitar and Sonar run as they do today. A human reviews and merges. No agent holds merge permission in Q4. This applies equally to PRs against product repos, the OS repos, and the agent registry.

### Observability, cost, and the run ledger

AgentCore traces to CloudWatch, including a GenAI Observability view per harness, session, and trace. Forward traces to Datadog through OpenTelemetry. **Unverified:** the exact forwarding path from AgentCore to the existing `ai-harness-otel` index.

AWS documentation does not describe per-agent or per-run cost attribution for AgentCore. The design therefore does not depend on it. The AI gateway records model cost by `run_id`, and the Temporal workflow records sandbox seconds from the Runtime session. The ledger sums them and joins on `agent_id` and `agent_version`. AWS cost allocation tags are a cross-check, not the source.

The **run ledger** (a small Postgres or DynamoDB table) is the source of truth for KR measurement. Each row holds issue start, Factory takeover, PR opened, review events, merge, failure, cost, tokens, and agent version. Deployment and defect data join in from Harness and Linear for the KR3 numerators and denominators.

## Q4 walking skeleton

Smallest slice that satisfies the Walking skeleton milestone:

1. Hand-write the `meeting-synthesis` definition and deploy it through the pipeline to a harness `STAGING` endpoint.
2. A schedule or manual trigger starts a Temporal workflow, which resolves the agent from the index.
3. The agent calls a model through the AI gateway, using a marketplace skill pinned by version.
4. It writes a sourced meeting record to the Engineering Senior Leadership OS through a PR.
5. The ledger shows the run's identity, agent version, trace link, and cost.

The first coding slice repeats the pattern with an OpenCode or pi container on Runtime, a Linear issue as input, and a PR as output.

## Build order and Q4 mapping

| Order | Work | OKR milestone |
|---|---|---|
| 1 | Spike A: OpenCode and pi in a Runtime container (MCP, git, tests, Bedrock through the gateway). Spike B: cost capture by `run_id`. | Platform decision gate |
| 2 | Agent definition schema, registry repo, CI skeleton | Walking skeleton |
| 3 | Run contract, Temporal workflow skeleton, agent index, ledger | Walking skeleton |
| 4 | Deploy pipeline for harness agents. Meeting-synthesis agent writing to the Eng Senior Leadership OS. | Walking skeleton |
| 5 | Agent builder agent. Scaffolder, project sync, and staleness check built through it. | Agent creation and companyOS |
| 6 | Deploy pipeline for Runtime agents. Linear to PR run on a factory repo, through Gitar and Sonar. | First Factory-completed issue |
| 7 | Add one non-fn-core repo, tune snapshots and limits | Factory pilot |

## Tripwires: when AWS stops working

Move a component to another host (ECS or EKS behind the same run contract) if any of these hold after the spikes:

1. OpenCode or pi cannot run a real coding task in a Runtime container with MCP, git, and tests.
2. The 2 vCPU, 8 GB, or 2 GB image limits block the non-fn-core repo test, and EFS or S3 Files does not fix it.
3. Runtime cannot reach GitHub from VPC mode without an egress path the security team accepts.
4. Per-run cost cannot be captured to within a tolerance you and Cory set.
5. Temporal workers cannot run reliably on AgentCore and ECS or EKS is needed (this moves workers only, not the agents).

fn-core is outside Q4 scope. Run it as a recorded spike: does the fn-core sandbox image fit the 2 GB and 8 GB limits.

## Risks and unknowns

- **Security docs.** The harness security page I read is the China regions version. Confirm isolation, network, and audit behavior in the target region. The page does not cover CloudTrail per-action audit, egress allowlisting, or GitHub token exposure.
- **Skills from git are trusted.** Callers can override the `skills` field per invocation. The run request therefore never accepts skills overrides. Only the deployed definition sets them.
- **Model parameters pass through.** `additionalParams` can redirect endpoints and override headers. Restrict it.
- **Self-modifying risk.** An agent that creates agents can widen its own reach. The guardrails on the builder (PR-only, no self-edit, human approval of tool tiers) are what contain this.
- **Harness maturity.** The CLI quickstart still installs `@aws/agentcore@preview` although the harness is listed as GA.
- **Small sample size.** With 20 completions as the target, percentage rates are volatile. Report counts with every rate.
- **Team capacity.** The build list above is wider than a team of four with other commitments can finish in one quarter. Items 5 and 7 are the first to slip.

## Questions for Chuck and Cory

1. **Where do definitions live?** A dedicated registry repo, or inside ai-toolbox next to the skills? This draft assumes a dedicated repo, so the approval rules for tools and skills can differ.
2. **Who is on the tool approval group** for tier 2 and tier 3 changes? Is security (Ben's team) a required reviewer from day one?
3. **Agent index home:** the same database as the run ledger, or separate?
4. **Temporal Cloud or self-hosted?** Who owns the security review?
5. **AI gateway:** buy or compose, and who owns it? It is the first piece with no existing foundation.
6. **Which non-fn-core repo** joins the pilot?
7. **Q4 OKR doc still lists the Platform and run contract milestone.** The Slack thread says to drop it. This draft keeps the run contract as a design rule and can seed the hosting ADR without a separate milestone.
8. **Who owns AWS account setup,** including the Bedrock IAM role that MOI-149 is waiting on?

## Sources

- [Amazon Bedrock AgentCore harness is now generally available (AWS)](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-harness-is-now-generally-available-go-from-idea-to-production-grade-agent-in-minutes/)
- [AgentCore harness vs. Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-vs-runtime.html)
- [AgentCore quotas and limits](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/bedrock-agentcore-limits.html)
- [AgentCore harness security (China regions version)](https://docs.amazonaws.cn/en_us/bedrock-agentcore/latest/devguide/harness-security.html)
- [Temporal: Amazon Bedrock AgentCore with Temporal serverless workers](https://temporal.io/blog/amazon-bedrock-agentcore-with-temporal-serverless-workers)
- [Classmethod: Codex CLI and Claude Code on AgentCore Runtime](https://dev.classmethod.jp/articles/bedrock-agentcore-runtime-codex-cli-claude-code/)
- [DoiT: AgentCore per-tenant cost attribution](https://www.doit.com/blog/blog-bedrock-agentcore-per-tenant-cost-attribution)
- Q4 2026 OKR (Fai), Fieldworks brainstorm and layer mapping, and the Slack DMs with Cory from 2026-10-05 to 2026-10-07
