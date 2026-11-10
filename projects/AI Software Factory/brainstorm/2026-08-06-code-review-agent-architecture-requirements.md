# Code Review Agent: Brainstorm / Discovery Notes

Date: 2026-08-06 · Goal: land on (1) an architecture and (2) a requirements set for a bespoke Field Nation code review agent

**Grounding research:** [research/2026-08-05-code-review-best-practices-human-and-agentic.md](../research/2026-08-05-code-review-best-practices-human-and-agentic.md)
**Related:** O1 KR2 (code review agent as playbook step) and O2 KR3 (hosting/execution loop) in [program/OKRs/h2-2026-okrs.md](../program/OKRs/h2-2026-okrs.md) · Gitar/Sonar evaluation in [knowledge/tech-context.md](../knowledge/tech-context.md)

---

## Summary / key decisions

**Session complete.** Both deliverables produced:
- [artifacts/2026-08-06-code-review-agent-architecture.md](../artifacts/2026-08-06-code-review-agent-architecture.md)
- [artifacts/2026-08-06-code-review-agent-requirements.md](../artifacts/2026-08-06-code-review-agent-requirements.md)

### What survived from Chuck's opening position

All five opening thoughts held, but three of them for different reasons than originally assumed:

| Opening thought | Outcome |
|---|---|
| DeepAgents on AWS, Bedrock as model provider | **Held** — but justified on *fixed named stages making coverage claims structurally true*, plus model-agnosticism serving Chuck's own cross-vendor principle. Not "it was the archived stack." |
| Linear as ticket source for correctness verification | **Held and strengthened** — ticket quality becomes an explicit reported input; no usable AC → "not verifiable," not "clean" |
| GitHub PR as trigger | **Held, substantially refined** — draft/bot skips, debounce, incremental re-review, cumulative PR-scoped artifact |
| Full agent tracing in a separate system | **Held** — native AgentCore Observability → CloudWatch, not hand-rolled. Surfaced a real conflict with the MOI-80/81 content firewall. |
| Feedback mechanism for disputes and missed findings | **Held, with the learning mechanism sharply constrained** — three tiers, fine-tuning rejected, nothing auto-applied |

### The four decisions that most shaped the design

1. **The gate is architecturally separate from the reviewer** (Q2). A reviewer that self-certifies reduces to "I found nothing, therefore ship it." Gate verdict = `f(independent risk signal, review findings)`.
2. **The reviewer is read-only and executes nothing** (Q3). Removes the FN-Core Docker-parity sandbox problem — the single biggest infra lift in the archived PEV architecture — from scope entirely.
3. **Fixed named review dimensions, not a free-form loop** (Q4). This is what makes "what I checked and judged fine" structurally true rather than a narrative the model writes about itself. It is the entire differentiator over Gitar.
4. **The agent never reads CI or Sonar; GitHub composes the merge decision** (Q5). Kills all aggregation logic and the sequencing dependency, and native GitHub auto-merge handles tier 3 for free.

### Chuck's opening position (to be tested, not assumed)

- **Architecture:** DeepAgents deployed to AWS, Bedrock as the model provider
- **Ticket context:** Linear is FN's source of tickets — agent should read the ticket to verify code correctness against intent
- **Trigger:** GitHub pull request
- **Tracing:** full agent tracing (tools called, execution path) — *not* surfaced on the PR, lives in a separate system for debugging
- **Feedback loop:** two directions — (1) human can dispute a finding ("this wasn't wrong, and here's why") so the agent learns; (2) human can tag something the agent missed for future learning

---

## Q&A log

### Q1 — The agent's job relative to Sonar; is the goal bug-finding or gating?

