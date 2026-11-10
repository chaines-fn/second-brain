# PR merge-gating: how the industry decides auto-merge vs. human review, and a requirements spec for the O1 code-review evaluation

**Research question:** How is the industry actually deciding whether an AI-reviewed PR should auto-merge versus route to a human, and what does that imply as a concrete set of requirements Chuck can feed to an agent (or use to score Gitar, Tessl, and Copilot) for gating logic in Field Nation's O1 code-review evaluation?

**Date:** 2026-08-25

**Scope:** Vendor-evaluation focused, per Chuck's direction — this is meant to produce a shared bar to score Gitar, Tessl, and native Copilot review against, not a build spec for `fn-review-agent` specifically (though the requirements are reusable there too). Weighted toward the two risk signals Chuck flagged as priorities: diff size/blast radius and security-sensitive paths. Excludes general code-review-quality research (already covered in [2026-08-05-code-review-best-practices-human-and-agentic.md](./2026-08-05-code-review-best-practices-human-and-agentic.md)) and the broader industry-trends context (already covered in [2026-08-25-code-abundance-thesis.md](./2026-08-25-code-abundance-thesis.md), which flagged "the auto-merge criteria question appears untouched" in FN's OKRs — this report is the direct follow-up closing that gap).

---

## Summary

Nobody in this research treats "should this merge automatically" as a property of how good the reviewer is. Every credible source — Meta's RADAR system, Amplitude's risk model, Intercom's and Customer.io's production auto-approval systems, and the vendor tooling (Gitar, Macroscope, Tessl) — treats it as a separate, explicit gating layer sitting on top of review: a risk classifier decides whether a human needs to look, and review quality is a different, though related, question. The classifiers converge on the same handful of inputs regardless of who built them: diff size and file count, which subsystem or path the change touches (with security/auth/payments/schema as a near-universal hard escalation regardless of score), test coverage and CI status, and author/change-type trust signals. Four independent production systems (Meta, Amplitude, Intercom, Customer.io) all land on some version of a risk-tiered funnel rather than a single yes/no verdict.

The second convergent finding is a strict separation of authority: nowhere in this research does an AI reviewer hold unilateral merge power outside platform-enforced branch protection. GitHub Copilot's review is architecturally a "Comment," never an "Approve" or "Request changes." Claude Code Review's check always resolves "neutral" so it structurally cannot block or approve. Gitar and Macroscope both frame their auto-merge as arming the platform's native auto-merge once their own checks clear — the actual authority stays in branch protection, required status checks, and required reviewers. The pattern across every source is "AI reviews, policy merges" — never "AI merges."

The third finding, and the one most directly answering Chuck's standing "how did it review" objection to Gitar: the industry is not converging on reviewer explainability as the trust mechanism. It's converging on auditable gating — documented eligibility criteria, a logged decision with evidence, a severity tally a human or a separate system can read, and a way to override. Amplitude's SOC 2 framing states this most plainly: "SOC2 doesn't need a human to click approve. What it does need is demonstrable controls: documented criteria, logged decisions, and a way to override." Intercom independently arrived at the same shape, engaging their auditor (Schellman) before scaling, not after. This reframes the O1 evaluation question: the deciding factor for Gitar/Tessl/Copilot may not be which one explains its reasoning best, but which one's gating layer produces the cleanest audit trail and the most configurable policy surface.

Of the three vendors in FN's active evaluation, Gitar is furthest along on gating mechanics specifically (configurable merge-blocking threshold, auto-approve criteria, and auto-merge that arms native branch protection), Tessl has the pieces (`tessl review run` gating plus separate Verifiers for invariant checks) but is Beta with a documented hosted-eval reliability gap, and Copilot's native review is structurally incapable of ever being the gating authority — it can only ever be an input to a gate built elsewhere (branch protection, a bot, or a separate policy engine). None of the three currently publishes the "what was checked and judged fine" artifact Chuck has held Gitar to; that gap is closer to a policy-file / REVIEW.md-style fix (see the 8/25 report) than something a vendor selection alone resolves.

---

## Findings

### The shape every production gating system converges on

