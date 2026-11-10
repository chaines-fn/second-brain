# WWT Phase 1 Daily Sync (calendar: "Obsidian Daily Sync + Stakeholders"), 2026-10-20

**Date & Time:** Tuesday, October 20, 2026, 8:45 CDT (15-minute slot, 8:45 to 9:00)

**Attendees (per calendar invite and Gemini notes):**
- Maria Saxton, Linear project lead for WWT Phase 1 (meeting organizer; OOO 10-15 to 10-19, returned 10-20)
- Andrew Kandels, engineering lead
- Todd Folie, QA and WWT-facing testing
- Chuck Haines, Sr. Principal Software Engineer
- Laura Quist (Douglas), Sr. Customer Success Executive (on invite; decline or acceptance varies by event)
- Ivan Sakhman, contractor, Frontend Engineer (on invite)
- Karol Tluscik, contractor (on invite)

**Original Notes:** [Gemini notes](https://docs.google.com/document/d/1ScbxgHCYaBKrluvD8JPpVCpJNApjhNcicy2lJy89wUA/edit?usp=meet_tnfm_calendar)

---

## Overview

This daily covered project page rollout timing, a batch of triage items, and the refinement schedule. The project page work includes a new URL that will take over the original production URL on deployment, and a fix in progress to remove the redirect delay on the work Flightboard link. The breakfix team had asked for timelines and experience details for the project Flightboard rollout. The team agreed to deploy the new projects page directly to production without a staged rollout, and the notes target a launch before Thanksgiving.

The triage review closed the company settings reset ticket, since it could not be reproduced and no further reports had come in. Andrew verified the custom-field work-order selection behavior works. The auto-dispatch limit was the most significant finding: a global limit of 50 items was being applied to auto-dispatch actions, and Todd raised it from a ParkPlace user who tried to dispatch 600 work orders. Andrew confirmed the limit on WWT and committed to scoping it to the intended action. Refinement was moved to the next day to focus on Match tickets.

## Key Notes

- **Project page rollout (decision):** The team agreed to deploy the new projects page directly to production without a staged rollout and take over the original URL. The notes say the launch targets before Thanksgiving. Note that the next day's notes describe an early-November one-shot rollout. The two notes differ on the target window, and the 10-21 notes say the date depends on testing. Treat the exact date as unknown until confirmed.

- **Work Flightboard redirect delay:** A fix is in progress to eliminate the redirect delay on the work Flightboard link. Andrew owns the project redirect fix. The notes do not link it to a ticket; OBS-643 (new project listing links to old page) and OBS-644 (work Flightboard still redirecting) are the likely related tickets.

- **Auto-dispatch limit (bug):** An incorrect global limit of 50 items applies to auto-dispatch. Andrew tested on WWT and confirmed the limit. The fix is to apply the constraint only to the intended scope. Linear tracks the limit as OBS-646 (Urgent, unassigned). Slack on 10-20 shows Cory Muir asking whether this limit came from a product decision or the flightboard work, and Andrew and Maria were tagged in triage.

- **Triage closures and checks:** The company settings reset ticket is closed (not reproduced; no further reports). Custom-field work-order selection works in testing (Andrew verified with a WWT custom field). The mobile schedule-edit issue cannot be reproduced at standard mobile viewports; Maria will ask the reporter for device and browser details, and the team will close it if no details come in. The location-name search regression reported by Zach Pock in Customer Success is being verified. The new Flightboard In-Flight Today and Tomorrow column is missing (later tracked as OBS-645).

- **Mobile landscape ticket:** Andrew will discuss a landscape mobile ticket with Sarah.

- **Refinement:** Maria moved the refinement session to the next day, focused on technical Match tickets that do not need stakeholder input, so Todd and Laura did not need to attend.

## Action Items

- **Andrew Kandels:** Update Sarah on the landscape and mobile ticket drawer issues.
- **Andrew Kandels:** Finish the remaining project testing ticket so the project page can be handed off.
- **Andrew Kandels:** Fix the project page URL redirect.
- **Andrew Kandels:** Investigate the 50-item auto-dispatch limit and restrict it to the intended action.
- **Andrew Kandels:** Verify whether the location-name filter is missing; reassign to backlog if so.
- **Maria Saxton:** Ask the mobile save-button reporter for device and browser details.
