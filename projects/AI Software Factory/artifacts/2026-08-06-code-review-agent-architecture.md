# FN Code Review Agent: Architecture

**Date:** 2026-08-06 · **Status:** Draft for review · **Owner:** Chuck Haines

---

## 1. Purpose and boundaries

Two separate systems, built together:

| System | Job |
|---|---|
| **Review agent** | Produce findings a human would otherwise have to find, plus an auditable record of what was examined and judged fine |
| **Merge gate** | Decide whether the PR is ready to merge, using an independent risk signal plus the review's findings |

### What it is not

**Not a scanner.** Sonar owns deterministic static analysis, lint rules, and security pattern matching. This agent does what a scanner structurally cannot: judgment against intent, architecture, and FN-specific convention.

**Does not execute code.** No tests, no builds, no sandbox. CI runs tests; Sonar runs scans.

**Does not aggregate other tools' results.** It never reads CI or Sonar output. It posts one check run; GitHub branch protection combines it with the others; GitHub native auto-merge handles the auto-land case.

### The core architectural claim

The differentiator is publishing **how it reviewed**: what was examined, what was judged fine, and what was not checked.

A free-form agent loop cannot credibly make that claim. It explores wherever the model decides, so coverage becomes a narrative the model writes about itself.

**Therefore the review pipeline is a fixed graph of named dimensions.** "What I checked" is the node list. "What I judged fine" is the nodes that returned clean. The coverage report is emitted from the graph's own execution, not from asking a model to summarize itself. Everything else in this document follows from that.

---

## 2. System diagram

```
GitHub PR event
      │
      ▼
┌─────────────────────────────────────────┐
│ Ingress: GitHub App → API Gateway →     │
│ Lambda (dedupe, debounce, correlation)  │
│ Filters: drafts, bots, non-triggers     │
└────────────────┬────────────────────────┘
                 │
                 ▼
        ┌────────────────┐
        │ RISK SCORER    │
        │ (deterministic)│
        │                │
        │ change type    │
        │ files touched  │
        │ path criticality│
        │ blast radius   │
        │ diff size      │
        │ compat surface │
        │                │
        │ NO model       │
        │ NO review input│
        │                │
        │ tier: trivial  │
        │  or scored     │
        └───────┬────────┘
                 │
      ┌──────────┴────────────┐
      │ trivial                │ scored
      ▼                        ▼
┌────────────────┐   ┌────────────────────────────────┐
│ REVIEW AGENT   │   │ REVIEW AGENT (AgentCore)        │
│ NEVER INVOKED  │   │ DeepAgents on LangGraph         │
│                │   │                                 │
│ (bypass)       │   │  ┌──── fixed dimension nodes ──┐│
│                │   │  │ 1 AC / ticket intent        ││
│ @fn-review     │   │  │ 2 Architecture fitness      ││
│ forces scored  │   │  │ 3 FN conventions & fit      ││
│ + full review  │   │  │ 4 Authorization & exposure  ││
│                │   │  │ 5 Test adequacy             ││
│                │   │  │ 6 Data / schema             ││
│                │   │  └──── subagents inside each ──┘│
│                │   │                                 │
│                │   │ read-only shallow clone         │
│                │   │ grep / glob / read / AST / git  │
│                │   │ MCP: Linear, GitHub             │
│                │   │ FN skills as dimension context  │
└───────┬────────┘   └──────────┬──────────────────────┘
        │                       │
        └───────────┬───────────┘
                   ▼
        ┌─────────────────────────┐
        │ GATE                    │
        │ f(risk tier/score,      │
        │   review findings)      │
        │ FAILS OPEN on error     │
        └──────────┬──────────────┘
                   ▼
   ┌───────────────────────────────────────┐
   │ OUTPUT                                │
   │ • 1 updating PR comment (coverage,    │
   │   or single-line skip notice)         │
   │ • inline comment per finding          │
   │ • check run (gate verdict)            │
   │ • run ID in footer                    │
   └───────────────────────────────────────┘

STORES
├── CloudWatch (AgentCore Observability)   full-content traces, 30d
├── CloudWatch Logs / S3 (Bedrock invocation logging)   full prompt/response
├── Datadog (ADOT fan-out + redaction)     content-free spans, ops + cost
└── S3 / DynamoDB   durable review records + feedback store, no expiry
```

---

## 3. Components

### 3.1 Ingress

GitHub App to API Gateway to Lambda. Nothing is installed in product repos.

