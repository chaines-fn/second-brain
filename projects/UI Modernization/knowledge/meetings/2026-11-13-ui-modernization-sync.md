# UI Modernization Sync: 2026-11-13

**Date & Time:** Friday, November 13, 2026, 7:45 CST (~10 minutes)

**Attendees:**
- Andrew Kandels: Engineering lead, organizer
- Chuck Haines: Sr. Principal Software Engineer
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Miroslav Isikiyski: Backend Engineer
- Karol Tluscik: Frontend (joining full-time)
- Maria Saxton: Linear project lead, WWT Fast Follows (on invite)
- Laura Quist: Sr. Customer Success Executive (optional invitee)
- Fayshal Uddin: Engineer, reviews/releases core-ui

**Original Transcript:** [Gemini notes and transcript](https://docs.google.com/document/d/1_uLB9Lkf_M0x1WYjHQicXj02-w6Tz38lUSigliIAT3w/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

Short status sync. Most updates were ticket-level. Yurii fixed a "start time quick" filter that was misbehaving because of request conflicts; the fix was minimal and was being tested. Andriy was finishing pagination data work and closing progress tickets. Ivan opened a PR for the CSV export column issue and raised a blocker: the PSS column does not render in QA because it depends on a data warehouse pipeline that does not exist in QA environments.

The group agreed on a workaround path for PSS in dev (ask Fayshal about a proxy or mock, or build a local mock from production data), and Miroslav reported that he still could not reach the OpenSearch index in dev while working on flightboard filter problems. Andrew said he would look for the right index and offered to export sample payloads from Kubernetes if dashboard access stays blocked.

The main process decision was for classic-page migration tickets: the team will specify work directly in Linear tickets rather than in repo docs, so the factory can pick them up. Andrew will take the tickets spreadsheet, add assignees, and sort the items; Chuck and Andrew will work top-down. Karol also presented a PR adding relative date range options to project flightboard on the work-order tab, which Andrew confirmed matches the intent.

---

## Key Notes

- **Start time quick filter (Yurii):** The filter was hidden on purpose as a helper filter in a separate config model. Yurii's fix adjusted that config with a minimal change after request conflicts surfaced. Andrew noted that the change did not remove the filter, only clarified the config abstraction.

- **PSS column missing in QA (Ivan, Miroslav, Andrew):** PSS depends on the data warehouse pipeline, which QA does not have. Andrew said data-heavy services usually lack QA support for this reason and suggested asking Fayshal for a proxy or mock, or making a local mock from production data.

- **OpenSearch access in dev (Miroslav):** Miroslav still could not access OpenSearch in dev. Andrew said the cluster naming was confusing (only one index, named provider data) and that he would try to locate the real database. Fallback: export sample documents from Kubernetes.

- **Relative date filter (Karol):** PR adds relative date range options to project flightboard on the work-order tab. Andrew confirmed this is the desired behavior. Karol still needed to fill in the PR description before marking it ready.

- **CSV export column issue (Ivan):** PR submitted and awaiting review comments. Ivan was addressing a review comment from Yurii's side before asking for review.

- **Classic-page ticket format (Chuck, Andrew, decision):** Team agreed to write classic-page specs directly in Linear tickets, not in repo documentation, to make factory processing easier. Chuck proposed having an agent create and review the Linear tickets. Andrew agreed.

- **Classic-page split (Andrew, Chuck):** Andrew will take the tickets tab of the spreadsheet, add names, and sort based on Tuesday's discussion. Chuck will work top-down as time allows, and Andrew will do the same.

- **Team change (Chuck):** Tashfiqul Islam joined to lead analysis and migration work for classic pages to Core UI. (Mentioned in the 11-16 Obsidian daily as well.)

---

## Action Items

- **Ivan Sakhman:** Connect with Fayshal Uddin about mock or proxy options for PSS data in dev.
- **Andrew Kandels:** Locate the OpenSearch database and data payloads, or find the code in the monorepo that separates provider and buyer problem types, for Miroslav.
- **Andrew Kandels:** Update the tickets spreadsheet with assignee names and do a first pass at sorting classic-page items.
- **Karol Tluscik:** Fill out the PR description for the relative date filter before marking it ready for review.
- **Chuck Haines (implied):** Work through the classic-page tickets top-down as time permits.
