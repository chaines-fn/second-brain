# Harness Evals vs. DeepEval: migration feasibility for ai-toolbox

**Research question:** Is it feasible to move ai-toolbox's evals from DeepEval to Harness Evals, without losing functionality — and if so, what's the plan of attack?
**Date:** 2026-09-28
**Scope:** ai-toolbox's eval suite only (skill-quality and skill-discovery evals). Excludes auditing other repos for DeepEval usage, per Chuck's call.

---

## Summary

MOI-105 already answered most of this. Asad ran a PoC comparing Harness Evals against ai-toolbox's DeepEval setup on the two things that matter — does a skill actually improve output quality, and does it get invoked when it should — and both passed cleanly: the `fn-api-design` skill scored 52% without the skill and 100% with it under Harness's `GEval`, and skill-discovery hit 6/6 on `ToolCorrectnessMetric`. The third leg, CI gating, is marked PARTIAL: the pytest/JUnit conversion works locally (10/10), but nobody has run it through an actual Harness pipeline yet. Chuck's own comment on the ticket, posted about twenty minutes before this research started, already reads as a verdict: "we should move forward with harness evals." This report exists to pressure-test that verdict against what's publicly known about the two products, and to turn it into a plan Chuck can hand to whoever picks up the migration ticket.

The short version: there's no functionality loss for what ai-toolbox actually uses today (a G-Eval-style LLM judge and a tool/skill-invocation-correctness check), but there are two things worth being explicit about before this becomes a ticket. First, both halves of "Harness Evals" are very new — the open-source `harness-evals` SDK is at v0.24.0 and self-describes as **Alpha**, and the hosted CI/CD product ("AI Evals") only came out of private beta in late July 2026. Second, despite the metric class names matching DeepEval's (`GEval`, `ToolCorrectnessMetric`), Harness's own docs say DeepEval only *influenced* the interface pattern — it's a separate implementation, not a compatible one. So this is a real migration (rewrite the eval code against a new API), not a config swap. Neither point is a blocker; both are things to size into the ticket.

The bigger practical dependency is internal, not vendor-side: the CI-gating validation that's still open in MOI-105 needs a live Harness pipeline run, and that's tangled up with the three Harness admin/config blockers (PR trigger UI, GitHub connector secrets, commit-status write permission) that have been sitting unresolved since the 9/16 standup, owned by Joe. Per Chuck's call, this migration ticket should absorb closing those out rather than just referencing them as a dependency, since the eval work can't actually finish CI validation without them.

---

## Findings

### What the internal PoC actually showed

MOI-105 ("Investigate Harness Evals vs Deep Eval in ai-toolbox") was created 9/3, worked 9/10 through 9/18, sat In Review for ten days, and was marked Done today (9/28) — the same day Chuck asked for this research, which lines up: Chuck's closing comment is what prompted this ticket request. Mykhailo's PoC report (linked from the 9/18 comment) tested against the three requirements the ticket itself defined: eval support for skill-vs-baseline comparison, eval support for skill-invocation/discovery testing, and CI support.

| Area | Test | Result | Detail |
|---|---|---|---|
| Skill quality | Ran the same `fn-api-design` evals with/without the skill, scored by Harness `GEval` | PASS | Score went from 52% (no skill) to 100% (with skill) — Harness reproduced the existing benchmark's semantics |
| Skill discovery | Tested whether `fn-api-design` auto-invokes from its description + a prompt, via the `skill.invoked` runtime event | PASS | 6/6 cases correct; Harness's `ToolCorrectnessMetric` handled the deterministic invocation check |
| CI gating | Converted eval results to pytest quality gates with JUnit output, run in an isolated Harness pipeline | PARTIAL | 10/10 passes locally; the actual Harness pipeline run is still pending |

Chuck's take, in his own words on the ticket: "Based on this it looks like we should move forward with harness evals." That's the internal feasibility verdict this plan is building on, per your call to treat the PoC as the research rather than re-litigating it.

### What's actually true about Harness Evals, cross-checked against the vendor

Two separate things both go by "Harness Evals," and it's worth keeping them apart because they carry different risk:

**`harness-evals`** is the open-source evaluation SDK (Apache 2.0, on PyPI and GitHub). As of this research it's at **v0.24.0** (released 9/24/2026 — four days before this report) and explicitly labeled **Alpha**. It's a real, reasonably well-designed framework — five evaluation dimensions (Correctness, Groundedness, Safety, Trajectory, Performance), 50+ metrics, a `GEval` class with rubric-level scoring, a `ToolCorrectnessMetric` paired with `ToolArgumentMatchMetric` for argument-level checks, OTel/Langfuse export. Its own docs say DeepEval and RAGAS "influenced" the `measure()`/`a_measure()` interface pattern but are "not hard dependencies" — meaning the metric *names* look familiar but the classes, constructors, and scoring internals are Harness's own. GitHub shows modest adoption: 15 stars, 7 forks, 18 open issues. This is not a mature, battle-tested library yet.

