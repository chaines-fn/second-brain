# Team Moissanite: Daily Standup
**Date:** 2026-09-30  
**Format:** Async (Slack thread) — Chuck in conflicting meeting  
**Participants:** Mykhailo Sakhnik, Chuck Haines, Asad Ali (via prior day context)

---

## Standup Updates

### Mykhailo Sakhnik
**Status:** In Progress  
**Completed since 2026-09-28:**
- Updated PR [ai-repo-score#23](https://github.com/fieldnation/ai-repo-score/pull/23) with new prompt from Chuck's skill-creator + agent-writing consolidation
- Added OpenCode support (test running on Go, Node/TS follow-up)
- Added fn-skill-creator check to repo scorecard

**Today's Focus:**
- Resolve minor issue with temporary file cleanup created by skill
- Continue work on [MOI-145: Migrate ai-toolbox evals from DeepEval to Harness eval](https://linear.app/fieldnation/issue/MOI-145/migrate-ai-toolbox-evals-from-deepeval-to-harness-eval)

**Blockers:** None mentioned; PR #23 now unblocked with new prompt

### Chuck Haines
**Status:** Unavailable for sync (in external meeting)  
**Prior Context (2026-09-28):**
- Approved PR #20 for merge
- Holding PR #23 pending prompt consolidation between skill-creator and agent-writing capabilities in ai-toolbox
- Working on combining these write-guidance contexts so the same rules apply to both agent-generated and skill-generated content

### Asad Ali
**Status:** Completed prior commitment  
**Prior Context (2026-09-28):**
- Moved adr-tools module to ai-toolbox in [PR #79](https://github.com/fieldnation/ai-toolbox/pull/79)
- Addressed all Gitar-bot code-review comments ahead of merge
- ADR skills (create/enforce/review) consolidation in ai-toolbox now awaiting review

---

## Key Decisions & Notes

- **Prompt consolidation approach:** Chuck is unifying the write-quality standards between agent-generated content (PRs, tickets) and skill-generated content (CLAUDE.md, READMEs, docs). This surfaces in both MOI-93 (humanize UI skill) and the ai-toolbox repo-scorecard prompt.
- **Eval migration path:** MOI-145 signals a formal shift from the custom DeepEval framework to Harness Eval for skill testing — Mykhailo now owns that transition.
- **No blockers:** Team is clean on dependencies and waiting time. PR #20 can merge immediately, PR #23 unblocked.

---

## Next Steps

1. **Mykhailo:** Complete temp-file cleanup in skills, then transition to MOI-145 (Harness eval migration work)
2. **Asad:** Await PR #79 merge signal from Chuck (likely depends on prompt consolidation completion)
3. **Chuck:** Complete prompt consolidation, then review/unblock PRs and continue pair work with Asad on ADR skill conflicts

---

## Metrics & Health

- **PR queue:** 2 PRs (ai-repo-score #20/#23) ready/nearly-ready; ai-toolbox #79 in review
- **Active OKR work:** MOI-110 (scorecard to 100%),MOI-145 (eval framework migration), MOI-93 (humanized skills)
- **Team velocity:** Steady — no blockers, clean handoffs, async coordination functioning well