- **Asked:** What is this agent's job relative to Sonar (already bought), and given that Meta's RADAR shows human-review reduction comes from risk-stratified gating rather than a smarter reviewer, is the goal defect-finding or gating?
- **Captured:**
  - **Correction to the workspace record: Gitar is NOT included in the current Sonar contract — it's a separate contract.** (Earlier workspace notes implied Gitar came bundled; that's wrong. `knowledge/tech-context.md` should be corrected.)
  - Vendors will still be explored in parallel, **but this brainstorm is deliberately scoped to the build option only.**
  - **Confirmed:** the bespoke agent should do what Sonar structurally can't, and should **consume Sonar findings as inputs** (aligns with MOI-78's feed-forward MCP).
  - **Chuck wants both, and considers them complementary, not either/or:** *"I want it to find the bugs that need to be fixed, but also give a gate on if the PR is ready for merge based on its review to help reduce human review."* Rationale: gating reduces human review, AND bugs the agent finds are bugs the human no longer has to find.
- **Flags:** none

### Q2 — What does the gate actually *control*?

- **Asked:** Advisory verdict, required-but-overridable status check, or a genuine auto-merge tier — and does the gate verdict come from the reviewer's own self-assessment or from an independent risk signal?
- **Captured:**
  - **DECISION: design for auto-merge (tier 3) from day one, ship advisory (tier 1) first, widen incrementally.** Chuck: *"We may never get to an auto merge state, but let's at least design for it and incrementally get there."* Auto-merge is explicitly not ruled out, but also not assumed.
  - **DECISION: the gate is architecturally SEPARATE from the code review agent.** Two things to build, not one. Chuck: *"I want to build both (the gating and the code review agent)."*
  - Accepted rationale for the separation: a reviewer that self-certifies reduces to "I found nothing, therefore ship it" — the gate's trustworthiness would be capped by the reviewer's own unmeasurable recall (automation-bias failure mode). Gate verdict must be `f(independent risk signal, review findings)`, where risk is computed from **change characteristics** (change type, files touched, blast radius, test coverage on touched paths), not from the reviewer's opinion of its own work.
  - Accepted rationale for two severity bars: **blocking findings precision-optimized, advisory findings recall-optimized** — resolves the "find every bug" vs. "low noise" tension per-finding instead of globally.
  - Supporting context surfaced: O1's motivating ~36-hour "PR ready → merged" number is **queue time, not review-effort time**, which makes the gate the larger velocity lever of the two components.
  - Implied requirement: **shadow mode on the pilot repo from the start**, to accumulate the false-negative evidence needed to graduate the gate from tier 1 → 2 → 3. Without the risk scorer running early there's no data to graduate on.
- **Flags:** whether any auto-merge tier is org-acceptable → Cory / Ben Moormeier / Mike Mueller (a governance call, not a technical one; possible agenda item for the 8/11 review already scheduled)

### Q3 — Hosting: where does it run, and does it need to execute code?

- **Asked:** Hosted long-running agent service vs. ephemeral CI compute — and does the reviewer need to execute code/tests (which would drag in the FN-Core Docker-parity sandbox lift)?
- **Captured:**
  - **DECISION: the reviewer is READ-ONLY over the repo. It does not execute code or tests.** Chuck: *"I do not expect the code review agent to need to run unit tests, we have other CI for that... just be able to review the actual code."*
  - Consequence: **the sandbox / FN-Core Docker-parity problem is out of scope entirely** — the single biggest infra lift in the archived PEV architecture does not apply to this agent.
  - Execution results become **inputs, not activities**: CI status/results, Sonar findings (MOI-78 feed-forward MCP), and coverage data are consumed as facts.
  - **DECISION: hosted service, not a per-repo GitHub Action.** Justified on durable state, not on runtime duration — code review is short-lived and event-driven, so the archived architecture's AgentCore/Temporal rationale (durable multi-day human gates) does *not* transfer. The real reasons: (a) the feedback/learning store must be central, shared across repos, and outlive any single PR run; (b) MOI-47 established that FN deliberately commits nothing into the 739 product repos; (c) centralized tracing, risk history, and cost/concurrency control.
  - **Working shape:**
    - *Trigger:* GitHub App webhook → API Gateway + Lambda (dedupe/correlation) — nothing installed in product repos
    - *Execution:* hosted container (AgentCore), shallow clone, read-only token
    - *State:* central store for feedback/learning, traces, risk history
    - *Gate:* separate lightweight service posting a GitHub check run
  - **Noted tension (not a blocker):** O2 KR3 and Q4 KR1 both name the code review agent as *the* candidate for proving FN's hosted-agent deployment pattern, creating an OKR incentive toward hosted regardless of need. Hosted is independently defensible on the central-state argument, so the OKR and the architecture happen to agree — but the decision rests on state, not on the OKR.
