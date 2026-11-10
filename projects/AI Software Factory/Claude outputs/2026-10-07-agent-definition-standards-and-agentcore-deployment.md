# Agent definition standards and AgentCore deployment

Research for the Fai architecture draft. Answers two questions: is there a standard for agent definitions we should use, and what does deploying an agent to AgentCore require (S3, identity, and the rest)? Sources are AWS AgentCore docs (retrieved 2026-10-07), the AgentCore release notes through October 2026, and the standards' own sites. Items marked **Unverified** were not confirmed in a source.

## Short answers

1. **No single standard covers a whole agent definition.** Open standards cover the layers: instructions (AGENTS.md), skills (Agent Skills), tools (MCP), and discovery and calling (A2A Agent Card). The one cross-framework definition language, Oracle's Open Agent Specification, has no AgentCore adapter and does not cover deployment, identity, or permissions. On AWS, the AgentCore harness config is the format you must be compatible with anyway.
2. **Recommendation: compose standards and do not invent a spec.** Make the agent definition a thin envelope. Its core mirrors the AgentCore harness config field for field, its instructions are a Markdown file, its skills are Agent Skills folders, its tools are MCP servers behind AgentCore Gateway, and a small `fn` block holds the parts only Field Nation needs (owner, tool tier, evals, triggers).
3. **Deployment to AgentCore is well supported.** Harnesses and harness endpoints have CloudFormation resources. Terraform has a harness resource. Runtime has API, CLI, and Terraform resources. The open questions are in the compatibility spike list at the end.

## Standards for agent definitions

| Standard | What it covers | Governance and status | Fit for Fai |
|---|---|---|---|
| **AGENTS.md** | Plain Markdown instructions. No schema. | Agentic AI Foundation (Linux Foundation). Read by Codex, Cursor, GitHub Copilot, Gemini CLI and others. | Use for `instructions.md` and for the OS repos. Because there is no schema, parsing varies by tool. |
| **Agent Skills** (`SKILL.md`) | A folder with `SKILL.md` (`name`, `description`, instructions) plus optional `scripts/`, `references/`, `assets/`. Progressive disclosure. | Originated at Anthropic, now an open standard. OpenCode, pi, Claude Code, Copilot, and many others list support. The AgentCore harness skills docs say they follow it. | Already the ai-toolbox format. Use as is. |
| **MCP** | Tool declarations: name, description, JSON Schema inputs and outputs. | Spec version 2025-11-25. Adopted across vendors. The most portable layer. | Tools are MCP servers registered as AgentCore Gateway targets. |
| **A2A Agent Card** | JSON manifest of identity, capabilities, skills, endpoints, and authentication, served at `/.well-known/agent-card.json`. | Linux Foundation, then the Agentic AI Foundation. v1.0 froze March 2026. AWS is a founding member. | A discovery and calling contract, not a build spec. Generate a card from the definition if agents must call each other. Not needed for Q4. |
| **Open Agent Specification** (Oracle Agent Spec) | Cross-framework agents and flows in YAML or JSON. Adapters for LangGraph, AutoGen, CrewAI, and the WayFlow runtime. | Oracle research project from October 2025. Governance and license were not stated on its docs page. | Closest thing to a full definition language. I found no AgentCore adapter, and the docs I read do not cover deployment, identity, or permissions. Watch it. Do not adopt it now. |
| **AgentCore harness config** | Model, system prompt, tools, skills, memory, limits, hooks, environment, authorizer, tags. Exposed as the API, CLI, and CloudFormation resource `AWS::BedrockAgentCore::Harness`. | AWS product, GA June 2026. | The format the deploy step must emit for harness agents. Mirror it. |

I did not find sources for Kubernetes `kagent` CRDs or Docker `cagent` definitions, so they are not assessed here.

### What this means for the definition file

Keep one file per agent with two parts:

- **Core config** that mirrors the AgentCore harness config, so deployment is a direct mapping and not a translation.
- **`fn` block** for what the platform does not model: owner, tool tier, evals, triggers, host type.

Everything the file points at is a standard format: instructions in Markdown, skills as Agent Skills folders, tools as MCP servers. If the ecosystem converges on a full definition standard later, the envelope can be regenerated into it. The pieces underneath will not change.

## How agents deploy to AgentCore

### Two hosts, two shapes

