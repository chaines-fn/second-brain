# Team Moissanite: Daily Standup — 2026-09-25 (Async)

**Date:** 2026-09-25
**Format:** Async Slack thread (Chuck unwell)
**Time:** Started 07:18:38 CDT

## Attendees
- Chuck Haines (out sick, posted thread request)
- Asad Ali (responded)
- Mykhailo Sakhnik (referenced as reviewing work)

## Standup Updates

### Asad Ali — MOI-105 (Harness Evals vs Deep Eval)
- Completed evaluation comparing Harness Evals against the team's custom Deep Eval setup
- Has evaluation scripts/code ready for review
- Will update Linear ticket with findings
- **Note:** Scripts written for evaluation only, no merge required
- Waiting on team review before proceeding

### In Progress
- MOI-109: Gitar/Copilot code review integration into repo-readiness score (ongoing — Mykhailo owns)
- ADR skills PRs: Asad demoed Option A format in previous standup; awaiting team feedback before raising PRs to ai-toolbox and ai-repo-score

## Blockers / Owned Items

### Harness CI Integration (from 2026-09-18)
Three unresolved items blocking CI setup (Vitalii flagged, Joe/Chuck own):
1. PR trigger UI creation (`ai_toolbox_validate`) — YAML ready, needs admin
2. GitHub connector secrets for AI Guild — not yet created
3. Commit status write permissions on GitHub App — needs org owner approval

### PR/Ticket Quality (from 2026-09-24)
- Chuck flagged: AI-written content in PR descriptions and Linear tickets is hard to read
- Team should use skills like `fn-write-clearly` to humanize output
- Each owner responsible for making their agent output readable

## Decision/Direction
- **Gitar selected for O1 code review** (moved away from Copilot evaluation)
- MOI-109: Repo-readiness score should require Gitar (not Copilot)

## Next Steps
1. Asad: await team review on MOI-105 findings
2. Mykhailo: update repo scorecard for Gitar integration (MOI-109)
3. Team: resolve Harness CI blockers (Vitalii's three items)
4. ADR work: await feedback on filename format choice (A vs B), then open PRs

---

**Last updated:** 2026-09-25 (sync)
