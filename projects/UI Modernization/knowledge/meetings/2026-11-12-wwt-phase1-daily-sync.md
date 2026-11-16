# Obsidian Daily Sync + Stakeholders (WWT Phase 1 Daily): 2026-11-12

**Date & Time:** Thursday, November 12, 2026, 8:45 CST (~15 minutes scheduled)

**Attendees:**
- Maria Saxton: WWT Phase 1 lead, organizer
- Chuck Haines: Sr. Principal Software Engineer
- Andrew Kandels: Engineering lead
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Karol Tluscik: Frontend Engineer
- Todd Folie: QA / WWT testing
- Laura Quist (Douglas): Sr. Customer Success Executive, WWT voice

**Original Transcript:** [Notes by Gemini](https://docs.google.com/document/d/1H57IBFziajiW6QBffe10kuFuaVSOfgCEYzGOpP02mjE/edit?usp=drivesdk&ouid=104527139387599317887) (notes only, no transcript attached)

---

## Overview

This daily clarified where the new project page work orders filter belongs and tracked two project Flightboard fixes. Maria confirmed the filter added in a previous ticket must sit on the work orders tab under the project page, not on the projects page. Karol resolved a local host issue with Chuck and will test the filter today and tomorrow morning, with a PR expected tomorrow.

Todd found a malformed filter label on the project Flightboard: the raw key start_time_quick appears in place of a proper label. Yurii will find the commit that introduced it, and Maria will open a placeholder ticket assigned to Yurii. On the global bookmarks work, Andrew confirmed that the backend highlight_pending_increases flag covers both expense and traditional spend limit increases. Andrew also said next Tuesday's rollout is not affected by the ongoing reduction incident remediation and may help by removing classic pages. Maria moved refinement to next Monday to discuss more classic pages.

---

## Key Notes

- **Project page work orders filter placement:** The filter belongs on the work orders tab under the project page, not the projects page. Karol tests today and tomorrow morning, then submits a PR.
- **start_time_quick label on project Flightboard:** Todd found a raw key showing instead of a formatted label. Yurii suggested removing it and will identify the offending commit. Placeholder ticket to be created by Maria.
- **Pending increases filter for global bookmarks:** Andriy asked whether the highlight_pending_increases backend flag includes expenses and traditional spend limit increases. Andrew confirmed it does.
- **Next Tuesday rollout:** Andrew said the planned rollout is not impacted by the reduction incident remediation. The rollout will also remove classic pages (per Andrew). Date is presumably next Tuesday (about 2026-11-17); Gemini did not state the date.
- **Refinement moved:** Maria moved refinement to next Monday to discuss more classic page work.

---

## Action Items

- **Karol Tluscik:** Submit the PR for the work orders tab filter under the project page after testing tomorrow morning.
- **Maria Saxton:** Create a placeholder ticket for the start_time_quick label issue on project Flightboard and assign it to Yurii Dmytriiev.
- **Yurii Dmytriiev:** Identify the commit that caused the start_time_quick label issue.
