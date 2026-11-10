# Obsidian Daily Sync + Stakeholders (WWT Phase 1 Daily Sync) - 2026-10-30

**Date & Time:** Friday, October 30, 2026, 8:45 CDT (~15 minutes)

**Attendees:**
- Maria Saxton - Linear project lead (organizer)
- Chuck Haines - Sr. Principal Software Engineer
- Andrew Kandels - Engineering lead
- Todd Folie - QA / WWT testing
- Laura Quist - Sr. Customer Success Executive (WWT)
- Andriy Kurenda - Contractor, Frontend Engineer
- Karol Tluscik - Contractor, Frontend Engineer
- Yurii Dmytriiev, Ivan Sakhman - Invited (no attendance noted)

**Source:** Google Meet Gemini notes (https://docs.google.com/document/d/1zft5_OjrMfmBnUVtekAZ2XtMnPYFMS4sw-2QOqex5oU/edit). Full transcript not retrieved this run; summary relies on Gemini notes.

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1zft5_OjrMfmBnUVtekAZ2XtMnPYFMS4sw-2QOqex5oU/edit)

---

## Overview

Testing review, merge rules, and bug validation were the main topics. Todd finished the assigned stakeholder review tasks. The team removed the "needs approval" filter from the filter options because its saving functionality was failing. The "in progress" filter exists on the API side but is not rendered on the old UI, and Yurii is looking into why.

The team also agreed a temporary CI/CD rule: pull requests can currently merge while health checks or tests are still running, and that will be blocked in GitHub Actions until the move to Harness, which will lock merges during active health checks. Chuck also ran manage-section issues through the software factory, and they need more validation. A mobile date picker bug on the flightboard edit schedule (Safari on iOS) was documented.

## Key Notes

- **Decision, temporary merge blocking:** PR merges will be blocked in GitHub Actions while health checks or tests are running, until the Harness transition. Andrew owns the change. Aligned by the group.
- **Needs approval filter removed:** Pulled from the filter options because saving failed. Yurii to investigate and report in Slack.
- **In progress filter not on old UI:** The filter exists on the API side but is not rendered on the legacy UI. Same investigation as above.
- **Mobile date picker bug:** Accessing the flightboard edit schedule via Safari on iOS shows an erroneous past-date error. Entering a 4-digit year shows only the first 2 characters. Logged as a bug ticket (later tracked as OBS-673).
- **Manage section issues:** Chuck processed these through the software factory. They need additional validation and review.

## Action Items

- **Yurii Dmytriiev:** Find out why the needs-approval filter is not rendered on the old UI and post findings in Slack.
- **Ivan Sakhman:** Implement backend code changes based on recent backend updates.
- **Andrew Kandels:** Configure GitHub Actions to block PR merges while health checks or tests are running.
- **Andrew Kandels:** Read and address comments on the reported issues.
