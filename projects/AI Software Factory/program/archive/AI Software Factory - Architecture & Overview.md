# AI Software Factory: Architecture & Overview

> ⚠️ **SUPERSEDED FOR H2 (as of 2026-07-14).** The 2026-07-13 H2 Plan Review deprioritized building out this AWS/LangGraph pipeline for H2 — see [Embed-First Pivot](./overview.md#embed-first-pivot--pilot-scope-narrowing-2026-07-13) in overview.md and the [meeting synthesis](./meetings/2026-07-13-h2-plan-review-ai-sdlc.md). Current H2 direction is manual, repo-by-repo enablement toward the Q3 OKR (PR merged via factory process by mid-August — [OKR Tracker](https://docs.google.com/spreadsheets/d/10iTDkoL2Qpnw-a-5Ceo1m9IXwhG3BHgvSu7IDNETCm0/edit)). This document still describes the target architecture for a later full build, not the current H2 plan — don't treat it as active guidance.

**Purpose:** Design for the AI Software Factory's Plan, Execute, Verify (PEV) loop, an agentic pipeline that takes a Linear ticket, produces a reviewed and tested pull request in GitHub, and keeps a human on the critical decision points. Built with LangChain **DeepAgents** on **LangGraph**, hosted on **AWS**, using **Claude and OpenAI models on Amazon Bedrock**.

**Scope:** Overall architecture, the phased delivery plan, and the supporting detail needed to start a concrete build. Code is illustrative and shows shape, not final implementation. This is the current go-forward candidate for the AI Software Factory build (the AWS-vs-GCP platform pick, M3, is still open — see [CLAUDE.md](../CLAUDE.md)).

**Owner:** Chuck Haines, Team Moissanite. Originated 2026-07-08; PRE-VERIFY added 2026-07-09 (credit: Mykhailo Sakhnik); open items resolved/assigned 2026-07-10 (see section 8); synced to the Lucid diagram's account, sandbox, observability, and Sonar updates on 2026-07-13 (see sections 1, 3, 4, 5, 6, 8).

*The AI Software Factory is the technical-build workstream of the broader **AI-DLC** initiative (rebranded 2026-07-09 from "AI Software Factory" as the umbrella name — see [program/overview.md](./overview.md#rebrand-to-ai-dlc-2026-07-09)).*

---

## 1. The PEV loop: agent topology

**Diagram:** [AI Software Factory Architecture (Lucid)](https://lucid.app/lucidchart/9b39ee36-446e-4192-b4f5-bb98fd66ef28/edit) — the maintained source of truth for the full agent topology, AWS runtime, and model plane. Kept outside this doc so it can be updated without a markdown diff.

In short: a Linear ticket enters at INTAKE, passes through the one-shot PRE-VERIFY contract check, then PLAN and PLAN REVIEW with a human approval gate, then EXECUTE, then VERIFY, which either ships (human merge gate), loops back to EXECUTE with findings, or escalates once the retry cap is hit.

### Node-by-node

**INTAKE:** lightweight node (no LLM). Takes a Linear issue ID, pulls the ticket, checks out a fresh branch, and seeds the shared filesystem with the ticket context.

**PRE-VERIFY:** DeepAgent on **Sonnet**, one shot, no retry loop. Builds the task contract Planner needs before any plan is drafted, shaped by ticket type (credit: Mykhailo Sakhnik, proposed 2026-07-09 in #moissanite-eng):

- **fix → reproducer-first.** A check that fails before the fix and passes after.
- **feat → acceptance-test-first.** Checks that may fail before implementation and must pass after.
- **chore → validation/safety-first.** Proof that existing behavior stays green before and after the change.

Output is a structured contract — verification commands, assumptions, risks, and the expected validation artifact — that becomes Planner's primary input alongside the raw ticket. Uses the sandbox to write and run the reproducer/acceptance test/baseline; read-only on source otherwise. If it can't produce a reproducible contract in its one attempt, it escalates to a human rather than handing Planner an ungrounded task, using the same escalation pattern as the Verify loop, just earlier and with no retry.

Ticket-type gating — rejecting tickets that don't match an accepted type or shape before they enter the loop at all — is a related but separate concern. It matters once intake runs unattended; for now the team curates which tickets go in, so this gate isn't load-bearing yet (see Open Items).

This is distinct from Asad's spec-driven development work, which operates *after* a ticket is accepted, keeping the plan and documentation complete for implementation. PRE-VERIFY runs *before* acceptance, and its job is narrower: prove the task has a provable contract at all.

**PLAN:** Planner DeepAgent on **Opus**. Reads the ticket, the task contract, and the codebase, writes an implementation plan and spec using `write_todos`. Read-only tools: Linear, repo filesystem and grep.

**PLAN REVIEW:** Reviewer DeepAgent on **OpenAI GPT-5.5** that spawns a **Devil's Advocate subagent** (also GPT-5.5). Using a different model family than the Opus planner reduces correlated blind spots. The subagent runs in isolated context with a brief to attack the plan: missing edge cases, wrong abstractions, security and performance risks, simpler alternatives, untested assumptions. The reviewer consolidates the critique and either sends the plan back for revision or forwards it. The plan is then posted to Linear and the run parks for human approval (see section 4).

**EXECUTE:** Execution orchestrator DeepAgent on **Sonnet**. It turns the approved plan into todos and delegates implementation to **OpenAI Codex coding subagents** (implementer, test-author). It uses the sandbox to build, run tests, and run git, and opens or updates the PR via GitHub. On feedback rounds it consumes Verify's findings as new todos and pushes fixes to the same branch.

**VERIFY:** Verification DeepAgent on **Sonnet**, with fresh, independent context so it is not anchored on the author's reasoning. It pulls the PR diff and reviews against the plan, the task contract, and FN standards; runs tests, linters, type checks, and security scans in the sandbox; ingests any human PR review comments so human feedback flows through the same loop; and pulls Sonar's findings via an MCP tool as an additional code-quality and security signal (Sonar's own PR-analysis check also runs on every push and posts directly to GitHub — see section 8, item 10). It then emits structured findings (severity, file and line, required change). For difficult review questions it consults an **Opus advisor subagent** (see section 2). It has read-only permissions on source. The findings are the contract back to Execute.

**SHIP and ESCALATE:** On a passing verdict, the run reaches the human merge gate, then merges and updates Linear. If the loop exhausts its retry cap while still failing, it transitions to ESCALATE and notifies a human rather than spinning silently.

### Loop control (owned by the orchestrator, not the prompt)

- State holds a `round` counter and the current `verdict`.
- PRE-VERIFY: one attempt; `contract_built = false` routes straight to ESCALATE, no retry.
- `pass`: human merge gate, then SHIP.
- `fail` and `round < cap`: increment round, route findings back to EXECUTE.
- `round = cap` and still failing: ESCALATE with a summary comment.

The round cap (default 3, and separately, PRE-VERIFY's one-shot limit) lives in orchestrator state so it survives restarts, never in an LLM's context.

---

## 2. Model strategy

Because DeepAgents assigns a model per agent, the factory places each model where it is strongest: a fast, mechanical model for the pre-check, deep reasoning for planning, an independent frontier model for the adversarial review gate, a coding-specialized model for implementation, and a fast model for routine verification with an escalation path for hard judgment.

| Agent | Model | Rationale |
|---|---|---|
| Pre-Verify | Sonnet | Fast, mechanical: write and run one reproducer/acceptance test/baseline. No deep reasoning needed for a one-shot check |
| Plan | Opus | Strongest reasoning to draft the implementation plan |
| Plan Review + Devil's Advocate | OpenAI GPT-5.5 | Frontier reasoning from a different family than the planner, so the critique is not correlated with the same blind spots |
| Execute orchestrator | Sonnet | Fast, cost-effective coordination of the build |
| Execute implementers (coding subagents) | OpenAI Codex | Purpose-built for code generation |
| Verify | Sonnet, with an Opus advisor | Sonnet handles routine review; escalates hard judgment calls to Opus |

**On the OpenAI choice:** GPT-5.5 is OpenAI's most capable frontier model on Bedrock and the right pick for the reasoning-heavy review gate. GPT-5.4 is the price and performance option, and Codex is the coding-specialized model used for implementation.

### The advisor pattern (Verify)

The Sonnet verifier keeps routine review cheap while borrowing Opus reasoning only when it is worth it. Mechanically it is a DeepAgents subagent (`opus-advisor`) invoked via the `task` tool; it runs in isolated context and returns one focused opinion, so it does not bloat the verifier's context. When to consult it:

- On ambiguous or borderline pass/fail calls, security or architecture judgment, or findings the verifier is not confident about.
- Pair voluntary consultation with a deterministic trigger, for example always consult on changes that touch auth, data, or migrations, so the "confidently wrong" case is bounded.

Because both the verifier and its advisor are Claude, this pairing could later use the native advisor tool if that agent runs on Claude Platform; on Bedrock it is the hand-rolled subagent above.

### Providers, all on AWS

Providers mix per agent without touching orchestration. Three planes are available; the data-boundary question is not a differentiator (FN's existing Anthropic agreement covers the Anthropic-operated plane), so the choice is driven by features, cost, and operational simplicity.

| Plane | Operated by | Boundary | Notable |
|---|---|---|---|
| Bedrock, Claude (Opus, Sonnet) | AWS | In AWS boundary | Default. Prompt caching; no server-side advisor tool |
| Bedrock, OpenAI (GPT-5.5, GPT-5.4, Codex) | AWS | In AWS boundary | Same IAM, billing, and commit; Responses API shape; used here for review and implementation |
| Claude Platform on AWS | Anthropic | Outside AWS boundary (acceptable) | Full native features including the built-in advisor tool, Skills, code execution; mostly beta; AWS Marketplace billing |

This plan uses OpenAI (GPT-5.5 for review, Codex for implementation) and Claude (Opus, Sonnet) together, all in Bedrock and in-boundary, so no new vendor relationship is required. The one integration wrinkle is that the OpenAI models use Bedrock's Responses API shape, a distinct integration per agent that LangChain supports. Keep the active provider set deliberately small, since each adds a client config, caching behavior, rate limits, and cost and observability surface.

---

## 3. Repo, branch, and sandbox strategy

**Fresh clone per run.** The run's durable identity lives in Git and Linear, not on the sandbox's disk, since the disk is disposable and, under distributed workers, any step may run on a different machine.

- **One branch per run**, deterministically named from the ticket (e.g., `ai-factory/<LINEAR-ID>`). INTAKE creates it, EXECUTE always pushes to it, and the PR targets `main`.
- **Handoffs flow through the branch and PR**, not memory. When the run parks or moves between phases, the source of truth is what is committed and what is in the PR and Linear, so a fresh clone reconstructs full context. The task contract, plan, and findings are written to the branch so they survive a wiped sandbox.
- **Idempotent PR handling.** Before opening a PR, check for an existing one on the branch and update it, never opening a duplicate on resume or retry.
- **Rebase on staleness.** If `main` moved during a long human wait, rebase (or re-plan) before the next round so Verify reviews against current `main`.

**Sandbox mirrors each repo's dev environment.** Tests run inside the sandbox, so the sandbox image must reproduce each repo's real toolchain. Two initial target repos, two parity images: **FN-Core** (PHP monolith — its Docker setup: services, DB, fixtures, env) and **provider-match** (NestJS/TypeScript — its Node toolchain, dependencies, test DB and fixtures). Each is a custom sandbox image built from the repo's own dev config (not a vanilla managed interpreter), warmed so per-run setup does not dominate wall-clock, with network access scoped to the registries and dependencies the tests require, and pushed to ECR. This is the single largest infrastructure lift in the build.

---

## 4. Human-in-the-loop and approvals

The PEV tenets map directly onto the gates:

- **Pre-Verify, agent-led, one shot.** No human involvement on the happy path; escalates immediately if it can't build a contract.
- **Plan, human-led.** A hard approval gate after Plan Review. No code is written until a human signs off on the plan.
- **Execute, agent-led.** Fully autonomous within the sandbox; humans do not touch it unless it escalates.
- **Verify, human-led.** The automated Verify agent gates the loop, human PR review comments feed back through it, and the final merge is a human gate. After the retry cap is reached, the run escalates with a summary.

### Approvals are asynchronous and event-driven

Approval does not come back through an open session. The plan is posted to Linear, the run parks, and the decision arrives later as a separate inbound webhook, possibly days later. This shapes the architecture:

1. **The wait releases all compute and resumes from an external event.** The run parks on a durable signal correlated to the ticket (run identity equals Linear issue ID) and resumes when the decision webhook fires. This is the strongest reason the production orchestrator wants genuine durable execution rather than a held-open session.
2. **Two triggers, one ingress.** A start trigger (a label added to an issue starts a new run) and a decision trigger (an approve or changes comment resumes an existing run). An API Gateway plus Lambda ingress routes them into a durable signal queue (EventBridge plus an SQS FIFO queue, with a DLQ for poison messages): if a run exists and is waiting at the plan gate, deliver the decision; if it is a start label with no run, start; otherwise acknowledge and drop.
3. **"Changes requested" is a bounded loop.** `/approve` proceeds to Execute; `/changes <notes>` routes back to Plan with the notes, regenerates, re-posts, and parks again, capped like the Verify loop, escalating on exhaustion.

**Idempotency and ordering.** Webhooks can be redelivered, duplicated, or arrive out of order. The ingress must dedupe on the event or comment ID, ignore decisions that do not match the currently awaited gate, and drop events with no matching parked run. Every inbound webhook is treated as at-least-once.

The same shape covers the merge gate and GitHub PR review events, so the whole factory is uniformly event-driven with durable waits at each human gate.

---

## 5. Orchestration and durable execution

The orchestrator decides "run Pre-Verify, then Plan, then Review, wait for approval, then Execute, then Verify, loop up to the cap." Durability means that if anything dies (worker crash, deploy, LLM timeout, a multi-day approval wait), the run resumes without losing completed work or duplicating side effects.

### LangGraph and Temporal solve different parts

**LangGraph** is a reasoning and control-flow engine. Durability comes from its checkpointer: state is saved between nodes, so a failure or interrupt resumes from the last completed node. It is native to DeepAgents with first-class human-in-the-loop. Two limits to design around: a crash mid-node re-runs that node from the start (keep nodes small and side effects idempotent), and the open-source library runs in a single process (distributed execution comes from LangGraph Platform or self-hosting).

**Temporal** is a durable execution engine. It persists every step and, on a crash, replays deterministically and resumes at the exact in-flight activity. It brings distributed workers, automatic retries with backoff, durable timers, and workflows that can wait days or weeks on a human without holding a process open. The cost is operational: a Temporal cluster (or Temporal Cloud) and the discipline of the workflow and activity split.

They compose. The mature pattern, supported by an official (pre-release) `temporalio[langgraph]` plugin, is Temporal for the durable macro workflow and DeepAgents for the reasoning inside each step. Graph nodes are tagged to run as Temporal activities (LLM, tool, and IO calls, which get retries and timeouts) or as workflow logic (deterministic routing). With Temporal owning durability, LangGraph's in-memory checkpointer is sufficient; human waits become durable signals; long histories use continue-as-new.

| Dimension | LangGraph state machine | Temporal (+ LangGraph plugin) |
|---|---|---|
| Primary job | Agent reasoning + control flow | Durable orchestration of long, side-effecting workflows |
| Durability granularity | Between nodes | Per activity; resumes the exact in-flight step |
| Crash mid-step | Node re-runs from its start | Completed activities replay from cache; only the in-flight one retries |
| Distribution | Single process (OSS) or LangGraph Platform | Distributed workers and task queues by design |
| Retries and backoff | Manual, node-level | Built-in per activity |
| Multi-day human waits | Possible via checkpoint plus external resume (you manage it) | Native durable signals, parks cleanly for days |
| Ops burden | Low | Higher, run a Temporal cluster or Temporal Cloud |
| Maturity | GA, native to DeepAgents | Temporal GA; LangGraph plugin pre-release |
| Best fit | Initial build, fast iteration | Production: concurrent tickets, multi-day gates, guaranteed completion |

**The plan** (see section 7): build on LangGraph, port the orchestration tier to Temporal for production. If a near-term production bet is needed before the plugin is GA, the split can be hand-rolled, a Temporal workflow whose activities each call `deep_agent.invoke(...)`, using only GA Temporal APIs.

### Guaranteeing durability

Whatever the orchestrator, the discipline is the same:

- **Persist to a real store.** On LangGraph, compile with a durable checkpointer (AgentCore Memory or Postgres) and `durability="sync"` for the factory; on Temporal, let the engine persist activity results.
- **Idempotency keys on every external write.** Check for an existing PR before opening one; key Linear transitions and PR comments off `(ticket_id, round)`. Assume every side-effecting step can run more than once (LangGraph replays a node from its start; Temporal activities are at-least-once).
- **Artifacts live outside agent memory**, on the branch, the PR, and a durably-backed filesystem, so a fresh worker reconstructs context.
- **The round counter is authoritative in orchestrator state**, never in a prompt.
- **Bound external calls with timeouts and retries** (Bedrock throttling, GitHub rate limits, flaky tests).
- **Escalation is a durable state**, not a dropped run.
- **Observability spans restarts:** AgentCore Observability plus CloudWatch and OTEL for agent and infra traces, all staying in the AWS boundary, with a trace identity that survives retries and resumes. DataDog is a fallback for agent tracing/logging only if AWS-native observability proves insufficient (decision, not default — see section 6). No LangSmith.

---

## 6. AWS runtime

The same DeepAgents and LangGraph code runs on either of two hosting models, and both call Claude and OpenAI on Bedrock. The difference is who operates the runtime, state, sandbox, identity, and observability.

**Account.** The factory runs in a greenfield, dedicated AWS account (not shared with other FN workloads), federated via IAM Identity Center SSO with least-privilege permission sets for the core team plus a CI/deploy role. POC provisioning is manual/console; production infrastructure is re-expressed in OpenTofu (Phase 3, section 7).

| Concern | Bedrock AgentCore (managed) | Self-managed (ECS Fargate or EKS) |
|---|---|---|
| Hosting | Serverless AgentCore Runtime; deploy a container | Containers on Fargate or EKS with autoscaling and patching |
| Durable state | AgentCore Memory as the LangGraph checkpointer | Postgres/RDS checkpointer, or Temporal's datastore |
| Session limits | 8 hr max per session, 100 MB payload, 15-min idle | Set your own; long runs unbounded |
| Sandbox | Code Interpreter, or a custom image | Your own sandbox container |
| Tool gateway | Gateway turns APIs and Lambda into MCP tools | Host MCP servers yourself |
| Identity and secrets | Identity, OAuth credential providers, session isolation | Secrets Manager plus your IdP |
| Guardrails and eval | Policy (Cedar) plus Evaluations on LangGraph traces | Custom eval harness (Phase 2) plus your own policy layer |
| Observability | Built-in OTEL to CloudWatch (AgentCore Observability) | CloudWatch/OTEL, with DataDog as fallback if AWS-native proves insufficient |
| Time to first deploy | Fast | Slower |
| Trade-off | Managed conveniences, some lock-in | Max control, more ops burden |

**Initial build on AgentCore.** It supplies the exact plumbing the factory would otherwise hand-build: Memory (checkpointer), Code Interpreter or a custom sandbox, Identity (GitHub and Linear OAuth), Observability, and Evaluations. As a live M3 platform candidate, the build doubles as M3 evidence. The code stays portable (pure DeepAgents and LangGraph, Bedrock via `langchain-aws`, MCP tools) so nothing hard-depends on AgentCore beyond a swappable checkpointer.

**The 8-hour session cap** is the key constraint: a run gated on a multi-day human approval outlives any session. This is handled by parking the wait durably (section 4) and resuming a fresh session, and it is a primary reason the production orchestration tier moves to Temporal on ECS or EKS, where waits live in the workflow rather than an open session. AgentCore's discrete services (Code Interpreter, Gateway, Identity) can still be used as tools from Temporal activities.

---

## 7. Delivery plan

**Phase 0, Foundations (spikes).**
Stand up the dedicated, greenfield AWS account (IAM Identity Center SSO; manual provisioning for the POC, OpenTofu for production); build the FN-Core and provider-match parity sandbox images (the largest lift); authorize the Linear and GitHub connectors, plus Sonar's PR-analysis check on both repos; stand up the API Gateway plus Lambda webhook ingress (EventBridge plus SQS FIFO plus DLQ) with dedupe and correlation; select the first set of well-bounded bug-fix tickets across both repos.

**Phase 1, POC (LangGraph on AgentCore).**
Implement the full PEV loop: Intake, Pre-Verify (Sonnet, one-shot task contract per ticket type, escalate on failure), Plan (Opus), Plan Review (GPT-5.5 with a single Devil's Advocate), async Linear approval, Execute (Sonnet orchestrating Codex implementers), Verify (Sonnet with Opus advisor), bounded feedback loop, human merge, Ship, with escalation via Linear comment. In-sandbox tests, durable checkpointer, idempotent writes. Goal: end-to-end on real bug-fix tickets; serves as M3 evidence.

**Phase 2, Harden and tune.**
Firm up durability (idempotency keys, resume paths) and observability (AgentCore Observability plus CloudWatch/OTEL; decide whether the DataDog fallback is needed). Validate the model choices on real tickets: Codex on implementation, GPT-5.5 on review, and the Opus advisor on Verify. Confirm the OpenAI-on-Bedrock Responses API integration in LangGraph. Finalize the approval command syntax and cap values. Add ticket-type gating ahead of Pre-Verify once intake starts running unattended.

**Phase 3, Productionize (Temporal).**
Port the orchestration tier to Temporal (plugin or hand-rolled), with workers on ECS or EKS and durable multi-day human waits. Generalize the webhook pattern to the merge gate and GitHub PR-review events, expand the Devil's Advocate into a panel (security, performance, simplicity), and broaden the ticket types the factory accepts.

---

## 8. Open items

Resolved 2026-07-10 in a brainstorm session with Chuck (see [brainstorm log](<../brainstorm/2026-07-10-ai-software-factory-open-items-and-buy-vs-build.md>)):

1. **Sandbox image** (largest lift): build and warm the FN-Core-parity image; define network posture. **Owner: Asad (tentative, working assumption as of 2026-07-10).**
2. **OpenAI-on-Bedrock integration:** wire the Responses API model calls (GPT-5.5, Codex) through LangGraph and confirm tool-calling behavior per agent. **Owner: Mykhailo (2026-07-10).**
3. **Advisor design (Verify): resolved.** Deterministic triggers = always consult the Opus advisor on changes touching auth, data, or migrations (confirmed, not just illustrative — see section 2). The native advisor tool via Claude Platform is a deliberate revisit-later item, not being adopted now (still mostly beta).
4. **Model validation: resolved on approach.** Validation happens inside Phase 1 (POC) against real bug-fix tickets — not a separate spike — since real-world PR-acceptance rates (35-50%, per 2026-07-09 industry research) run well below benchmark numbers (74-78%), so benchmark performance alone won't be sufficient evidence. No fallback model named in advance; decide if/when Codex or GPT-5.5 underperforms.
5. **Approval command syntax: locked in as-is** — `/approve` and `/changes <notes>`, no changes from the original sketch.
6. **Cap values: confirmed as-is** — plan-loop and Verify-loop caps = 3 each; Pre-Verify stays one-shot with no retry.
7. **Branch and rebase policy: resolved.** Naming convention (`ai-factory/<LINEAR-ID>`) stays as-is. Staleness rule: auto-rebase on small/mechanical diffs; if the rebase actually conflicts, trigger a re-plan rather than force through, since a conflict signals the plan's codebase assumptions may be stale too.
8. **Webhook semantics:** confirming Linear and GitHub retry, duplicate, and ordering behavior to size the dedupe logic. **Resolved on approach:** validate this during the early Phase 0 webhook-ingress spike rather than resolving it in advance (no live connector access to confirm it ahead of time anyway).
9. **Ticket-type gate (deferred, confirmed correct as of 2026-07-10):** Pre-Verify builds a task contract but doesn't yet reject tickets by type/shape before they enter the loop. Not load-bearing while the team curates tickets by hand; needs a real design once intake runs unattended (credit: Mykhailo Sakhnik, 2026-07-09). The 2026-07-09 industry research (Böckeler's hands-on critique of Kiro/Spec Kit/Tessl — none distinguish problem sizes) is supporting evidence that this ticket-type-differentiated approach is already the right shape, not something to reconsider.

### Buy vs. build (raised by 2026-07-09 industry research, discussed 2026-07-10)

10. **Verify's code-review function vs. the AI code-review market** (CodeRabbit, Greptile — ~$420M ARR category): not replacing Verify's bespoke review, which is contract/plan-aware in a way generic PR-review tools aren't. **Update 2026-07-13, reflected in the Lucid diagram:** Sonar (already validated in its own POC — see CLAUDE.md) is now wired into the architecture as an *additional* signal, not a replacement — a Sonar PR-analysis check runs on every push and posts findings straight to GitHub, and Verify separately queries Sonar via an MCP tool as another input alongside the PR diff and human review comments. The open question about layering in Greptile specifically (cross-file/full-codebase indexing relevant to FN-Core's monolith) is separate from the now-adopted Sonar integration and remains unscheduled.
11. **Copilot's existing PR code review (separate from the Factory):** flagged as a real quality gap independent of the Factory build — Copilot often misses issues despite repo instructions, and structurally can't use skills/context defined in the repo (fully self-contained on its own built-in instructions). Agreed: a small, focused spike comparing CodeRabbit/Greptile against Copilot, explicitly testing whether either tool can actually ingest repo-level skills/context or has the same blind spot under a different vendor. **Owner: not yet assigned.**
12. **AWS Security Agent / AWS DevOps Agent** (announced re:Invent 2025, in preview): map onto Verify's `security_scan` step and a post-production incident/root-cause loop the Factory hasn't built yet, respectively. Both **parked, not active work**: Security Agent revisit is contingent on the M3 AWS-vs-GCP decision landing on AWS; DevOps Agent revisit is deferred to post-implementation, and should be evaluated against Joe's existing "Op Cop" (a custom Datadog-based production-investigation agent — not yet documented elsewhere in this workspace) rather than in a vacuum.

---

## Appendix: illustrative skeleton

Shape, not final code. Model IDs are placeholders to confirm against current Bedrock inference-profile IDs. OpenAI models on Bedrock use the Responses API path.

```python
from deepagents import create_deep_agent
from langgraph.graph import StateGraph, START, END

OPUS   = "bedrock_converse:us.anthropic.claude-opus-4-8-v1:0"
SONNET = "bedrock_converse:us.anthropic.claude-sonnet-5-v1:0"
GPT5   = "bedrock:openai.gpt-5-5"     # via Bedrock Responses API
CODEX  = "bedrock:openai.codex"       # via Bedrock Responses API

# --- phase agents (model per section 2) ---
pre_verifier = create_deep_agent(model=SONNET, tools=[repo_grep, sandbox_run],
    system_prompt=PRE_VERIFIER_PROMPT)
    # one-shot: builds a task contract (reproducer/acceptance/safety-first by ticket type)
    # emits contract_built=True/False; no retry, routes to ESCALATE on False

planner = create_deep_agent(model=OPUS, tools=[linear_read, repo_grep],
    system_prompt=PLANNER_PROMPT)

devils_advocate = {
    "name": "devils-advocate",
    "description": "Attack the plan: edge cases, risks, simpler alternatives.",
    "system_prompt": DEVILS_ADVOCATE_PROMPT, "model": GPT5,
}
plan_reviewer = create_deep_agent(model=GPT5, subagents=[devils_advocate],
    system_prompt=REVIEWER_PROMPT)
    # plan is posted to Linear; the run parks and resumes on the approval webhook (section 4)

codex_implementer = {
    "name": "implementer",
    "description": "Write the code changes for the assigned plan items.",
    "system_prompt": IMPLEMENTER_PROMPT, "model": CODEX,
}
codex_test_author = {
    "name": "test-author",
    "description": "Write and update tests for the change.",
    "system_prompt": TEST_AUTHOR_PROMPT, "model": CODEX,
}
executor = create_deep_agent(model=SONNET,
    tools=[github_open_pr, github_push, run_tests],
    subagents=[codex_implementer, codex_test_author],
    system_prompt=EXECUTOR_PROMPT)

opus_advisor = {
    "name": "opus-advisor",
    "description": "Advise on hard review calls: security, architecture, borderline pass/fail.",
    "system_prompt": ADVISOR_PROMPT, "model": OPUS,
}
verifier = create_deep_agent(model=SONNET, subagents=[opus_advisor],
    tools=[github_get_diff, github_get_review_comments, run_tests, security_scan],
    permissions=[{"operations": ["write"], "paths": ["src/**"], "mode": "deny"}],
    system_prompt=VERIFIER_PROMPT)

# --- orchestrator owns control flow and the round cap ---
def route_after_preverify(state):
    return "plan" if state["contract_built"] else "escalate"

def route_after_verify(state):
    if state["verdict"] == "pass":  return "ship"
    if state["round"] >= 3:         return "escalate"
    return "execute"

g = StateGraph(FactoryState)
g.add_node("intake", intake); g.add_node("pre_verify", run(pre_verifier))
g.add_node("plan", run(planner))
g.add_node("review", run(plan_reviewer)); g.add_node("execute", run(executor))
g.add_node("verify", run(verifier)); g.add_node("ship", ship); g.add_node("escalate", escalate)
g.add_edge(START, "intake"); g.add_edge("intake", "pre_verify")
g.add_conditional_edges("pre_verify", route_after_preverify,
    {"plan": "plan", "escalate": "escalate"})
g.add_edge("plan", "review")
g.add_edge("review", "execute")            # after human approval
g.add_edge("execute", "verify")
g.add_conditional_edges("verify", route_after_verify,
    {"execute": "execute", "ship": "ship", "escalate": "escalate"})
g.add_edge("ship", END); g.add_edge("escalate", END)
factory = g.compile(checkpointer=checkpointer)   # AgentCore Memory or Postgres
```

### Integration reference

- **Linear** (remote MCP, OAuth): read issue, description, comments, and labels at INTAKE and PLAN; gated writes (state, comments, PR link) at SHIP and ESCALATE.
- **GitHub** (`github-mcp-server`, OAuth): branch, commit, and PR at EXECUTE; diff and review-comment reads at VERIFY; gated review comments.
- **Sonar** (PR-analysis check plus MCP tool): PR-analysis check runs on every push to FN-Core and provider-match, posting findings directly to GitHub; VERIFY separately queries Sonar via the MCP tool as an additional code-quality/security signal (see section 8, item 10).
- **Auth:** OAuth credential providers on AgentCore Identity, or Secrets Manager with short-lived tokens self-managed. Never store secrets in code or docs.

---

## Sources

- [Deep Agents overview, LangChain docs](https://docs.langchain.com/oss/python/deepagents/overview)
- [langchain-ai/deepagents (GitHub)](https://github.com/langchain-ai/deepagents)
- [What is Amazon Bedrock AgentCore, AWS docs](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html)
- [Host LangGraph agents on AgentCore Runtime](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html)
- [Integrate AgentCore Memory with LangGraph](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-integrate-lang.html)
- [AgentCore quotas and limits](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/bedrock-agentcore-limits.html)
- [Claude models on Amazon Bedrock](https://aws.amazon.com/bedrock/anthropic/)
- [OpenAI frontier models on Amazon Bedrock](https://aws.amazon.com/bedrock/openai/)
- [Claude Platform on AWS](https://aws.amazon.com/claude-platform/)
- [The advisor strategy (Anthropic)](https://claude.com/blog/the-advisor-strategy)
- [GitHub official MCP server](https://github.com/github/github-mcp-server)
- [Temporal, LangGraph integration](https://docs.temporal.io/develop/python/integrations/langgraph)
- [LangGraph, Durable execution](https://docs.langchain.com/oss/python/langgraph/durable-execution)
- [LangChain, LangGraph vs Temporal](https://www.langchain.com/resources/langgraph-vs-temporal)
- Pre-Verify concept: Mykhailo Sakhnik, #moissanite-eng thread, 2026-07-09 ([Slack](https://fieldnation.slack.com/archives/C0AQW1KRQ20/p1783526340597049))
