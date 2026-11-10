# WWT Phase 1 Daily Sync - 2026-10-09

**Date & Time:** Friday, October 9, 2026, 8:45-9:00 CDT (Gemini notes generated 08:44 CDT)

**Attendees (per Gemini invite list):**
- Maria Saxton - WWT Phase 1 project lead, meeting organizer
- Chuck Haines - Sr. Principal Software Engineer
- Andrew Kandels - Engineering lead (joined mid-meeting)
- Laura Quist (Douglas) - Sr. Customer Success Executive, WWT voice (absent, time-sensitive conflict)
- Todd Folie - WWT-facing QA/customer contact
- Yurii Dmytriiev - Contractor, Frontend Engineer
- Ivan Sakhman - Contractor, Frontend Engineer (absent)
- Andriy Kurenda - Contractor, Frontend Engineer
- Fayshal Uddin - Engineer, reviews/releases core-ui changes

**Source:** [Gemini notes](https://docs.google.com/document/d/1x0kOTOGmG0HPITNZThbgUmMjAJL5x2OCVRICfHRRm-4/edit)

Note: summary is based on the Gemini-generated notes. Gemini flagged that its notes should be checked against the source before relying on them externally. "Sarah" in the next steps is unidentified in the notes and is likely a transcription error.

---

## Overview

Short standup focused on accessibility and mobile fixes for Flightboard, logo cache work, and the next steps for the final redirect release. No new bug reports were raised in the meeting. Todd and Maria reported no bug reports or negative feedback across three Slack channels since the full rollout.

## Key Notes

- **Flightboard accessibility:** Yurii merged accessibility PRs covering the Flightboard and project details pages. Tracked as OBS-570 (Mobile accessibility, Urgent), now Done per Linear as of 2026-10-09.
- **Mobile UI:** Yurii implemented a side-scroll pattern for tabs and filters to fix stacking. Todd marked the mobile change as done, and the group recorded mobile completion for the Flightboard filters and tabs as aligned.
- **Count delay:** A known delay in the message/count feature is tied to eventing. Fixing it long term needs eventing work, so there is nothing to test right now. Andrew owes confirmation of the timing delay once bandwidth allows.
- **Logo cache bumps:** Andriy finished cache bumps across all 10 repositories (OBS-575, Done). Reverse PRs that remove the cache bumps are drafted and will stay as drafts until November 7 (tracked as OBS-576, In Progress).
- **Project page migration demo:** Todd will demo the project page data grid changes to Mari (WWT stakeholder) on Monday morning, 2026-10-12. Testing scope for the project page is lighter than Flightboard because the data grid was already validated for WWT.
- **Redirect and experiment termination:** Andrew is preparing the final redirect PR. Merging it terminates the active experiment and removes the ability to revert users to the old Flightboard. Maria proposed a Tuesday or Wednesday release the week of 2026-10-12, after the Monday milestone. The group still needs to confirm the merge date early next week. Related tickets: OBS-572 (Buyer redirects and /flightboard URL, Urgent, Todo) and OBS-573 (Previous to new FB settings conversion, Urgent, Todo).
- **Pending issue details:** Maria is waiting on more information from "Sarah" about a recently reported issue. There is not enough data to work from yet.

## Decisions

- Mobile changes for Flightboard filters and tabs are marked done.

## Action Items

- **Todd Folie:** Verify mobile stacking and scroll patterns for filters and tabs.
- **Andrew Kandels:** Confirm the timing delay on the count feature. Prepare the final redirect PR.
- **Andriy Kurenda:** Finalize reverse PRs after November 7 and keep them as drafts until then.
- **Todd Folie:** Demo project page data grid changes to Mari on Monday, 2026-10-12.
- **The group:** Decide the merge date for the redirect PR and experiment termination. Confirm early next week.
- **Sarah (unidentified):** Provide details on the recently reported issue.
