# UI Modernization Sync - 2026-10-14

**Date & Time:** Wednesday, October 14, 2026, 7:45 CDT (~11 minutes; transcript ends at 11:00)
**Source:** Google Meet Gemini notes and transcript, [transcript doc](https://docs.google.com/document/d/1MqXYRes3RxYUWGxjwgHvFSUjbUQnRumRYFwa6SC888I/edit) (organizer: Andrew Kandels)
**Attendees (invited):** Andrew Kandels (organizer, tech lead), Chuck Haines (Sr. Principal Engineer), Yurii Dmytriiev (contractor, frontend), Ivan Sakhman (contractor, frontend), Andriy Kurenda (contractor, frontend), Miroslav Isikiyski (backend), Maria Saxton (WWT Phase 1 / project lead), Fayshal Uddin (core-ui reviews and releases), Laura Quist (CS, optional), Karol Tluscik (joining the team)
**Speaking in transcript:** Yurii, Andrew, Miroslav, Chuck, Ivan, Andriy, Karol (only welcomed)
**Notes quality:** Gemini-generated. Transcript contains transcription errors (for example, "movie publishing" appears to mean the MUI publishing issue, and "Exos" is unverified). Review before relying on specifics.

**Relationship Context:** n/a (group meeting)

---

## Overview

The team added Karol Tluscik to the daily sync and team board. Andrew described the migration plan for the Match-owned pages (routing, ADR decisions, reusable components, and analytics syncing) and said Chuck had reviewed it the day before with only minor feedback. Four Match migration tickets have verified acceptance criteria and are in the to-do column. Priority order for this work is bugs first, Project Flightboard second, and migrate Match third.

Project templates and the project management features are complete and ready for QA. Andrew proposed handing the full project page (templates, manage project, break-fix) to Laura Quist and Todd Folie for testing, with Lindsay added to test break-fix across roles under OBS-545. Admin role smoke testing is considered sufficient for every tab except break-fix.

Code quality decisions carried forward from the Oct 13 lint discussion: newly introduced or modified code should fix its Sonar issues, while pre-existing issues are covered by repo-wide cleanup tickets that Andrew created.

---

## Key Notes

- **Karol Tluscik joined the team:** Yurii asked that Karol be added to the daily meeting. Andrew added Karol to the call and team board. Karol is ramping down other projects and expects to reach 100% allocation on this team in about one to two weeks.

- **Match migration plan:** Andrew wrote the plan covering migration order, routing, and component reusability. He asked that drawers (for example, the work order drawer and the Flightboard) and provider popups be built as reusable packages from the start, because each will have a second consumer soon (the ADR says to wait for a second consumer, but Andrew judged that these already have one). Analytics syncing (Amplitude, Datadog, RUM) also needs to be planned because the Match service has its own instrumentation. The plan still needs final approval from Chuck and the team that owns Match.

- **Priority order for migration work:** Bugs first, Project Flightboard second, Match migration third. Lower-priority migration tickets will be picked up when capacity allows.

- **Project page testing:** Templates are the last project piece. Andrew proposed handing testing to Laura and Todd, with Lindsay added at Chuck's suggestion for break-fix (OBS-545) because of her domain expertise. Yurii asked whether admin-only smoke testing was enough. Andrew said yes, except break-fix, which needs multiple roles (admin, dispatcher, manager).

- **Sonar policy:** Andriy asked whether to fix Sonar issues in the templates PR. Chuck and Andrew said to fix issues in code the PR added or changed, and to defer only issues that are covered by a repo-wide cleanup ticket and do not overlap the PR's own code.

- **Movie publishing issue (likely MUI publishing):** Andrew asked Andriy to take the publishing ticket. Recent merges to main are publishing V9 builds on the 8.22 branch (for example, 8.22 versions instead of 9.x), so consumers on 9.x do not pick up updates. This affects logo updates. Related to OBS-574 (open).

---

## Action Items

- **Chuck Haines:** Review the Match migration planning document for accuracy and completeness. Chuck said he already reviewed it with only minor items; Andrew's next-steps list still assigns a final pass.
- **Ivan Sakhman:** Implement the PSS column (Flightboard backlog item, OBS-578, which depends on backend OBS-583).
- **Miroslav Isikiyski:** Pick up core-ui tickets from the refined to-do list. Backend PR for the current work is in review.
- **Andriy Kurenda:** Fix the MUI publishing version issue (V9 versions publishing on the 8.22 branch). Also resolve Sonar issues in the templates PR and request review (the templates PR is ready and needs merge).
- **Yurii Dmytriiev:** Remove the Exos dependency from the project (as transcribed; confirm the package name). Remove Axios from peer dependencies. Pick up core-ui Match tickets. Add a small ticket for stricter layer ESLint checks.
- **Andrew Kandels (implied):** Create the calendar event for the draft-PR merge (Nov 7 per OBS-576, to confirm). Post the project page testing instructions (see the Obsidian daily notes from today). Hand the project page to Laura, Todd, and Lindsay for testing.
- **Laura Quist, Todd Folie, Lindsay (implied):** Test templates, manage project, and break-fix (OBS-545). Lindsay tests break-fix across roles.
