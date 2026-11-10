# UI Modernization Sync - 2026-10-19

**Date & Time:** Monday, October 19, 2026, 7:45 CDT (~10 minutes, transcript ends at 00:09:34)

**Attendees:**
- Chuck Haines: Sr. Principal Software Engineer (cross-cutting technical direction)
- Andrew Kandels: Engineering lead, day-to-day technical lead and meeting organizer (joined after the start; Chuck noted he was not sure where Andrew was)
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Miroslav Isikiyski: Backend Engineer
- Karol Tluscik: Joined full-time this quarter (role and title still to be confirmed)

**Original Transcript:** [Gemini notes and transcript](https://docs.google.com/document/d/1gQMszxMRvoP-EWnoqv_WZ1rUW75V5N7hWqQ37pAxcuY/edit?usp=meet_tnfm_calendar)

---

## Overview

This was a short status sync held the morning after the 10-16 sync was skipped. The team reported on the work they had shipped and what they were picking up next. The main delivery news was that Miroslav deployed the project page conversion bug fix (OBS-631, Done) early this morning, tested on QA first, and shipped the PSS column changes that Ivan requested in the same deploy. Andriy merged the templates list and a version validation script for releases, and he took on the project Flightboard pagination bug as the next item.

The meeting also closed out a Sonar question. Yurii's CI run was failing on Sonar magic-number findings that all sat in style and test files. Rather than fix or suppress each one, Chuck deactivated the magic-numbers rule so checks would pass, and Yurii is re-running CI. This is a change from the 10-15 plan to list false positives for review. Yurii also re-enabled ESLint rules that had been turned off during the migration so the new packages have the intended structure.

Ivan's mobile responsiveness fixes merged with a stakeholder label so they can be reviewed in another meeting. Andrew confirmed that setting overflow to visible on the data grid means the whole grid renders and users scroll the page once. Andrew said about five items remain on the board and that he is still grooming the Match-related tickets, which takes time because the AI-generated descriptions are broad.

---

## Key Notes

- **Project page conversion bug (OBS-631):** Miroslav fixed the urgent bug where the new project conversion did not load in QA, verified it on QA, and deployed it early this morning. The PSS column changes Ivan requested were deployed at the same time. Miroslav is still working on the favicon cache persistence fix (old logo still showing) and plans to open a PR soon.

- **Sonar magic-number rule deactivated:** Sonar kept flagging magic numbers in style and test files. Yurii confirmed only those files were affected. Andrew checked and Chuck agreed to deactivate the rule so the build warnings clear. Yurii needs a new CI run. This replaces the 10-15 plan to compile a false-positive list. The rule is off for style and test files only, per the Gemini summary, though the transcript says "deactivated" without scoping. Confirm the exact scope in Sonar.

- **ESLint rules re-enabled:** During the migration several ESLint rules were disabled to move faster. Yurii re-enabled them to keep the structure consistent across the new packages.

- **Mobile responsiveness (Ivan):** Approved mobile fixes merged, and a stakeholder label was added for review in another meeting. With overflow set to visible, the whole data grid renders and users scroll the page once instead of scrolling inside the grid.

- **Analytics and notification ticket (Karol):** The ticket has an analytics part and a notification part. Karol asked how to test it. Andrew suggested a browser extension smoke test and a side-by-side screenshot to show the expected format. Karol will test after the daily sync.

- **Templates list and version validation (Andriy):** Andriy merged the templates list and pushed a version validation script. The Gemini notes say this is the seat tag with the validation script. The script checks versions, and Andriy was waiting on a build to confirm.

- **Project Flightboard pagination bug (Andriy, next task):** Andriy took the project Flightboard pagination bug as the only available item on the board. This matches OBS-558 ("Project flightboard 'All' not paginating all data," In Progress, High). It is relevant to the 10,000-row result cap question (OBS-637), so treat its outcome as input to that open question.

- **Board grooming (Andrew):** Five items remain on the board. Andrew is grooming the Match-related tickets (Migrate Match to Core UI) and says it takes time because the AI-generated descriptions are broad.

---

## Action Items

- **Karol Tluscik:** Smoke test the analytics and notification features for the ticket after the daily sync, and report results to the team.
- **Karol Tluscik:** Share a side-by-side screenshot showing the required analytics format.
- **Yurii Dmytriiev:** Run a new CI build now that the magic-numbers rule is deactivated, and confirm Sonar passes.
- **Andriy Kurenda:** Work the project Flightboard pagination bug (OBS-558) today. (Implied from the discussion.)
- **Miroslav Isikiyski:** Finish the favicon cache persistence fix and open the PR for review. (Implied from the discussion.)
- **Andrew Kandels:** Continue grooming the Match-related tickets on the board. (Implied from the discussion.)
- **Ivan Sakhman:** Continue mobile responsiveness issues. The stakeholder-labeled items need review in the next stakeholder meeting. (Implied from the discussion.)
- **Chuck Haines:** Confirm the exact scope of the Sonar magic-numbers deactivation (style and test files only, or repo-wide). (Implied, not stated in the meeting.)
