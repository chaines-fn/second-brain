# Obsidian Daily Sync + Stakeholders - 2026-10-19

**Date & Time:** Monday, October 19, 2026, 8:45 CDT (~30 minutes scheduled)

**Attendees (per calendar invite; Gemini notes list the topics, not who spoke):**
- Maria Saxton: Organizer, Linear project lead for WWT Phase 1 (out of office through 10-19, so likely absent)
- Andrew Kandels: Engineering lead
- Todd Folie: QA and testing with WWT
- Laura Quist (Douglas): Sr. Customer Success Executive, primary WWT voice
- Andriy Kurenda: Contractor, Frontend Engineer
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Fayshal Uddin: Engineer, reviews and releases
- Karol Tluscik: Joined full-time this quarter
- Chuck Haines: Sr. Principal Software Engineer

**Original Transcript:** [Gemini notes](https://docs.google.com/document/d/1R1HYE2x6qK6iOJI6V2pGTWFOo2QNFDs2vOuRuyvla5U/edit?usp=meet_tnfm_calendar)

**Source note:** The Gemini summary covers four topics. Two long detail paragraphs (mobile layout and schedule-edit bugs, and PSS loading) were truncated in the extract, so those points come from the summary lines only.

---

## Overview

The sync covered the state of the project page routing work, mobile bugs on the new Flightboard, the custom-field search bug, and PSS performance. Andrew reported that the project one link-routing patch is nearly ready. Links on the new project list still navigate users back to old pages, and the fix needs to keep users on the new URL structure. Testers will verify that clicking links stays on the new experience, and testing can begin the following day.

On saved filters, the relative date filters feature merged the prior Friday. Todd tested it twice and confirmed it works for today and tomorrow. Laura asked about it after a buyer reached out. Andrew also resolved the mobile landscape-mode display issue reported by customers. The schedule-edit dialog on mobile still hides some buttons, and the root cause is under investigation.

The biggest open item is PSS (Provider Success Score) performance on the project Flightboard. Andrew said the secondary database queries for PSS create scale risk and block data filtering. The team did not decide on an approach. Options include baking PSS into the work-order index or lazy-loading the column on the front end. This is the same problem tracked as OBS-638. Amplitude event tracking on the project Flightboard has been live for about 20 days, so data should be accumulating.

---

## Key Notes

- **Project One link routing:** Routing patch is underway. The known problem is that links on the new project list do not keep users in the new experience. Testers must confirm that URLs keep the new structure so users do not land on old pages.

- **Relative date filters for saved filters:** Merged the prior Friday. Todd tested twice and verified behavior for today and tomorrow. This is the fix for OBS-488 ("This week" filters saving a fixed date).

- **Mobile layout bugs:** Andrew resolved the landscape-mode layout issue. The schedule-edit dialog on mobile hides buttons, and the root cause is still being investigated. This connects to OBS-634 and to the buyer report that the Save button is not visible on some devices. Andrew's theory is the production Salesforce chat icon overlapping the Save button, which is not present in test environments.

- **Custom field search interaction bug:** Event pass-through causes custom-field searches to trigger on previous work orders. This matches OBS-633 (searching by custom fields shows the sidedrawer for the prior view).

- **PSS performance (open decision):** Secondary database queries for PSS limit scale and block filtering. Options are to bake PSS into the work-order index or to lazy-load it on the front end. The team deferred the choice pending an internal discussion. Related tickets: OBS-638 (High) and OBS-637 (10,000-row result cap warning). The 10,000-row cap question still matters because a silent truncation would also affect PSS filtering.

- **Amplitude tracking and training:** Event tracking on the project Flightboard finished about 20 days ago. Laura is new to Amplitude and asked how to build reports. Andrew pointed to the existing naming ticket and suggested training from Lindsay. The group will identify an internal expert to run training on Amplitude report setup.

---

## Action Items

- **Andrew Kandels:** Patch project link navigation so users stay in the new experience. Testing starts the following day.
- **Andrew Kandels:** Find the root cause of the hidden buttons in the mobile schedule-edit dialog.
- **Andrew Kandels:** Develop a performant PSS loading approach, either baking data into the database or front-end lazy loading. (Owner from the Gemini notes.)
- **The group:** Identify an internal expert to run Amplitude report-setup training. (Owner not named in the notes.)
