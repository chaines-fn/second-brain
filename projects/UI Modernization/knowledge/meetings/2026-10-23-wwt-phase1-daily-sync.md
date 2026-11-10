# WWT Phase 1 Daily Sync (calendar: "Obsidian Daily Sync + Stakeholders"), 2026-10-23

**Date & Time:** Friday, October 23, 2026, 8:45 CDT (15-minute slot, 8:45 to 9:00). Gemini notes title shows 08:44 CDT.

**Attendees (per calendar invite and Gemini notes):**
- Maria Saxton, Linear project lead for WWT Phase 1 (meeting organizer)
- Andrew Kandels, engineering lead
- Todd Folie, QA and WWT-facing testing
- Chuck Haines, Sr. Principal Software Engineer
- Laura Quist (Douglas), Sr. Customer Success Executive (on invite)
- Yurii Dmytriiev, Ivan Sakhman, Andriy Kurenda, Karol Tluscik (engineering, on invite)

**Original Notes:** [Gemini notes](https://docs.google.com/document/d/1f0fAYOzNGqz7cSWohuIv0XdA5V6awCyaGlla08M4RQI/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

This daily covered backlog cleanup and readiness of the new project page for testing. Andrew confirmed the project page ticket is closed and that the workflows on the new project page should work end to end, so the feature was handed off to the testing team. Bugs found during testing go into the "Testing and bug fixes" milestone. The team also reviewed a sorting persistence bug that is hard to reproduce, and triaged two location-name and mobile tickets.

## Key Notes

- **Project page testing handoff:** Testing covers the net-new components on both the project page and the details page: header, templates, deliverables, planner, edit, and upload. Lindsay and the break/fix crew started on the manage project tab. Todd began testing the previous day and found minor bugs: filtering issues and pills not updating. Andrew's position is that since everything is merged, any malfunction is a bug. Maria asked the team to log bugs in the Testing and bug fixes milestone so they get prioritized.
- **Sorting persistence bug:** Yurii could not reproduce it across several columns, tabs, and page reloads. Andrew said an earlier fix covered the original bug, but a rare timing race condition likely still causes it occasionally, and a loader may be needed. Andrew and Todd concluded it is not tied to one column or tab. Yurii continues the investigation. This appears related to OBS-654 (Done tab due date sort does not persist, Urgent, In Progress under Yurii), but the link is not confirmed in the notes.
- **Rollout timeline:** Maria will contact "Mario" (transcription; most likely Mari Aune in GTM) next week to get definitive project rollout timelines.
- **Location name search ticket (OBS-640, Triage in Linear):** Reporter (Zach) says the location-name search dropdown disappeared after a recent update. Andrew checked the old Flightboard via manage clients and found the filters look identical to the new ones, so the exact field the user wants is unclear. Maria will send a screenshot of the old Flightboard to the reporter to ask which field they mean before the ticket goes to active work. Andrew will also update the ticket with the saved location name.
- **Mobile schedule-save ticket (OBS-642):** The customer who reported that a buyer could not save schedule changes from mobile confirmed it works now. The team agreed to close it rather than treat it as a blocker, with a note that it can be reopened if it recurs. Linear shows OBS-642 as Canceled.

## Action Items

- **Todd Folie:** Retest the project page now that updates are merged.
- **Maria Saxton:** Contact Mari (transcribed as "Mario") next week for definitive rollout timelines.
- **Yurii Dmytriiev:** Continue the sorting persistence investigation and ship a fix or a loader.
- **Maria Saxton:** Send the old Flightboard screenshot to the location-name reporter and ask which field they need.
- **Andrew Kandels:** Update the saved location ticket with the reporter's name.

## Linear activity seen during this run (not discussed in the meeting)

- OBS-655 "FN e2e tests update for new flightboard" (Andrew, Urgent) moved to In Progress today.
- OBS-647 "Error after URL entry on page load" closed Done.
- OBS-654 "Done tab due date sort does not persist" (Yurii, Urgent) In Progress.