Responsibilities: signature verification, event filtering, debounce, dedupe, correlation ID assignment.

**Permissions, structurally enforced rather than prompt enforced:**

| Scope | Level |
|---|---|
| `contents` | read |
| `pull_requests` | write |
| `checks` | write |
| `contents: write` | never |

### 3.2 Risk scorer

Separate from the review agent and independent of its findings. This is the most important structural decision in the design.

If the reviewer also decided merge readiness, the gate would reduce to "I found nothing, therefore ship it," with trustworthiness capped by the reviewer's own unmeasurable recall. A confident-looking clean verdict is more dangerous than no verdict.

**v1 inputs, all static and computable from diff plus repo, with no CI dependency:**

- Change type (feature, fix, refactor, config, dependency, migration)
- Files and paths touched, and criticality of those paths
- Blast radius (call sites, exported surface, event topics touched)
- Diff size and file count
- Backward compatibility surface (schema, public API, event contract)

Deliberately not LLM based and not informed by the review. Deterministic, cheap, auditable, and independently improvable.

**Output is a tier, not just a score.** Every PR classifies into exactly one of two tiers:

| Tier | Criteria | Consequence |
|---|---|---|
| `trivial` | Every changed file is documentation, configuration, or a dependency manifest — no other file type touched | Review agent is never invoked. See 3.4. |
| `scored` | Everything else | Existing risk-score behavior; feeds the gate as before |

`trivial` classifies on change type alone, not diff size or blast radius, to keep the false-negative surface narrow. A `@fn-review` comment always forces `scored` classification and a full review for that run, regardless of the automatic tier.

### 3.3 Review agent

**Harness:** DeepAgents on LangGraph. **Runtime:** Bedrock AgentCore. **Models:** Bedrock, selectable per dimension.

Outer graph is fixed named dimensions. Inner exploration is DeepAgents subagents. Deterministic skeleton, agentic muscle.

| # | Dimension | Scope | Key inputs |
|---|---|---|---|
| 1 | **AC / ticket intent** | Does the code do what the ticket asked | Linear issue, linked spec |
| 2 | **Architecture fitness** | Design fit, blast radius, platform consistency | Repo ADRs, call graph |
| 3 | **FN conventions & platform fit** | FN-specific conventions a scanner has no rules for | `fn-api-design`, `fn-ddl-design`, `fn-event-integration` skills |
| 4 | **Authorization & data exposure** | Authz logic, tenant isolation, endpoint exposure | Diff, auth middleware, route config |
| 5 | **Test adequacy** | Are new branches covered, are the tests meaningful | Diff, test files |
| 6 | **Data / schema** | Migration safety, DDL standards, compatibility | `fn-ddl-design`, schema registry |

**Dimensions 3 and 4 are scoped away from Sonar's lane deliberately.** Because the agent is blind to Sonar's output, overlapping scope would produce the same finding twice from two bots, and asymmetrically so, since on static pattern findings the agent is probabilistic where Sonar is deterministic. Dimension 3 therefore covers only FN conventions Sonar has no rules for, and dimension 4 covers authorization reasoning rather than pattern matching. A scanner catches injection patterns; it does not catch "this leaks another company's work orders."

These two dimensions are where FN's differentiated knowledge lives, and the ai-toolbox skills that encode it already exist.

**Context strategy: agentic exploration, no vector index.** Tool-based repo access (grep, glob, read, AST, git) for in-the-moment investigation. Persistent storage is reserved for what cannot be rediscovered from the working tree: past review decisions, disputes, conventions.

**Ticket lookup, fallback chain:**

1. Linear native GitHub PR-to-issue linkage (primary)
2. PR title conventional commit scope, e.g. `feat(MOI-64):` (secondary; note `feat(api):` is also valid, so a scope parse alone is unreliable)
3. Branch name (tertiary)
4. Nothing resolves: dimension 1 reports `not verifiable, no ticket identified` and the gate downgrades

**Graceful degradation is a first-class behavior.** Specs and ADRs are supported inputs but no FN repo uses them yet. Where an input is absent, the dimension reports it ("no ADRs found," "ticket has no usable AC") rather than producing a confidently clean verdict against nothing.

**Model selection is per dimension and empirical.** Model family for review should differ from the family engineers author with, to avoid correlated blind spots. Model tier should not be assumed; it is measured against FN repos via the eval harness.

### 3.4 Gate

```
verdict = f(independent risk score, review findings)
```

