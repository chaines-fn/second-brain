# Obsidian Daily Sync + Stakeholders (WWT Phase 1 Daily): 2026-11-11

**Date & Time:** Wednesday, November 11, 2026, 8:45 CST (~15 minutes scheduled)

**Attendees:**
- Maria Saxton: WWT Phase 1 lead, organizer, stakeholder review owner
- Chuck Haines: Sr. Principal Software Engineer
- Andrew Kandels: Engineering lead
- Yurii Dmytriiev: Contractor, Frontend Engineer (absent for part of the holiday period, reproduced the bundle ID issue earlier)
- Ivan Sakhman: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Karol Tluscik: Frontend Engineer, joining full-time for this quarter
- Todd Folie: QA / WWT testing, reports production bugs
- Laura Quist (Douglas): Sr. Customer Success Executive, WWT voice

**Original Transcript:** [Notes by Gemini](https://docs.google.com/document/d/1FxpuIgw24hRfQdofZoodeWpBRqL81Evs9XH00S1npg0/edit?usp=drivesdk&ouid=104527139387599317887) (notes only, no transcript attached)

---

## Overview

This daily was a stakeholder review of ready-to-test tickets, with rollout planning for the following week. Several tickets were closed or verified. The magnifying glass search fix now keeps the user on the active tab instead of switching to the All tab, and Todd tested it during the meeting and marked it complete. A relative date filter ticket (696) was merged by Karol but landed on the project listing page instead of the work orders tab under the project page. Maria kept it in stakeholder review and told the team not to test it until Karol can correct the placement the next day.

Maria also set the rollout planning for the project Flightboard changes for next week. Andrew will coordinate the exact time of day for deployment with Maria so the team can monitor usage and be ready for user questions.

---

## Key Notes

- **Magnifying glass search keeps active tab:** Enhancement is ready and was tested by Todd during the meeting. Marked complete.
- **Ticket 696 relative date filters in the wrong place:** Karol merged relative date options, but they were added to the project listing page rather than the work orders tab under the project page. Testing paused until Karol reworks it. Maria left the stakeholder-review label on the ticket as a reminder.
- **Bundle ID filter ticket closed:** Yurii documented reproduction steps, and Maria and Andrew agreed they were sufficient to close the ticket without more testing.
- **Time awaiting approval filter verified:** Todd tested it the previous day and confirmed it works, so the ticket was closed.
- **Mass approval decimal fix closed:** Ivan found and fixed the issue (bonus and additional payment fields defaulted to 0.50 and blocked decimal entry) while working another task. The fix is merged with tests, so Maria closed the ticket.
- **Rollout next week:** A project Flightboard rollout is targeted for next week. Timing (time of day) is still to be set so the team can respond to user issues.

---

## Action Items

- **Andrew Kandels:** Discuss rollout timing with Maria to set the exact time of day for deploying the project Flightboard changes next week.
- **Unassigned (implied, Karol Tluscik):** Discuss ticket 696 with Karol at tomorrow's follow-up to fix the misplaced relative date filters on the project listing page.