**Harness AI Evals** is the hosted product — the piece that runs as a native step in a Harness CI/CD pipeline alongside Build/Test/Deploy, with pass/fail thresholds that block releases, plus RBAC/audit/OPA governance. Harness's own blog post announcing it is dated **7/21/2026** and was still inviting people to "request to start the beta" at launch — so as of today it's roughly two months past its beta announcement. No public pricing; that'll need an actual account conversation, distinct from whatever FN's existing Harness CI/CD contract covers (worth checking whether AI Evals is bundled or a paid add-on before this goes far).

Neither of these is a red flag on its own — FN is already using Harness for CI/CD, so there's an existing vendor relationship and some in-house Harness familiarity (Joe has admin access; a Gitar service account already lives in Harness). But "actively evolving, low-adoption product two months out of beta" is a real characteristic to plan around: expect API surface to shift, expect gaps in documentation, and don't assume long-term support commitments the way you would with an established tool.

### Feature parity: what would actually be lost

DeepEval's full metric catalog is broad — RAG-specific metrics (contextual precision/recall, faithfulness), conversational metrics (knowledge retention, role adherence), safety metrics (bias, toxicity, PII), multi-modal metrics, plus the Confident AI cloud platform for dataset/golden management and production monitoring. None of that is what ai-toolbox is actually exercising today, per how MOI-105 scoped the comparison: it's using DeepEval for exactly two things — an LLM-judge quality score (`GEval`) and a tool-invocation correctness check (`ToolCorrectnessMetric`-equivalent) — both of which the PoC confirmed Harness Evals covers. So for the current use case, there's no functionality gap.

The caveat: if ai-toolbox evals ever grow into RAG-style or conversational skill testing, harness-evals' metric library, while broad on paper (50+), is newer and thinner in practice than DeepEval's. That's a "watch this" for later, not a blocker now.

One thing worth flagging rather than assuming: the 8/25 code-abundance research note recorded that an 8/18 weekly (sourced from auto-generated notes, flagged as directional, not verbatim) may have already moved FN's stated direction toward "bespoke database evaluations over external frameworks," which would supersede rather than complement DeepEval. MOI-105's framing ("the team's custom Deep Eval setup") suggests DeepEval is still the actual standard in ai-toolbox today, so this migration question is live — but it's worth a five-second gut-check with Chuck on whether that 8/18 direction ever actually landed, since it would change what "migrate away from DeepEval" even means.

### The real dependency: Harness CI blockers

MOI-105's CI-gating leg is PARTIAL because there's no live Harness pipeline run yet. Separately — and this is the connection worth making explicit in the ticket — the team has had three Harness CI/CD configuration blockers open since the 9/16 standup, still unresolved as of 9/25, owned by Joe:

1. PR trigger UI creation (`ai_toolbox_validate`) — YAML is ready, needs Harness admin to create it
2. GitHub connector secrets for AI Guild — not yet created
3. Commit-status write permissions on the GitHub App — needs org-owner approval

These are the same category of "Harness admin/config" work that a live eval pipeline run would also need. There's a real chance closing these out is what actually unblocks MOI-105's last PARTIAL item, not a separate parallel effort. Per Chuck's call, the migration ticket should include closing these three out rather than just linking them as a dependency.

---

## Plan of attack (ticket-ready)

**Suggested ticket title:** Migrate ai-toolbox evals from DeepEval to Harness Evals

**Suggested description:**
MOI-105 validated Harness Evals as a viable replacement for ai-toolbox's DeepEval setup — skill-quality scoring (`GEval`) and skill-invocation correctness (`ToolCorrectnessMetric`) both passed against existing benchmarks. This ticket executes the migration: port the existing eval suite to `harness-evals`, resolve the outstanding Harness CI/CD configuration blockers needed to run evals as a real pipeline gate, cut DeepEval over, and retire it once Harness is running in CI. Scope is ai-toolbox's eval suite only.

**Task breakdown:**

1. **Confirm licensing/cost of Harness AI Evals against FN's existing Harness contract** — before writing code, check with the Harness account rep whether the hosted AI Evals module is included in the current CI/CD contract or is a separate line item. (Owner: Chuck/Joe — this is a commercial question, not an engineering one.)

