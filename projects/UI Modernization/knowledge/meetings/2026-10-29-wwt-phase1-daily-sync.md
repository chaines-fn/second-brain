# WWT Phase 1 Daily Sync (Obsidian Daily Sync + Stakeholders) - 2026-10-29

**Date & Time:** Thursday, October 29, 2026, 8:44 CDT (scheduled 15 minutes, 8:45 to 9:00)

**Attendees:**
- Maria Saxton - Lead for WWT Phase 1 Fast Follows, organizer
- Todd Folie - QA/testing, WWT-facing
- Laura Quist - Sr. Customer Success Executive, primary WWT voice
- Andriy Kurenda - Contractor, Frontend Engineer
- Andrew Kandels - Engineering lead
- Ivan Sakhman - Contractor, Frontend Engineer
- Yurii Dmytriiev - Contractor, Frontend Engineer
- Karol Tluscik - Joined this quarter for extra bandwidth
- Chuck Haines - Sr. Principal Software Engineer

Gemini notes record Maria, Todd, Laura, and Andriy as the active speakers.

**Original Transcript:** [Notes by Gemini](https://docs.google.com/document/d/1DI5rxxQRtmwt4LOa7WQF70fP08Zabk1fQrw-ncu_xwU/edit?usp=drivesdk&ouid=104527139387599317887) (no separate transcript tab was linked)

---

## Overview

This was a short check-in focused on retesting status and one new bug. Todd had barely started the pending retesting and set aside a couple of hours today, so Maria deferred that item to a later review. Engineering had no new open items to report. Laura reported a new Urgent Flightboard bug, OBS-668 (in Linear, created 10-28 by Maria and assigned to Andriy). When a user selects a saved bookmark, adds a filter, and clicks save, the save defaults to the first bookmark in the list instead of the one already selected, so users have to reselect the bookmark to update it.

Andriy has a PR open for OBS-668 and plans to merge it soon. Once merged, he will mark it stakeholder ready so Laura and Todd can validate the fix. Maria will link the ticket to the Slack thread so it is tracked in both places, and she asked Todd to keep posting new issues in chat so she can open tickets for them.

---

## Key Notes

- **Retesting status:** Todd has not made much progress on the pending retesting and only has a couple of hours today. Maria deferred the item for later re-evaluation rather than pressing for an update.

- **OBS-668 (saved bookmark default):** Expected behavior is that the selected bookmark is the default target when saving, so the user can update it directly. Actual behavior is that the first bookmark in the list is the default. Linear shows Urgent, In Review, assigned to Andriy Kurenda, with a Flightboard label. Andriy confirmed there are no open questions on the bug and that the PR is ready to merge. Validation is planned by Laura and Todd after it is marked stakeholder ready.

- **Issue intake process:** Maria asked Todd to post future issues in the chat so she can create tickets from them, rather than leaving reports only in threads.

---

## Action Items

- **Maria Saxton:** Link OBS-668 to the relevant Slack message.
- **Andriy Kurenda:** Merge the OBS-668 PR and change the ticket status to stakeholder ready.
- **Laura Quist and Todd Folie:** Validate the OBS-668 fix once it is stakeholder ready.
- **Maria Saxton:** Create a ticket whenever Todd posts a new issue in chat (ongoing).
- **Todd Folie:** Complete pending retesting when time allows (implied, from the deferral; no date set).