- **Flags:** none

### Q4 — Harness and orchestration shape

- **Asked:** DeepAgents/LangGraph vs. Claude Agent SDK, free-form agent loop vs. fixed named review stages, and is the proposed dimension list right?
- **Captured:**
  - **DECISION: DeepAgents on LangGraph confirmed — but on a new justification.** Not "it was the archived stack." The real reason: **fixed, named review dimensions make the coverage claim structurally true rather than self-reported.** A free-form loop can only report what it happened to do that run, so coverage becomes a narrative the model writes about itself. With a graph, "what I checked" *is* the node list and "what I judged fine" *is* the nodes that returned clean — emitted from the graph's execution, not from asking a model to summarize itself. This is what makes Chuck's transparency bar defensible instead of aspirational.
  - **DECISION: fixed named dimensions as the outer graph; DeepAgents-style subagents doing open-ended exploration inside each dimension.** Deterministic skeleton, agentic muscle.
  - **DECISION: starting dimension set (v1, "makes sense to start with"):**
    1. AC / ticket-intent verification
    2. Architecture fitness (design fit lives here)
    3. Standards & conventions
    4. Security
    5. Test adequacy
    6. Data / schema
  - Report ranking should follow Google's design-first priority ordering, so architecture fitness and AC verification outrank standards/style in the output.
  - **Claude Agent SDK considered and set aside.** Its genuine pull was native consumption of FN's ~186 `SKILL.md` files / ai-toolbox marketplace ("reviewed against the same standards the author was told to follow"). Tie broken toward DeepAgents on two grounds: (a) **Chuck's own cross-vendor principle** from the OKR doc — engineers author with Claude Code and Copilot, so a Claude reviewer inherits correlated blind spots (a Claude-written bug may be a Claude-invisible bug); DeepAgents on Bedrock allows a different model family for review; (b) the comparative-eval finding that **Haiku outperformed Sonnet** on review tasks only pays off if models are swappable per dimension and evaluated empirically.
  - Skills are not lost by this choice: `SKILL.md` is markdown with frontmatter, so loading FN standards skills as per-dimension context is a loader, not an architecture. Tier 2 knowledge plane stays reusable.
- **Flags:** cross-vendor model choice per dimension is an empirical question, not a design-time one → needs an eval before locking models (see eval requirements, later)

### Q5 — Trigger semantics

- **Asked:** Which PR events fire a review, how re-reviews behave, what gets skipped, how it sequences against CI/Sonar, and how oversized PRs are handled.
- **Captured:**
  - **DECISION: trigger set confirmed.**
    - Fire on `opened` (non-draft), `ready_for_review`, `synchronize` (debounced ~60s so a push burst is one review), `reopened`, plus an explicit `@fn-review` comment command for on-demand re-runs
    - **Skip draft PRs entirely** — a draft isn't asking for review; largest single cost saver across 739 repos
    - **Skip bot dependency PRs** by default (Renovate auto-merge owns that lane per MOI-47)
    - **Incremental on `synchronize`:** review new commits + re-check whether previously-raised open findings are resolved; don't re-review the full diff each push
    - **Coverage artifact is PR-scoped and cumulative** — one persistent, updating comment across the PR's life (takes Gitar's architecturally-correct single-artifact shape, fills it with FN's content)
  - **DECISION / CORRECTION to Q1: the review agent does NOT consume CI or Sonar results.** Chuck: *"Those are independent things that will report on the PR and this agent doesn't need to concern itself with those."* This narrows the Q1 "consumes Sonar findings as inputs" decision — Sonar feed-forward (MOI-78) is for **authoring** agents, not the reviewer.
  - **Consequence, and it simplifies the design: GitHub composes the merge decision, not the agent.** The agent posts one check run; branch protection ANDs it with CI and Sonar checks; **GitHub's native auto-merge** handles the tier-3 case (merge when all required checks pass). So no aggregation logic, and the earlier "immediate review / deferred gate" two-phase split **dissolves** — nothing to wait for, review and check run both post immediately. Lower latency than the proposed design.
  - **DECISION: oversized-PR self-reporting.** Past ~200-400 LOC review quality degrades (three independent studies). The agent reviews anyway but emits *"this PR exceeds the size where review is reliable"* as a first-class finding and downgrades gate confidence. Creates organic pressure toward smaller PRs, which is the real fix.
  - **Risk scorer stays on static signals for v1** (change type, files touched, blast radius, computable from diff + repo) rather than CI-derived coverage data — keeps the gate free of CI dependencies too, consistent with the above.
