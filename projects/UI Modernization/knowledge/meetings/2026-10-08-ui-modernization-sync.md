# UI Modernization Sync - 2026-10-08

**Date & Time:** Thursday, October 8, 2026, 7:45-8:00 CDT (recording ran about 11 minutes)

**Attendees:**
- Andrew Kandels - Engineering lead, meeting organizer
- Chuck Haines - Sr. Principal Software Engineer
- Ivan Sakhman - Contractor, Frontend Engineer
- Andriy Kurenda - Contractor, Frontend Engineer
- Yurii Dmytriiev - Contractor, Frontend Engineer (invited, mentioned in discussion, not recorded speaking)
- Fayshal Uddin - Engineer, reviews/releases core-ui (invited)
- Miroslav Isikiyski - Backend Engineer (invited)
- Laura Quist (Douglas) - Sr. Customer Success Executive (invited)
- Maria Saxton - WWT Phase 1 project lead (invited, tentative)

**Original Transcript:** [Gemini notes and transcript](https://docs.google.com/document/d/1CprMmzmsRuhdl0giVEirXnAdOMxBxFqDAhL_5qk0OEQ/edit?usp=drive_web&tab=t.fdwkafyjdfq)

---

## Overview

Short standup-style sync covering three threads: the logo cache bump work, a sorting regression, and a buyer report about changed dashboard layouts. Andriy has the UI pull request for the logo cache bump approved and building (the build is running close to an hour). Five of the PRs are ready for review and two more are in progress, roughly eight PRs total across several repos, touching about 13 changes to put the cache bump in place. Andriy will ask Maria to verify the UI PR against the ticket, then post a list of seven PRs in chat so they can merge one at a time. Andrew noted that most of the PRs need owner approval from teams, and that Chuck and he are likely members of most of those teams.

On bugs, the team agreed the mobile issue is low risk because few buyers use mobile, and Yurii is already on it. Ivan reproduced a sorting regression that Andrew had noticed, which he believes came from moving files into packages, and he is testing a fix before opening a PR. The dashboard layout complaint from one buyer looks like user action rather than a system change: Ivan verified that old and new Flightboard settings sync in both directions, and Andrew suspects an admin reset company default columns. Andrew will follow up with the buyer, and Ivan will add screenshots and close the item if the reorder check holds.

Two other items came up. The redirects fix is intentionally postponed until 100% of buyers are on the new Flightboard, because the feature-flag experiment needs old-to-new redirects until the flag can be retired. Chuck also announced that a new FN UI skill was released in the AI toolbox the previous day, with current branding, logos, colors, and fonts, intended for engineering work on core-ui and replacing older agent rules and local skills.

---

## Key Notes

- **Logo cache bump:** Andriy's UI PR is approved and in the final build. Five PRs are ready and two are in progress. Andriy will post the merge list after Maria verifies the UI PR against the ticket. Andrew estimated about eight PRs across repos, about 13 changes in total, for the cache bump. Ownership of PRs without a clear owner team is being identified, with Andrew and Chuck likely covering most of them. (Context: this follows the Cloudflare `/logos/*` caching issue noted on 2026-10-06 in tech-context.md.)

- **Mobile bug:** Andrew assessed the mobile issue as a minor risk. The buyer's own comment acknowledged the product is not optimized for mobile, and this issue predates the migration. Yurii is handling it.

- **Sorting regression:** A sorting bug that had been working was found broken when Andrew checked the day before. Ivan reproduced it and believes it came from the move of files into packages. A fix is in test, and a PR will follow once tests pass.

- **Dashboard layout report (old vs. new Flightboard):** Ivan confirmed that changes in the old settings show up in the new view and the reverse. Andrew's working theory is that an admin reset company default columns, which would apply to all users of that company. Andrew will push back to the buyer and ask CS to help reach them. Closing the ticket depends on confirming that a column reorder on old carries over to new, which Ivan says he already did with a good result. He will add screenshots.

- **Redirects (postponed by design):** The redirect fix cannot be done until 100% of buyers are on the new Flightboard. Without it, the experiment cannot function, and opt-outs cannot be honored. It can move forward once the team is comfortable retiring the feature flag.

- **Auto-dispatch and board hygiene:** Ivan opened a PR for a missed auto-dispatch filter and will request review. The time-value PR is already merged. One transaction-related PR is still open. Andrew and Ivan agreed to close out resolved items so the board stays accurate.

- **FN UI skill (Chuck):** Released in the AI toolbox the day before this meeting. It includes new branding, logos, colors, and fonts, and it references React, styling, and the branding guide. Engineering teams should use it for core-ui additions. It is specific to software engineering. The company-wide FN style skill is separate and is intended for companywide use.

- **Design system navigation:** Andrew noticed that the Field Nation design system link moved from an in-app link into the application menu. Standalone Cloud Design access still works. Andrew suspects a teammate changed the menu setup. Chuck is unsure what the design-system skill links to and may relink it.

---

## Action Items

- **Andriy Kurenda:** Ping Maria Saxton to verify the logo cache bump UI PR against the ticket requirements.
- **Andriy Kurenda:** Once Maria confirms, post the list of seven PRs in chat so they can be merged one by one.
- **Andrew Kandels and Chuck Haines (implied):** Identify owning teams for PRs that lack an owner, and request approvals from those teams.
- **Ivan Sakhman:** Request review for the auto-dispatch missed-filter PR.
- **Ivan Sakhman:** Finish verifying the sorting fix, then open the sorting PR.
- **Ivan Sakhman:** Add old-vs-new settings screenshots to the dashboard layout ticket, and close it once the validation is confirmed.
- **Andrew Kandels:** Follow up with the buyer about the dashboard layout report (with CS support as needed).
- **Ivan Sakhman (implied):** Close out the merged time-value item and the other resolved board items so the board is accurate.
- **Chuck Haines (implied):** Check whether the design-system skill link in the menu still points to the correct artifact.
