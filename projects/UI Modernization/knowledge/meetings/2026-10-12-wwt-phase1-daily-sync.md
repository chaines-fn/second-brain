# WWT Phase 1 Daily Sync - 2026-10-12

**Source:** Google Meet Gemini notes, [transcript doc](https://docs.google.com/document/d/11OEejT7DCcWu9o_Pl9epWgBO2YxUZI4NutrwZvJEyiQ/edit) (8:45 CDT)
**Attendees (invited):** Maria Saxton (organizer), Andrew Kandels, Chuck Haines, Yurii Dmytriiev, Ivan Sakhman, Andriy Kurenda, Todd Folie, Laura Quist, Fayshal Uddin
**Notes quality:** Gemini-generated; review before relying on specifics.

## Summary

Bug reviews and migration plans were discussed. The team is batching the project page migration tickets for one cutover, and Maria proposed removing the main Flightboard experiment flag.

## Flightboard bug fixes and messages

- Work order approval address bug (OBS-580): Andrew validates the fix. Maria agreed to close the sorting ticket (OBS-571, sorting across tabs) once Andrew validates. The address bug has low occurrence because it only hits remote work orders.
- Sorting across tabs (OBS-571) verified fixed on QA1 and production.
- Side drawer message counts: admin label removed and totals match after suppressing closing notes.
- Message counts lag up to one hour. Messages do not flow through the Kafka real-time eventing path. Andrew hopes the incoming team modernizes eventing.

## Project page migration

- Final migration ticket is OBS-539 (project template listing page DataGrid migration), expected around Friday 2026-10-16.
- Decision: batch all project page migration tickets and test them together once the final ticket merges.
- Decision: direct cutover via a single PR that replaces routes. No fallback button, since the page is lower traffic.
- Andrew will click-test on a new route before cutover.

## Spectrotel custom field bug

- Molly reported disappearing column headers and no results when searching by purchase order through a custom field on the new Flightboard. Andrew could not reproduce it. Maria asked Molly for a screen recording or loom. Andrew is assigned to investigate alongside Travis Lang's MUI issue. Tracked as OBS-584 in Slack (Linear Asks bot, 10-12).

## Private feedback column

- Approved work orders show the private feedback window as closed on the new Flightboard. Laura noted the column predates the grid migration. Andrew will push back on the ticket and suggest the team closest to the feature own it.
- Maria reviewed backlog items from Laura: universal bookmarks, horizontal scrolling (known issue), and time zone column selection.

## PSS and pending request/assignment columns on project Flightboard

- Code originally blocked the pending request and assignment columns from project Flightboard at initial release. Andrew confirmed work is underway to remove the block after backend work completes.
- Todd and Laura will meet with Mari about introducing these updates to users.
- Slack context: Diamond's PSS column was intentionally scoped to main Flightboard. Andrew decided to expand it to project Flightboard. Ivan's question led to a new backend ticket, OBS-583 (Miroslav), with OBS-578 as the frontend-side context.

## Main Flightboard experiment removal

- Maria proposed removing the main Flightboard experiment around Tuesday or Wednesday (10-13 or 10-14) to reduce loading delays.
- Decision depends on Datadog error spikes or new bug reports from Andrew. Fallback is to fix issues as they arise.

## Next steps

- Andrew: validate the address requirement fix on approved work orders.
- Andrew: root-cause the private feedback column status for approved work orders.
- Andrew: investigate the disappearing column headers report (OBS-584).
- Maria: disable the main Flightboard experiment flag.

## Related Slack context (10-12, not in the meeting)

- Karol Tluscik joined #proj-ui-modernization. Andrew says he is joining this quarter for extra bandwidth.
- Miroslav deployed OBS-548 (service company admin messages in the messages side drawer). He also opened OBS-583.
- Andriy shared the list of PRs that remove logo cache suffixes. They merge only after Nov 7 (OBS-576).
- Andriy is working on OBS-539.
- Andrew left #flightboard-enhancements this morning.