| | Harness (config agents) | Runtime (coding agents) |
|---|---|---|
| You provide | Configuration | A container image that serves HTTP |
| Loop | Managed, Strands-based | Yours (OpenCode or pi) |
| Create | `CreateHarness` | `create_agent_runtime` |
| Update | `UpdateHarness` creates a new immutable version | An update creates a new immutable version |
| Environments | Named endpoints pinned to a version | Named endpoints pinned to a version |
| IaC | CloudFormation `AWS::BedrockAgentCore::Harness` and `HarnessEndpoint`. Terraform `aws_bedrockagentcore_harness`. | Terraform `aws_bedrockagentcore_agent_runtime` and `..._agent_runtime_endpoint`. CloudFormation resource names for Runtime were not confirmed. |
| Invoke | `InvokeHarness` with the harness ARN and a `runtimeSessionId` of 33 or more characters | `InvokeAgentRuntime` with the runtime ARN, a `runtimeSessionId`, and a `qualifier` |

The harness itself runs inside a Runtime, so both share quotas, session isolation, and observability.

### Harness deployment

1. Create the execution role (see Identity below).
2. Create the harness. Required fields are `HarnessName`, `ExecutionRoleArn`, and `Model`. Optional fields include `SystemPrompt`, `Tools`, `Skills`, `Memory`, `Hooks`, `AllowedTools`, `MaxIterations`, `MaxTokens`, `TimeoutSeconds`, `EnvironmentVariables`, `Environment`, `EnvironmentArtifact`, `AuthorizerConfiguration`, and `Tags`.
3. Every update creates a new integer version. The `DEFAULT` endpoint moves to the newest version automatically. Every other endpoint stays put until you change its target version.
4. Create named endpoints (for example `staging` and `prod`) with `TargetVersion`. Promotion is an update to `TargetVersion`. Rollback is the same update with an earlier version. The docs describe the update but not rollback, so test it.
5. Poll `get-harness` until `READY` before invoking.

**Constraints that affect our definition format:**

- `HarnessName` must match `^[a-zA-Z][a-zA-Z0-9_]{0,39}$` (letters, digits, underscores, 40 characters), and changing it replaces the resource. Use snake_case agent IDs.
- Endpoint names allow 48 characters. `TargetVersion` is an integer up to five digits. Our git tags (SemVer) need a mapping to harness version numbers in the agent index.
- A harness takes up to 20 hooks, 50 tags, and 64 `AllowedTools` entries.
- The harness caps iterations, output tokens, and wall time. It has no dollar cap, so spend limits belong in the AI gateway.
- `UpdateHarness` replaces the whole filesystem mount list, so send the full list each time. Infrastructure as code handles this naturally.
- Because `DEFAULT` always moves, Temporal must invoke a named endpoint, never `DEFAULT`.

### Runtime deployment (OpenCode or pi)

1. Build an `linux/arm64` image that serves `POST /invocations` and `GET /ping` on port 8080. Push it to ECR.
2. Call `create_agent_runtime` with the container URI, a network configuration (`PUBLIC` or `VPC`), the role ARN, and lifecycle settings (`idleRuntimeSessionTimeout`, `maxLifetime`).
3. Create named endpoints and move them between versions with `update_agent_runtime_endpoint`.
4. The runtime does not run a model loop for you. The container entrypoint reads the agent definition, configures OpenCode or pi, and runs the task.

The harness `environmentArtifact` option is different. It lets you bring a container to a harness, but the harness overrides the image's `ENTRYPOINT` and `CMD`, so the Strands loop still runs. It is a way to preinstall tools for a harness agent, not a way to run OpenCode.

**Runtime Instances (new in August 2026).** A second compute type runs agents on EC2 instances that AgentCore manages in your account. Sessions can last up to 14 days, GPU families are supported, one session can host several agents on a shared filesystem, and networking must be VPC. Linux `x86_64` and `arm64` both work. The docs I read did not give instance sizes. This matters for two tripwires: the default 8 hour session cap and the 2 vCPU, 8 GB microVM limit (from the quotas page). It is the first thing to check for the fn-core sandbox question.

### Infrastructure as code

- CloudFormation resources exist for the harness and the harness endpoint. A harness update bumps its `Version` attribute, and an endpoint only moves when its `TargetVersion` changes.
- Terraform has `aws_bedrockagentcore_harness`. The listing I found did not include a harness endpoint resource, so that piece may need CloudFormation or an API call. Runtime, runtime endpoint, gateway, and an API key credential provider have Terraform resources.
- CDK `aws-bedrockagentcore` L2 constructs graduated to stable in May 2026. Harness coverage in them is **unverified**.
- Field Nation already has an OpenTofu module standard (`fn-opentofu-module`), so OpenTofu with the AWS provider is the natural default. Version requirements for the AgentCore resources were not stated.
- The `agentcore` CLI (`agentcore create`, `agentcore add harness`, `agentcore deploy`) scaffolds a project and deploys it. Its project file format was not documented in the pages I read, so do not build on it for the pipeline.

### S3

S3 appears in four places:

1. **Skills.** `{"s3": {"uri": "s3://bucket/skills/name/"}}`. The execution role needs `s3:GetObject` and `s3:ListBucket`. Each skill must be 1 GB or smaller. Skills are fetched once per session and re-fetched when a new session starts. Over a VPC they work through an S3 gateway endpoint without a NAT gateway.
2. **Skill pinning.** The S3 config has no object version field, and the Git config has no branch, tag, or commit field. The GA announcement says Git skills can be pinned to a commit or branch, but the skills reference does not show how. **Treat pinning as unverified.** The safe pattern is immutable prefixes (`skills/<name>/<version>/`), published by the ai-toolbox release pipeline, so a version in the definition always points at fixed content.
3. **Workspace mounts.** An S3 Files access point mounts a bucket as a filesystem under `/mnt` and syncs bidirectionally with the bucket. It requires VPC mode. A harness takes up to two EFS and two S3 Files mounts. This fits the plan of keeping repo snapshots in S3 and fetching the git delta at session start. Whether a mount can be made read-only was not documented, so enforce read-only with the access point and bucket policy.
4. **Runtime artifacts.** Runtime Instances accept a container image or an S3 source.

Private git skill sources need a personal access token held in AgentCore Identity and internet egress (a NAT gateway), with a 60 second clone timeout. Prefer S3 skills in VPC mode.

### Identity

Four separate pieces:

**1. Who can invoke an agent (inbound).**

- IAM (SigV4): the caller needs `bedrock-agentcore:InvokeHarness` and `bedrock-agentcore:InvokeAgentRuntime` on the harness. This is the simplest fit for Temporal workers calling agents. It does not carry a per-user identity to tools. The docs say SigV4 support for that is planned.
- JWT: a `customJWTAuthorizer` with a discovery URL and allowed clients or audiences. Okta works as the identity provider. The Okta doc covers discovery URLs and audiences, and says to add a `client_id` claim to access tokens because Okta omits it by default. JWT is what carries per-user identity, so it is the path for interactive human runs later.
- **Unverified:** whether one harness can accept both SigV4 and JWT callers.
- Callers who should not run raw commands must not be granted `bedrock-agentcore:InvokeAgentRuntimeCommand`. The harness `allowedTools` setting does not restrict that API.

**2. What the agent can do in AWS (execution role).**

- One role per harness, trusted by `bedrock-agentcore.amazonaws.com`.
- The base policy covers Bedrock model invocation, CloudWatch logs and metrics, X-Ray, and the AgentCore workload identity tokens. Optional statements add S3 skill reads, API key and OAuth2 credential provider reads, `bedrock-agentcore:InvokeGateway`, hook targets (Lambda, SNS, EventBridge), and private ECR pulls.
- The sample uses `"*"` in places. Scope everything for production.
- The role policy references a workload identity named `harness_<agentName>-*`. That supports giving every agent its own identity. **Unverified:** whether it is created automatically per harness.
- Our pipeline should generate the role from the agent definition. The tool tier decides which optional statements it gets.

**3. What the agent can reach (outbound credentials).** AgentCore Identity's token vault stores credentials behind credential providers:

- API key providers hold model provider keys, MCP header secrets (referenced as `${arn:...}`), and private git tokens. They are backed by Secrets Manager.
- OAuth2 providers include an `OktaOauth2` vendor and a `CustomOauth2` option with on-behalf-of token exchange. Private Key JWT (July 2026) signs with a KMS key so the private key never leaves KMS.
- The Consent Portal (September 2026) lets end users grant an agent access on their behalf.
- Gateway outbound auth can be IAM, none, or OAuth through a credential provider.

**4. GitHub access.** The docs I read do not describe a GitHub App flow. Options: a personal access token or machine user in the token vault (simple, one shared identity), a user-delegated GitHub OAuth flow, or a short-lived GitHub App installation token minted by the Temporal workflow per run and injected into the session. The third best matches "each agent is its own principal with repo-scoped, PR-only access." It sits outside AgentCore Identity, so it is the main identity gap to spike.

### Tool access and enforcement

- The `agentcore_gateway` tool type exposes every tool on the referenced gateway to the agent. Narrowing by name with `allowedTools` globs (for example `@server/tool`) only guides the model. It is not a security boundary.
- Real enforcement: Cedar policies on the gateway (by caller, condition, and arguments) and IAM. A practical shape is one gateway per tool tier, with Cedar rules naming which agents may use each.
- Lifecycle hooks (September 2026) run at `before_invocation`, `before_tool_call`, `after_tool_call`, and `after_invocation`. A Lambda hook can return a synchronous allow or deny. SNS and EventBridge targets get non-blocking events. Use a hook to log every tool call to the run ledger and to deny pushes to protected branches. Review the hook docs before treating `after_tool_call` as an audit signal for inline functions.
- The harness can call a custom OpenAI-compatible endpoint (`apiBase`, September 2026). That settles how agents reach the AI gateway. **Unverified:** whether per-run headers (a run ID for cost capture) can be sent with it.