2. **Resolve the three outstanding Harness CI blockers** (carried over from 9/16, still open 9/25): PR trigger UI creation for `ai_toolbox_validate` (needs admin), GitHub connector secrets for AI Guild, commit-status write permission on the GitHub App (needs org-owner approval). These block both the toolbox create-trigger workflow and, very likely, this migration's own CI validation.

3. **Port the skill-quality eval suite** — rewrite the `fn-api-design` (and other skill) quality tests against `harness-evals`' `GEval`, including its rubric-level scoring option, which DeepEval's version doesn't have. Re-validate the same 52%→100% benchmark the PoC already established, plus run it across the rest of ai-toolbox's skills, not just `fn-api-design`.

4. **Port the skill-discovery eval suite** — rewrite the invocation-correctness tests against `ToolCorrectnessMetric`/`ToolArgumentMatchMetric`, using the `skill.invoked` runtime event as the PoC did. Confirm the 6/6 result holds across the full skill set, not just the one skill tested in the PoC.

5. **Stand up the CI pipeline gate** — take the pytest/JUnit conversion that already passes locally (10/10) and run it as an actual Harness pipeline step, dependent on task 2 being unblocked. Set pass/fail thresholds and confirm a failing eval actually blocks the pipeline the way DeepEval's current gate does.

6. **Parallel-run period** — run both DeepEval and Harness Evals in CI side by side for a defined window (suggest one to two sprints) before removing DeepEval, so any drift in scoring or missed cases shows up before the old system is gone.

7. **Cut over and retire DeepEval** — once parallel-run results match and the team's comfortable, remove the DeepEval dependency and CI step from ai-toolbox, update any docs/READMEs that reference it.

8. **Document the new eval setup** — update ai-toolbox's contributor docs to reflect the new `harness-evals` API, since it's not a drop-in syntax match for anyone who wrote DeepEval-based tests before.

**Suggested owner:** Asad, since he ran the PoC and already has the evaluation scripts — though worth checking his bandwidth against MOI-97/98 and the Gitar work.

**Dependencies/risks to note on the ticket itself:**
- `harness-evals` SDK is Alpha (v0.24.0) and the hosted AI Evals product is ~2 months past beta launch — expect API drift; don't treat this as a mature, stable dependency.
- Pricing/licensing for the hosted module is unconfirmed — resolve before or in parallel with the engineering work, not after.
- Task 2's Harness admin/org-owner approvals have been open for two weeks already; if those stay stuck, task 5 stays blocked regardless of engineering progress on 3–4.

---

## Key uncertainties

- Whether the 8/18-weekly signal about "bespoke database evaluations over external frameworks" superseding DeepEval ever actually landed as a decision, or was just a discussion point that got overtaken by MOI-105's framing. Worth a quick confirm with Chuck since it would reframe what this migration is actually replacing.
- Exact pricing/licensing terms for Harness AI Evals — not publicly disclosed; needs a real conversation with the account team.
- Full detail of Mykhailo's PoC report (`harness_evals_vs_deepeval_report.md`, attached to the MOI-105 comment) couldn't be pulled in full — the file's signed URL was too long for the fetch tool to retrieve. The comment's own summary table above is the report's stated bottom line, so this shouldn't change the verdict, but if there's methodology detail Chuck wants before finalizing the ticket, it's worth opening that attachment directly from Linear.
- Whether ai-toolbox's DeepEval usage today includes anything beyond the two use cases MOI-105 tested (e.g., any conversational or RAG-style skill tests) that wouldn't show up in this comparison.

---

## Sources

- [MOI-105 — Investigate Harness Evals vs Deep Eval in ai-toolbox](https://linear.app/fieldnation/issue/MOI-105/investigate-harness-evals-vs-deep-eval-in-ai-toolbox) (internal, Linear)
- [AI Evals - The Quality Gate for AI Agents | Harness](https://www.harness.io/products/ai-evals)
- [Ship AI Agents You Can Trust… (Harness AI Evals launch blog, 7/21/2026)](https://www.harness.io/blog/introducing-ai-evals)
- [Catch AI Regressions Before They Ship with AI Evals in CI/CD](https://www.harness.io/blog/catch-ai-regressions-before-they-ship-with-ai-evals-in-ci-cd)
- [harness-evals on PyPI](https://pypi.org/project/harness-evals/)
- [harness/harness-evals on GitHub](https://github.com/harness/harness-evals)
- [DeepEval — Metrics Introduction](https://deepeval.com/docs/metrics-introduction)
- Internal: `knowledge/meetings/` standups 2026-09-15 through 2026-09-25 (Harness CI blocker tracking); `program/overview.md`; `research/2026-08-25-code-abundance-thesis.md`
