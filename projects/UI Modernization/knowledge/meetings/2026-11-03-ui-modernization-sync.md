# UI Modernization Sync, 2026-11-03

**Date & Time:** Tuesday, November 3, 2026, 7:45 CST (~14 minutes)

**Attendees:**
- Andrew Kandels, Engineering lead, meeting organizer
- Chuck Haines, Sr. Principal Software Engineer
- Yurii Dmytriiev, Contractor, Frontend Engineer
- Ivan Sakhman, Contractor, Frontend Engineer
- Andriy Kurenda, Contractor, Frontend Engineer
- Karol Tluscik, Joining full-time this quarter
- Miroslav Isikiyski, Backend Engineer
- Fayshal Uddin, Engineer (reviews/releases core-ui)
- Laura Quist (Douglas), Sr. Customer Success Executive (optional invitee, did not speak)
- Maria Saxton, Linear project lead (tentative, out today; did not attend)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1hR4YWeCNOmCZCv189OOdzAZ_xcYG7w33-gL_t8IG_20/edit)

---

## Overview

The sync was short and mostly status. Provider Match work is moving: Ivan raised the feature provider drawers PR, Andriy's provider filters draft is rebased on Ivan's changes and waits for Ivan's PR to merge, and Karol merged the calendar feature and is building the provider card. Yurii reported two tooling wins from the Ox migration: the printer now saves about 9 seconds, and ESLint dropped from roughly 500 seconds to roughly 200 seconds. Ox is a first step because some ESLint plugins do not yet have matching Ox rules, and those will be ported by hand later.

The main discussion was the end-to-end suite. Andrew reported that the critical E2E suite that blocked releases was fixed last week, and that the full suite is still being repaired because engineers doing larger monolith tickets rely on it. Yurii has fixed about 20 failing tests, mostly selectors, and is still working through staff and provider role failures. Because a rerun takes about 30 minutes, he is tagging failed tests to rerun only those. Andrew also noted that the new Flightboard build has cut retries by about 50 percent across the whole suite, which should give a more consistent run time of roughly 10 minutes.

The team also settled a scope question: everything should target the new Flightboard. There are no redirect paths for the old UI, so the E2E work does not need old selectors.

---

## Key Notes

- **Critical E2E suite restored; full suite still in progress:** Andrew fixed the critical suite last week, which was the release blocker. The full suite is a pain point for engineers doing larger tickets, so the team aims to fix it within about a week. Andrew said it is not a one-hour emergency.

- **Exclusive support for the new Flightboard (decision):** The team agreed the new Flightboard is the only supported version. There are no redirects or fallback paths to the old UI, so E2E selectors and test coverage should target the new build only.

- **Retry reduction from the new architecture:** Andrew's analysis of pre- and post-release runs shows the new Flightboard build cut retries by about 50 percent across the suite, not just Flightboard tests. The old UI tightly coupled rendering to data APIs, so slow responses produced timeouts that retried three times and then showed up as flaky. Andrew expects a more consistent run time of about 10 minutes. This was described as a side effect, not a tracked KPI.

- **Local E2E debugging limits:** Yurii asked about running tests locally. Andrew tried and found the tests need a cluster of about 100 microservices, with internal Kubernetes namespaces and seeded test data, so local runs are not practical. Workarounds the team is using: log in as a staff user, use the failing test's user ID to impersonate the user, and let Claude take over the browser to inspect real test data. Yurii also used a proxy server and a web MCP to speed up single-test runs. Random 500 errors happen during service deploys and restarts, so retries are sometimes needed.

- **Test environment choice:** Test1 and test2 run the same services. Test1 has less traffic and contention, so it is better for debugging. Test2 is the environment used for main E2E runs. Andrew also confirmed test2 has already been moved to the new Flightboard.

- **Ox lint and format migration:** Yurii migrated the printer and ESLint to Ox. ESLint runtime went from about 500 seconds to about 200 seconds. Some ESLint plugins still need manual ports because Ox lacks matching rules. This is the first step of the 10-29 decision to adopt Ox lint and format while keeping ESLint plugin rules working.

- **Provider Match PR sequencing:** Andriy's provider filters PR is blocked until Ivan's PR merges. Andriy has started the connected-route ticket, which can be partly done now but needs other PRs merged to finish. Karol's provider card work continues in its own PR.

- **OBS-661 (project board past 10k):** Miroslav is still on this and hopes to finish today. This is the elasticlist backend work that lets the project Flightboard load past 10,000 rows.

- **Data block API key blocker:** Miroslav needs a data block API key. Andrew has DM'd Ian, who manages those resources, and has not heard back. If Ian asks for a ticket, Andrew will create one and dig in. Miroslav offered to join the thread to follow up directly.

- **Obsidian refinement canceled:** Andrew canceled today's refinement session because Maria is out and the team already understands ticket scope. The calendar entry disappeared and then reappeared without an invite, which is why Miroslav asked.

---

## Action Items

- **Andriy Kurenda:** Update the provider filters PR after Ivan's PR is merged. Continue the connected-route ticket in the meantime.
- **Yurii Dmytriiev:** Keep fixing the failing E2E tests, including the staff and provider role issues. Report progress to the team tomorrow (2026-11-04). Yurii also plans to review the full E2E context tomorrow.
- **Andrew Kandels:** Follow up with Ian on the data block API key status (requested 2026-11-02).
- **Miroslav Isikiyski:** Join or monitor the data block API key thread so the follow-up does not depend on one person.
- **Ivan Sakhman (implied):** Merge the feature provider drawers PR so Andriy's provider filters PR can follow. Continue with the date picker fix and endpoints.
- **Karol Tluscik (implied):** Finish the provider card component and test it.
- **Miroslav Isikiyski (implied):** Finish OBS-661 today.
