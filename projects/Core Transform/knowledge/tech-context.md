# Core Transform (Pearl) — Tech Context

*Relevant technical background for this project.*

## Tech Stack (Under Evaluation)

The team is in an active evaluation phase. Nothing is locked in yet — this is the landscape as of early April 2026.

| Category | Options Being Evaluated | Current Lean | Notes |
|---|---|---|---|
| **LLM Provider** | AWS Bedrock | Bedrock | Using Pearl Sandbox AWS account (400844546951) |
| **Agentic Framework** | PydanticAI, LangGraph + LangChain, Strands | **Decided: PydanticAI** | Aligned 2026-04-14 — native Bedrock client, Temporal integration, type-safe, mature eval tooling. Strands is explicitly NOT the go-forward framework. |
| **Runtime** | AWS AgentCore Runtime vs self-managed EKS | EKS (strong candidate) | Not a commitment yet externally, but Chuck/Spencer/Mike aligned on being bullish about EKS with AWS. AgentCore Runtime still being evaluated for specific components only. |
| **Durable Execution** | Temporal Cloud, Step Functions, others | Temporal Cloud (hosted) | Mike Mueller's strong input: lean toward hosted/managed to minimize self-hosted maintenance burden — FN doesn't have a large SRE/platform team. PydanticAI has native Temporal integration. Don't commit externally — frame as "evaluating options." |
| **Language** | Python | Python | Using `uv` for versioning/package/virtualenv management |
| **Observability** | CloudWatch + Pydantic Logfire + OTel → Datadog APM | Layered approach | CloudWatch forwarding for AWS-native telemetry, Logfire for agent reasoning traces (native to PydanticAI), OTel exports to Datadog APM (FN's existing standard). Stack not yet decided — AgentCore Observability could complement or complicate. |
| **Build/Deploy** | Cloud Native Buildpacks (CNB) | CNB | Python buildpack exists in fieldnation org |
| **MCP** | AgentCore Gateway, FastMCP on EKS, Gravitee MCP gateway | Open | FN has no MCP layer yet. AgentCore Gateway is a candidate. Spencer leans toward FastMCP in EKS. Gravitee (FN's incoming API gateway) also has MCP/AI agent management capabilities worth evaluating. |
| **Guardrails/Policy** | AgentCore Policy (Cedar), Bedrock Guardrails | Open | Two-layer defense-in-depth: Guardrails filters what the model *says*, Policy filters what the agent *does*. Cedar enforcement at tool-call boundary addresses 1% error tolerance on financial/assignment decisions. |
| **Auth (inbound + outbound)** | AgentCore Identity + Token Vault | Open (critical gap) | Agentic auth is the blurriest gap. FN's current auth is Cognito user pool + custom auth layer, NOT truly OAuth2 compliant. No app registrations. Ian Loverink's core-ops team owns auth. AgentCore Identity feasibility uncertain given FN's non-standard setup. |
| **Agent Memory** | AgentCore Memory, Bedrock Knowledge Bases, self-rolled (Neo4j/LangChain, GraphRAG, Pinecone/Weaviate) | Open — leaning on external specialist | Pearl wants long-term memory only (short-term state is Pearl's responsibility). Key unknowns for specialist: Batch API hydration control, dedicated vs shared index isolation (neighbor leakage risk), Bedrock KBs as cleaner alternative. Portability concern if AgentCore can't give sufficient control over ingestion. Per 2026-04-15 Spencer/Chuck/Mike arch discussion. |

## Key Technical Implications

- **Pearl owns the reference architecture** — Explicitly established 2026-04-14. Safin/NetDev's POC work is NOT the reference architecture — Pearl is. This needs to be reconciled in the Safin meeting (2026-04-15).
- **Component-level AgentCore adoption, not wholesale** — Evaluating which AgentCore components add value regardless of runtime choice. The meta-question for every component: (1) Does it work standalone without AgentCore Runtime? (2) What's the minimum commitment surface? Those answers determine adoptable toolkit vs platform lock-in.
- **Hybrid execution model** — The architecture needs to support a flexible split between LLM reasoning loops (agentic) and deterministic/rules-based workflow execution. Not everything should be an agent.
- **SDK/Platform mindset** — The goal is a common base SDK that product teams can build on, not one-off agent implementations.
- **IAM/permissions** — Early friction with Bedrock IAM policies in the sandbox. Kimberly is tracking missing read-only actions and coordinating with LAZ team for custom JumpRole policies.
- **Gravitee coexistence** — FN's integration team is setting up Gravitee API Gateway to replace AES (Ambassador Edge Stack) for Integration v3. Gravitee also has Identity/Access Management and AI Agent Management capabilities. Routing topology between Gravitee and AgentCore Gateway needs to be worked out.
- **Tenant isolation is critical** — FN's tenancy model is complex: buyer_id (company_id) is the primary tenant anchor. Providers belong to a Service Company or a "ghost company" (so they always have a company_id). Within a Buyer: Buyer > Client > Projects. Strict isolation needed especially for upmarket/government clients.
- **Context Engine (emerging) as the common glue** — Semantic, tenant-isolated context store distinct from structured API retrieval. Purpose: user preferences, industry knowledge, relational patterns enabling personalization and explainability across agents. Still early/vague; sourcing and ingestion quality is the harder problem than retrieval. Scope isolation / namespacing will be keyed off Mike Mueller's Resource Authorization Service (EAV context store, Kafka-driven) — the authoritative answer for "does this user have access to this resource." Per 2026-04-15 arch discussion.

## Open Questions / TBD

- AgentCore component-by-component evaluation — Gateway, Policy, Identity, Memory, LLM Gateway all being assessed for standalone usability outside AgentCore Runtime
- What product use cases will be prioritized first (Linda Lo sending use cases by mid-April)
- On-site workshop targeting late May / early June for concept vision sign-off
- Q2 commitments — Maria driving this, team still forming plans
- Safin/NetDev reconciliation — meeting 2026-04-15 to align NetDev POC work with Pearl's reference architecture
- AgentCore Identity feasibility given FN's non-standard auth (Cognito + custom layer, not OAuth2 compliant)
- Gravitee vs AgentCore Gateway for MCP layer — shorter reach for Gravitee given existing adoption?
- Agent memory architecture: AgentCore Memory vs Bedrock Knowledge Bases vs self-rolled graph/vector stack. Waiting on external AWS specialist to answer batch-API hydration control, index isolation strategy (dedicated vs shared with metadata filtering), and hooks for managed auto-memory.
- Context Engine sourcing/ingestion strategy — what existing unstructured/JSONB data in FN already qualifies, chunking/vectorization approach, how to auto-update long-term memory from agent interactions without manual cron-driven indexing.

## What NOT to Say Externally

- That EKS is decided — it's a "strong candidate," not a commitment
- That Temporal is decided — frame as "evaluating durable execution options"
- That observability tooling is decided — we're exploring combinations
- Q3 automation commitments or H2 product timelines
- Specific details about Travis's strategic vision or legal/IC classification concerns