Meta's RADAR (already covered in depth in the 8/5 report) is the clearest instance of the pattern: eligibility gates → static heuristics → a machine-learned Diff Risk Score → LLM review → deterministic validation, with a single percentile threshold on the risk score separating auto-land from human-routed diffs. The mechanism is worth restating precisely because it's more concrete than the earlier summary captured: the score is expressed as a percentile threshold (P5, P25, P50...), where a lower percentile is more conservative — P5 means only the safest 5% of diffs qualify. Widening from the 25th to the 50th percentile raised Meta's auto-approval rate to 60.31% while keeping the safety ratio roughly constant (revert rate ~1/3 of baseline, incident rate ~1/50, both measured across the funnel, not per-percentile-bucket). The load-bearing detail for FN: this is only a defensible dial if you have revert/incident telemetry per score bucket to recalibrate against, a deterministic validation backstop that catches score under-estimates, and stable signal quality (churn, blast radius, authorship). Without that infrastructure, a risk-score threshold becomes an unverifiable guess rather than a tuned control — which argues for static path-based routing as the first-order gate at FN before attempting anything RADAR-style score-based.

Three independent, non-Meta production systems reach the same shape from completely different starting points. **Amplitude's** risk model scores size, scope, which part of the codebase is touched, test coverage, and whether the change touches public APIs or data models; low risk plus resolved review comments auto-merges, anything medium-or-above routes to a human via CODEOWNERS. **Customer.io's** three-layer system (context building → heuristic evaluation → decision) runs a "progressive complexity model" that treats complexity and risk as genuinely separate dimensions: Phase 1 measures architectural scope objectively (lines, files, boundaries crossed); Phase 2 amplifies that score by which risk domain is touched (auth/encryption/permissions score higher, and touching multiple domains takes the highest); only in Phase 3 do quality signals like test coverage, feature flags, and rollback plans raise confidence — notably, they never reduce the underlying complexity/risk score, only the confidence that a complex-but-real change is safe. Their published results: 500+ AI approvals, zero production incidents, merge time 3.5 hrs → 33 min for AI-approved PRs, high-risk PRs correctly taking longer (~1 day) because they're routed to humans rather than rushed. **Intercom's** system decomposes review into independent sub-agent passes (problem-statement quality, intent-match, safety, correctness, best-practices) rather than one monolithic verdict, and reports the sharpest quality result in this research: AI-authored, AI-approved backend code reverted at 0.53% versus 5.39% for human-authored/human-approved code (10x), with 19.2% of all PRs fully auto-approved and zero reverts across their initial 100+-PR pilot. Intercom's most quotable finding directly undercuts an assumption worth naming explicitly to Chuck: "those product changes that did cause outages in the past? They were all reviewed and approved by humans. Human review is not a guarantee of safety." All three explicitly reject rubber-stamping as the failure mode they're designing against, not the one AI review introduces — they frame the AI gate as reducing rubber-stamping relative to a human under time pressure, not adding a new risk on top of a clean baseline.

**Macroscope's "Approvability"** is the most productized version of this pattern and the closest packaged analog to what FN would buy rather than build. It runs three sequential hurdles — Ownership (CODEOWNERS-based, off by default), Eligibility (a dedicated LLM agent verdict classifying the PR as `SUGGEST_APPROVABLE` or `SUGGEST_HUMAN_REVIEW` against change type, scope, complexity, schema impact, author trust, and open review comments), and Correctness (a separate bug-finding pass; any MEDIUM-or-higher finding blocks approval regardless of the eligibility verdict) — and only posts a real GitHub `APPROVE` review when all three clear. Its default escalation list reads almost identically to Amplitude's and Customer.io's: schema changes with breaking impact, significant runtime behavior changes, gating logic itself (feature flags, permission checks, routing rules), major refactors, infrastructure/deployment changes, and — called out with "full stop" — security, authentication, billing, or sensitive data. It also enforces two structural rules worth carrying into FN's own spec regardless of vendor: the tool cannot approve its own PRs, and changes to the policy file that governs approval (`.macroscope/approvability.md`) can never themselves be auto-approved.

### The generic industry framing (maturity model and risk categories)

