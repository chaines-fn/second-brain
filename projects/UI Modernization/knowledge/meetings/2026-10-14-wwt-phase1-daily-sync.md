# WWT Phase 1 Daily Sync - 2026-10-14

**Date & Time:** Wednesday, October 14, 2026, 8:44 CDT (calendar title on this date: "Obsidian Daily Sync + Stakeholders")
**Source:** Google Meet Gemini notes and transcript, [notes doc](https://docs.google.com/document/d/1mtOt1odRmOG16In4xS_5eg9ZAmqHEbDnlvYp1HhWiow/edit) (shared by Maria Saxton)
**Attendees (invited):** Maria Saxton (organizer, WWT Phase 1 lead), Andrew Kandels (tech lead), Chuck Haines, Yurii Dmytriiev, Ivan Sakhman, Andriy Kurenda, Todd Folie (QA/testing), Laura Quist (CS, customer voice), Karol Tluscik (joining the team)
**Notes quality:** Gemini-generated. Only the Gemini summary and decisions were reviewed for this entry; the full transcript was not reviewed in detail. Review before relying on specifics.

**Relationship Context:** n/a (group meeting)

---

## Overview

The meeting covered virtualization on the main Flightboard, testing for the project page migration, and a new customer-reported search behavior issue. The team agreed to remove virtualization from the main Flightboard for now because pagination remains active there, and that is the likely cause of disappearing column headers that Spectrotel reported. Maria noted that a fix for disappearing headers (triggered by adding multiple filters) was merged, and Todd will try to reproduce it with Spectrotel account data.

Andrew confirmed the project page is ready for testing in all environments and will post connection instructions, including the `/new` route. Lindsay joined for break-fix testing and Todd and Laura will cover project listing and details. The team also agreed to use the modernization Slack channel for new bug reports so duplicates and commentary stay visible.

Andrew moved four backend technical tickets into to-do to unlock upcoming business-focused work. Maria said remaining backlog items, including Match tickets, will be refined next week after Andrew reviews the specification output.

---

## Key Notes

- **Virtualization removed from the main Flightboard (decision):** The team aligned to remove virtualization for now because pagination is still active on the main Flightboard. The notes do not link this decision to the disappearing-headers bug or the Edge versus Chrome issue, so that link is unconfirmed. Andrew moved the virtualization ticket to the correct board. Confirm the Edge and Chrome explanation with Andrew (still open from the Oct 12 notes).

- **Disappearing column headers (Spectrotel, OBS-584):** Maria flagged that adding multiple filters on the main Flightboard removes column headers. The original report came from the Spectrotel account through Molly Grailer. A fix was merged; Maria reviewed it. Todd will attempt to reproduce using the Spectrotel account data. Status: not yet confirmed as reproduced or fixed.

- **Project page migration testing:** Andrew confirmed the project page is ready to test in all environments. He will post the connection instructions and URLs, including the `/new` route. Testing can also use production with real customer data. Lindsay joined to evaluate the break-fix tab (OBS-545). Todd and Laura will test project listing and project details.

- **Custom field search behavior (new bug from Insight):** Laura reported that searching by custom field values (for example, WO number or job number) gives different results when Apply is clicked versus Enter is pressed. The result can open the sidedrawer of a previously viewed work order. Maria recommended submitting it in Slack rather than Linear so duplicate reports and commentary stay visible. Laura posted the ticket in #proj-ui-modernization with a Gong recording (09:02 CDT).

- **Backlog and technical tickets:** Andrew moved four technical backend tickets without business context into to-do to unlock future business work. Maria said the remaining backlog, including Match tickets, goes through refinement next week after Andrew reviews the spec output.

---

## Action Items

- **Andrew Kandels:** Post the project page connection instructions and URLs for testing, covering all environment access details.
- **Todd Folie:** Try to reproduce the disappearing column header issue on the main Flightboard using Spectrotel account data. Also test the project listing and details (implied).
- **Laura Quist:** Submitted the custom field search ticket in the modernization Slack channel with reproduction steps and video (done 09:02 CDT).
- **Engineering group (implied):** Triage the custom field search ticket once it is submitted and decide next steps based on the documentation Laura provided.
- **Maria Saxton (implied):** Refine the remaining backlog, including Match tickets, next week.
- **Lindsay (implied):** Test break-fix (OBS-545) across roles.
