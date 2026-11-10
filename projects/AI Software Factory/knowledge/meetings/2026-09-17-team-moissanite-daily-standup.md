# Team Moissanite: Daily Standup — 2026-09-17 (Async)

**Date & Time:** Wednesday, September 17, 2026 (async thread, morning)

**Attendees (Slack thread):**
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead (thread organizer)
- Mykhailo Sakhnik — Contractor, repo-readiness toolchain (status update)
- Vitalii Belinskyi — Contractor, ai-toolbox infrastructure (personal leave notice)

**Channel:** #moissanite-eng (Slack thread: https://fieldnation.slack.com/archives/C0AQW1KRQ20/p1789649519923619)

---

## Overview

Chuck declared an async standup due to the AI Guild meeting scheduled for the morning. Mykhailo provided status on two in-progress tickets: MOI-114 (repo scorecard packaging for easy install) and MOI-105 (Harness Evals vs. Deep Eval comparison). Vitalii notified the team he'd need to work evenings/nights this week due to a family emergency (grandmother's Parkinson's flare-up). Chuck acknowledged and expressed support.

---

## Key Notes

- **MOI-114 (repo scorecard packaging) ready for implementation review:** Mykhailo has completed the preparation work and is seeking Chuck's green light and directional input on the approach before proceeding. Comments on the ticket outline the strategy.

- **MOI-105 (Harness Evals vs Deep Eval) in active investigation:** Mykhailo is running multiple scenarios to compare the two evaluation frameworks' behavior and output quality in the ai-toolbox context.

- **Windows and Linux support flagged as a future need:** Chuck noted the team has Windows and Linux users and asked whether repo-scorecard packaging can support both platforms, or if macOS-only (the current state) is acceptable for v1. Mykhailo indicated Windows is out of scope for v1 but acknowledged it will be needed eventually.

- **Distribution strategy clarity needed:** Chuck asked about the Homebrew tap repo and clarified that GitHub direct download + manual installation is acceptable for v1, with Windows support prioritized over Homebrew for the future.

- **Vitalii working compressed hours this week:** Vitalii will be available in evenings and nights while managing family care; team expressed support without requiring any schedule changes from him.

---

## Action Items

- **Mykhailo Sakhnik:** Proceed with MOI-114 implementation per the outlined strategy once Chuck reviews and approves; continue MOI-105 scenario runs.

- **Chuck Haines:** Review MOI-114 comments and provide go-ahead or directional feedback.

- **Team:** Continue async/evening-flexible standup pattern while Vitalii is managing family care; no timeline change expected.

---

## Open Questions / Next Steps

- Detailed evaluation results from MOI-105 (Harness vs Deep Eval comparison) pending scenario completion.
- Rollout of the packaged repo scorecard tool to the 100-repo target list once MOI-114 lands.

---

*This standup was conducted asynchronously in Slack due to a conflicting AI Guild meeting on Chuck's schedule. Format mirrors the daily standup structure to maintain continuity with the team's review/synthesis practice.*
