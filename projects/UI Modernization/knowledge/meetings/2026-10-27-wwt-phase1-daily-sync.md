# WWT Phase 1 Daily Sync - 2026-10-27

**Date & Time:** Tuesday, October 27, 2026, 8:45 CDT (calendar: "Obsidian Daily Sync + Stakeholders"; Gemini notes only, no transcript text)

**Attendees (invited per Gemini; presence not confirmed from notes):**
- Maria Saxton - Linear project lead, WWT Phase 1 (organizer)
- Andrew Kandels - Engineering lead
- Todd Folie - Production bug reporting and WWT-facing testing
- Chuck Haines - Sr. Principal Software Engineer
- Laura Quist - Sr. Customer Success Executive, primary WWT voice (dropped at 10am per calendar note)
- Yurii Dmytriiev - Frontend Engineer
- Andriy Kurenda - Frontend Engineer
- Ivan Sakhman - Frontend Engineer
- Miroslav Isikiyski - Backend Engineer
- Fayshal Uddin - Attendee (role not on file)
- Karol Tluscik - Joined this quarter

**Original Notes:** [Gemini notes (Google Doc)](https://docs.google.com/document/d/1yrOhpWDt9-abYln3swBQzS_a7fAxLkj-9yGuNM1JxPQ/edit?usp=drivesdk&ouid=104527139387599317887)

Note: no calendar event named "WWT Phase 1 Daily Sync" appeared today. The 8:45 "Obsidian Daily Sync + Stakeholders" slot has been the WWT daily in prior notes, so it is treated as that meeting here.

---

## Overview

The project page bug fixes are nearly complete. Yurii Dmytriiev reported finishing the fixes, with smoke tests and small UI issues left. Todd Folie recommended merging the current fixes before running another retest round, and the group agreed. Todd will review the merged changes, and Yurii will post in Slack when the build is ready for retesting. The team also confirmed that the elastic list load work for projects past 10,000 items is on Yurii's roadmap.

Two new issues came up. External partner users (such as WWT, or service partners acting as other buyers) do not see the required banner on core UI pages. This feature was never built on core UI. Andrew Kandels said the fix is a frontend-only replication of legacy behavior with no backend security dependency, but the owning team is not yet identified (Granite, Diamond, or Quartz were mentioned). Separately, a role-switcher ticket shows the acting company, not the user's own company with their default role, in the company and role list. Andrew explained the new system treats the account as the acting company, while the legacy navigation showed the true origin state, so navigation needs changes to match legacy behavior.

The PSS ticket on the Flightboard also needs lazy-loading and route-support verification. Team ownership (Quartz vs. Diamond, managed by Resza) is unclear. The group cancelled today's refinement session because the team is not prepared and has enough existing work, deferring to next week or ad hoc.

---

## Key Notes

- **Project page retest sequencing:** Yurii asked whether to keep retesting while fixing bugs or merge first. Consensus was to merge completed fixes, then retest. This matches the UI sync decision the same morning.

- **External partner banner missing on core UI:** Partner and acting-user banners were never supported on core UI. Andrew wants a developer synced with the owning team to replicate it. An external partner account exists on QA1 for testing. This is an open ownership item, not yet assigned.

- **Role switcher navigation:** Behavior is correct for the acting-company model, but users switching roles expect legacy navigation that shows their true origin company and default role. Requires navigation changes, tracked as a ticket.

- **PSS on Flightboard:** Lazy loading and supported routes need verification with Ivan Sakhman. Andrew will ping Resza to confirm which team (Quartz or Diamond) owns the integration.

- **Elastic list past 10k:** Confirmed on Yurii's roadmap after his current ticket. This ties to the scale-risk items tracked earlier (OBS-434/442 history).

- **Refinement cancelled:** Deferred to next week or ad hoc. No decision needed from Chuck.

---

## Action Items

- **Todd Folie:** Review project page bug-fix changes after they merge, ahead of retesting.
- **Yurii Dmytriiev:** Post a Slack update when project page fixes are ready for retesting (explicit).
- **Andrew Kandels:** Identify the owning team for the external partner banner and set up a sync with its developers (explicit).
- **Andrew Kandels:** Identify which team (Quartz or Diamond) owns the PSS Flightboard ticket, by pinging Resza (explicit).
- **Andrew Kandels:** Message the team manager about PSS ticket support and lazy loading, and sync with Ivan Sakhman (explicit).
- **Chuck Haines:** No explicit action items assigned in this meeting.
