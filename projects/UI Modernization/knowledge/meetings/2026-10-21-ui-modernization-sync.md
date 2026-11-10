# UI Modernization Sync, 2026-10-21

**Date & Time:** Wednesday, October 21, 2026, 7:45 CDT (~13 minutes; transcript ends at 00:13:18)

**Attendees:**
- Andrew Kandels, engineering lead and day-to-day technical lead (meeting organizer)
- Yurii Dmytriiev, contractor, Frontend Engineer
- Ivan Sakhman, contractor, Frontend Engineer
- Andriy Kurenda, contractor, Frontend Engineer
- Karol Tluscik, joined the team this quarter, ramping to full allocation
- Chuck Haines, Sr. Principal Software Engineer (on the invite; did not speak in the transcript)
- Invited but not present per transcript: Miroslav Isikiyski (referred to as "Mero" in the transcript; Andriy said he was working that morning but could not attend stand-up), Fayshal Uddin, Maria Saxton (tentative)

**Original Transcript:** [Gemini notes and transcript](https://docs.google.com/document/d/1ZfBl77H28WA2WdgPECBFoGt7ilw3Li_RLq-sDH8OLXc/edit?usp=meet_tnfm_calendar)

---

## Overview

The short stand-up covered three threads: Sonar and ESLint rule integration, PR and migration status, and the pagination design for large project datasets. The Sonar discussion ended in agreement to enable the ESLint Sonar plugin rules, with simpler rules as errors and the rest as warnings to start. Yurii will open the PR with fixes in a warning state. Andrew noted the one risk: if a Sonar rule is later changed or disabled, the matching ESLint rule has to be updated by hand.

The pagination discussion was the most substantive item. Andriy had built a local front-end pagination approach that needs backend changes, and he was concerned it reverses the earlier backend work and could make chunked responses slower. Andrew walked through the data routing: active work orders come from an elastic list endpoint under 1,000 items with no pagination; all work orders come from a second route capped at 10,000 items and loaded asynchronously; configuration data comes from a third route. For datasets above 10,000, Andrew proposed keeping the 10,000 batch size and looping over further batches (20K and beyond) into Redux or RTK state while the page stays interactive. That requires restoring limit and offset parameters on the backend. Andriy confirmed the front end is ready and will ask Miroslav to add the pagination flags back to the endpoint so they can test it.

## Key Notes

- **Sonar and ESLint rules:** Yurii pointed out the Sonar ESLint plugin was missing after a recent migration. He proposed enabling its rules with simpler ones as errors and others as warnings to start, so existing errors get fixed and future ones get caught. Andrew said the Sonar MCP setup has a similar effect, so overlap is acceptable, but warned that changing or disabling a Sonar rule means updating the ESLint counterpart manually. The team agreed to try it. The transcript does not address Chuck's 10-19 decision to deactivate the magic-numbers Sonar rule.

- **Notifications and Smart Scope:** Karol completed the notifications PR with screenshots and it is ready for review. Karol also received a green light to implement redirection for the Smart Scope project, a previous project of hers, and will prepare one small PR for it.

- **Filters API and empty column:** Ivan submitted the filters API for review and picked up the empty-column issue. He will continue Sonar or migration work after that.

- **Andriy's draft PR and mirror backing:** Andriy updated the draft PR for the November 7 items after the mirror backing changes with image caching. He is also working on Sonar fixes and the pagination issue.

- **Pagination architecture (decision):** The team agreed to restore pagination parameters on the backend endpoint so the front end can load large datasets in chunks. Andrew's rationale: the primary goal is a fully interactive page, and background loading of the rest of a large project costs time only for very large projects. Andriy's concern was that removing the parameters earlier reverted the performance work. Andriy confirmed his approach is the right one and will test once the flags return.

- **Related finding (Slack, 2026-10-21 06:21 CDT):** Andriy posted that on projects with more than 10,000 work orders, the project Flightboard "All" tab shows only the first 10,000, because the project POST endpoint in elasticlist returns at most 10,000 rows with no way to fetch the next set. The response includes metadata indicating truncation. This ties to OBS-558 and to the open question of whether the project view silently truncates (OBS-637). It was not discussed in the stand-up.

- **Refinement later today:** Andrew will prepare additional tickets, with a focus on the Match work, for the refinement session that followed the meeting.

## Action Items

- **Yurii Dmytriiev:** Open the Sonar fixes PR with the ESLint Sonar plugin rules in a warning state for review.
- **Karol Tluscik:** Prepare the Smart Scope redirection PR (one small change).
- **Andriy Kurenda:** Ask Miroslav to add the pagination flags back to the backend endpoint, then test pagination performance once the backend change lands.
- **Miroslav Isikiyski (implied):** Add pagination parameters (limit and offset) back to the elasticlist endpoint. Andriy is the one asking, so confirm with Andriy.
- **Andrew Kandels:** Prepare additional tickets for the upcoming refinement session. Also confirm Miroslav's availability, since Andrew wants this item moved sooner.
- **Ivan Sakhman (implied):** Continue the empty-column fix and the filters API review follow-up.
