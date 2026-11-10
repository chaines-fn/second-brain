# Team Moissanite: Async Standup — 2026-09-30

**Context:** Chuck had a conflicting meeting that day and asked the team to standup async in the #moissanite-eng Slack thread.

**Attendees (reporting):**
- Chuck Haines (facilitated async)
- Mykhailo Sakhnik
- Asad Ali (implied, but no update in thread captured)

---

## What We Did

### Mykhailo Sakhnik

**Completed:**
- Updated [ai-repo-score PR #23](https://github.com/fieldnation/ai-repo-score/pull/23) to use new prompt
- Added OpenCode support to ai-repo-score
- Added check for `fn-skill-creator` in the repo readiness tooling

**In Progress:**
- Resolving minor issue with cleaning up temporary files created by the skill
- Started MOI-145: [Migrate ai-toolbox evals from DeepEval to harness eval](https://linear.app/fieldnation/issue/MOI-145/)

### Chuck Haines

**Actions Taken:**
- Approved [ai-repo-score PR #20](https://github.com/fieldnation/ai-repo-score/pull/20)
- Held [ai-repo-score PR #23](https://github.com/fieldnation/ai-repo-score/pull/23) pending prompt improvements

**Work in Progress:**
- Combining "writing for agents" and "skill-creator" in ai-toolbox to streamline tooling
- Reviewing Asad's ADR work (adr-tools migration to ai-toolbox)

### Asad Ali

**Completed:**
- Moved adr-tools to ai-toolbox repo ([PR #79](https://github.com/fieldnation/ai-toolbox/pull/79))
- Addressed Gitar bot review comments on the PR

---

## Key Signals

**Repo Readiness Progress:** Mykhailo's ai-repo-score work is advancing on multiple fronts (prompt improvements, skill integration checks, temp file cleanup). This feeds directly into MOI-110 and KR1 work.

**Eval Infrastructure Migration:** MOI-145 (DeepEval → Harness Eval) started. This is infrastructure foundational to O2 agent work.

**Skill Infrastructure Consolidation:** Chuck is actively consolidating "writing for agents" and "skill-creator" in ai-toolbox — an efficiency play that reduces duplication and improves DX for skill builders.

**ADR Consolidation:** Asad's adr-tools migration to ai-toolbox (PR #79) continues the pattern of pulling scattered tools into the ai-toolbox registry.

---

## Status

- **Blockers:** None surfaced.
- **Decisions:** None.
- **Next standup:** 2026-10-01 (daily cadence). Redwan still in calendar but rolled off project earlier in Q3; check attendance.

---

*Synthesized from [Slack thread](https://fieldnation.slack.com/archives/C0AQW1KRQ20/p1790770750030009), 2026-09-30 async standup.*
