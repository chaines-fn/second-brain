# Obsidian Daily Sync + Stakeholders (WWT Phase 1 Daily), 2026-11-05

**Date & Time:** Thursday, November 5, 2026, 8:45 CST (~1 hour calendar slot, meeting ended earlier per notes)

**Attendees:**
- Maria Saxton, Linear project lead, WWT Phase 1 (calendar organizer)
- Chuck Haines, Sr. Principal Software Engineer
- Andrew Kandels, Engineering lead
- Laura Quist (Douglas), Sr. Customer Success Executive
- Todd Folie, QA / production testing for WWT
- Yurii Dmytriiev, Contractor, Frontend Engineer
- Ivan Sakhman, Contractor, Frontend Engineer
- Andriy Kurenda, Contractor, Frontend Engineer
- Karol Tluscik, Joining full-time this quarter (role to confirm)
- Attendee names in the Gemini notes are partly unreadable ("someone in Kuala Lumpur 840 (Baker Center, 8)"). Likely Maria or Chuck; the transcript attribution is unverified.

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1o90ZBWSElme-akCIC3v3G3SzSllJVA31C-u5-o4AYEc/edit?usp=drivesdk&ouid=104527139387599317887) (Gemini notes only; no transcript tab was included)

---

## Overview

The daily covered ticket closures, the project page rollout, and a set of new requests from WWT. The main product question was whether the PSS (provider success score) column on the project Flightboard had to ship before the projects page could roll out. The team agreed it does not: the column is a fast follow, since the ticket already addresses making the board more efficient. The team is still working out a bulk PSS route with Fayshal, which ties to the same efficiency work discussed in the UI sync.

Laura and Todd raised two customer-driven gaps on the project Flightboard: relative date filters (such as created in the last two weeks or last month) exist on the main Flightboard but not the project board, and open-text search should search within the active tab instead of jumping to the All tab. Both were accepted into the backlog as documented tickets. The team also agreed that the Match page rollout needs a feature flag and a rollback path, given its traffic. Andrew suggested refactoring MUI navigation so provider-facing pages can share it ahead of upcoming pages that serve buyers and providers.

---

## Key Notes

- **Project Flightboard PSS column is not a release blocker:** Andrew and Todd confirmed the PSS column addition is a fast follow for the projects page. The efficiency ticket already covers the load-cost concern.

- **Bulk PSS route coordination:** Andrew had not yet synced with Fayshal. Miroslav will reach out to find an existing route or add one, possibly through an elastic list. This matches the UI sync action item (OBS-638). Status: open.

- **Project management ticket:** Andrew will merge the project management ticket and retest it. There was a question about whether it should be one Core UI integration or the existing UI implementation; Andrew will confer with Chuck.

- **Team capacity and Match enablement:** Andrew said the team has spare capacity while catching up on Match tasks and proposed pulling in items from the RUM and enablement backlogs. Someone argued against starting the Harness migration now, since pausing and restarting it would be disruptive. The team agreed Match needs a feature flag with rollout and rollback because of high traffic. The Match page rollout must follow the main Flightboard pattern. Andrew will ask the Diamond or Quartz teams whether they want to validate Match.

- **Relative date filters on project Flightboard (Todd, Andrew):** The project board lacks relative date options in the filter dialog that the main Flightboard has. The team agreed it fits current capacity; a backlog ticket will be documented.

- **Open-text search should stay in the active tab (Laura):** Searching currently redirects users to the All tab, which conflicts with user expectations and CS feedback. Searching within the active tab matches the main Flightboard pattern. A backlog ticket will be documented.

- **Deferred "last routed" ticket (OBS-152):** Someone considered picking it up, but Andrew noted it needs backend work and eventing from Miroslav, who is already on two other backend tickets. Andrew proposed MUI navigation refactoring for provider-facing pages instead. The team agreed getting ahead of that requirement is worth the documentation effort.

---

## Action Items

- **Andrew Kandels:** Merge the project management ticket and retest the implementation.
- **Andrew Kandels:** Ask Diamond or Quartz team members whether they want to validate the Match page rollout.
- **Chuck Haines (implied):** Follow up on the AI factory (Warp) status with the team.
- **Chuck Haines or Maria Saxton (implied, unverified attendee):** Discuss Match enablement with Ashley.
- **Chuck Haines or Maria Saxton (implied, unverified attendee):** Document the relative date filter requirement for the project Flightboard and add it to the backlog.
- **Chuck Haines or Maria Saxton (implied, unverified attendee):** Document the request to scope open-text search to the active tab and add it to the backlog.
- **Miroslav Isikiyski (implied):** Coordinate with Fayshal on the bulk PSS route (OBS-638).