Three tiers. Design for 3 from day one, ship 1 first, widen on evidence.

| Tier | Behavior | Status |
|---|---|---|
| 1 | Advisory. Posts verdict, humans still required | Ship first |
| 2 | Required check, human approval still required to merge | Graduate on evidence |
| 3 | Low risk plus clean plus all checks green, GitHub native auto-merge, no human approval | Design for, may never enable |

Graduation requires accumulated false-negative data, which is why shadow mode runs from day one. Without the risk scorer live early there is nothing to graduate on.

**`trivial`-tier PRs bypass the review agent entirely.** The gate posts a `skipped — low risk` verdict at tier 1, sourced only from the risk scorer's classification. `skipped` is not `clean`: it means no dimension executed, not that six dimensions came back empty. A `@fn-review` comment always forces a full review and reclassifies the PR as `scored` for that run.

**The gate fails open.** Agent error or timeout yields a neutral "review unavailable," never a failure. An agent outage that blocked merges org-wide would end the project. The gate can only add confidence, never remove the ability to ship.

**A disputed blocking finding moves the gate to "needs human adjudication," never to pass.**

### 3.5 Output

One updating PR-scoped comment, plus inline comments, plus a check run.

```
## FN Review — ready with 2 blocking findings

| Dimension | Verdict | Examined | Confidence |
|---|---|---|---|
| AC / ticket intent       | 1 finding       | MOI-64 (3 AC), diff vs. all 3        | High   |
| Architecture fitness     | clean           | Repo pattern, 4 call sites           | Medium |
| FN conventions           | 1 finding       | fn-ddl-design, fn-event-integration  | High   |
| Authorization & exposure | clean           | 2 new endpoints, tenant scoping      | High   |
| Test adequacy            | clean           | 3 new tests vs. 2 new branches       | Medium |
| Data / schema            | not applicable  | No migrations or DDL in diff         | n/a    |

Not checked: cross-repo consumers of the changed Kafka topic (out of scope, single-repo review)
No ADRs found in this repo. Architecture verdict is based on observed code patterns only.

### Blocking
blocking: `migrations/...`  → [inline]
blocking: `handler.go:88`   → [inline]

<details>Advisory (3) · Per-dimension detail · Evidence trail · Review history</details>

Run: agentcore-session-a1b2c3
```

Design rules, each doing specific work:

- **The `Examined` column is load-bearing.** It must be specific enough to be falsifiable ("MOI-64, all 3 AC"), not generic ("reviewed acceptance criteria"), or it is theater.
- **`not applicable` is not `clean`.** `clean` must mean "I looked and it was fine," or the artifact is worthless.
- **An explicit "not checked" line.** Forced acknowledgment of scope limits, and the honest home for structural gaps: cross-repo effects, cumulative architecture drift, and "wrong thing to build" beyond ticket scope.
- **Coarse confidence, never percentages.** A displayed numeric score anchors the reader's own judgment and invents precision that is not there.
- **Severity taxonomy: blocking, recommended, nit, question.**
- **Per-dimension detail is collapsed by default**, richer than the table row, expandable on demand.
- **Every finding gets an inline comment on its line**, prefixed with severity. Nits post as GitHub suggested changes where possible, so a nit is one click rather than a paragraph. Nits are capped at roughly 10 per PR, with overflow rolled into the summary so blocking findings are not buried.
- **Run ID in the footer** equals the AgentCore session ID, so no correlation layer is needed.

**Skip-tier output is a single line, not a table.** No dimension table, no coverage claims, because no dimension executed:

```
## FN Review — skipped, low risk

Dependency bump only, no code changes. Full review: comment `@fn-review`.

Run: agentcore-session-a1b2c3
```

### 3.6 Finding identity

Three required behaviors depend on it, and file plus line will not work because lines shift on rebase:

1. Never re-post an unresolved finding
2. Resolve the thread when the issue is fixed, and mark it resolved in the summary
3. Never re-raise a finding a human disputed

**Fingerprint = hash(dimension, finding category, enclosing symbol, normalized surrounding code).** This distinguishes "same finding, moved" from "new finding."

This is a component rather than an implementation detail: getting it wrong produces duplicate-comment behavior that kills adoption, and it is the primary key the feedback store uses.

### 3.7 Trigger semantics

| Event | Behavior |
|---|---|
| `opened` (non-draft) | Full review |
| `ready_for_review` | Full review |
| `synchronize` | Incremental: new commits plus re-check of open findings. Debounced ~60s so a push burst is one review |
| `reopened` | Full review |
| `@fn-review` comment | On-demand re-run |
| Draft PRs | Skipped entirely |
| Bot dependency PRs | Skipped |

