# AI Software Factory — Implementation Plan (Ticket-Ready)

> ⚠️ **SUPERSEDED FOR H2 (as of 2026-07-14).** The 2026-07-13 H2 Plan Review deprioritized the AWS production build-out described here in favor of an embed-first approach — see [Embed-First Pivot](./overview.md#embed-first-pivot--pilot-scope-narrowing-2026-07-13) in overview.md and the [meeting synthesis](./meetings/2026-07-13-h2-plan-review-ai-sdlc.md). Current H2 plan starts from the Q3 OKR (PR merged via factory process by mid-August — [OKR Tracker](https://docs.google.com/spreadsheets/d/10iTDkoL2Qpnw-a-5Ceo1m9IXwhG3BHgvSu7IDNETCm0/edit)), not this phased ticket plan. Keep for reference if/when the full AWS build resumes (no earlier than Q4 per the 7/13 discussion).

**Purpose:** A build plan for the LangGraph + AWS PEV factory, structured so each story can become a Linear ticket. Covers Phases 0–3, from human-run provisioning to AI-buildable code.

**Companion docs:** [AI Software Factory - Architecture & Overview.md](<./AI Software Factory - Architecture & Overview.md>) (architecture + rationale; renamed 2026-07-09 from "LangGraph PEV POC.md") · [AI Software Factory Architecture (Lucid)](https://lucid.app/lucidchart/9b39ee36-446e-4192-b4f5-bb98fd66ef28/edit) (system/deployment view + PEV loop flow, merged into one diagram; replaces the retired AISF System Architecture.drawio and LangGraph PEV POC.drawio files).

**Owner:** Chuck Haines, Team Moissanite · 2026-07-08; PRE-VERIFY step added to the loop 2026-07-09 (credit: Mykhailo Sakhnik) — reflected below in E1.2 and E1.6.

*The AI Software Factory is the technical-build workstream of the broader **AI-DLC** initiative (rebranded 2026-07-09 — see [program/overview.md](./overview.md#rebrand-to-ai-dlc-2026-07-09)).*

---

## How to use this document

Each **Epic** is a body of work (candidate Linear project or epic). Each **Story** underneath is a candidate ticket. Story IDs (`E0.1-1`) are placeholders — replace with `MOI-###` on creation.

**Tag conventions** (per interview):

- `Type: Human` — provisioning / setup / decisions a person does (mostly Chuck/Joe/Redwan on infra + architecture).
- `Type: AI-buildable` — code the contractors write (with core-team review), and eventually the factory can write itself.
- `Owner:` — suggested lead. Coding leads = contractors (Vitalii, Asad, Mykhailo); infra + architecture leads = Chuck / Joe / Redwan. `Review:` names the architecture/infra reviewer where relevant.
- `Depends on:` — hard prerequisites (blocking).

**Key decisions baked in** (confirmed 2026-07-08):

1. Scope runs through **all phases (0–3)**.
2. **Greenfield, dedicated AWS account.** Manual provisioning for the POC; migrate to **OpenTofu** for production.
3. First target repos: **FN-Core** (PHP monolith) and **provider-match** (NestJS/TypeScript). Two parity sandbox images.
4. **Agent tracing stays in AWS** (AgentCore Observability + CloudWatch + OTEL). **DataDog is the fallback** for agent tracing/logging if AWS-native proves insufficient. **No LangSmith.**
5. Ticket structure: **epics + stories, no estimates.**
6. **Sonar wired in as an additional Verify signal** (synced from the Lucid diagram, 2026-07-13): a PR-analysis check runs on every push to both repos and posts findings to GitHub; VERIFY separately queries Sonar via an MCP tool. Additive only — doesn't replace Verify's bespoke, contract-aware review (see architecture doc, section 8, item 10).

> Note: Linear isn't authorized in this session, so tickets aren't auto-created. Once Linear is connected (and with your go-ahead — writes need approval per workspace rules), these epics/stories can be pushed directly.

---

## Epic map

| Phase | Epic | Title | Primary type |
|---|---|---|---|
| 0 | E0.1 | AWS account & baseline (greenfield) | Human |
| 0 | E0.2 | Integrations & auth (Linear, GitHub, Sonar, secrets) | Human + AI |
| 0 | E0.3 | Webhook ingress (API GW + Lambda + event bus) | AI-buildable |
| 0 | E0.4 | Sandbox parity images (FN-Core + provider-match) | Human + AI |
| 0 | E0.5 | Bug-fix ticket selection & eval baseline | Human |
| 1 | E1.1 | PEV app scaffolding & model config | AI-buildable |
| 1 | E1.2 | INTAKE + PRE-VERIFY + PLAN agent | AI-buildable |
| 1 | E1.3 | PLAN REVIEW + Devil's Advocate + async approval | AI-buildable |
| 1 | E1.4 | EXECUTE agent + PR handling | AI-buildable |
| 1 | E1.5 | VERIFY agent + findings loop | AI-buildable |
| 1 | E1.6 | Orchestrator control (cap, routing, ship/escalate) | AI-buildable |
| 1 | E1.7 | Deploy to AgentCore + durable state + observability | Human + AI |
| 1 | E1.8 | End-to-end POC runs on real tickets | Human + AI |
| 2 | E2.1 | Durability hardening | AI-buildable |
| 2 | E2.2 | Observability & evaluation | AI + Human |
| 2 | E2.3 | Model validation | Human + AI |
| 2 | E2.4 | Approval grammar & cap tuning | AI-buildable |
| 2 | E2.5 | Security & guardrails review | Human |
| 3 | E3.1 | IaC migration to OpenTofu | Human + AI |
| 3 | E3.2 | Temporal orchestration port | AI-buildable |
| 3 | E3.3 | ECS/EKS production runtime | Human |
| 3 | E3.4 | Generalize event-driven gates | AI-buildable |
| 3 | E3.5 | Reviewer panel expansion | AI-buildable |
| 3 | E3.6 | Broaden ticket types & rollout | Human + AI |

---

# Phase 0 — Foundations (spikes)

Goal: everything the POC needs before the loop can run — account, auth, ingress, sandbox, target tickets.

## E0.1 — AWS account & baseline (greenfield)

Stand up a dedicated, isolated AWS account for the factory. Manual/console for now; captured so it can be re-expressed in OpenTofu later (E3.1).

**E0.1-1 · Create dedicated AWS account & attach to org**
`Type: Human` · `Owner: Chuck` · `Review: Joe`
Create the greenfield account, attach to the FN AWS Organization, apply org SCP guardrails, set billing alerts and a cost budget.
*Acceptance:* Account exists under the org; billing alarm + budget configured; root locked down with MFA.

**E0.1-2 · Wire IAM Identity Center (SSO) & role model**
`Type: Human` · `Owner: Chuck` · `Review: Joe`
Federate the account via IAM Identity Center; define least-privilege permission sets for admins (core team) and a CI/deploy role.
*Acceptance:* Chuck/Joe/Redwan can SSO in with scoped roles; a deploy role exists for later automation.

**E0.1-3 · Baseline networking (VPC, subnets, endpoints)**
`Type: Human` · `Owner: Joe` · `Review: Chuck`
Create VPC with private subnets, NAT, and VPC endpoints for Bedrock, S3, Secrets Manager, ECR, CloudWatch. Default to private; no public compute.
*Acceptance:* Private subnets route to AWS services via endpoints; egress is controlled; documented CIDR plan.

**E0.1-4 · Enable Bedrock model access**
`Type: Human` · `Owner: Chuck` · `Review: Redwan`
Request/enable Bedrock access for Claude (Opus, Sonnet) and OpenAI (GPT-5.5, Codex) in the chosen region; confirm inference-profile IDs and quotas.
*Acceptance:* All four models invokable from the account; current inference-profile IDs recorded; quota/throttling limits noted.

**E0.1-5 · Account guardrails & logging baseline**
`Type: Human` · `Owner: Joe` · `Review: Chuck`
Turn on CloudTrail, AWS Config, GuardDuty; central log bucket; baseline Bedrock Guardrails policy stub.
*Acceptance:* Audit logging on; security-baseline services active; log retention set.

## E0.2 — Integrations & auth

Connect Linear, GitHub, and Sonar as MCP tools (plus Sonar's native PR-analysis check) with least-privilege OAuth, stored securely.

**E0.2-1 · Provision Linear app/OAuth & scopes**
`Type: Human` · `Owner: Redwan` · `Review: Chuck`
Register the Linear integration; scope to read issues/comments/labels and gated writes (state, comments, PR links). Define the start label and comment grammar reservation.
*Acceptance:* Linear OAuth issues a token with correct scopes; start-label name agreed; stored in Secrets Manager (E0.2-3).

**E0.2-2 · Provision GitHub app/OAuth & scopes**
`Type: Human` · `Owner: Redwan` · `Review: Chuck`
Register a GitHub App (or OAuth) scoped to the two target repos: branch/commit/PR, diff + review-comment reads, gated review comments.
*Acceptance:* GitHub app installed on FN-Core + provider-match with least-privilege scopes; token stored securely.

**E0.2-3 · Secrets & credential-provider wiring**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Joe`
Store Linear/GitHub tokens in Secrets Manager; configure AgentCore Identity OAuth credential providers; establish short-lived-token retrieval pattern. No secrets in code/docs.
*Acceptance:* App code fetches creds at runtime from Secrets Manager / AgentCore Identity; rotation path documented; secret-scan clean.

**E0.2-4 · MCP tool clients for Linear & GitHub**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Thin, tested tool wrappers the agents call (Linear read/write, GitHub read/write) with idempotent write helpers keyed on `(ticket_id, round)`.
*Acceptance:* Unit-tested clients; idempotent PR-open and comment helpers; read paths verified against both repos. `Depends on: E0.2-1, E0.2-2, E0.2-3`

**E0.2-5 · Configure Sonar PR analysis on target repos** *(new, synced from Lucid diagram, 2026-07-13)*
`Type: Human` · `Owner: Redwan` · `Review: Joe`
Wire Sonar's PR-analysis check to run on every push to FN-Core and provider-match, posting findings to GitHub; provision the Sonar MCP tool for VERIFY to query directly.
*Acceptance:* Sonar check runs on every PR push and comments on GitHub; Sonar MCP tool reachable from the factory account. `Depends on: E0.2-2`

## E0.3 — Webhook ingress

The event front door: two triggers (start label, decision comment), one ingress, durable and idempotent.

**E0.3-1 · API Gateway + Lambda webhook endpoint**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Joe`
HTTPS endpoint receiving Linear + GitHub webhooks with signature verification.
*Acceptance:* Signed webhooks accepted, unsigned rejected; deployed behind API Gateway; logs to CloudWatch.

**E0.3-2 · Router logic: start vs decision, dedupe, correlate**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Classify events (start label → new run; approve/changes comment → resume); dedupe on event/comment ID; correlate run identity to Linear issue ID; drop events with no matching parked run.
*Acceptance:* Duplicate deliveries deduped; out-of-order/decisions-with-no-run handled; unit tests cover redelivery and mismatched gates. `Depends on: E0.3-1`

**E0.3-3 · Durable signal queue (EventBridge + SQS FIFO + DLQ)**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Joe`
Convert accepted webhooks into durable signals for the orchestrator; DLQ for poison messages; at-least-once semantics documented.
*Acceptance:* Events survive downstream restarts; DLQ catches failures; replay-safe. `Depends on: E0.3-2`

## E0.4 — Sandbox parity images (biggest lift)

Reproduce each repo's real dev toolchain so in-sandbox tests are trustworthy.

**E0.4-1 · FN-Core Docker-parity sandbox image**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
Build an image from FN-Core's Docker dev config (services, DB, fixtures, env). Warm it so per-run setup doesn't dominate wall-clock.
*Acceptance:* FN-Core test suite runs green in-sandbox from a fresh clone; documented build; cold-start time measured. `Depends on: E0.1-3`

**E0.4-2 · provider-match (NestJS/TS) parity sandbox image**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Redwan`
Same for provider-match: Node toolchain, deps, test DB/fixtures.
*Acceptance:* provider-match tests + lint + typecheck run green in-sandbox from a fresh clone. `Depends on: E0.1-3`

**E0.4-3 · Sandbox network posture & registry access**
`Type: Human` · `Owner: Joe` · `Review: Chuck`
Scope sandbox egress to only the registries/deps tests require; push images to ECR.
*Acceptance:* Sandbox reaches required registries only; images in ECR; egress rules documented. `Depends on: E0.4-1, E0.4-2`

**E0.4-4 · Fresh-clone + branch bootstrap in sandbox**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
Deterministic clone + branch (`ai-factory/<LINEAR-ID>`) creation and repo seeding on run start.
*Acceptance:* Given an issue ID, sandbox produces a clean checkout on the named branch, reproducibly. `Depends on: E0.4-1, E0.4-2`

## E0.5 — Bug-fix ticket selection & eval baseline

Pick the tractable first targets and capture ground truth to judge the POC.

**E0.5-1 · Curate first well-bounded bug-fix tickets**
`Type: Human` · `Owner: Chuck` · `Review: Redwan`
Select a small set of well-bounded bug fixes across FN-Core and provider-match (clear repro, bounded blast radius, existing tests where possible).
*Acceptance:* 6–10 candidate tickets identified with links, repro notes, and expected fix shape.

**E0.5-2 · Ground-truth & scoring rubric**
`Type: Human` · `Owner: Chuck` · `Review: Joe`
For each ticket, record the human fix (diff) and a scoring rubric (correctness, iteration count, review pass/fail) to compare against factory output.
*Acceptance:* Rubric agreed; ground-truth diffs stored for comparison.

---

# Phase 1 — POC (LangGraph on AgentCore)

Goal: the full PEV loop running end-to-end on real bug-fix tickets, doubling as M3 platform evidence.

## E1.1 — PEV app scaffolding & model config

**E1.1-1 · Repo, project layout & CI skeleton**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Create the factory repo (DeepAgents + LangGraph), packaging, lint/test CI, and an `AGENTS.md`.
*Acceptance:* `pip install` + tests run in CI; project structure documented.

**E1.1-2 · Bedrock model bindings (Claude + OpenAI Responses API)**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Configure per-agent model clients via `langchain-aws`: Claude (Messages API) and OpenAI (Bedrock Responses API); centralize model IDs.
*Acceptance:* Each model invokable from code with a smoke test; tool-calling confirmed per provider. `Depends on: E0.1-4`

**E1.1-3 · Shared state schema & virtual filesystem**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Define `FactoryState` (round, verdict, ticket, artifacts) and the shared filesystem for plan/diffs/findings.
*Acceptance:* State round-trips through the checkpointer; artifacts written to the branch/S3, not agent memory.

**E1.1-4 · Durable checkpointer (AgentCore Memory / Postgres)**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Joe`
Compile the graph with a durable checkpointer, `durability="sync"`; make it swappable (Memory for POC, Postgres/RDS later).
*Acceptance:* A killed run resumes from the last completed node; checkpointer is config-swappable.

## E1.2 — INTAKE + PRE-VERIFY + PLAN agent

**E1.2-1 · INTAKE node (no LLM)**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Pull the Linear issue, create the branch, seed the filesystem with ticket context.
*Acceptance:* Given an issue ID, produces a seeded working state + branch. `Depends on: E0.2-4, E0.4-4`

**E1.2-2 · PRE-VERIFY agent (Sonnet, one-shot task contract)** *(new, added 2026-07-09, credit: Mykhailo Sakhnik)*
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
DeepAgent on Sonnet, one shot, no retry loop. Builds the task contract Planner needs, shaped by ticket type: fix → reproducer-first, feat → acceptance-test-first, chore → validation/safety-first. Uses the sandbox to write and run the reproducer/acceptance test/baseline; read-only on source otherwise. Emits `contract_built=True/False`; on `False`, routes straight to ESCALATE — no retry.
*Acceptance:* Given a ticket of each type, produces a structured contract (verification commands, assumptions, risks, expected validation artifact) written to the branch; a ticket without a reproducible contract escalates instead of reaching Planner. `Depends on: E1.2-1, E0.4-4`

**E1.2-3 · PLAN agent (Claude Opus)**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Planner DeepAgent: reads the ticket, the PRE-VERIFY task contract, and the codebase (read-only tools), emits an implementation plan/spec via `write_todos`.
*Acceptance:* Produces a structured, SDDF-style spec artifact on the branch for a sample ticket, grounded in the task contract. `Depends on: E1.1-3, E1.2-2`

## E1.3 — PLAN REVIEW + Devil's Advocate + async approval

**E1.3-1 · PLAN REVIEW agent + Devil's Advocate subagent (GPT-5.5)**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Reviewer DeepAgent spawns an isolated Devil's Advocate that attacks the plan (edge cases, risks, simpler alternatives); reviewer consolidates and revises or forwards.
*Acceptance:* Review produces a consolidated critique; can send a plan back for revision (bounded). `Depends on: E1.2-3`

**E1.3-2 · Post plan to Linear & park run**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Redwan`
Post the plan as a Linear comment and park the run durably on a signal correlated to the issue ID.
*Acceptance:* Plan appears in Linear; run releases compute and waits durably. `Depends on: E1.3-1, E0.3-3`

**E1.3-3 · Resume on approval / route on changes**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
`/approve` → EXECUTE; `/changes <notes>` → back to PLAN with notes, regenerate, re-post, re-park (capped).
*Acceptance:* Both paths work end-to-end from a real Linear comment; changes loop respects the cap. `Depends on: E1.3-2`

## E1.4 — EXECUTE agent + PR handling

**E1.4-1 · EXECUTE orchestrator (Claude Sonnet)**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Turns the approved plan into todos, delegates to coding subagents, uses the sandbox to build/test/git.
*Acceptance:* Converts a plan into applied code changes on the branch for a sample ticket. `Depends on: E1.3-3, E0.4-4`

**E1.4-2 · Codex coding subagents (implementer + test-author)**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Redwan`
OpenAI Codex subagents write code + tests for assigned plan items.
*Acceptance:* Subagents produce compiling changes + tests; integrate with the executor. `Depends on: E1.1-2, E1.4-1`

**E1.4-3 · Idempotent PR open/update**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Joe`
Check for an existing PR on the branch and update it; never open duplicates on resume/retry.
*Acceptance:* Re-running EXECUTE updates the same PR; targets `main`. `Depends on: E0.2-4, E1.4-1`

## E1.5 — VERIFY agent + findings loop

**E1.5-1 · VERIFY agent (Sonnet, read-only on source)**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
Fresh-context verifier: pulls PR diff, reviews vs plan + FN standards, runs tests/lint/typecheck/security scan in-sandbox, emits structured findings (severity, file:line, required change).
*Acceptance:* Produces machine-readable findings; write-denied on `src/**`. `Depends on: E1.4-3`

**E1.5-2 · Ingest human PR review comments**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Redwan`
Fold GitHub PR review comments into the same findings contract so human feedback flows through the loop.
*Acceptance:* Human comments appear as findings routed back to EXECUTE. `Depends on: E1.5-1`

**E1.5-3 · Opus advisor subagent**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
Isolated Opus advisor for hard calls; voluntary consultation plus deterministic triggers (auth/data/migrations).
*Acceptance:* Advisor consulted on trigger paths; returns one focused opinion without bloating verifier context. `Depends on: E1.5-1`

**E1.5-4 · Ingest Sonar findings via MCP tool** *(new, synced from Lucid diagram, 2026-07-13)*
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
VERIFY queries the Sonar MCP tool for code-quality/security findings on the PR and folds them into the same findings contract as tests/lint/security-scan/human-comment findings. Additive signal only — does not replace VERIFY's own review.
*Acceptance:* Sonar findings appear in VERIFY's structured output alongside its own findings, attributed by source. `Depends on: E0.2-5, E1.5-1`

## E1.6 — Orchestrator control

**E1.6-1 · Round cap, verdict routing, feedback loop**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Orchestrator owns the round counter and routing: pass → merge gate; fail & round<cap → EXECUTE with findings; round=cap → ESCALATE. Cap lives in state, along with PRE-VERIFY's separate one-shot limit (`contract_built=false` → ESCALATE directly, no retry, distinct from the round cap).
*Acceptance:* Loop routes correctly across all three verdict branches, plus the PRE-VERIFY → PLAN/ESCALATE branch; both caps survive restart. `Depends on: E1.2-2, E1.5-1`

**E1.6-2 · SHIP + human merge gate**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Redwan`
On pass, park at the human merge gate; on approval, merge and update Linear.
*Acceptance:* Merge only after human approval; Linear updated on ship. `Depends on: E1.6-1`

**E1.6-3 · ESCALATE as durable state**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
On round-cap exhaustion, or on a PRE-VERIFY contract-build failure, transition to ESCALATE with a summary Linear comment; never spin silently or drop the run.
*Acceptance:* Both exhausted-cap runs and failed-contract runs land in ESCALATE with a human-readable summary. `Depends on: E1.6-1`

**E1.6-4 · Idempotency keys on all external writes**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Joe`
Key Linear transitions/comments and PR actions off `(ticket_id, round)`; assume at-least-once everywhere.
*Acceptance:* Replaying any node produces no duplicate side effects. `Depends on: E1.6-1`

## E1.7 — Deploy to AgentCore + durable state + observability

**E1.7-1 · Containerize & deploy to AgentCore Runtime**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Joe`
Package the graph as a container; deploy to AgentCore Runtime with session isolation.
*Acceptance:* A run triggers and completes on AgentCore from a webhook. `Depends on: E1.1-1, E1.6-1`

**E1.7-2 · AgentCore Memory as checkpointer**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Wire AgentCore Memory as the durable checkpointer in-cluster.
*Acceptance:* Runs resume across sessions using AgentCore Memory. `Depends on: E1.1-4, E1.7-1`

**E1.7-3 · AWS-native observability wiring**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Emit agent traces via AgentCore Observability + OTEL to CloudWatch; trace identity survives retries/resumes. Keep tracing in-boundary.
*Acceptance:* End-to-end trace for a run visible in CloudWatch, spanning a restart. (DataDog fallback deferred to E2.2.) `Depends on: E1.7-1`

**E1.7-4 · Handle the 8-hour session cap across human waits**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Confirm parked runs outlive the session cap and resume in a fresh session via the durable signal.
*Acceptance:* A run parked >8h (simulated) resumes correctly. `Depends on: E1.7-2, E1.3-2`

## E1.8 — End-to-end POC runs

**E1.8-1 · First green end-to-end run (provider-match)**
`Type: Human + AI` · `Owner: Chuck` · `Review: Joe`
Run a curated provider-match bug fix through the full loop to a merged PR.
*Acceptance:* One real ticket goes ticket→merged PR with human gates; scored against rubric. `Depends on: E1.7-1, E0.5-2`

**E1.8-2 · First green end-to-end run (FN-Core)**
`Type: Human + AI` · `Owner: Chuck` · `Review: Redwan`
Same on the harder FN-Core target.
*Acceptance:* One FN-Core ticket completes end-to-end; failure modes documented. `Depends on: E1.8-1, E0.4-1`

**E1.8-3 · POC results write-up (M3 evidence)**
`Type: Human` · `Owner: Chuck` · `Review: Cory`
Summarize outcomes vs rubric, cost/latency, and the AWS-vs-GCP M3 read from real runs.
*Acceptance:* Written summary suitable for the M3 platform decision. `Depends on: E1.8-2`

---

# Phase 2 — Harden & tune

Goal: make the POC dependable and validate the model choices before productionizing.

## E2.1 — Durability hardening

**E2.1-1 · Resume-path test matrix**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Joe`
Systematically kill at each node and confirm clean resume with no duplicate side effects.
*Acceptance:* Test matrix covers every node; all resume cleanly. `Depends on: E1.6-4`

**E2.1-2 · Branch staleness / rebase policy**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
If `main` moved during a long wait, rebase (or re-plan) before the next round so VERIFY reviews against current `main`.
*Acceptance:* Stale-branch scenario handled deterministically. `Depends on: E1.6-1`

**E2.1-3 · Bounded external calls (timeouts, retries, backoff)**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Joe`
Wrap Bedrock/GitHub/Linear/test calls with timeouts + retries (Bedrock throttling, rate limits, flaky tests).
*Acceptance:* Transient failures retried; no unbounded hangs.

## E2.2 — Observability & evaluation

**E2.2-1 · Run dashboards & alerting (CloudWatch)**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Dashboards for run outcomes, round counts, latency, cost/model, escalation rate; alert on stuck/failed runs.
*Acceptance:* Dashboard live; alerts fire on a simulated stuck run. `Depends on: E1.7-3`

**E2.2-2 · DataDog fallback decision & (if needed) integration**
`Type: Human + AI` · `Owner: Chuck` · `Review: Joe`
Evaluate whether AWS-native tracing meets needs; if not, integrate DataDog for agent tracing/logging.
*Acceptance:* Documented decision; DataDog wired only if AWS-native is insufficient. `Depends on: E2.2-1`

**E2.2-3 · Eval harness against ground truth**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
Automate scoring of factory output vs the E0.5 rubric across the ticket set.
*Acceptance:* Batch eval produces per-ticket scores + aggregate. `Depends on: E0.5-2, E1.8-2`

## E2.3 — Model validation

**E2.3-1 · Validate Codex on implementation**
`Type: Human + AI` · `Owner: Redwan` · `Review: Chuck`
Confirm Codex quality/iteration count on real fixes; define a fallback (e.g., Sonnet) if it underperforms.
*Acceptance:* Evidence-based keep/swap decision for Codex. `Depends on: E2.2-3`

**E2.3-2 · Validate GPT-5.5 on plan review**
`Type: Human + AI` · `Owner: Chuck` · `Review: Joe`
Confirm the different-family reviewer adds value (uncorrelated catches) vs cost.
*Acceptance:* Evidence-based keep/swap decision for GPT-5.5. `Depends on: E2.2-3`

**E2.3-3 · Validate Opus advisor triggers**
`Type: Human + AI` · `Owner: Chuck` · `Review: Redwan`
Tune voluntary + deterministic advisor triggers to bound "confidently wrong" without over-consulting.
*Acceptance:* Trigger set tuned with data; consult rate + catch rate recorded. `Depends on: E2.2-3`

## E2.4 — Approval grammar & cap tuning

**E2.4-1 · Finalize Linear comment grammar**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Lock `/approve`, `/changes …` syntax and how notes pass into the re-plan; document for reviewers.
*Acceptance:* Grammar parsed robustly; documented; edge cases (typos, partial commands) handled. `Depends on: E1.3-3`

**E2.4-2 · Confirm plan-loop & verify-loop caps**
`Type: Human + AI` · `Owner: Chuck` · `Review: Joe`
Validate cap values (default 3 each) against real run data; confirm PRE-VERIFY stays one-shot with no retry.
*Acceptance:* Cap values set with rationale. `Depends on: E2.2-3`

**E2.4-3 · Ticket-type gate design (deferred)** *(new, added 2026-07-09, credit: Mykhailo Sakhnik)*
`Type: Human + AI` · `Owner: Chuck` · `Review: Redwan`
PRE-VERIFY builds a task contract per ticket type but doesn't yet reject tickets by type/shape before they enter the loop. Not load-bearing while the team curates tickets by hand (E0.5-1); design a real gate once intake runs unattended.
*Acceptance:* Gate design documented and, if intake is moving toward unattended operation, implemented ahead of Phase 3 rollout. `Depends on: E1.2-2, E3.6-1`

## E2.5 — Security & guardrails review

**E2.5-1 · Least-privilege & secrets audit**
`Type: Human` · `Owner: Joe` · `Review: Chuck`
Audit IAM, tool scopes, secret handling; confirm no secrets in code/logs/traces.
*Acceptance:* Audit passes; findings remediated. `Depends on: E1.7-1`

**E2.5-2 · Bedrock Guardrails / policy layer**
`Type: Human + AI` · `Owner: Chuck` · `Review: Joe`
Define guardrails (prompt-injection resistance on ingested ticket/PR text, output policy) and a Cedar-style permission policy.
*Acceptance:* Guardrails active; injection test suite passes. `Depends on: E2.5-1`

---

# Phase 3 — Productionize (Temporal)

Goal: durable, distributed, multi-day production operation; broaden scope.

## E3.1 — IaC migration to OpenTofu

**E3.1-1 · Codify account baseline in OpenTofu**
`Type: Human + AI` · `Owner: Joe` · `Review: Chuck`
Re-express Phase 0 account/network/IAM/secrets as OpenTofu modules with remote state.
*Acceptance:* `tofu apply` reproduces the baseline in a clean account; state backend secured. `Depends on: E0.1-*`

**E3.1-2 · Codify app & runtime infra in OpenTofu**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Joe`
Modules for ingress, ECR, RDS, ECS/EKS, Temporal, observability.
*Acceptance:* Full environment stands up from OpenTofu; documented per-module inputs. `Depends on: E3.1-1, E3.3-1`

## E3.2 — Temporal orchestration port

**E3.2-1 · Introduce Temporal workflow/activity split**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Port the orchestration tier to Temporal (official `temporalio[langgraph]` plugin if GA, else hand-rolled with GA APIs); tag nodes as activities vs workflow logic. Agent code unchanged.
*Acceptance:* PEV loop runs under Temporal with identical behavior; activities get retries/timeouts. `Depends on: E1.6-1`

**E3.2-2 · Durable multi-day human waits as signals**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Chuck`
Plan/merge gates become Temporal signals; long histories use continue-as-new.
*Acceptance:* A multi-day parked run resumes on signal; history stays bounded. `Depends on: E3.2-1`

**E3.2-3 · Swap checkpointer to RDS Postgres**
`Type: AI-buildable` · `Owner: Contractor (Vitalii)` · `Review: Joe`
With Temporal owning durability, move LangGraph state to in-memory/Postgres as appropriate.
*Acceptance:* Runs durable under Temporal + Postgres; AgentCore Memory dependency removed. `Depends on: E3.2-1`

## E3.3 — ECS/EKS production runtime

**E3.3-1 · Stand up Temporal (Cloud or self-hosted) + workers**
`Type: Human` · `Owner: Joe` · `Review: Chuck`
Provision Temporal (Cloud or cluster) and worker fleet on ECS Fargate/EKS with autoscaling.
*Acceptance:* Workers process tasks; cluster monitored. `Depends on: E0.1-3`

**E3.3-2 · Concurrent multi-ticket execution**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Support many runs in parallel with task queues; isolate per-run sandboxes.
*Acceptance:* N concurrent tickets run without cross-contamination. `Depends on: E3.3-1, E3.2-1`

## E3.4 — Generalize event-driven gates

**E3.4-1 · Merge gate & GitHub PR-review events via same ingress**
`Type: AI-buildable` · `Owner: Contractor (Asad)` · `Review: Chuck`
Extend the webhook pattern (dedupe/correlate) to the merge gate and GitHub PR-review events.
*Acceptance:* Merge and PR-review events drive the loop through the unified ingress. `Depends on: E0.3-3, E1.5-2`

## E3.5 — Reviewer panel expansion

**E3.5-1 · Expand Devil's Advocate into a panel**
`Type: AI-buildable` · `Owner: Contractor (Mykhailo)` · `Review: Chuck`
Add security, performance, and simplicity reviewers (multi-agent inferential review) at the plan and/or verify gates.
*Acceptance:* Panel runs; findings deduped/consolidated; measurable catch-rate lift. `Depends on: E2.3-2`

## E3.6 — Broaden ticket types & rollout

**E3.6-1 · Expand beyond well-bounded bug fixes**
`Type: Human + AI` · `Owner: Chuck` · `Review: Redwan`
Gradually admit larger/feature tickets; adjust weight classes (Quick/Standard/Full) and gates.
*Acceptance:* New ticket classes run with acceptable quality; criteria documented. `Depends on: E3.2-1, E3.5-1`

**E3.6-2 · Onboarding & rollout to more repos/teams**
`Type: Human` · `Owner: Chuck` · `Review: Cory`
Document AGENTS.md requirements and onboard additional repos/teams.
*Acceptance:* At least one repo beyond the initial two is onboarded with a runbook. `Depends on: E3.6-1`

---

## Cross-cutting notes

- **Artifacts live outside agent memory** — on the branch, PR, S3, and durable checkpointer — so a fresh worker reconstructs context. Applies to every AI-buildable story.
- **Every external write is idempotent** and keyed on `(ticket_id, round)`; assume at-least-once delivery everywhere (webhooks, node replays, Temporal activities).
- **No secrets in code, docs, logs, or traces.** Runtime fetch from Secrets Manager / AgentCore Identity only.
- **Portability guardrail:** keep code pure DeepAgents + LangGraph + Bedrock (`langchain-aws`) + MCP tools so nothing hard-depends on AgentCore beyond a swappable checkpointer — this is what makes the Phase 3 Temporal port low-risk.

## Open items to resolve during the build

Carried from the architecture doc, mapped to where they get closed: sandbox network posture (E0.4-3); OpenAI-on-Bedrock Responses API tool-calling (E1.1-2); advisor triggers (E1.5-3 → E2.3-3); model validation (E2.3); approval grammar (E2.4-1); cap values (E2.4-2); branch/rebase policy (E2.1-2); webhook retry/duplicate semantics (E0.3-2); ticket-type gate ahead of PRE-VERIFY, deferred while tickets are hand-curated (E2.4-3, credit: Mykhailo Sakhnik). The M3 AWS-vs-GCP lock-in is closed by the E1.8-3 POC write-up. Sonar as an additional Verify signal (architecture doc item 10) is closed by E0.2-5 and E1.5-4. The separate CodeRabbit/Greptile-vs-Copilot blind-spot spike (architecture doc item 11) remains open and unassigned.
