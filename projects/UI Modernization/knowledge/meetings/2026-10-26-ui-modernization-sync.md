# UI Modernization Sync - 2026-10-26

**Date & Time:** Monday, October 26, 2026, 7:45 CDT (~10 minutes; transcript ends at 00:09:40)

**Attendees:**
- Chuck Haines - Sr. Principal Software Engineer (heard in transcript)
- Andrew Kandels - Engineering lead, day-to-day technical lead and meeting organizer (heard in transcript)
- Miroslav Isikiyski - Backend Engineer (heard in transcript)
- Karol Tluscik - Joining full-time this quarter, role to confirm (heard in transcript)
- Ivan Sakhman - Contractor, Frontend Engineer (heard in transcript)
- Yurii Dmytriiev - Contractor, Frontend Engineer (invited; Andrew said he was out, not present)
- Maria Saxton - Linear project lead, WWT/Core-UI portfolio (invited, not heard)
- Andriy Kurenda - Contractor, Frontend Engineer (invited, not heard)
- Fayshal Uddin - Engineer, reviews and releases core-ui changes (invited, not heard)
- Laura Quist (Douglas) - Sr. Customer Success Executive (invited, not heard)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/10vcrWjTztleCljh9VDWOY4jMZkjKxSe1iakF-aQCRAU/edit)

---

## Overview

This was a short check-in, shifted an hour earlier for the weekend time change in Europe (the US change is not until next weekend, so the next sync moves an hour again). Most of the time went to status. Karol is reproducing the user-context failure that shows up during peak hours by simulating sporadic timeouts locally, and has put the provider endpoints into review. Ivan has a PR up fixing endpoint comments for provider profiles and reputation, and is picking up bugs from the SL alongside his to-do list.

The more substantive topic was what the new Flightboard now does with errors. Andrew explained that the new Flightboard catches and toasts every error. Before, contract service, user context, and reputation service failures (mostly timeouts under high load around noon) were only logged to the console and never reached users. Now they are visible, users are reporting them, and the team is working through them one at a time. Andrew also said the ATS (Playwright) tests broke when Harness was deprecated and the old Flightboard route was removed. The locators are out of date, and he is working through the critical tests first with AI help.

Miroslav is on OBS-651 (contract calls failing and raising an error toast), then Datadog source maps (OBS-650) so error traffic is readable, then backend pagination for some endpoints that Andriy will ticket and assign to him.

---

## Key Notes

- **Harness deprecation broke ATS tests:** Flightboard changes and the removal of the Harness route invalidated the locators in the ATS Playwright suite. The tests only passed while the app still ran on the old Flightboard code path. Andrew is fixing the critical ones first and may ask for help if they are not resolved today. Related: OBS-655 (e2e tests for new Flightboard), and ui PR #8717 changed Work Orders tab locators, per the #eng-help thread on 10-23.

- **New Flightboard surfaces previously hidden errors:** The new Flightboard catches and toasts all errors. Contract service failures, user context failures, and reputation service failures, mostly timeouts at high load around noon, used to only log. They now reach users, which is why the related tickets are arriving. Andrew's view is that these errors were probably leaving the page without the right data, so fixing them should improve correctness as well as surface area.

- **User context issue (Karol):** Reproduction is happening during peak hours. Andrew's suggested approach is to add a sporadic timeout locally (fail twice, pass on the third attempt) because local dev runs through a proxy. Karol plans to test that way.

- **Provider endpoints in review:** Karol put provider endpoints into review. Ivan has a PR fixing endpoint comments for provider profiles and reputation.

- **Bug priority and release timing:** Andrew said there is no release-day plan yet for the project-related bugs, so they will be added as they come in. Urgent bugs get high priority once filed. Some may be marked pre-existing and not urgent. Ivan can start on SL bugs he has seen before the ticket exists. Andrew has already approved a PR covering three of them.

- **Backend pagination follow-up (Miroslav):** Miroslav coordinated with Andriy by private message. Andriy needs backend pagination on some endpoints and will create a ticket to assign to Miroslav after the source maps work. This is the same pagination thread as OBS-558 (project Flightboard "All" capped at 10k rows), which is still waiting on Andrew's approval for the search_after proposal. This sync did not confirm whether that ticket is the one being discussed.

- **Meeting time:** The sync moved one hour earlier this week because Europe's clocks changed before the US. Karol flagged it. Andrew will ping earlier next Monday as well.

---

## Action Items

- **Miroslav Isikiyski:** Open the PR for OBS-651 (contract call failing and creating an error toast).
- **Miroslav Isikiyski:** After OBS-651, work on the Datadog source maps ticket (OBS-650) so obfuscated error traffic is readable.
- **Miroslav Isikiyski (implied):** After source maps, take the backend pagination ticket Andriy is expected to create and assign.
- **Andriy Kurenda (implied):** Create the backend pagination ticket and assign it to Miroslav.
- **Andrew Kandels:** Fix the ATS/Playwright locators broken by the Flightboard changes, critical tests first. Ask for help if not done today.
- **Karol Tluscik:** Reproduce and test the user context issue locally with simulated sporadic timeouts. Follow up on the provider endpoints in review.
- **Ivan Sakhman:** Address review comments on the endpoint PR (provider profiles, reputation). Pick up SL bugs as they come in, coordinating with Andrew when a ticket is not yet filed.

---

## Notes on the source

- Gemini's notes were reviewed against the transcript. Two terms may be transcription errors and are kept as written: "SL" (unclear what it refers to) and "ATS" (kept as stated). Gemini also rendered Andriy as "Andriyon." The transcript has a closing line addressed to "Mike," which is not a meeting participant and was not used.
- Yurii was out, per Andrew. Maria, Andriy, Fayshal, and Laura were invited, but none appear in the transcript.
- Andrew said "the 845 one" for the WWT daily, which follows this sync.
