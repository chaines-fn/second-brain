# UI Modernization Sync, 2026-11-10

**Date & Time:** Tuesday, November 10, 2026, 7:44 CST (~15 minutes)

**Attendees:**
- Andrew Kandels, Engineering lead, organizer
- Chuck Haines, Sr. Principal Software Engineer
- Yurii Dmytriiev, Contractor, Frontend Engineer
- Ivan Sakhman, Contractor, Frontend Engineer
- Miroslav Isikiyski, Backend Engineer
- Andriy Kurenda, Contractor, Frontend Engineer
- Karol Tluscik, Frontend (joining full-time this quarter)
- Maria Saxton, Linear project lead (tentative on invite; not in the transcript)
- Laura Quist (Douglas), Sr. Customer Success Executive (optional invitee; not in the transcript)
- Fayshal Uddin, Engineer, reviews and releases core-ui (invited; not in the transcript)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1UHm0M_gY8ch1eu-nvWWl_E_2FduJ4YsquBaWNtsMl1k/edit)

---

## Overview

Short, status-focused sync. Chuck opened by noting the team has been handling a lot recently, and Maria had already cancelled the WWT daily and asked for async updates because of the production incident remediation. Most of the time went to a round-the-room status update rather than new decisions.

The backend work for the project Flightboard is now largely in place. Miroslav released the elastic list endpoint for project Flightboard that handles more than 10k objects, with a sanity check showing no data errors. He also had a sync with FO (transcript is unclear; likely Fayshal) about PSS on Flightboard and is adopting a different approach that requires rewriting parts of his earlier work. Andriy can now pick up the front-end 10k pagination work. The project listing page is on hold until other parts land, and roughly five or six logo cache removal PRs are still waiting for approval now that the November 7 date has passed.

The meeting also surfaced a strategic pivot. Chuck and Andrew agreed to move the team toward converting classic pages using the software factory while the factory tokens are still free for another couple of weeks. Warp is currently unavailable because Chuck turned off its GitHub access during a security incident. He plans to ask Cory Muir (transcribed as "Corey") to re-enable it.

---

## Key Notes

- **Project Flightboard 10k+ backend released:** Miroslav released the elastic list endpoint for project Flightboard early today. It handles more than 10k objects, and a sanity check found no data errors. Andriy is picking up the front-end pagination work next.

- **PSS approach revised:** After a sync with FO on PSS for Flightboard, Miroslav is switching to a different approach that requires rewriting some of his earlier work from scratch. He is finalizing it today. He believes the new approach is the better architecture.

- **Universal bookmarks:** Ivan has a PR for universal bookmarks open and is addressing review comments before requesting another review. He also asked for review on a couple of other PRs.

- **OBS-671 (project discrepancy):** Andrew had already code-reviewed this PR. The group agreed Andrew will push a quick update, merge it, and have Miroslav retest. Chuck noted he had not yet reviewed or tested it.

- **Logo cache removal and navigation search:** Andriy has the query-string logo cache removal PRs ready. The November 7 date has passed, and five or six are still waiting for approval. A navigation search fix is also in review. This is the follow-up to the Nov 7 reverse-PR plan in OBS-576.

- **Date filters and tab search:** Karol created a PR that adds predefined date filter options (for example, this week and this month), which were available in the previous version. The tab search fix returns to the searched tab instead of the default tab. Andrew said the tab behavior was intentional originally and is now changing as a feature update, not a bug fix.

- **Warp contract validation:** Yurii used Warp to scan backend services and check UI to backend contracts. This surfaced a bundle ID bug, which he fixed in a PR that also fixes a filter bug. He is building a small contract validation tool and asked how expensive token use would be for that approach.

- **E2E tests:** Yurii fixed almost all E2E tests and is preparing to merge the E2E PR. Three remaining broken tests were split into a separate ticket and branch.

- **Client page and global bookmarks:** Laura and others want to update the client page to match the Flightboard UI and add filters. Andriy will start on global bookmarks alongside the 10k pagination work. Yurii may bring this to today's grooming session.

- **Warp availability:** Warp is running but has no GitHub access. Chuck turned it off during a GitHub security incident. Chuck said the risk is low because everything goes through PRs, and he would ask Cory Muir to turn GitHub access back on. Update from Slack at 09:14 CST: Chuck posted that Warp is back on, and Andrew asked to add fn_ete_playwright and herman to its repo config.

- **Pivot to classic pages (decided):** Andrew and Chuck agreed to pivot the team toward converting classic pages with the factory now, while tokens are free for a couple more weeks. Andrew estimated about six pages and said these should be quick to scope and convert. Andrew will also create match conversion tickets (opt-in, opt-out, and redirect experiment items) and talk with Cory Muir about bringing in other pages.

- **Availability:** Chuck will be out Wednesday, November 11, and is traveling Tuesday and Thursday for a workshop in New York. Andriy's vacation is scheduled from November 23 through December 4 (per Slack).

---

## Action Items

- **Ivan Sakhman:** Address review comments on the universal bookmarks PR and request review when updates are done.
- **Andrew Kandels:** Create tickets for match conversion (opt-in, opt-out, redirect experiment) for the team to pick up.
- **Andrew Kandels:** Talk with Cory Muir about bringing in other classic pages sooner. (Chuck said he would DM both of them.)
- **Andrew Kandels:** Update, merge, and request retest of OBS-671 (project discrepancy PR).
- **Andriy Kurenda:** Fix 10k pagination on the front-end side now that the backend is done.
- **Andriy Kurenda:** Start the global bookmarks client page with new features and filters.
- **Karol Tluscik:** Finish the PR description for the date filtering work and post it to the review channel.
- **Yurii Dmytriiev:** Fix the three remaining broken E2E tests in a separate branch.
- **Chuck Haines:** Ask Cory Muir to re-enable Warp GitHub access.
- **Miroslav Isikiyski (implied):** Finalize the revised PSS approach for project Flightboard today.
