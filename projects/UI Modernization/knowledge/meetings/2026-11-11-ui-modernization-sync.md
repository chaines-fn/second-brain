# UI Modernization Sync: 2026-11-11

**Date & Time:** Wednesday, November 11, 2026, 7:45 CST (~12 minutes)

**Attendees:**
- Andrew Kandels: Engineering lead, organizer, day-to-day technical lead
- Chuck Haines: Sr. Principal Software Engineer
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Miroslav Isikiyski: Backend Engineer
- Invited, no speaking turns recorded: Laura Quist (Sr. Customer Success Executive), Maria Saxton (WWT Phase 1 lead), Fayshal Uddin, Karol Tluscik

**Original Transcript:** [Notes by Gemini](https://docs.google.com/document/d/1WPc5-5BvbCb1TGQdMCH-2eqy0JRRAcxBtf62JHo8Bek/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

This short sync was mostly a status round, with one substantive topic: Andrew presented a draft two-track plan for modernizing the remaining classic pages (old PHP and Angular pages). The plan ranks pages by vulnerability, by how non-contract (open to any buyer, including new sign-ups) they are, by AI-assisted effort, and by velocity. Andrew explained that many of these pages mix UI and backend in one PHP file, so each page must first be refactored into standardized APIs before it can be lifted into Core UI. Andrew estimated the full scope could run to the end of the year if adopted in full. Dead features, such as a military background check, are candidates for removal pending product experience sign-off.

Chuck and Andrew agreed the team should run this mass-lift work through the Warp factory as part of the proof-of-concept agreement, with the local harness as a fallback if the factory fails or has a security incident. Chuck also raised that some legacy pages may be replaced by Omni work, so the plan needs to avoid redundant rework. Andrew will finish iterating on the plan and send it to Chuck for review today. Integration end tests on main were discussed as well: the critical fixes are in, three of Andrew's PRs passed on main the previous night, and flaky failures and test contention remain.

---

## Key Notes

- **Two-track classic page modernization plan:** Andrew's plan prioritizes the most vulnerable, least contract-gated pages first. Andrew noted that one legacy page is ranked first mostly because of its security holes. Chuck questioned that ranking because he believes a replacement is planned, and suggested the team may "bite the bullet" on it for a while longer. (Gemini transcript is unclear on the page's name; it appears as "war" and may need confirmation.)
- **Legacy PHP pages need API refactoring before the UI lift:** Each PHP page combines UI, backend, and form posting in one file. The work is to move backend logic into standardized APIs (using existing agent harness instructions) and then lift the UI into Core UI. Andrew called the API step the blocker for the UI lift.
- **Dead feature removal:** Andrew proposed asking product experience (PX) for sign-off to remove features nobody actively uses, such as the military background check. Removal would shorten the work.
- **Scope and timing:** If the full plan were adopted, the work would extend to the end of the year. Andrew expects the team will do one variation, not all of it.
- **Factory use for mass lift:** Andrew said the tickets can go straight into a sprint with minimal upfront spec, using standard Core UI tests. Chuck said the factory is a good fit for this volume and that the vendor expects heavy use under the proof-of-concept agreement. Andrew was concerned about burning about a month of six engineers' time through the factory. Chuck said the local harness is a valid fallback and that failures in the factory are possible.
- **Data platform and legacy page removal:** Chuck said he will talk to Mike about the data platform's stated intention to remove legacy pages, because he is not sure where that stands.
- **Launch priorities:** Andrew said the project page launch and the Match page (nearly done) should not be paused for the modernization work.
- **Integration end tests on main:** Critical fixes are in and Yurii has fixed most frontend failures, with a small set left. Three of Andrew's PRs passed on main the previous night. One flaky failure was fixed by Ivan. Heavy test usage across the team causes contention.
- **Sprint progress:** Miroslav expected to finish the slideboard efficiency work after syncing with FO and reviewed ticket 706 (a provider-only filter problem). Andriy started global bookmarks for the project board, requested by Laura, with hanging core-ui PRs ready for review. Ivan found a decimal amount issue in an approval model DPR and created a story for it.

---

## Action Items

- **Andrew Kandels:** Review the hanging core-ui PRs that Andriy flagged so they can merge.
- **Andrew Kandels:** Finalize the two-track plan and capacity details and send it to Chuck for review today.
- **Ivan Sakhman:** Fix the decimal amount issue in the approval model DPR by working the story he created.
- **Chuck Haines:** Talk to Mike about the data platform's intention to remove legacy pages.
- **Andrew Kandels (implied):** Keep iterating on the modernization plan and share it more broadly. Andrew intends to bring in Tashfiqul to help.
