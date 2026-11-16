# UI Modernization Sync: 2026-11-16

**Date & Time:** Monday, November 16, 2026, 7:45 CST (~10 minutes)

**Attendees:**
- Andrew Kandels: Engineering lead, organizer
- Chuck Haines: Sr. Principal Software Engineer
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Miroslav Isikiyski: Backend Engineer
- Karol Tluscik: Frontend (joining full-time)
- Maria Saxton: Linear project lead (on invite, tentative)
- Laura Quist: Sr. Customer Success Executive (optional invitee, did not join)
- Fayshal Uddin: Engineer, reviews/releases core-ui (on invite)
- Tashfiqul Islam: Classic-page migration lead

**Original Transcript:** [Gemini notes and transcript](https://docs.google.com/document/d/1LL8JO1Jc59NcM4_nqJXZ9eJVjUUPQ96WZDHxwElwVmQ/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

Status sync. Andriy merged query-string removal across all eight repos (the OBS-576 logo cache reversal) and finished the backend for the pagination mirror. He adjusted the frontend and was waiting on final GitHub review. Two PRs he had approved on Friday were no longer relevant because of issues, so they need re-approval. Andriy was also trying to finish the hanging provider match route today, which overlaps with a PSS issue for providers.

The group agreed to focus on provider match first and to take classic-page tickets later, toward year end and into January. Miroslav started a batch of backend tickets beginning with OBS-720 (predefined API), with a PR to follow after local testing. He also put the problematic main-flightboard filter back into a waiting state to align with Andrew in Slack. Andrew warned that API tickets adding to the core API will get wider review (bounded contexts and naming), so approvals may be slower than usual.

---

## Key Notes

- **Query string removal (Andriy):** Done. Ticket closed; all eight repos merged.

- **Pagination mirror (Andriy):** Backend complete and frontend adjusted; awaiting final review before requesting approval.

- **Provider match / PSS (Andriy, Ivan):** Andriy is finishing the provider match route, targeted for today. Ivan raised a PR for the provider services system on Friday and is pushing review fixes. Priority order agreed: match providers first, classic pages later.

- **Classic-page migration timing (Andrew, Yurii):** Work runs through roughly year end into January. Most tickets are a single feature with a backend ticket that unlocks frontend tickets. Specs are linked from each ticket's resources. Developers can start frontend work early with a local proxy and follow the spec, or wait for backend changes.

- **Core API scrutiny (Andrew):** Tickets that add to the core API will get extra review from multiple people on bounded-context and naming questions. Expect slower approval on those tickets.

- **Classic-page notification feature (Karol):** Started one of the classic-page tickets (notification feature) and is working with Claude to determine what is missing.

- **Predefined API batch (Miroslav):** Started the batch beginning with OBS-720. Opened a PR and is testing locally. Not blocked.

- **Chuck availability:** Limited over the next three days and unavailable Wednesday 11-18. Andrew will record the 9:00 follow-up so Chuck can watch it later.

- **Toshi onboarding:** Toshi is organizing go-to-market communication and migration work in Linear, using multiple projects. Andrew will confirm Toshi's start and attendance at the 9:00 follow-up.

- **Session note:** Andriy reported intermittent one-minute disconnects with the AI tool during requests. Andrew attributed this to a partial outage on the status page.

---

## Action Items

- **Andriy Kurenda:** Complete the provider match route, including the remaining backend work and the PSS issue, today.
- **Miroslav Isikiyski:** Push the PR for the predefined API ticket OBS-720 after local testing.
- **Andrew Kandels:** Confirm Toshi's inclusion and start date at the 9:00 follow-up.
- **Andrew Kandels:** Record the 9:00 meeting so Chuck can watch it later.