- **Flags:** none

### Q6 — Sonar overlap in the dimension set

- **Asked:** Given the agent no longer reads Sonar, dimensions 3 (Standards & conventions) and 4 (Security) are where it will duplicate Sonar's output as PR noise — should those be rescoped?
- **Captured:**
  - Problem stated: blind to Sonar, the agent will re-report Sonar's findings (static patterns, lint rules, injection/crypto/auth smells) — the exact comment-spam failure mode the design is trying to beat. Asymmetric too: on that class of finding the agent is probabilistic where Sonar is deterministic, so on disagreement the agent is more likely wrong and the engineer has no adjudication path.
  - Options weighed: (1) cut both dimensions; (2) rescope to the judgment layer only; (3) read Sonar purely for dedupe (rejected — recouples to Sonar availability and reintroduces the sequencing dependency Q5 just removed).
  - **DECISION: option 2, rescope to the judgment layer.** Applies the Q1 principle ("do what Sonar structurally can't") one level deeper. Sonar keeps the deterministic lane.
  - **Revised dimension set (v1, final for this session):**
    1. **AC / ticket-intent verification**
    2. **Architecture fitness** (design fit lives here; ranked first in output per Google's design-first ordering)
    3. **FN conventions & platform fit** — *renamed from "Standards & conventions."* Scoped to FN-specific conventions Sonar has no rules for: `fn-api-design`, `fn-ddl-design`, `fn-event-integration`, repository pattern, etc.
    4. **Authorization & data exposure** — *renamed from "Security."* Scoped to authorization logic, tenant isolation, whether an endpoint should be public. Explicitly **not** pattern-matching. (Sonar catches injection patterns; it does not catch "this leaks another company's work orders.")
    5. **Test adequacy**
    6. **Data / schema**
  - Strategic note: dimensions 3 and 4 are where FN's differentiated knowledge already lives — the ai-toolbox skills encode conventions no off-the-shelf scanner will ever have rules for. That's the moat, and it's already built.
- **Flags:** none

### Q7 — The output artifact, inline comments, and multi-round behavior

- **Asked:** What the agent posts on the PR, how findings are structured, and how repeat review rounds interact with the artifact.
- **Captured:**
  - **DECISION: single updating PR-scoped comment, structured as a coverage table + blocking findings + collapsed detail.** Approved shape:
    - Header: gate verdict + one-line summary
    - Table columns: **Dimension | Verdict | Examined | Confidence**
    - Explicit **"Not checked"** line below the table
    - **Blocking** findings listed, linked to inline comments
    - Collapsed `<details>`: advisory findings, per-dimension detail, evidence trail, review history
  - **The `Examined` column is the load-bearing element** — the exact thing Gitar refuses to provide. Must be specific enough to be falsifiable ("MOI-64, all 3 AC") not generic ("reviewed acceptance criteria"), or it's theater.
  - **DECISION: "not applicable" is a distinct verdict from "clean."** If a PR touches no DB, the data dimension reports *not applicable*. Conflating the two destroys the artifact's trust value — "clean" must mean "I looked and it was fine."
  - **DECISION: explicit "not checked" disclosure.** The aviation/radiology mitigation (forced positive acknowledgment of scope limits). Honest home for the structural gaps: cross-repo effects, cumulative architecture drift, "wrong thing to build" beyond ticket scope.
  - **DECISION: coarse confidence (high/medium/low), never percentages.** Anchoring research found displaying an arbitrary numeric score measurably shifts reviewers' own judgment; a percentage invents precision that isn't there and anchors harder.
  - **DECISION: severity taxonomy reduced to blocking / recommended / nit / question.** Full Conventional Comments is richer than engineers will learn.
  - **DECISION (Chuck's addition): per-dimension detail lives in the collapsed section, collapsed by default, richer than the table row.** User expands if they want more.
  - **DECISION (Chuck's addition): ALL findings get an inline comment on the specific line**, not just blocking ones. This revises the earlier "blocking-only inline" proposal. Guardrails to keep it from becoming spam:
    - Every inline comment carries a severity prefix (`blocking:` / `recommended:` / `nit:`) so engineers triage by scanning
    - Nits post as GitHub **suggested changes** where possible — one click to accept instead of a paragraph to read
    - **Cap nits per PR (~10)**, overflow rolls into the summary, so blocking findings don't get buried on large PRs
  - **DECISION: multi-round behavior = update the original summary comment in place, plus a delta comment only on material verdict change.** Rationale: GitHub barely notifies on comment *edits*, so a pure in-place update can let round 2 land silently. Delta posts only when new blocking findings appear or blocking clears — not on every push. Review history (rounds 1..N) lives in the collapsed detail.
  - **DECISION: three inline-comment behaviors across rounds:**
    1. **Never re-post an unresolved finding** (the classic duplicate-spam bug in these tools)
    2. **Resolve the thread when the issue is actually fixed**, and mark it resolved in the summary — an agent that visibly acknowledges a fix is a materially different trust experience
    3. **Never re-raise a finding a human disputed** (hook into the feedback loop)
  - **NEW REQUIREMENT surfaced by this question — stable finding identity across commits.** All three behaviors above depend on it, and file:line won't work since lines shift on rebase. Needs a **finding fingerprint**: hash of dimension + finding category + enclosing symbol + normalized surrounding code, so "same finding, moved" is distinguishable from "new finding." Called out as a **first-class architectural component**, not an implementation detail — getting it wrong produces exactly the duplicate-comment behavior that kills adoption. It is also the **primary key the feedback store keys off**.
- **Flags:** open risk (accepted, not resolved) — whether the coverage table survives contact with engineers who just want to know what to fix, or becomes the section everyone collapses and ignores. Worth watching during the pilot.

### Q8 — Tracing and observability

- **Asked:** Where traces go, what's captured, retention, whether a run ID surfaces on the PR, and what AWS/AgentCore provides natively.
- **Captured:**
  - **Policy conflict surfaced and resolved.** MOI-80/81 deliberately keep every `OTEL_LOG_*` content flag unset (prompts `<REDACTED>`, `tool_input` absent). Correct for third-party coding assistants exporting *engineers' prompts to vendor SaaS* — but it makes a review agent **undebuggable**, since "what tools it called and how it executed" is exactly the stripped content. Different threat model justifies a different policy here: FN's own first-party agent, reviewing FN's own code, in FN's own AWS boundary. **This must be an explicit, documented decision**, not an accident, or it reads as violating the firewall precedent.
  - **DECISION: 30-day retention** for full-content traces.
  - **DECISION: run ID goes in the PR comment footer** so a bad review is traceable back to its session. Trace *detail* still does not render on the PR.
  - **DECISION: use the native AWS stack, not hand-rolled S3 trace storage** (verified against current AWS docs, 2026-08-06). Four components:
    1. **AgentCore Observability → CloudWatch** as primary trace store. Emits distributed traces, span-level structured logs, and metrics under the `bedrock-agentcore` namespace, natively OTel, routed to CloudWatch by default. Ships **Agents / Sessions / Traces views** plus a GenAI Observability dashboard (session volume, invocation latency, token usage, error rates), filterable by agent ID and session ID. No build. **CloudWatch per-log-group retention makes the 30-day policy declarative config, not a cleanup job.**
    2. **Bedrock model invocation logging → CloudWatch Logs (+ S3 for >100 KB payloads)** as the full-content layer. Captures full request data, response data, and metadata for every Bedrock call. **A config toggle, not code.** Disabled by default — so enabling it *is* the explicit policy decision, which is the right shape for that call.
    3. **Content-free spans → Datadog** via ADOT collector fan-out: one collector, two exporters, redaction processor on the Datadog branch. Ops/cost monitoring with no policy exception. Note: this agent runs its own collector, so it is **not** subject to the getDX-pinned `remote-settings.json` constraint flagged in MOI-80.
    4. **Durable review record → S3/DynamoDB** — *correcting an earlier suggestion in this session that S3 was for traces.* This store holds findings, fingerprints, gate inputs and verdict, and eventually the human outcome. **Different lifecycle from traces:** must outlive 30 days because it is the eval dataset and the feedback store's substrate. Traces = ephemeral debug; review records = durable data.
  - **Run ID = AgentCore session ID.** AgentCore already has a session primitive and associates traces with sessions, so no correlation layer needs building.
  - Per-run trace contents: run/session ID bound to PR + commit SHA, a span per dimension node, every tool call with args and results, model + tokens + cost per node, finding fingerprints emitted, gate inputs and computed verdict, and (added later) human outcome — merged, disputed, overridden. That last field is what turns traces into an eval dataset rather than logs.
  - **LangSmith considered, deferred.** Genuinely better at reasoning-level "why did the agent decide that," but it's a new vendor for a problem not yet proven to exist. Revisit only if CloudWatch's trace view proves too coarse.
- **Flags:**
  - **Bedrock invocation logging is configured per account and region, not per application** — enabling it captures every Bedrock call in that account, not just the review agent. Argues for the review agent in its own AWS account, or at minimum knowing the toggle is org-wide before flipping it. → Chuck / whoever owns the AWS account structure
  - Content-trace policy exception needs explicit sign-off rather than being assumed → likely same governance conversation as the auto-merge tier (Cory / Ben / Mike)

### Q9 — The feedback and learning loop

- **Asked:** How disputes and missed-finding tags are captured, stored, and actually change future behavior — and whether the PR author should be barred from disputing their own blocking findings.
- **Captured:**
  - **DECISION: capture via line-anchored commands, reason mandatory.** `@fn-review disagree: <reason>` as a reply to the inline comment; `@fn-review missed: <description>` on the relevant line (captures code context). A bare 👎 teaches nothing — the reason is what makes it learnable.
  - **DECISION: harvest three passive signals too** — they'll outnumber explicit feedback by an order of magnitude and cost nothing: finding **fixed** = validated finding (best label available, zero friction); thread **resolved without a code change** = probable false positive; finding **ignored and merged anyway** = weak negative. These need no triage; they're eval labels.
  - **DECISION: three-tier learning model, approved.**
    1. **Deterministic suppression** — disputed fingerprint hard-suppressed. No model involved, immediately reliable; makes Q7's "never re-raise a disputed finding" work.
    2. **Retrieval-time injection** — feedback records retrieved by repo + dimension + finding category, injected as context ("previously disputed here: X, because Y"). Matches Qodo's post-mortem conclusion: persist only what the agent can't rediscover from the working tree, i.e. past review decisions and team conventions.
    3. **Promotion to a skill or Sonar rule** — recurring feedback gets codified. **Strategically the important tier:** converts tacit correction into a standard that improves *authoring* agents too, so the feedback loop feeds the Tier 2 knowledge plane instead of dead-ending in a private memory store.
  - **REJECTED: fine-tuning.** Data volume orders of magnitude too low, cycle too slow to iterate on, and it destroys the ability to explain why behavior changed — which torches the auditability that is the point of this agent. Chuck's "tweak to the model" is therefore scoped to **dimension prompts/instructions, skills, or Sonar rules — not model weights.**
  - **Suppression-attack failure mode raised:** if disputes auto-suppress, engineers can silence the reviewer by disagreeing; the agent degrades toward saying nothing, fastest on the teams with the most to hide, and it presents as the reviewer simply getting more agreeable over time.
  - **DECISION (Chuck's resolution): nothing is auto-fed back. Disputes and misses are flagged into a human-review queue; a human decides whether it becomes a prompt/skill/rule change.** This is the primary safety mechanism, in place of restricting who can dispute.
  - **DECISION: in-PR quieting is separated from cross-PR learning** (refinement, since a pure approval-gated model would re-raise disputed findings on the next push to the same PR):
    - *In-PR, immediate, no approval:* a dispute silences that finding for the rest of **this** PR. Not learning — just not nagging. Zero durable effect.
    - *Cross-PR (repo- or org-scoped):* nothing changes without human triage.
  - **DECISION: dispute changes the agent's chattiness, never the record.** The artifact still shows "1 blocking finding — disputed by author, unresolved." This is what makes author self-dispute safe.
  - **DECISION: a disputed blocking finding moves the gate to "needs human adjudication," not to pass.** Dispute never opens the gate on its own.
  - **DECISION: drop the earlier "author cannot dispute their own blocking finding" constraint** — unnecessary friction once the record persists and dispute can't open the gate.
  - **DECISION: track dispute rate per dimension as a first-class metric.** A high-dispute dimension is miscalibrated; this is the most valuable improvement signal in the system.
  - Retained from the earlier proposal: **suppressions scoped to repo + fingerprint (never global), and they expire / need re-validation** so memory can't accumulate into permanent blindness.
  - **Governance precedent to reuse:** ai-toolbox skill changes already require `@fieldnation/principals` approval (per MOI-48), so **tier 3 promotion inherits an existing approval path.** Repo-scoped tier 2 changes could sit with the repo's tech lead.
- **Flags:**
  - Triage queue needs a named owner and cadence — unowned queues die. Weekly batch looks right at pilot volume. → Chuck
  - Where the triage queue physically lives (Linear project? dashboard? artifact?) → undecided

### Q10 — Linear ticket lookup and the ticket-quality dependency

- **Asked:** How the agent locates the Linear ticket, and how it handles tickets with no usable acceptance criteria.
- **Captured:**
  - **DECISION: ticket quality is an explicit, reported input.** If a ticket has no usable AC, dimension 1 reports **"not verifiable"** — *not* clean — names why, and the gate downgrades. Prevents the silent failure where the agent produces a confident "clean" verdict against a ticket that said nothing. Same self-reporting move as the oversized-PR guard.
  - **DECISION: ticket ID comes from the PR title. FN uses conventional commits**, so titles look like `chore(TICKET): …` / `feat(MOI-64): …` — parse the scope for the Linear issue key.
- **Flags:** whether the agent also reads linked specs and ADRs (not just the ticket) → still open, see Q11

### Q11 — Spec/ADR inputs, eval strategy, and pilot scope

- **Asked:** Should specs and ADRs be agent inputs, how is the agent evaluated, and should the pilot be `engineering-dbt`?
- **Captured:**
  - **Ticket lookup refinement:** conventional-commit scope isn't reliably a ticket (`feat(api):` vs `feat(MOI-64):`), so use a **fallback chain — Linear's native GitHub PR-to-issue linkage as primary, PR title scope parse second, branch name third.** If nothing resolves, dimension 1 reports "not verifiable — no ticket identified" and the gate downgrades.
  - **DECISION: build spec/ADR input support now, but expect it unused at launch.** Chuck: *"Spec and ADRs will eventually land as inputs, although no repository is currently using them. So let's build in the support for them for when they do exist."* Implication: the agent must **degrade gracefully and visibly** — dimension 1 reads ticket + linked spec (if present), dimension 2 reads repo ADRs (if present), and the `Examined` column names which were found, so a repo with no ADRs shows "no ADRs found" rather than a fake-confident architecture verdict. This is the same not-applicable-vs-clean discipline from Q7.
  - Reinforcing context: O1 KR2 already mandates OpenSpec-style specs per ticket and ADRs with expiration dates for the embed model, so these inputs should populate over time — and they're the explicit substitute for the tacit architectural knowledge the research says diff-level review is structurally blind to.
  - **Eval strategy (recommended, accepted implicitly):** shadow mode on the pilot repo **is** the eval harness, not a separate exercise. Backtest against already-merged PRs with known outcomes, then live-shadow without posting. Labels come free from Q9's passive signals. Two metrics that matter: **per-dimension precision on blocking findings** (gates whether the gate can ever open) and **false-negative rate on PRs later reverted or hotfixed** (the only honest measure of gate safety). Extends FN's existing DeepEval-in-CI pattern. Justification: the comparative eval found **92% F1 degradation** moving LLM reviewers from curated benchmarks to real uncurated PRs, and **Haiku beat Sonnet** on review tasks — per-dimension model choice must be measured, not assumed.
  - **Pilot repo: deliberately out of scope for this session.** Chuck: *"I'll figure that out once we implement the agent."* Noted for later: `engineering-dbt` (Repo 1, MOI-64) is a **weak first target for this specific agent** — dbt makes dimension 6 (data/schema) everything while dimensions 2 and 4 barely fire, so it would validate roughly one-sixth of the design. A Go or NestJS service exercises more dimensions and `tech-context.md` already flags those as most agent-friendly.
- **Flags:** pilot repo selection → Chuck, deferred to implementation time

### Q12 — Non-functional defaults (completeness backstop)

- **Asked:** Five remaining non-functional items, proposed as defaults. All approved ("I think this all looks good").
- **Captured:**
  - **DECISION: the gate FAILS OPEN.** Agent error or timeout → check run goes to a neutral "review unavailable" state, never a failure. An agent outage that blocks merges org-wide would end the project. **The gate can only add confidence, never remove the ability to ship.**
  - **DECISION: latency target p50 < 5 min, p95 < 15 min.** Must beat a human noticing the PR exists; against a 36-hour baseline, sub-15-minutes is effectively instant.
  - **DECISION: read-only on code, write only on PR conversation.** GitHub App scopes: `contents: read`, `pull_requests: write`, `checks: write`. **Never `contents: write`.** Structural (not prompt-based) mitigation for the agentic-tool-deleted-prod failure mode in the research.
  - **DECISION: language sequencing — Go and NestJS first, React/React Native second, PHP monolith last.** Per `tech-context.md`, the monolith has low harnessability (custom framework, no conventions to lean on, uneven test coverage): highest-value target, worst place to prove the concept.
  - **DECISION: cost controls before rollout, not after** — per-PR token budget, per-repo daily cap, and short-circuit dimensions that resolve to not-applicable. Six dimensions × 739 repos needs a ceiling that exists in advance.
- **Flags:** none

---

## Open flags (pending input)

Carried into both deliverables' "Open items" sections.

- ~~Correct `knowledge/tech-context.md`: Gitar is a separate contract from Sonar~~ — **applied 2026-08-06**
- **Is any auto-merge tier org-acceptable?** → Cory / Ben Moormeier / Mike Mueller. Governance, not technical. Candidate agenda item for the 8/11 review already scheduled.
- **Content-trace policy exception sign-off** (running the review agent with full-content traces, against the MOI-80/81 firewall precedent) → same group
- **Bedrock invocation logging is per account and region, not per application** — enabling it captures every Bedrock call in the account. Argues for a dedicated AWS account. → Chuck / AWS account owner
- **Triage queue owner, cadence, and physical home** → Chuck. Weekly batch looks right at pilot volume; unowned queues die.
- **Pilot repo selection** → Chuck, deferred to implementation. Note `engineering-dbt` (MOI-64) would meaningfully exercise only ~1 of 6 dimensions.
- **Per-dimension model selection** → Chuck, blocked on the eval harness existing (CR-130)
- **Where the O1 "bespoke vs. vendor" decision lands** — this session covered build only; the Gitar demo and Ali call are still pending Chuck's scheduling
