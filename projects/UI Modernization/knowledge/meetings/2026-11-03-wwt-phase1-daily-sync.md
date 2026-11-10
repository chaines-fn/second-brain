# WWT Phase 1 Daily Sync, 2026-11-03

**Date & Time:** Tuesday, November 3, 2026, 8:45 CST (~15 minutes). Calendar title: "Obsidian Daily Sync + Stakeholders."

**Attendees:**
- Andrew Kandels, Engineering lead, on-call this afternoon
- Chuck Haines, Sr. Principal Software Engineer
- Todd Folie, QA / WWT-facing testing
- Laura Quist (Douglas), Sr. Customer Success Executive
- Yurii Dmytriiev, Contractor, Frontend Engineer
- Andriy Kurenda, Contractor, Frontend Engineer
- Ivan Sakhman, Contractor, Frontend Engineer
- Karol Tluscik, Joining full-time this quarter
- Maria Saxton, Linear project lead (struck through on the invite; out today)

**Original Transcript:** [Google Doc notes](https://docs.google.com/document/d/1AgwXCPqlhS7v_J0wD_uEp2OAxmGAWItoMCpdZn6WGD4/edit) (Gemini notes only; no separate transcript was reviewed)

---

## Overview

The daily focused on PR review and release timing for the new Flightboard. One filter fix was approved and merged, and the mobile date ticket (OBS-673) is waiting on Todd's review. Andrew said he will merge the flightboard redirect fix this afternoon, after peak traffic, so the team can watch for issues while he is on call. That fix stops a white-screen flash when buyers pick a menu item. The team also agreed to close merged tickets on the board to keep it clean.

The main new issue is OBS-690, a Linear item filed yesterday and marked Urgent. A saved filter loads with the wrong name and format, and the Created Date is dropped. Todd will investigate, and Andrew committed to working on it after reviewing the support detail. Laura's auto-dispatch regression from yesterday was resolved after the fix passed E2E tests, and the team is waiting on final customer confirmation.

On rollout, the customer experience teams have received the project Flightboard tools, with training and meetings scheduled. Some users complained about drag and drop being inefficient. Todd said zooming out and applying changes across columns is faster for them. Andrew noted a vertical drag option could help on smaller screens. Both agreed much of the pushback is typical resistance to change.

---

## Key Notes

- **Redirect fix timing (decision):** Andrew will merge the flightboard redirect PR this afternoon, once peak traffic has passed and he is on call to catch any problems. The fix prevents a white-screen flash on menu clicks. A prior fix covered the homepage but not menu navigation.

- **Board hygiene:** Merged tickets should be closed on the board once they land. Karol and Andrew confirmed some backend tickets were merged and can be closed.

- **OBS-690 (Urgent, Todo):** Saved filters load with the wrong name and format, and the Created Date column is omitted. Todd is investigating and has the support video. Andrew said he would work on it after reviewing the support detail. This is the same symptom family as the saved-filter namespace and column persistence issues tracked earlier, but the link is not confirmed.

- **Auto-dispatch regression (Laura):** A tier-loading regression from the 11-02 contract ticket change (OBS-651) was fixed in PR #720. E2E tests passed after the fix. The team is waiting for final customer confirmation.

- **OBS-673 (mobile date picker):** Waiting on Todd's review of the mobile Safari date ticket. Ivan is working on it in parallel with the endpoint work.

- **Rollout feedback on project Flightboard:** Customer experience teams are receiving project tools with meetings and training. Some users find drag and drop inefficient. Todd said zooming out and bulk-applying changes across columns works for them. Andrew suggested a vertical drag option for smaller screens. Both agreed much of the feedback is resistance to change, not a product defect.

- **Refinement canceled:** The Obsidian refinement session was canceled today. Maria is out, and the team has enough ticket scope to proceed.

---

## Action Items

- **Todd Folie:** Review the mobile date ticket OBS-673 and send feedback.
- **Andrew Kandels:** Merge the flightboard redirect PR this afternoon after peak traffic passes.
- **Todd Folie:** Investigate the triage ticket OBS-690 (incorrect created date format and saved filter loading).
- **Andrew Kandels (implied):** Work on OBS-690 after reviewing the support detail.
- **The group:** Close merged tickets on the board once they land.
