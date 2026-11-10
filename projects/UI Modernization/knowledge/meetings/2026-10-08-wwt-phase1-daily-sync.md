# WWT Phase 1 Daily Sync - 2026-10-08

**Date & Time:** Thursday, October 8, 2026, 8:45-9:00 CDT (scheduled; Gemini notes generated 08:43 CDT)

**Attendees:**
- Maria Saxton - WWT Phase 1 project lead, meeting organizer
- Chuck Haines - Sr. Principal Software Engineer
- Andrew Kandels - Engineering lead
- Laura Quist (Douglas) - Sr. Customer Success Executive, WWT voice
- Todd Folie - WWT-facing QA/customer contact (reports production bugs)
- Yurii Dmytriiev - Contractor, Frontend Engineer
- Ivan Sakhman - Contractor, Frontend Engineer
- Andriy Kurenda - Contractor, Frontend Engineer
- Fayshal Uddin - Engineer, reviews/releases core-ui changes

**Original Transcript:** [Gemini notes](https://docs.google.com/document/d/1OWYo8faGh7FCgGL8QpHjzLsQi-mBkxzqCxkPdWce1BI/edit?usp=drivesdk&ouid=104527139387599317887)

Note: this summary is based on the Gemini-generated notes in the document. The document did not include a full raw transcript, so details should be checked against the source before they are relied on externally.

---

## Overview

The team reviewed the first full day after the Flightboard rollout reached all remaining buyers. Andrew submitted the final rollout approval at 7:00 CDT this morning, moving all remaining users, including opted-out and high-risk buyers, to the new Flightboard. Chuck and Maria agreed to keep the feature flag active for now so they can watch power-user feedback before the legacy Flightboard and its redirect logic are removed. The brand refresh was deployed yesterday, and a few logos and favicons were missed and need cache busting.

Customer Success reported issues in three areas: a mobile accessibility bug, a data sorting problem, and a dashboard layout that a buyer described as drastically rearranged. Chuck shared Andrew's theory that the buyer may have accidentally applied a reset to company default. Maria noted the ticket description was vague and needs more detail. Laura also reported a new bug: mass approving remote work orders on the new Flightboard incorrectly prompts for a company address for tax purposes. Laura acknowledged that mass-approval testing was not done before release. The group agreed that incoming customer bug fixes take priority over the project page migration work.

Yurii presented the mobile redesign: an adapted projects page with a column grid, swipeable tabs, a search bar, a separate filter row, and a three-dot action menu. Todd suggested flipping the menu vertically and placing it to the right of the edit button to save space, and Yurii agreed. Yurii plans to push the mobile changes today.

---

## Key Notes

- **Flightboard rollout status:** Andrew submitted the final rollout approval at 7:00 CDT on 2026-10-08 to move all remaining users, including opted-out and high-risk buyers, to the new Flightboard. Chuck and Maria agreed to keep the feature flag in place temporarily to monitor power-user feedback. The legacy Flightboard and its redirect logic will be removed after all users have transitioned. The team also noted that board loading delays stemming from the flag-driven redirect will be resolved once that legacy logic is removed.

- **Brand refresh:** Deployed on 2026-10-07 per the notes. Some logos and favicons were missed and need cache busting. Andriy submitted changes, and Maria is testing them along with favicon updates across tabs.

- **Dashboard layout complaint:** Customer Success reported a dashboard layout that changed drastically. Chuck shared Andrew's theory that a user may have applied a reset to company default. Maria noted that the ticket is vague and needs reproduction steps. Maria owns the follow-up with CS. This is the same report discussed in the UI Modernization Sync earlier this morning.

- **Mass approval bug on remote work orders:** Laura reported that mass approving remote work orders on the new Flightboard prompts users to enter a company address for tax purposes. Maria and Todd requested reproduction steps, a Loom recording, or a specific work order ID to investigate the UI logic. Laura is responsible for submitting the report.

- **Prioritization decision:** Maria decided that incoming customer support bugs take priority over the ongoing project page migration work. The team agreed to prioritize incoming bug fixes ahead of the project page migration. Maria and Andrew will mark reported bugs as urgent and prioritize them accordingly.

- **Mobile redesign:** Yurii presented the adapted projects page and mobile navigation. Todd's suggestion to flip the ellipsis and move it beside the edit button was accepted. Todd gave positive feedback, and Yurii will push the changes today. Mobile accessibility was flagged by Customer Success as an issue.

- **Legacy Flightboard removal:** Planned after confirmation that all users have moved to the new version. Still gated on the feature-flag monitoring period.

---

## Action Items

- **Maria Saxton:** Investigate the dashboard layout bug by gathering more information and reproduction steps from the CS team.
- **Laura Quist:** Submit the bug report for the remote work order mass-approval issue, with reproduction steps, a Loom recording, or a work order ID.
- **Yurii Dmytriiev:** Push the mobile UI changes to fix action button placement and navigation structure.
- **Maria Saxton and Andrew Kandels:** Mark reported bugs as urgent and prioritize them ahead of the project page migration.
- **The group:** Remove the legacy Flightboard after confirming all users have transitioned to the new version (not yet scheduled; gated on monitoring of the feature flag).