**Multi-round behavior.** The summary comment updates in place. Because GitHub does not reliably notify on comment edits, a short delta comment is also posted when the verdict materially changes (new blocking findings appear, or blocking clears), not on every push. Rounds 1..N live in the collapsed review history.

**Oversized PRs.** Review quality degrades past roughly 200 to 400 LOC. The agent reviews anyway, emits an explicit oversized-PR finding, and downgrades gate confidence. This turns a silent quality cliff into visible pressure toward smaller PRs.

---

## 4. Observability

Full-content traces are required, because "what tools it called and how it executed" cannot be diagnosed from token counts and durations. This agent runs first-party, against FN code, inside FN's AWS boundary, which is what makes a full-content policy appropriate here. It is an explicit documented decision, not a default.

| Store | Contents | Retention |
|---|---|---|
| **CloudWatch** via AgentCore Observability | Distributed traces, span-level logs, metrics under `bedrock-agentcore`. Native OTel. Provides Agents, Sessions, and Traces views plus a GenAI Observability dashboard (session volume, latency, tokens, errors), filterable by agent and session ID | 30 days, enforced by log-group retention config |
| **CloudWatch Logs / S3** via Bedrock model invocation logging | Full request, response, and metadata for every Bedrock call. Inline under 100 KB, spilling to S3 above that. A config toggle rather than code, disabled by default | 30 days |
| **Datadog** via ADOT fan-out with redaction processor | Content-free spans: run counts, per-dimension latency, tokens, cost, error rates, verdict distribution | Datadog default |
| **S3 / DynamoDB** | Durable review records: findings, fingerprints, gate inputs and verdict, human outcome. Must outlive 30 days because it is the eval dataset and feedback substrate | No expiry |

Traces are ephemeral debug data; review records are durable data. Two stores, two policies.

**Per-run trace contents:** run and session ID bound to PR plus commit SHA · a span per dimension node · every tool call with arguments and results · model, tokens, and cost per node · finding fingerprints emitted · gate inputs and computed verdict · human outcome (merged, disputed, overridden). The last field is what turns traces into an eval dataset rather than logs.

Trace detail is not rendered on the PR. Only the run ID appears there.

---

## 5. Feedback and learning

**Capture**, line-anchored with a mandatory reason, because the reason is what makes it learnable:

- `@fn-review disagree: <reason>` as a reply to an inline comment
- `@fn-review missed: <description>` on the relevant line, capturing code context

**Plus four passive signals**, which will outnumber explicit feedback by an order of magnitude and cost nothing:

| Signal | Meaning |
|---|---|
| Finding fixed | Validated finding, the best label available |
| Thread resolved with no code change | Probable false positive |
| Finding ignored and merged | Weak negative |
| Bot comment reacted with 👎 | Weak negative, eval label only |

Passive signals need no triage. They are eval labels.

**Three learning tiers:**

1. **Deterministic suppression.** A disputed fingerprint is hard-suppressed. No model involved, immediately reliable.
2. **Retrieval-time injection.** Feedback retrieved by repo, dimension, and category, injected as context. Persists only what the agent cannot rediscover from the working tree.
3. **Promotion to a skill or Sonar rule.** Recurring feedback gets codified. This is the strategically important tier: it converts tacit correction into a standard that improves authoring agents too, so the loop feeds the shared skill library rather than dead-ending in a private memory store.

**Fine-tuning is rejected.** Data volume is orders of magnitude too low, the cycle is too slow to iterate on, and it destroys the ability to explain why behavior changed, which is the auditability this agent exists to provide. "Tweak the model" means dimension prompts and instructions, skills, or Sonar rules, never weights.

### Suppression safety

An unmoderated feedback loop is a suppression vector. If disputes auto-suppress, engineers can silence the reviewer by disagreeing, and the agent degrades toward saying nothing, fastest on the teams with the most to hide. It would not look like abuse. It would look like a reviewer that got quieter and more agreeable over time.

Four properties make author self-dispute safe, so no restriction on who may dispute is needed:

1. **Nothing is auto-applied.** Disputes and misses queue for human triage; a human decides whether anything changes.
2. **Dispute changes chattiness, never the record.** The artifact still shows "1 blocking finding, disputed by author, unresolved."
3. **A disputed blocking finding moves the gate to "needs human adjudication," never to pass.**
4. **Suppressions are scoped to repo plus fingerprint, never global, and they expire**, so memory cannot accumulate into permanent blindness.

