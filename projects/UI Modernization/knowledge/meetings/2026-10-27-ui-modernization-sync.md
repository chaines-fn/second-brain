# UI Modernization Sync - 2026-10-27

**Date & Time:** Tuesday, October 27, 2026, 7:45 CDT (~16 minutes, ended 8:01 CDT)

**Attendees:**
- Andrew Kandels - Tech lead, organizer (day-to-day lead)
- Chuck Haines - Sr. Principal Software Engineer
- Yurii Dmytriiev - Frontend (architectural refactor, Track 2 standardization)
- Ivan Sakhman - Frontend
- Andriy Kurenda - Conversion/infra
- Miroslav Isikiyski - Backend
- Fayshal Uddin - Attendee (role not on file)
- Karol Tluscik - Joined this quarter
- Laura Quist - Invited, optional (Customer Success; not in transcript)
- Maria Saxton - Invited, tentative (not in transcript)

**Original Transcript:** [Gemini notes and transcript (Google Doc)](https://docs.google.com/document/d/1mEmW8JjV620cJ5dj6pPRkqKGBlYpJI4w3VxM3_llO0A/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

The sync opened with Andriy Kurenda reporting he had used 97% of the 250-unit cloud usage limit, about 70% of it on Opus 5.5. Chuck Haines suggested stepping down models (Opus to Sonnet, Sonnet to Haiku) and described an experimental `/advisor` pattern in Claude Code where Haiku handles work and escalates to Sonnet for harder problems. Andrew Kandels said he had used Haiku almost exclusively that month at roughly 150 units with no major quality drop. Andriy agreed to try lighter models starting November 1. The group also compared cloud accounts: Yurii and Ivan were on personal subscriptions, which Chuck flagged as skewing developer-experience cost-per-PR metrics (about $30 per PR for Andriy and Miroslav versus about $0.22 for Yurii and Ivan). Yurii and Ivan agreed to move to Field Nation cloud accounts so that the team can serve as the baseline for the software factory comparison.

On the board, Andriy is finalizing the provider match repo and has a CDN image fix for local testing. The 10k-limit request has a backend task for Miroslav and a frontend draft ready for review. Miroslav confirmed the contractor co-failing fix works on QA1 and is working on a Datadog RAM error-tracking task. Yurii fixed the project listing page filter bug and plans smoke tests, and the group agreed to merge all page bug fixes before a new retest round with the product team. Andrew reported a new bug on the manage projects page, which another team is testing.

The main technical thread was the end-to-end (E2E) suite. Most Field Nation E2E tests reach features through the Flightboard, so they broke when the Flightboard changed. Andrew has the critical-path tests green again in the separate E2E repo, with a full run taking about 1.5 hours on shared environments. The group decided E2E coverage stays limited to mission-critical business flows, not secondary pages like project listing (near 0% traffic). Andrew also raised the open question of whether core-ui needs a PR-label-triggered E2E gate like the main UI repo has, since running it on every push is not feasible.

---

## Key Notes

- **Cloud usage and model selection:** Andriy hit 97% of a 250-unit cloud limit, mostly from Opus 5.5. Decision: try Sonnet and Haiku from November 1. Miroslav's workflow (plan with a larger model, implement with a cheaper one) was shared as a way to reduce token burn. Caveat from Chuck: turn `/advisor` off when using Sonnet as the main model, because a bug caused Sonnet to advise itself and increased token usage.

- **Cloud account consolidation for metrics:** Chuck pointed out that DX cost-per-PR metrics are distorted by personal subscriptions (Yurii and Ivan around $0.22 per PR versus about $30 for Andriy and Miroslav). Yurii and Ivan agreed to switch to Field Nation cloud codes. Chuck noted limits can be raised to 250 through Conductor One. The data is being used as the human-team baseline for the software factory comparison, not for performance evaluation.

- **Merge-then-retest for project pages:** The team agreed to merge all page bug fixes (including Sonar branches from Yurii) before starting a new retest round with the product team. Todd Folie (in the 8:45 sync) reviews the merged changes.

- **E2E suite scope and gating:** Andrew decided E2E tests should cover only mission-critical business paths, not secondary pages such as project listing. Running the full suite per push is infeasible given the shared environments and resource limits. Andrew suggested bringing Playwright-style checks into the core-ui repo for localized runs, and raised a label-gated CI option as an open question. Yurii will get a walkthrough of the E2E architecture and where tests live.

- **RSS column bulk-action endpoints:** Ivan submitted a PR for ranking endpoints and asked whether the backend has bulk-action endpoints for RSS columns (the one-by-one endpoints exist). Andrew had not verified this. Chuck noted PSS is owned by the Platinum team. Andrew will look for a Platinum contact and bring the question to the 8:45 meeting with Maria. Chuck mentioned the project-section break-fix list came back from the testing team.

- **Open items noted in the chat:** Miroslav tracks a 10k-limit backend task and the contractor co-failing fix verified on QA1. Andrew will post the manage-projects bug ticket.

---

## Action Items

- **Andriy Kurenda:** Try Sonnet and Haiku in place of Opus starting November 1 to manage cloud usage (decided).
- **Yurii Dmytriiev and Ivan Sakhman:** Switch to Field Nation cloud codes for tracking (decided). Ivan's specific item: switch environment from individual cloud codes to Field Nation codes.
- **Yurii Dmytriiev:** Finish project listing smoke tests and small UI fixes; merge Sonar branches (implied from discussion); post a Slack update when page bug fixes are ready for retest.
- **Andrew Kandels:** Create tickets for manage-projects page bugs (implied from discussion, "I'll post that into some tickets soon"); review the ranking-endpoints PR; check the contractor-failing fix on QA1; demonstrate the E2E suite architecture to Yurii; find a Platinum contact on RSS bulk-action endpoints and raise it at the 8:45 meeting.
- **Miroslav Isikiyski:** Complete the backend portion of the 10k-limit request (mirror's task, per notes); finish the Datadog RAM error-tracking task.
- **Chuck Haines:** Available to raise cloud limits (up to 250) via Conductor One on request.
- **Maria Saxton or Andrew:** Decide on core-ui E2E gating (label-triggered CI vs. localized Playwright). No owner assigned; flagged as open.

---

## Pending decisions for Chuck

- Whether core-ui should adopt a PR-label-triggered E2E gate as the main UI repo does, given the shared-environment constraints. No decision was made.