### Network

Sessions run on the public network by default. VPC mode needs interface endpoints for `ecr.dkr` and `ecr.api`, an S3 gateway endpoint, and `bedrock-runtime` if the agent calls Bedrock. Without them, sessions fail to start. EFS and S3 Files mounts need VPC mode. Cloning from GitHub needs an egress path, so VPC mode plus repo access means a NAT gateway or equivalent with an allowlist.

### Observability

Traces go to CloudWatch. Since July 2026, spans can go to a per-agent log group (`/aws/bedrock-agentcore/runtimes/<agent_id>-<endpoint_name>`), and new agents use it by default. That requires CloudWatch Transaction Search, `logs:PutResourcePolicy` on the role, and ADOT 0.18.0 or later. Harness tags (up to 50) can carry `agent_id`, `owner`, and version for cost allocation after the tags are activated in billing. Per-run cost still comes from the AI gateway.

## Compatibility mapping

| Definition field | AgentCore target | Note |
|---|---|---|
| `agentId` | `HarnessName` or runtime name | snake_case, 40 characters, replacement on change |
| `fn.owner`, `agentId`, version | `Tags` | Up to 50 |
| `harness.model` | `Model` | Provider types: Bedrock, OpenAI (with `apiBase`), Gemini, LiteLLM |
| `instructions.md` | `SystemPrompt` | Rendered at deploy time |
| `harness.skills` | `Skills` | Prefer S3 with versioned prefixes |
| `harness.tools` | `Tools` | `agentcore_gateway`, `remote_mcp`, built-ins |
| `harness.allowedTools` | `AllowedTools` | Model guidance only |
| `harness.maxIterations`, `maxTokens`, `timeoutSeconds` | `MaxIterations`, `MaxTokens`, `TimeoutSeconds` | No dollar cap |
| `harness.memory` | `Memory` | Managed by default, or disabled |
| `harness.hooks` | `Hooks` | Up to 20 |
| `fn.toolTier` | Gateway choice, Cedar policy, role statements | Enforced outside the harness |
| `fn.triggers` | Intake Lambda and Temporal schedules | Not an AgentCore concept |
| `fn.evals` | CI only | Not deployed |
| Runtime agents: `image`, `lifecycle`, `network` | `containerConfiguration`, `lifecycleConfiguration`, `networkConfiguration` | Container contract on port 8080 |

## Compatibility spike list

1. Invoke a named harness endpoint. The docs show `InvokeHarness` with a harness ARN but I found no endpoint qualifier.
2. Confirm Git skill pinning, or commit to S3 versioned prefixes.
3. Run a harness with both SigV4 and Okta JWT callers, or decide to split by harness.
4. Choose and test the GitHub credential path.
5. Mount an S3 Files access point read-only with a repo snapshot, and test the git delta fetch through a NAT allowlist.
6. Check whether each harness gets its own workload identity automatically.
7. Get Runtime Instances sizing and test the fn-core image.
8. Run OpenCode and pi on Runtime with MCP, git, and tests.
9. Send a run ID header through the harness `apiBase` path to the AI gateway.
10. Build a Lambda hook that logs tool calls and denies a protected-branch push.
11. Confirm OpenTofu coverage for harness endpoints and Runtime, or fall back to CloudFormation.

## Sources

- [AgentCore harness overview](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness.html), [get started](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-get-started.html), [environment and filesystem](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-environment.html), [skills](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-skills.html), [tools](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-tools.html), [versioning and endpoints](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-versioning.html), [security](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/harness-security.html)
- [AgentCore Runtime versioning](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agent-runtime-versioning.html), [custom agent container contract](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/getting-started-custom.html), [Runtime Instances](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-instances-how-it-works.html)
- [AgentCore Identity with Okta](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/identity-idp-okta.html)
- [AgentCore release notes](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/release-notes.html)
- [CloudFormation AWS::BedrockAgentCore::Harness](https://docs.aws.eu/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-harness.html), [HarnessEndpoint](https://docs.aws.eu/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-harnessendpoint.html)
- [Terraform resources for AgentCore (AWS Fundamentals)](https://awsfundamentals.com/terraform/bedrockagentcore/bedrockagentcore-harness)
- [Agent Skills overview](https://agentskills.io/)
- [A2A v1.0 Agent Card (Agentic AI Foundation)](https://aaif.io/blog/a2a-v1-0-a-builder-s-guide-part-1-discovery-tasks-and-clients)
- [Open Agent Specification docs](https://oracle.github.io/agent-spec/26.1.2/)
- [Declarative agent specification survey (Zylos)](https://zylos.ai/research/2026-04-22-declarative-agent-specification-configuration-languages/)
