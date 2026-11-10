# WWT Phase 1 Daily Sync (calendar: "Obsidian Daily Sync + Stakeholders"), 2026-10-21

**Date & Time:** Wednesday, October 21, 2026, 8:45 CDT (15-minute slot, 8:45 to 9:00)

**Attendees (per calendar invite and Gemini notes):**
- Maria Saxton, Linear project lead for WWT Phase 1 (meeting organizer; back from OOO 10-20)
- Andrew Kandels, engineering lead
- Todd Folie, QA and WWT-facing testing
- Chuck Haines, Sr. Principal Software Engineer
- Laura Quist (Douglas), Sr. Customer Success Executive (on invite; decline or acceptance varies by event)
- Ivan Sakhman, contractor, Frontend Engineer (assigned follow-ups)
- Miroslav Isikiyski, backend engineer (invited; "Mero" in the transcript, not confirmed present)
- Karol Tluscik, contractor (on invite)

**Original Notes:** [Gemini notes](https://docs.google.com/document/d/1bS0F1xGPDm6k2nLsC7qcLT3rUU06-8ndgrihlMHls-8/edit?usp=meet_tnfm_calendar)

---

## Overview

This daily covered three topics: PSS (Provider Success Score) column performance on the project Flightboard, the project page rollout timeline, and the route ticket that gates project page testing. On PSS, the team recognized that the current approach loads 100 rows from a separate database and combines the data in code, which will not hold at 10,000 rows, and that sorting and filtering are not supported for the PSS column. The team agreed to extend the lazy-loading pattern already used elsewhere to the PSS column for the whiteboard ticket, and Ivan will report the backend needs.

On the project page, the discussion centered on timing. Maria raised concerns about a November release because the projects page carries 0% of traffic with fewer than 20 buyers using it. Todd noted that final store project work wraps around November 15 ahead of Black Friday. Maria proposed a one-shot 0-to-100 rollout in early November, pending stable testing, and argued that a staged rollout risks missing low-traffic volumes during experiment hydration. The team has not committed: the notes list the timeline as needing further discussion, with the final call contingent on testing. If the release slips past early November, it moves to after Thanksgiving.

## Key Notes

- **PSS performance and database architecture:** PSS lives in a separate database. The current approach loads 100 rows and combines them with work-order data in code. That fails at 10,000 rows, and PSS sorting and filtering are unsupported. The team agreed to extend the lazy-loading pattern to the PSS column as part of the whiteboard ticket. This is a performance and scope decision, not a final design: it is the same direction as the "lazy loading" option in OBS-638 and the project Flightboard PSS approach that was still open on 10-19.

- **Project page rollout timing:** Maria proposed a one-shot 0-to-100 cutover in early November, contingent on stable testing. Her argument: staged rollouts for low-traffic features may never reach enough users to validate during experiment hydration. Todd flagged the November 15 store-project wrap-up before Black Friday. Concerns about November adoption were raised. Status: needs further discussion; no final commitment.

- **Route ticket 643 (OBS-643):** Route ticket 643 must be completed before project page testing can begin. Andrew reported Miroslav has the ticket but has not started it, and Andrew could not pick it up the previous day. Maria set it to high priority so it is pulled next. On 10-21 at 07:12 CDT Miroslav posted in Slack that he finished OBS-644 (a related menu/redirect fix, PR sent to Andrew) and is continuing on OBS-643.

- **Lazy-loading follow-up for Ivan:** Ivan will verify which columns the previous lazy-load implementation covers, apply the pattern to the PSS column, and communicate the backend support the PSS column needs.

## Action Items

- **Andrew Kandels:** Ping Sarah to pass the two stakeholder-review items to the customer. The notes name "Sarah" without a last name; she is listed in people.md as Sarah Welter (unconfirmed role).
- **Andrew Kandels:** Lead the refinement session focused on Match tickets.
- **Ivan Sakhman:** Review the previous lazy-load implementation to confirm which columns it used.
- **Ivan Sakhman:** Apply the lazy-load pattern to the PSS column.
- **Ivan Sakhman:** Identify and communicate the backend support needed for the PSS column.
- **Miroslav Isikiyski (implied):** Complete route ticket 643 (OBS-643) so project page testing can start.
- **Maria Saxton (implied):** Decide the November rollout date after testing results are in.
