# Obsidian Daily Sync + Stakeholders (WWT Phase 1 Daily): 2026-11-16

**Date & Time:** Monday, November 16, 2026, 8:45 CST (~10 minutes). Calendar title: "Obsidian Daily Sync + Stakeholders."

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
- Tashfiqul Islam: Classic-page migration lead

**Original Transcript:** [Gemini notes and transcript](https://docs.google.com/document/d/1bWeTGF5r8HLEmrHSPNhsoRNYFVtQVkQhlGD3MKI1WW8/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

Board ticket review and deployment planning. Three stakeholder-review tickets are ready for Todd's validation: the CSV export column checkboxes, the relative date option on project flightboard (Karol's rework), and the start time quick filter (ticket 712). Yurii closed the main ticket for the end-to-end fixes and created a separate, smaller ticket for the last three E2E tests, because the main ticket was blocking the flightboard from E2E testing. Maria will close the main ticket once Todd finishes validation.

The CSV export checkbox ticket came back for retest because the checkboxes did not update on the board when clicked. Ivan had tested it during implementation. OBS-638 (PSS on flightboard efficiency) is in review and expected to go live shortly. The project page deployment is planned for tomorrow (Tuesday 11-17) in the afternoon, to be confirmed by Andrew and checked on by Maria. Flightboard UX tweaks were moved to the refinement session.

---

## Key Notes

- **CSV export checkbox (triage):** Two Salesforce cases are tied to it. Todd agreed to retest. Tashfiqul asked whether changes reach QA1; the answer was that changes usually go straight to production, and lower environments are used for larger changes.

- **Relative date option (Karol):** Needs Todd's retest before closing. Start time quick filter (ticket 712) is in the same validation batch.

- **E2E test split (Yurii):** The main ticket closed. A separate small ticket covers the remaining three E2E tests.

- **OBS-638 PSS efficiency (Andrew):** In review, going live shortly. Once Todd validates the three review items, those tickets can close.

- **Project page deployment (Andrew, Todd, Maria):** Planned for tomorrow afternoon. Andrew returns around noon and can start the deployment any time after that. Todd is in meetings until 5 PM, so afternoon timing works. Maria will set a reminder to check in with Andrew.

- **Classic-page refinement invite (Maria):** Todd and Laura are removed from the classic-page technical discussion series. Maria will update the invite.

- **Flightboard UX tweaks (Tashfiqul, Andrew):** Small UX adjustments made this week will be discussed at the upcoming refinement session, run by Andrew and Tashfiqul with Maria attending.

---

## Action Items

- **Todd Folie:** Test the stakeholder-review tickets (CSV export checkboxes, relative date option on project flightboard, start time quick filter 712) before they are closed.
- **Maria Saxton:** Update the classic-page refinement invite to remove Todd and Laura.
- **Maria Saxton:** Set a reminder to check in with Andrew on the afternoon project page deployment timing.
- **Andrew Kandels (implied):** Deploy the project page tomorrow afternoon, 11-17.
- **Maria Saxton (implied):** Close the main E2E ticket once Todd's validation is complete.