Independent of any single vendor, a useful generic framing has emerged (First AI Movers' 2026 enterprise guide, synthesizing the same GitHub/Stripe/Shopify/Uber precedents as the 8/25 report's case studies) that's worth adopting as shared vocabulary rather than reinventing FN's own: a six-level maturity model from **L0 manual review** through **L3 controlled auto-merge** (native auto-merge plus merge queue, low-risk lanes only) to **L6 bounded autonomous merge lanes** (policy-as-code, canary checks, auto-revert, signed provenance — but only inside pre-approved, low-blast-radius classes; "L6 is not 'AI can merge anything.'"). Layered on top, a four-category PR risk taxonomy: **Category A** (safe auto-merge — docs, formatting, generated snapshots, low-risk dependency patches), **Category B** (AI-fixable/helper-PR — narrow, well-understood mechanical fixes that go through the same gates as any PR), **Category C** (human review required — product logic, architecture, public API contracts, data models, auth/RBAC, billing, privacy, security, infrastructure), and **Category D** (blocked or split required — huge mixed-risk diffs, anything an automation can't bound). This gives FN a vocabulary that already exists industry-wide rather than a bespoke taxonomy the team would otherwise have to defend from scratch in a leadership conversation.

The same source's explicit list of what AI review tools should never do is worth stating directly since it's a hard requirement regardless of vendor: an AI reviewer must not bypass branch protection or rulesets, must not self-approve a PR it authored or co-authored, must not direct-merge outside a queue, must not read/write secrets the workflow doesn't need, must not be the sole reviewer on auth/payments/secrets/infrastructure/schema/deletion paths, must not act on prompt-injected content from the diff itself (the diff is untrusted input per OWASP's LLM Top 10, since it can embed content from external sources), and must run with read-only default permissions.

### The two signals Chuck flagged: blast radius and security-sensitive paths

**Blast radius / diff size** shows up in every system in this research as a first-class, usually the first-computed, signal — but the sophistication of how it's computed varies a lot and matters for vendor comparison. The crudest version (line count, file count) is what most tools default to. More sophisticated versions trace actual impact: Macroscope's agent "traces the call graph through Macroscope's AST reference graph" rather than just measuring diff size; Customer.io's system explicitly separates "architectural scope" (files/lines/boundaries crossed) from risk amplification (which domain is touched) as two different phases rather than one blended score; a dedicated "Blast Radius Analysis" pattern documented independently (an MCP-distributed Claude Code skill) frames the goal as mapping ripple effects via a code knowledge graph or semantic tracing/grep fallback, scoring on security sensitivity, execution flow, and caller density — conceptually close to what a bespoke FN tool would need to build if no vendor's blast-radius computation is trustworthy on FN's PHP monolith specifically (recall from `tech-context.md`: the monolith has low harnessability, so call-graph tracing tools built against clean-architecture codebases may not transfer well — this is a real evaluation risk to test directly against `fieldnation/fieldnation`, not assume).

