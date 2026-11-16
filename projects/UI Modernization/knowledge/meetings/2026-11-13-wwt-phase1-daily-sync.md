# Obsidian Daily Sync + Stakeholders (WWT Phase 1 Daily): 2026-11-13

**Date & Time:** Friday, November 13, 2026, 8:43 CST (~10 minutes). Calendar title: "Obsidian Daily Sync + Stakeholders."

**Attendees:**
- Maria Saxton: Linear project lead, WWT Phase 1 Fast Follows (organizer)
- Andrew Kandels: Engineering lead
- Chuck Haines: Sr. Principal Software Engineer
- Todd Folie: QA / WWT testing
- Laura Quist: Sr. Customer Success Executive (WWT voice)
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Karol Tluscik: Frontend (joining full-time)
- Tashfiqul Islam: Joined to lead classic-page analysis and migration

**Original Transcript:** [Gemini notes and transcript](https://docs.google.com/document/d/1U_qMkJcgPWDcC8fAFe1gLRLOabVr1N55hHM10h2_INM/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

Maria led a review of open stakeholder items. Karol had reworked a pending triage item and it was close to ready for Todd's testing. Maria also introduced Tashfiqul Islam, who will help with classic-page migration analysis and serve as a point of contact on the classic-page work.

The main discussion was a triage ticket from a Pivotal user who cannot highlight and copy values in custom fields on the flightboard, because clicking a cell opens inline editing. Laura demonstrated the issue. The group agreed that the platform should support both inline editing and text selection for copying across editable column types. The ticket was added to the to-do list for anyone with bandwidth. A second, low-priority CSV export bug (added fields export but do not show as board columns) was mentioned and not discussed further.

---

## Key Notes

- **Triage item rework (Karol):** Reworked before the meeting. Todd will review it later today or on Monday to confirm it is ready for testing.

- **Custom field copy issue (Pivotal, triage):** Clicking a custom field such as Site ID opens an edit box, but users cannot select or copy the value. Andrew proposed a mouse-up trigger so selection works while inline editing still opens. Yurii and Laura questioned whether location and work-order ID columns behave the same way; the group agreed the fix should cover all editable columns. Decision: update all editable column fields to support both highlight-to-copy and click-to-edit.

- **Tashfiqul Islam (new):** Introduced as the lead on classic-page migration analysis and conversion to Core UI, bringing background and history in the space.

- **Deployment environment question (Tashfiqul, Maria, Andrew):** Changes typically go straight to production. Lower environments are used optionally for larger changes.

---

## Action Items

- **Todd Folie:** Review the reworked triage item today or Monday to confirm it is ready for testing.
- **Unassigned:** Fix custom field selection so users can highlight and copy cell values while keeping inline editing on flightboard columns. Ticket is on the to-do list.
