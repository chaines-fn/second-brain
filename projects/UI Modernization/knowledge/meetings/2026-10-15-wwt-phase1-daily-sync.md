# WWT Phase 1 Daily Sync - 2026-10-15

**Date & Time:** Thursday, October 15, 2026, 8:45 CDT (calendar: "Obsidian Daily Sync + Stakeholders")

**Organizer:** Maria Saxton (OOO 10-15 to 10-19; left the slot on the calendar for team use)

**Attendees per Gemini notes:** Andrew Kandels (ran the meeting), Todd Folie, Ivan Sakhman, Andriy Kurenda, Chuck Haines, Karol Tluscik, Yurii Dmytriiev (invited). Maria Saxton and Laura Quist declined.

**Notes Source:** [Gemini notes, Google Doc](https://docs.google.com/document/d/1hYqiEEZLvpn-J4sPmpDH34l00rnjsPq7guruutbn6z0/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Summary

Testing status review with bug updates and ticket assignments. Andrew reported that the project code is complete but the QA environment needs roughly one more day of setup before the team can hand it off for testing. Maria was absent, so Andrew ran the meeting.

## Key notes

- **OBS-584 (disappearing column headers, Spectrotel):** Merged and deployed before Todd's testing. Ivan confirmed the timing, and Todd marked it done. Gemini's notes contain an ambiguous merge date ("two days ago, around the 15th"). Linear shows OBS-584 Done on 2026-10-15 at 13:47 CDT.
- **QA environment for the project page:** Project code is complete. Andrew said at least one more day in QA because of a couple of missed setup items.
- **Stakeholder-review labels:** Ivan added stakeholder-review labels to bug-fix tickets and moved them to the top of the list. Todd agreed to test them.
- **Today filter rollover:** Andrew noted a flightboard filter bug where a saved "today" filter rolls over to the next day. This is OBS-585 (Urgent, In Review).
- **Ticket review assignments for Todd:** Today filter (OBS-585), auto-dispatch filter (OBS-553), and the "contains" operator for the Condition column (OBS-587). Todd will find time to look at them.

## Action items

- **Todd Folie:** Test the stakeholder-review fixes (today filter, auto-dispatch filter, contains filter).
- **Andrew Kandels:** Share the list of items ready for testing in the team channel.
- **Ivan Sakhman:** Keep stakeholder-review labels on the bug-fix tickets.

## Linear status after the meeting (2026-10-15)

- OBS-584 Done (13:47 CDT).
- OBS-583 ([BE] PSS column on main FB to project FB, Miroslav) Done (10:51 CDT).
- OBS-629 (remove virtualization render on main FB, Ivan, Urgent) In Review.
- OBS-633 (custom-field search opens sidedrawer for prior view, Ivan, Urgent, created by Maria 10-14) In Review.
- OBS-634 (inline schedule edit on mobile missing action buttons, Ivan) In Progress. Ivan could not reproduce it in Slack.