**Security-sensitive paths** are treated with near-total consistency as a deterministic override that trumps whatever the learned/LLM risk score says — every source in this research (Amplitude, Macroscope, Customer.io's risk-amplification phase, the generic Category C/D taxonomy) escalates auth, payments, secrets, schema/data-model changes, and infrastructure to mandatory human review regardless of how clean the diff otherwise looks. The mechanism for encoding this is consistently CODEOWNERS plus, increasingly, GitHub's native **Required Reviewer Rule** (GA as of Feb 2026 per this research), which lets a team require specific team approval on path patterns independent of ownership — e.g., forcing two security-team approvals on any change under `auth/` regardless of who authored it. This is a deterministic, platform-native mechanism that composes with, rather than depends on, whichever AI reviewer FN picks — meaning FN can and arguably should implement the security-path override at the CODEOWNERS/branch-protection layer regardless of which vendor wins O1, rather than trusting any vendor's own path-sensitivity logic as the sole safety net. Sonar's own Quality Gate (relevant since FN has already signed Sonar, and Gitar is now a Sonar product) implements an adjacent but distinct control at this layer: a "Sonar way" gate condition requires all new Security Hotspots to be reviewed before the gate passes, and gates can additionally require zero new vulnerabilities or a minimum security rating on new code specifically — this is a deterministic, non-LLM control that could sit alongside any AI reviewer's gating decision as the "deterministic validation" stage RADAR's architecture calls for.

### Where Gitar, Tessl, and Copilot stand against this pattern today

**Gitar** has the most complete native gating implementation of the three as of this research. It supports **Merge Blocking** (an org-level configurable severity threshold — Critical/Important/Suggestion — above which Gitar submits a blocking review), a separate **Auto-Approve** feature (criteria-based, distinct from merge blocking), and **Auto-Merge**, which explicitly "arms your platform's native auto-merge" once Gitar's own approval clears — Gitar's docs state plainly that auto-merge "respects all existing branch protection settings, required reviews, and status checks," consistent with the "AI reviews, policy merges" pattern found everywhere else in this research. Its **Repository Rules** mechanism (`.gitar/rules/*.md`, natural-language, e.g. "assign security team when authentication code changes") is a real, shipping instance of the configurable-policy-file pattern Macroscope and Anthropic's REVIEW.md concept both independently converge on — and this is exactly what MOI-98 is already testing. Findings are categorized (Security/Bug/Performance/Edge Case/Code Quality) with severity (Critical/Important/Suggestion), which is usable raw material for a REVIEW.md-style "named pass" tagging scheme even though Gitar doesn't currently publish a positive "what I checked and judged fine" artifact — the gap flagged repeatedly in the 8/5 and 8/25 reports. Net: Gitar's gating mechanics are largely there; the open question is whether MOI-98's Repository Rules test can close the transparency gap without a vendor product change.

**Tessl** has the component pieces but earlier-stage and less proven at FN's scale. `tessl review run` can gate a PR on a skill's review score, and **Verifiers** are a separate mechanism for invariant checks (structured as name, instruction, relevant-when condition, context, scope, and a checklist) — conceptually this checklist structure is close to what a REVIEW.md-style named-pass system would need, and closer to a deterministic backstop than an LLM-only verdict. However, Mykhailo's 8/17 independent investigation (already logged in `tech-context.md`) found Tessl's hosted eval pipeline itself failed to complete a run and rejected 2 of 3 generated scenarios for rubric leakage — a direct caution against trusting Tessl's own gating/scoring infrastructure as production-ready without independent verification, on top of its explicitly Beta code-review status.

**GitHub Copilot's** native review is structurally incapable of ever being the gating authority by design, not by current limitation — its review is "a 'Comment' review, not an 'Approve' or 'Request changes' review," a deliberate choice mirrored by Anthropic's own Claude Code Review check (which "always completes with a neutral conclusion so it never blocks merging through branch protection rules," with the vendor's own stated recommendation being to read the severity breakdown out of the check run in your own CI if you want to gate on it). This means Copilot can only ever be an *input signal* to a gate FN builds elsewhere (a bot reading its severity output, or branch protection keyed on a separate check) — it cannot itself implement any of the merge-authority requirements below. Separately, MOI-97's finding (marketplace-distributed skills don't reach Copilot's automated review, only repo-committed ones) is directly relevant to whichever policy-file mechanism FN picks for the security-path override — if Copilot is part of the eventual stack, its policy source has to be committed per-repo, not delivered via the `fn-platform` marketplace bundle.

---

## Requirements: a merge-gating spec to feed an agent or score vendors against

The following is organized to be directly usable either as an evaluation rubric (score Gitar/Tessl/Copilot against each requirement: native / partial / absent) or as a build spec if handed to an agent implementing gating logic for `fn-review-agent` or a policy layer wrapping any of the three vendors.

**Risk classification**

1. The system must classify every PR into at least three tiers (e.g., auto-merge-eligible / needs-human / blocked-needs-split), not a single pass/fail verdict — every production system in this research (RADAR, Amplitude, Customer.io, Macroscope) uses a multi-tier classification, never binary.
2. The classification must separately score architectural scope/blast radius (files touched, lines changed, subsystem boundaries crossed) and risk-domain sensitivity (what the change touches), rather than blending them into one number — Customer.io's phase separation is the clearest evidence this distinction matters in practice.
3. Blast-radius scoring should trace actual impact (call graph, execution path, caller density) where the codebase supports it, not just line/file counts — but this must be validated against FN's PHP monolith specifically before being trusted, given its documented low harnessability.
4. Security-sensitive paths (auth, payments/billing, secrets, PII, schema/data-model migrations, infrastructure/deployment config) must be a deterministic override that forces human review regardless of how the learned/LLM score classifies the diff — implemented at the CODEOWNERS / branch-protection layer so it holds independent of which AI reviewer is in use.
5. The policy defining risk tiers and path sensitivity must live in a versioned, repo-committed file the tech lead or platform team controls (a `.gitar/rules`-, `.macroscope/approvability.md`-, or `REVIEW.md`-style artifact) — not a vendor default that can't be inspected or overridden.

**Merge authority**

6. The tool must never hold unilateral merge authority outside platform-enforced branch protection and required status checks — "AI reviews, policy merges," not "AI merges."
7. Any auto-merge feature must arm the platform's native auto-merge/merge-queue mechanism rather than replace it, so branch protection, required reviewers, and required checks stay authoritative.
8. The tool must not approve a PR it authored or co-authored (self-approval prohibition).
9. Changes to the gating policy file itself must never be eligible for auto-approval by the system that policy governs.

**Transparency and audit**

10. Every gating decision must be logged with the criteria version, the evidence considered, and the verdict, retrievable indefinitely — not just the PR's final state.
11. The system should publish a machine-readable severity tally per review pass (bugs, security, compliance, etc.), not just a single aggregate verdict.
12. Approval and validation should be logged as separate signals (Amplitude's SOC 2 framing) so an audit can distinguish "the AI said yes" from "the deterministic checks actually passed."
13. Where feasible, the system should publish what it checked and judged fine, not only what it flagged — this is the specific gap identified in Gitar today and is the direct target of MOI-98's Repository Rules test.

**Deterministic safety net**

14. Auto-merge eligibility must require passing deterministic checks (CI green, lint, type-check, security/SAST scan) in addition to any LLM verdict — never LLM-only.
15. The system should not be able to approve a PR that weakened the tests protecting the change it's approving (e.g., a fix that edits or deletes the failing test it was meant to satisfy).
16. Where FN has Sonar Quality Gate available, its new-code security-hotspot and vulnerability conditions should sit as an independent deterministic gate alongside any AI reviewer's verdict, not be superseded by it.

**Threshold governance**

17. Whatever threshold or score decides auto-merge eligibility must be exposed and owner-controlled, not a fixed vendor default — teams need to be able to move it deliberately, the way RADAR's percentile or Gitar's severity threshold both allow.
18. If a learned/scored threshold is used, FN needs revert/incident telemetry per score bucket to justify moving it — without that telemetry, prefer static path-based routing over a scored threshold, since an unmeasured dial is a guess, not a control.
19. A documented human-override path must exist at any stage, and a kill switch must let a single operator disable the automated path org-wide or per-repo during an incident.

**Trust and prompt-injection safety**

20. The reviewer must treat PR diff content as untrusted input and must not act on instructions embedded within it (indirect prompt injection per OWASP LLM01).
21. The reviewer's platform token/permissions should default to read-only, widened only for the specific write actions a feature requires (e.g., applying a suggested fix).

**Feedback and measurement**

22. The gating system needs its own feedback loop: when a human catches a PR that shouldn't have auto-merged, that case should feed back into recalibrating the risk model or policy, not just get logged as an incident.
23. FN should track, per repo, at minimum: auto-merge rate, revert rate split by auto-merged vs. human-reviewed, time-to-merge by risk tier, and — closest to a leading indicator of a broken gate — the rate of PR review findings that cite the gating policy itself trending toward zero (the 8/25 report's Anthropic-sourced metric: if it isn't falling, the policy isn't being enforced or has drifted from what the skill/rule encodes).

---

## Vendor scorecard against the requirements (as of this research)

| Requirement area | Gitar | Tessl | Copilot native review |
|---|---|---|---|
| Multi-tier risk classification | Has severity tiers (Critical/Important/Suggestion) feeding merge-blocking threshold | Has a review score gate (`tessl review run`) — scoring methodology not independently verified | None — comment-only, no gating concept at all |
| Blast-radius/diff-scope scoring | Not confirmed in docs reviewed — worth testing directly | Not confirmed — Verifiers are invariant-based, not diff-scope-based | N/A |
| Security-path deterministic override | Repository Rules (`.gitar/rules/*.md`) can encode this in natural language — MOI-98 is actively testing | Not confirmed as a native feature; would need to sit in Verifiers or CODEOWNERS externally | N/A — must be implemented entirely outside Copilot (CODEOWNERS/branch protection) |
| Merge authority stays with platform | Yes — explicitly "respects all existing branch protection settings, required reviews, and status checks" | Not independently confirmed | Yes by design — Comment-only, cannot approve or block |
| Self-approval prohibition | Not confirmed in docs reviewed | Not confirmed | N/A — cannot approve anything |
| Audit trail / logged decisions | Dashboard comment persists and updates in place; formal audit-log retrieval not confirmed | Not confirmed | Comments are visible in PR history but not a structured audit artifact |
| Publishes "what was checked and judged fine" | No — the identified gap; MOI-98 is the live attempt to close it via Repository Rules | Not confirmed | No |
| Deterministic backstop (CI/SAST) integration | Auto-merge requires other checks to pass, so it composes with any required CI/SAST check | Verifiers are explicitly deterministic-adjacent (checklist-based) | N/A — would rely entirely on separate branch protection |
| Owner-controlled threshold | Yes — org-level configurable severity threshold | Not confirmed as configurable | N/A |
| Policy-file mechanism reaches the tool | Yes, natively (`.gitar/review`, `.gitar/rules`) | Uses Tessl's own skill/config format | Confirmed (MOI-97): only repo-committed skills reach Copilot's automated review, not marketplace-distributed ones |
| Reliability of vendor's own gating/eval infra | No independent stress test found in this research | Mykhailo's 8/17 investigation found the hosted eval pipeline failed to complete a run and rejected 2/3 scenarios for rubric leakage — a real caution | N/A (no eval infra of its own for this purpose) |

Read this table as "what's structurally possible or already shipped," not as a final ranking — several cells (blast-radius scoring, self-approval, audit-log retrieval) reflect what wasn't confirmed in public docs rather than confirmed absence, and are worth direct questions to each vendor or a hands-on test against a real FN repo before scoring.

---

## Key uncertainties

Several vendor capability claims in the scorecard above (blast-radius computation, self-approval prohibition, audit-log retrieval) are absent-from-docs rather than confirmed-absent — worth a direct question to Gitar/Sonar and Tessl, or a hands-on probe against a real repo (similar to how MOI-97 and MOI-98 are already probing Copilot and Gitar respectively), before treating them as gaps.

None of the vendor-reported figures in this research (Gitar's, Macroscope's, Tessl's own capability claims) come from an independent benchmark — consistent with the same caution flagged in the 8/5 report for the broader tool landscape.

Intercom's, Customer.io's, and Amplitude's published numbers are all self-reported and, per the pattern noted in the 8/25 report for Amplitude specifically, generally lack a denominator, a controlled baseline, or third-party verification — directionally useful as evidence that the general approach works, not as a benchmark FN should expect to hit out of the box.

Whether FN's PHP monolith supports the kind of call-graph/blast-radius tracing that Macroscope and the newer blast-radius tooling depend on is untested — the monolith's documented low harnessability (custom framework, uneven test coverage) is a real reason this could work worse on `fieldnation/fieldnation` than on the clean-architecture codebases these tools were evidently built against. Worth an explicit test rather than an assumption either way.

The RADAR percentile-threshold approach requires revert/incident telemetry per score bucket that FN does not yet have (KR1's scorecard tracks aggregate revert rate, not bucketed by a risk score) — treat learned-threshold calibration as a later-phase capability, not something to expect from day one of any vendor's gating feature.

---

## Sources

**Risk-tiered gating architecture and industrial precedent:**
- [Automating Low-Risk Code Review at Meta: RADAR (arXiv:2605.30208)](https://arxiv.org/abs/2605.30208)
- [Risk-Score Threshold Calibration for Auto-Approval — AgentPatterns.ai](https://agentpatterns.ai/code-review/risk-score-threshold-calibration/)
- [AI Pull Request Auto-Merge: Enterprise Guide 2026 — First AI Movers](https://radar.firstaimovers.com/ai-pull-request-auto-merge-enterprise-guide-2026)

**Production auto-approval case studies:**
- [AI is approving our pull requests: Here's how we made it safe — Intercom](https://www.intercom.com/blog/ai-is-approving-our-pull-requests-heres-how-we-made-it-safe/)
- [How we taught AI to approve pull requests — Customer.io](https://customer.io/learn/how-we-work/how-we-taught-ai-to-approve-pull-requests)
- Amplitude, "Speedrunning Software" — covered in depth in [2026-08-25-code-abundance-thesis.md](./2026-08-25-code-abundance-thesis.md)

**Productized auto-approval tooling:**
- [What Is Approvability? AI Auto-Approve Pull Requests — Macroscope](https://macroscope.com/content/what-is-approvability-auto-approve-safe-pull-requests)
- [Code Change Blast Radius: Prevent Costly AI Coding Mistakes — Pharaoh](https://pharaoh.so/blog/code-change-blast-radius/)
- [Blast Radius Analysis | Claude Code Skill for Risk Scoring](https://mcpmarket.com/ko/tools/skills/blast-radius-impact-analysis)

**Vendor gating mechanics (O1 candidates):**
- [Gitar — Code Review docs (merge blocking, auto-approve, auto-merge, Repository Rules)](https://docs.gitar.ai/features/code-review)
- [Tessl — configuration and Verifiers docs](https://docs.tessl.io/reference/configuration)
- [Tessl code-review/gating skill registry examples](https://tessl.io/registry/skills/github/every-app/open-seo/merge-ready--merge-ready)

**Deterministic gates and platform-native mechanisms:**
- [Understanding quality gates — SonarQube Cloud/Server docs](https://docs.sonarsource.com/sonarqube-cloud/standards/managing-quality-gates/introduction-to-quality-gates)
- [Required reviewer rule is now generally available — GitHub Changelog](https://github.blog/changelog/2026-02-17-required-reviewer-rule-is-now-generally-available/)
- [Required review by specific teams now available in rulesets — GitHub Changelog](https://github.blog/changelog/2025-11-03-required-review-by-specific-teams-now-available-in-rulesets/)

**Confidence-threshold and safety framing:**
- [When Can We Trust LLM Graders? Calibrating Confidence for Automated Assessment (arXiv:2603.29559)](https://arxiv.org/html/2603.29559)
- OWASP Gen AI Security Top 10 (prompt injection, LLM01) — cited via the First AI Movers guide above

**Related internal context:**
- [2026-08-05-code-review-best-practices-human-and-agentic.md](./2026-08-05-code-review-best-practices-human-and-agentic.md) — RADAR and the Gitar transparency disagreement, first covered
- [2026-08-25-code-abundance-thesis.md](./2026-08-25-code-abundance-thesis.md) — Amplitude's risk model and the "auto-merge criteria question appears untouched" gap this report closes
- [knowledge/tech-context.md](../knowledge/tech-context.md) — MOI-97 (Copilot skill reach), MOI-98 (Gitar Repository Rules), Tessl eval-reliability findings

---

## Addendum (2026-08-25): a concrete implementation design using Gitar's Auto-Approve + Repository Rules

Chuck proposed a specific pairing after reading Gitar's docs directly: use `.gitar/config/approve.md` as the positive gate (auto-approve only if FN's criteria are met, otherwise fall through to human review) and Repository Rules (`.gitar/rules/*.md`) to explain *why* a human review is required when auto-approve doesn't fire. This is a good design and maps directly onto Requirements 1, 4, 5, and 13 above using features Gitar already ships — no vendor product change needed, and it's a concrete test plan for MOI-98.

### How the two features actually compose (confirmed from Gitar's docs)

Gitar's gating runs through three independent mechanisms per PR, in this order: **Block Merge** evaluates the review verdict against a severity threshold and, if breached, submits a platform-native "request changes" (GitHub) or withheld-approval (GitLab) review — this is a hard stop regardless of anything else. Only if there are no blocking findings does **Auto-Approve** run: it checks configured criteria (org settings plus `.gitar/config/approve.md`, combined with AND logic) against PR metadata, changed files, and the review results — explicitly *not* CI status, which Gitar's own docs say must be enforced separately via branch protection. If criteria are met, Gitar posts an approving review; if not, Gitar simply does nothing — no approval, no block, the PR just sits waiting on whatever your branch protection already requires. That "do nothing" behavior is exactly the "otherwise don't auto-approve" outcome Chuck wants, and it's already Gitar's native default once real criteria are configured — the design work is entirely in what `approve.md` says, not in any extra plumbing. **Auto-Merge** is a third, optional layer that only arms after Auto-Approve fires, and even then only executes once GitHub's own required status checks and required-approval count are separately satisfied — meaning FN's existing (or planned) branch protection stays the final authority no matter how `approve.md` is written.

Repository Rules are a genuinely separate engine — evaluated on PR open, new commits, CI failure, metadata changes, and merge/close — that can post comments, apply labels, assign reviewers, and call integrations (Jira/Linear/Slack) based on natural-language `when` conditions. Because path- and diff-based conditions are knowable at PR-open time, a rule can fire immediately, independent of whether the code review or auto-approve evaluation has finished.

### Recommended design

**Org-level `approve.md` criteria (the FN-wide non-negotiable baseline):** encode the security-sensitive-path exclusion here so it applies to every repo regardless of what a repo-level file says — e.g. "Never approve if the PR modifies files under `auth/`, `payments/`, `billing/`, any secrets/credentials handling, or database schema/migration files. Never approve changes to CODEOWNERS or `.gitar/` configuration. Never approve PRs with unresolved review comments." Org and repo criteria combine with AND, so this baseline can't be loosened by a permissive repo-level file — only tightened further.

**Repo-level `.gitar/config/approve.md` (repo-specific extensions):** add the diff-size/blast-radius threshold and any repo-specific low-risk allowlist here, since "small, self-contained" means something different for `engineering-dbt` (Emerald) than for `fieldnation/fieldnation` — e.g. "Only approve if the diff touches 5 or fewer files and the change is a documentation update, test-only change, or a mechanical rename/formatting fix with no logic changes."

**A paired Repository Rule that explains the "no" case**, not just detects the "yes" case — this is the piece that actually answers your "how did it review" bar. Something like:

```
---
title: "Human Review Required — Sensitive Path or Scope"
description: "Explain why auto-approve did not fire"
when: "PR modifies files under auth/, payments/, billing/, secrets handling, or database schema/migration files, OR touches more than 5 files"
actions: "Add 'needs-human-review' label, assign the relevant CODEOWNERS reviewer, and post a comment stating which specific condition (path or diff size) triggered mandatory human review"
---
```

The value of this pairing over either piece alone: `approve.md` silently withholds approval, which reads to an engineer as "Gitar didn't get to it yet" rather than "Gitar deliberately declined." The rule turns that silence into the exact "what I checked and why I didn't approve" statement this whole evaluation has been missing from Gitar.

### Three real gaps to close before trusting this in production

**Drift between the two conditions.** `approve.md` and the Repository Rule are two separate natural-language evaluations — there's no guarantee Gitar interprets matching prose identically across both engines. If the two conditions drift apart (even by wording), you can get a PR that's silently unapproved with no explanation, or a rule that fires explaining human review is needed on a PR Gitar actually did approve. Mitigation: maintain the sensitive-path list in exactly one place and copy it verbatim into both files rather than writing it twice from memory; re-verify both after any edit using `gitar display:verbose` on a test PR that should trip the condition.

**`approve.md` cannot see CI, test status, or (per this research) call-graph blast radius** — only PR metadata, changed files, and review findings. Treat its diff-size check as a shallow proxy (file count, path patterns), not the sophisticated call-graph tracing Macroscope's approvability agent does. The deterministic backstop (CI green, Sonar Quality Gate's new-code security-hotspot check) has to keep living in branch protection regardless of how well `approve.md` is written — this was Requirement 14 above, and Gitar's docs confirm it's a hard architectural boundary, not a configuration choice.

**Don't make `approve.md` the only place the security-path exclusion lives.** Configure CODEOWNERS plus GitHub's Required Reviewer Rule for `auth/`, `payments/`, schema files, etc. independently of Gitar, so the exclusion holds even if `approve.md` has a bug, a wording gap, or Gitar has an outage. This is Requirement 4 from the spec above, and it's the belt-and-suspenders version of what you're already building with the Rule-based explanation — one layer explains the decision, the other layer enforces it even if Gitar's reasoning fails.

**One practical constraint to confirm before committing to multiple explanatory rules:** Repository Rules are capped at 5 per organization on Gitar's Pro plan (unlimited on Enterprise). If FN wants separate rules per sensitive category (auth, payments, schema, infra) rather than one consolidated rule, confirm which plan tier the signed contract is on first — worth a quick check against the order form Sonar sent 8/24.

### Recommendation

Build it exactly as proposed — `approve.md` as the gate, a Repository Rule as the explainer — with the org/repo split above so the security-path baseline can't be accidentally loosened per-repo, and pair both with a CODEOWNERS/Required Reviewer Rule backstop that doesn't depend on Gitar at all. This is a concrete, shippable test plan for MOI-98 rather than a separate design exercise: point that ticket at exactly this pairing, and the "did Repository Rules close the transparency gap" question the 8/25 report left open gets a real answer instead of a hypothesis. Happy to draft the actual `approve.md` and rule file content for a specific repo if you want a starting point to test against.
