# UI Modernization Sync - 2026-10-13

**Source:** Google Meet Gemini notes, [transcript doc](https://docs.google.com/document/d/1JsQNHlwIFZbmhIlBXqMDF-5_2r6ShRtzbcELy-guqAI/edit) (7:45 CDT, organizer Andrew Kandels)
**Attendees (invited):** Andrew Kandels (organizer), Chuck Haines, Yurii Dmytriiev, Ivan Sakhman, Andriy Kurenda, Miroslav Isikiyski, Fayshal Uddin, Laura Quist (optional), Maria Saxton (tentative)
**Notes quality:** Gemini-generated; review before relying on specifics. Transcript runs about 11 minutes.

## Summary

Draft PRs for the project page work are ready across all repos, with the template listing page data grid nearly done. Yurii finished feature extraction and turned on Turbo boundaries and ESLint enforcement. Ivan's foundation MUI fix surfaced a lint scope question, which the team settled by limiting lint checks to touched files.

## Template listing page and draft PRs

- Andriy completed draft PRs for all repos, with caching added so they can be merged later. Andrew will create a calendar event to merge them. The notes say "the seventh," which probably means Nov 7 given OBS-576 (confirm).
- Andriy finished the template listing page data grid locally. The ticket asked for a project filter, but the backend API already scopes to the current project, so the filter is redundant. Andrew confirmed it was a leftover from sharing the API with the main templates list. Andriy will skip it. Andrew noted this is probably unique to this ticket, since Flightboard and clients already handle this differently.

## Feature extraction and lint

- Yurii finished feature extraction. One final small PR will make all features flat with no cross-imports.
- Yurii flagged a layout bug: on smaller browser sizes it is hard to scroll to the bottom of the data grid. Andrew said temporary loss of scroll during resizing is acceptable if a refresh fixes it. The open question is the minimum supported screen size. Yurii will test and check with Laura in Slack.
- TurboRepo scripts and Turbo boundaries are fully enabled. Yurii will turn on ESLint in an error state.
- Decision: ESLint fixes are limited to files a developer touches. Fixing the foundation MUI package's column-header issue triggered ESLint errors in about 280 files, so Ivan will fix only the files he touched. Andrew will open a follow-up ticket to clean up lint at the package level.

## Service deployments and PSS

- Miroslav deployed the service company admin messages service on 10-12.
- Miroslav is finishing the PSS columns ticket for project Flightboard (OBS-583, per Slack) and aims to submit it for review by end of day.
- Miroslav will pick up backlog work after this. Andrew said more tickets will come out of sprint refinement later that day.

## Bugs and error monitoring

- Ivan could not reproduce bug 573 (different settings) and added screenshots. Maria already contacted the sales rep for the affected buyer. Andrew will close the ticket if there is no response within one day.
- Andrew asked the team to pick up an unassigned Datadog error about a filter mass-actions null split. He said it is low priority; it has happened only a few times.
- Datadog errors are down significantly. The remaining errors are mostly timeouts and network issues.

## Next steps

- Andrew: create a calendar event to merge the draft PRs; create a maintenance ticket to bring packages to a clean lint state; close OBS-573 if no sales-rep response in a day.
- Yurii: submit the final feature extraction PR; test the browser-resize scrollability bug and confirm with Laura; enable ESLint in error state.
- Miroslav: finish and submit the PSS columns ticket.
- Ivan: fix the foundation MUI column-header issue (touched files only).

## Related Slack context (10-12 to 10-13, not in the meeting)

- OBS-579 (Sarah Welter, new Flightboard challenges/bugs, filed 10-08): thread active through 10-13 09:19 CDT.
- OBS-585 (filed by Maria 10-12): Today filter saves with the creation date instead of a relative value. Similar to OBS-488 (relative time filters), status to confirm.
- OBS-584 (Molly Grailer, Spectrotel disappearing column headers): Andrew could not reproduce on 10-12 and is waiting for a recording. Gemini notes say Andrew is also looking at a MUI issue alongside Travis Lang; Ivan's foundation MUI column-header finding (10-13) may be related. Unconfirmed.
- Maria's read (10-12): Edge and Chrome render the virtualized grid differently, which explains Ctrl+F gaps in Edge. Andrew asked to confirm.
- A chat bubble overlaps the paging controls on the new Flightboard (Mari Aune, 10-12 thread).
- Chuck asked the team (10-13, 8:03 CDT) whether PR #605 has too many components in one file. Yurii and Andrew were tagged.
- Karol Tluscik asked for local environment values (10-13); the thread has one reply.