**Scope split.** In-PR quieting is immediate and needs no approval: a dispute silences that finding for the rest of the current PR, which is not learning, just not nagging. Cross-PR learning always requires triage.

**Dispute rate per dimension is a first-class metric.** A high-dispute dimension is miscalibrated, and this is the most valuable improvement signal in the system.

**Approval routing.** Org-wide skill or rule promotion goes through principal-level approval. Repo-scoped changes can sit with the repo's tech lead.

---

## 6. Evaluation

Shadow mode is the eval harness, not a separate exercise. Backtest against already-merged PRs with known outcomes, then live-shadow without posting. Labels come from the passive signals above.

| Metric | Why |
|---|---|
| Per-dimension **precision on blocking findings** | Determines whether the gate can ever open |
| **False-negative rate on PRs later reverted or hotfixed** | The only honest measure of gate safety |
| **Time-to-first-human-decision and open-PR queue depth**, sourced from the existing repo scorecard pipeline | The system-level throughput proof. Per-dimension precision alone doesn't show whether review stopped being the bottleneck |

Throughput is not measured by this pipeline directly. It is pulled from the repo scorecard already live per pilot repo, so the agent's success is judged against the same throughput signal the org already tracks.

Published benchmark accuracy for LLM reviewers degrades sharply on real uncurated PRs, and larger models do not reliably outperform smaller ones on this task. Per-dimension model choice is therefore measured against FN repos, never assumed.

---

## 7. Non-functional requirements

| Area | Decision |
|---|---|
| **Failure mode** | Fails open. Neutral "review unavailable," never a check failure |
| **Latency** | p50 under 5 minutes, p95 under 15 minutes |
| **Permissions** | `contents: read`, `pull_requests: write`, `checks: write`. Never `contents: write` |
| **Language sequencing** | Go and NestJS first, then React and React Native, then the PHP monolith last (custom framework, no conventions to lean on, uneven test coverage: highest value, worst place to prove the concept) |
| **Cost control** | Per-PR token budget, per-repo daily cap, short-circuit on not-applicable dimensions, trivial-tier bypass for docs/config/dependency-only PRs. The ceiling must exist before rollout |

---

## 8. Build sequence

| Phase | Deliverable | Gate tier |
|---|---|---|
| **0** | Ingress, read-only clone, one dimension (AC / ticket intent), trace pipeline, run ID | None, shadow only |
| **1** | All six dimensions, coverage artifact, finding fingerprinting, inline comments | Tier 1 (advisory) |
| **2** | Risk scorer, gate check run, feedback capture, triage queue | Tier 1, shadow scoring |
| **3** | Eval harness on backtest plus live shadow data, per-dimension model selection | Tier 2 on evidence |
| **4** | Skill and rule promotion path, widened risk threshold | Tier 3 if approved |

Phase 0 proves the hardest and most differentiated thing first: AC verification against a real ticket, with a real trace behind it. It is the dimension most likely to fail, and if it does not work the rest of the design does not matter.

---

## 9. Open items

| Item | Owner |
|---|---|
| Is any auto-merge tier organizationally acceptable | Engineering leadership. Governance, not technical |
| Full-content trace policy sign-off | Engineering leadership |
| Bedrock invocation logging is per account and region, not per application, so enabling it captures every Bedrock call in the account. Argues for a dedicated AWS account | Chuck / AWS account owner |
| Triage queue owner, cadence, and physical home. Weekly batch looks right at pilot volume | Chuck |
| Pilot repo selection. Should exercise more than one or two dimensions meaningfully | Chuck |
| Per-dimension model selection, blocked on the eval harness | Chuck |

---

## 10. Accepted risks

- **The coverage table may not survive contact with engineers** who only want to know what to fix, and could become the section everyone collapses and ignores. It is the core differentiator, so this is the primary adoption risk.
- **"Technically correct but the wrong thing to build" remains a structural gap**, not a maturity gap. Reading the ticket and spec narrows it; nothing closes it. The "not checked" line is where this stays honest.
- **Cumulative architecture drift is invisible to any per-PR reviewer.** ADRs are the partial substitute, and no FN repo has them yet.
- **The review target is drifting.** As engineers increasingly author with AI assistance, the code under review may trend toward the profile AI reviewers catch least reliably, independent of how good this agent is.
