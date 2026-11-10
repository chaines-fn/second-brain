# UI Modernization Sync - 2026-10-29

**Date & Time:** Thursday, October 29, 2026, 7:44 CDT (~16 minutes)

**Attendees:**
- Andrew Kandels - Engineering lead, meeting organizer
- Chuck Haines - Sr. Principal Software Engineer
- Yurii Dmytriiev - Contractor, Frontend Engineer
- Ivan Sakhman - Contractor, Frontend Engineer
- Andriy Kurenda - Contractor, Frontend Engineer
- Miroslav Isikiyski - Backend Engineer
- Karol Tluscik - Joined this quarter for extra bandwidth

Invited but did not speak: Laura Quist (Sr. Customer Success Executive), Maria Saxton (tentative), Fayshal Uddin (Engineer, reviews/releases core-ui).

**Original Transcript:** [Notes by Gemini and transcript](https://docs.google.com/document/d/1kEHOkDNfHNH48_SIbJc98BEgNVB9SrndajKvHA2pFus/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

The sync was short and mostly about tooling and a high-severity vulnerability. Karol took a new ticket for the provider availability view after Ivan's overlapping work on it was folded into another PR, and Andrew confirmed the remaining Provider Match tickets are ready to pick up. Andriy is waiting on Fayshal's approval for the Provider Match repo bump, has fixed several Flightboard filtering bugs, and is working through Sonar cleanup. Yurii already had a PR open for the vulnerability Chuck reported, which bumps the Node minor version to 24.

The main discussion was about build and tooling speed. Yurii noted the test suite is now over 7,000 tests and proposed testing the Rust-based React compiler behind a feature flag for local development while production stays on Babel. Andrew agreed to switch since it consolidates compilation and optimization into one tool. The group also agreed to adopt Ox lint and Ox format while keeping existing ESLint plugin rules working during the transition, and Chuck and Andrew agreed to try Bun for UI builds and linting (not as a backend runtime). Andrew tied this to the build-time KPI, one of the two key OKRs.

---

## Key Notes

- **Vulnerability fix and Node version:** Chuck reported a high-severity vulnerability with a roughly seven-day remediation window. Yurii's PR bumps Node from 24 to the next minor version now. Chuck will review it when he gets home. Yurii asked whether to jump to Node 26 (current LTS) directly. Chuck's call: do the minor bump now to meet the deadline, and open a separate ticket to move to Node 26 to avoid more friction later. Yurii will update Node versions weekly as part of regular dependency maintenance.

- **Rust-based React compiler:** The codebase still uses Babel. Yurii proposed a feature flag so developers can use the Rust-based React compiler locally while production stays on Babel. His comparison showed matching output for memoization and other optimizations, with the compiler covering about 10% more functions than Babel (roughly 2,000 functions checked). Andrew asked about stability; Yurii said it is part of React's official toolchain and recommended by the product team. Andrew agreed to switch since it combines compilation and optimization into one tool.

- **Ox lint and format:** Yurii recommended starting with Ox lint and Ox format. Ox runs existing ESLint plugin rules on a Rust engine, so rules that Ox does not yet cover still work. Andrew's concern was that teams bringing other unlinted systems into core-ui would otherwise each need to update configs, so compatibility matters. Chuck and Andrew agreed to evaluate all three (compiler, linter, formatter) together. Yurii will open small, step-by-step PRs.

- **Bun for build and lint:** Chuck raised Bun as a build accelerator (he uses it on Vercel deployments). Yurii reported it is much faster and uses standard npm packages. Andrew was hesitant about Bun as a backend runtime because of ecosystem churn and possible security scanner flags, but agreed it is a low-risk place to test since core-ui only uses it for building and linting, not runtime. Consensus: worth trying, with output parity and tests as the bar.

- **Board blockers:** Andriy asked why many "To Do" tickets show as blocked. Andrew said the planning document applied sequencing order as if it were a technical dependency. Most of those tickets can start now, with perhaps one that truly needs another ticket's code. Andrew has updated the planning skill and docs and will clear inaccurate blockers on the board.

- **Team capacity:** Karol is on the provider availability view ticket (assigned to him after Ivan's partial overlap). Andriy is blocked on the Provider Match repo bump pending Fayshal's review questions. Ivan is fixing comments on a Sonar cleanup PR before requesting review, then continuing with endpoint work.

- **Pending items to Miroslav:** Andrew acknowledged he owes Miroslav a few items and will complete them as soon as he can. Miroslav had a quick question that he said can wait.

---

## Action Items

- **Ivan Sakhman:** Address comments on the Sonar cleanup PR and submit for review.
- **Andriy Kurenda:** Get approval from Fayshal on the Provider Match repo bump and follow up on his questions.
- **Chuck Haines:** Review the Node vulnerability fix PR when he returns home.
- **Yurii Dmytriiev:** Create a ticket to upgrade Node to version 26 LTS (scheduled as tech-debt remediation).
- **Yurii Dmytriiev:** Update Node versions weekly as part of dependency maintenance (implied, per discussion).
- **Yurii Dmytriiev:** Open a PR for the Ox lint and format transition, step by step, keeping ESLint plugin compatibility.
- **Andrew Kandels:** Update the project board to remove inaccurate blocker dependencies and clarify which tickets are truly blocked.
- **Andrew Kandels:** Deliver the pending items owed to Miroslav as soon as possible.
- **Unassigned (implied):** Evaluate the Rust-based React compiler behind a feature flag and Bun for UI build and lint. Yurii volunteered for the compiler and Ox work; Bun ownership was not explicitly assigned.
