# UI Modernization Sync - 2026-11-02

**Date & Time:** Monday, November 2, 2026, 7:45 CST (~15 minutes)

**Attendees:**
- Andrew Kandels - Engineering lead (organizer)
- Chuck Haines - Sr. Principal Software Engineer
- Yurii Dmytriiev - Contractor, Frontend Engineer
- Ivan Sakhman - Contractor, Frontend Engineer
- Karol Tluscik - Joining full-time this quarter (provider-match work)
- Andriy Kurenda - Contractor, Frontend Engineer
- Miroslav Isikiyski - Backend Engineer
- Fayshal Uddin - Engineer (reviews/releases core-ui)
- Maria Saxton - Linear project lead (tentative on invite)
- Laura Quist - Optional invitee

**Source:** Google Meet Gemini notes (https://docs.google.com/document/d/1-LGRjL8RB1fPijsD3kOBrjGMbZEgg4apaoHe6s1wo5g/edit). The full verbatim transcript was not retrievable in this run (the document exceeded the read limit), so this summary relies on the Gemini summary and next steps. Attendance in the Gemini summary appears limited to Karol and Ivan speaking, so treat it as partial.

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1-LGRjL8RB1fPijsD3kOBrjGMbZEgg4apaoHe6s1wo5g/edit)

---

## Overview

The sync was short and reported no blockers. Karol finished the pull request that adds the models and calendar for provider match on the QA environment. Her next task is to move the provider card components into the following pull request. Ivan is still working the mobile schedule date picker bug reported on Friday (Safari on iOS shows an incorrect past-date error, and a 4-digit year is truncated to 2 characters). He will move to backend endpoints once that bug is fixed.

Per Slack on 10-30 and 11-02, Yurii is reviewing and opening follow-on PRs (including #712), expects to close the rollback PR #704 on Monday after its React compiler migration to Rust shipped with no errors, and is working on Sonar issues and healthcheck speedup.

## Key Notes

- **Provider match progress:** The models and calendar PR for provider match is complete and on the QA environment. Provider card components are next. Related tickets from Slack and Linear: OBS-603 (availability view, In Review, Karol), OBS-601 (provider card and PSS popovers, In Progress, Karol), OBS-606 (provider filter bar, In Progress, Andriy), OBS-602 (provider drawer, Todo, Ivan).
- **Mobile schedule date picker (Urgent):** Ivan is on the mobile date picker bug (Safari/iOS, past-date error, 4-digit year input). Status is in progress. The overview tracks this as OBS-673.
- **Zero blockers reported:** Karol and Ivan both said nothing is blocking them.

## Action Items

- **Ivan Sakhman:** Finish the mobile schedule date picker fix, then start the provider endpoint work.
- **Karol Tluscik:** Move provider card components into the next pull request.
- **Yurii Dmytriiev (implied):** Close the rollback PR #704 Monday, continue the Sonar cleanup and healthcheck speedup, and work through the next provider match migration tickets.
