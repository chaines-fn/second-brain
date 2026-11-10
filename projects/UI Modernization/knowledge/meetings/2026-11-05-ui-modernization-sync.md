# UI Modernization Sync, 2026-11-05

**Date & Time:** Thursday, November 5, 2026, 7:45 CST (~15 minutes)

**Attendees:**
- Andrew Kandels, Engineering lead, day-to-day technical lead (meeting organizer)
- Chuck Haines, Sr. Principal Software Engineer
- Yurii Dmytriiev, Contractor, Frontend Engineer
- Ivan Sakhman, Contractor, Frontend Engineer
- Andriy Kurenda, Contractor, Frontend Engineer
- Isikiyski Miroslav, Backend Engineer
- Karol Tluscik, Joining full-time this quarter (role and title still to confirm)
- Invited, did not speak: Laura Quist (optional), Maria Saxton, Fayshal Uddin, Todd Folie (not invited)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1p3fL5SWm5X4wAjJ7dimeTVSTjHgQQ8EYO-Sz6ACPlL0/edit?usp=drivesdk&ouid=104527139387599317887) (Gemini notes and transcript; transcript is computer generated and may contain errors)

---

## Overview

The sync was mostly status, with the main discussion centered on pending dependency merges and the PSS (provider success score) loading approach on the Flightboard. Miroslav deployed OBS-661 (project Flightboard with 10k records) to production and is waiting on PR feedback. Andrew explained that the current PSS column loads scores for every row at once, which does not scale on the new Flightboard. The team needs a browser-callable endpoint that fetches PSS for a set of IDs, so lazy loading can work like the main app's paginated approach. Miroslav will sync with Fayshal first to find where that route lives or where to build it, and then pick up OBS-638.

Build and test health was the second theme. Yurii migrated Node.js dependencies to the latest LTS (26) and fixed some vulnerabilities, with a PR up for review. He also added caching for node modules and turbo cache reuse. The team discussed moving CI to Harness for better Docker layer caching and shared core workflows. Chuck said it needs to go on the backlog, and Andrew noted the team has not yet been able to create workflows there. Yurii also prepared an E2E PR that fixes most of the 63 previously failing tests. The team agreed to quarantine the remaining flaky tests so PRs can merge, and Yurii will coordinate with Risol on the automation team to validate the change.

Chuck closed by pointing the team to the Warp software factory: a tour and demo were scheduled for 9:00 CST, and he sent access instructions by email. That tour is the start of the team's Warp Factories adoption decided on 11-04.

---

## Key Notes

- **Provider card PSS and popovers (Karol):** Karol has the provider card PSS column and all popovers ready for review. He is waiting for Yurii's PR (Storybook and Node LTS bump) to merge, then he will rebase and mark it ready.

- **PSS loading inefficiency and endpoint need (Andrew, Miroslav):** Andrew said the Flightboard loads PSS for all rows in one call to a separate index, which is inefficient at scale. The main app only ever sees about 100 rows because of pagination, but the new Flightboard could request every row. The team wants lazy loading (fetch only visible rows), which requires a browser-accessible endpoint that takes a list of IDs. Andrew said PSS can be pulled from the database or Elasticsearch one at a time today, but no bulk route is known to exist. Miroslav owns OBS-638 and will sync with Fayshal first. Andrew also noted the Flightboard PSS work is tied to the same question that came up in the 11-05 Obsidian daily (bulk route coordination).

- **Node 26 LTS and vulnerability fixes (Yurii):** Yurii migrated Node.js to LTS 26 and fixed several vulnerabilities, with a PR open. This closes the Node minor-bump work discussed on 10-29 (VULN-387). Note: the Gemini notes say "Yurii migrated NodeJS dependencies to the latest LTS version 26"; the transcript confirms a PR is prepared, so treat "merged" as unverified.

- **Build caching and Harness CI (Yurii, Andrew, Chuck):** Yurii added caching for node modules and reuse of the turbo repo cache, and asked when the team will switch to Harness CI. Chuck said it needs to go on the backlog. Andrew said he has not been able to edit Harness workflows and needs help from the platform team to get access. Andrew's view: Harness has org-level core workflows and better Docker layer caching, so caching benefits would apply across repos. Yurii's concern: turbo cache support in Harness (Andrew said it works, since Harness supports GitHub Actions-style YAML).

- **E2E fixes and quarantine (Yurii, Andrew):** Yurii prepared an E2E PR that updates selectors and some test logic, fixing most of the 63 previously failing tests. Remaining flaky tests are unresolved. **Decision (aligned):** mark the remaining flaky E2E tests as quarantined so PRs can merge, and research them in follow-up work. Andrew said Risol from the automation team is waiting on this PR and may help validate it.

- **Error issue PR and ranking settings (Ivan):** Ivan has a ready PR for an error issue and updated the provider manage and provider draw PRs. He will wait for Yurii's PRs to merge and rebase before picking up ranking settings.

- **OBS-604 coordination (Andriy, Ivan):** OBS-604 blocks Andriy from finishing the providers page. Ivan partially has it and will check whether Andriy can take it over. Andriy is building the providers page locally; it stays a draft until dependent PRs merge.

- **Whiz Secrets public key alert (Andrew, Chuck):** A PR was flagged by Whiz Secrets for a public Google key. Neither Andrew nor Chuck currently has permission to suppress or ignore the finding. Ivan or Ben likely has access; Chuck will reach out.

- **Warp Factories kickoff (Chuck):** Chuck sent access instructions for the Warp software factory and scheduled a 9:00 CST tour and demo. He said the live environment will be shared even if the Warp presenter does not attend. This follows the 11-04 decision to adopt Warp Factories for dev work.

- **Assignments:** Miroslav will pick up the PSS route (OBS-638) after syncing with Fayshal. Andriy starts on the search task while his page waits on merges.

---

## Action Items

- **Karol Tluscik:** Rebase the provider card PR once Yurii's Storybook/Node PR merges, then mark it ready for review.
- **Isikiyski Miroslav:** Sync with Fayshal on the bulk PSS approach and prepare for OBS-638 (Flightboard PSS efficiency). Ping Andriy on Slack if blocked.
- **Andriy Kurenda:** Start the search task while the providers page waits on dependent PRs. Coordinate with Ivan on OBS-604.
- **Yurii Dmytriiev:** Contact Risol on the automation team to validate and merge the E2E PR, and coordinate on quarantining flaky tests.
- **Ivan Sakhman:** Pick up ranking settings after Yurii's PRs merge. Verify OBS-604 with Andriy.
- **Chuck Haines:** Get Whiz Secrets access (reach out to Ivan or Ben) to suppress the public key warning.
- **Chuck Haines (implied):** Add Harness CI migration to the backlog and help unblock Harness workflow access for Andrew and Yurii.
- **Andrew Kandels (implied):** Review and approve pending PRs, including Yurii's E2E PR and the Node LTS PR.
