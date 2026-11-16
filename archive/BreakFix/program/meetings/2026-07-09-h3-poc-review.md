# Standardized Service Territory H3 POC Review — 2026-07-09

**Date & Time:** Thursday, July 9, 2026, ~10:00 AM CDT (~65 minutes)

**Attendees:**
- Lynsay Faucett — PM, GARNET (organizer)
- Jeff Novotny — NetDev internal customer, primary SLATE stakeholder
- Ryan Schulte — PM, SLATE
- John Shaw — Group PM, cross-stream (closed the meeting)
- Mobarok Hossain — Engineering Lead, SLATE (ran the demo)
- Rezwan Islam — data scientist/strategy, H3 experience (no formal team on file)
- Chuck Haines — Sr. Principal Engineer
- Jake Edmond — "data team" per prior meeting notes (**unconfirmed** — likely the speaker mislabeled "Minneapolis 802 (Baker Center, 8)" in the transcript; flagged for confirmation)
- Alyssa Eby — UX, GARNET (invited; no speaking role in the portion reviewed)
- Melanie Payeur — GTM Lead (invited; no speaking role in the portion reviewed)
- Mia Mu — Data Science (invited; no speaking role in the portion reviewed)
- Amran Hossain — Engineering Manager (invited; no speaking role in the portion reviewed)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/18FsFLiadT1oUEvze37wjg9rk95ECV4fsrC9i5M4MFKU/edit)

**Note on source coverage:** Synthesized from Gemini's full quick notes, decisions, next steps, and detailed (timestamped) summary covering the entire meeting, plus the raw transcript verbatim through roughly the first 30 minutes. The transcript's back half (~00:30–01:04) was not read verbatim due to document size — the Key Notes below for that period rely on Gemini's detailed summary rather than direct quotes.

---

## Overview

Lynsay opened the session framing it as SLATE's stakeholder review of moving standardized service territories from pure zip-code definitions toward H3 hexagonal cells plus AWS isolines, with Mobarok demoing a Minneapolis build off Jeff's master territory list. The first half was demo-heavy; the second half opened up to Q&A, which Jeff used to stress-test the model hard.

The core architectural point that resolved most of Jeff's concerns: hex cells are a visualization/territory-management layer only, while isolines (30-minute drive-time shapes from a point) remain the actual dispatch computation, and zip codes remain both the buyer-facing key and the underlying matchmaking layer. Jeff pushed on edge cases — hexes straddling rivers, hexes spanning multiple zip codes or territories, with Manhattan, Long Island, and San Francisco called out as the hardest test cases — and the group converged on a hex-to-zip lookup table as the fix for split-zip confusion, while confirming isolines already prevent bad dispatches even when a hex visually crosses a boundary.

No final call was made on H3 resolution or on whether zip codes get retired long-term. The practical conclusion was that as long as buyers "speak in zip codes," Field Nation needs to keep maintaining a zip library and assigning zips to hex territories for reporting — dual maintenance looks likely for now, though Lynsay pushed back on whether buyers actually understand zip boundaries or just default to them as the current standard. A separate location-standardization initiative (geocoding/validating addresses before H3 conversion) was flagged as a dependency. John closed the meeting on time, punting deeper resolution to Jeff's in-person visit next week.

---

## Key Notes

- **Hex = visualization, isoline = dispatch computation.** Chuck's clarification defused most of Jeff's boundary-crossing worries: hex cells never drive actual provider matchmaking; an isoline from a point does. A hex touching both sides of a river doesn't cause a bad dispatch even though it can look odd on the map.

- **Zip codes stay the computation + buyer-facing key.** Rezwan repeated this multiple times: matchmaking keeps running on zip code data, H3 is additive visualization/color-coding on top. No re-architecture of dispatch logic.

- **Hex-to-zip lookup table proposed for the "split zip" problem.** Jeff's core worry (citing his Comcast/Unicus experience, where buyers define territories by zip) was a zip that spans two hex territories. Rezwan's fix: a static lookup table assigning each zip to one territory, so a buyer query by zip always gets one consistent answer regardless of hex overlap.

- **H7 resolution is a working default, not a locked decision.** Gemini's own "Decisions" section flagged H7 as "Aligned," but the actual conversation only showed a leaning — Jeff ruled out H8/H9 as too granular ("a pain in the butt") but the Next Steps list still names finalizing resolution (including half-mile/quarter-mile options) as open. Don't treat H7 as final.

- **Manhattan, Long Island, and San Francisco are the stated hard cases.** Jeff wants to see H3 demoed there before trusting the model — dense zip/water boundaries and transportation barriers are the likely failure modes for a single global hex grid. Mobarok owns that demo.

- **Location standardization is a prerequisite, separate initiative.** Chuck and John flagged a parallel workstream to geocode/validate buyer-provided addresses via third-party services before H3 conversion, since H3 quality depends on a legitimate coordinate.

- **Flock tracker coordinate format is an open question.** John raised a buyer scenario with lat/long-only asset trackers (no address). Rezwan/Mobarok confirmed H3 is native to lat/long, but John still needs to verify with Flock whether their system actually outputs lat/long vs. some other grid format.

- **No decision on retiring zip codes.** Practical take (Jake, Jeff): as long as buyers think in zip codes (Unicus's 25-mile-radius-to-zip-list conversion cited as industry norm), FN has to keep a zip library and assign zips to hex territories for reporting. Lynsay questioned whether that's a real buyer requirement or just inertia — worth revisiting.

---

## Decisions

**Aligned:**
- Hex cells are visualization/territory-management only; zip codes + AWS isolines remain the computation layer — no change to backend matchmaking logic.
- H7 used as the working default resolution for the visualization layer (not formally locked — see caveat above).

**Needs Further Discussion:**
- Whether to permanently dual-maintain zip codes alongside H3, or work toward an H3-only future.
- Formal comparative write-up of H3 vs. zip code use cases/pros/cons, ahead of next week's deep dive with Jeff.
- Final H3 resolution size.

---

## Action Items

- **Mobarok Hossain:** Demo H3 visualizations for Manhattan and Long Island specifically.
- **The group:** Finalize H3 resolution size — still open despite the H7 lean.
- **The group:** Stand up a recurring zip code maintenance process so hex-territory assignments don't go stale.
- **John Shaw:** Confirm with Flock whether their tracker coordinate format is lat/long, and whether current H3 tooling supports it.
- **The group:** Write up a formal H3-vs-zip comparative analysis for next week's deep dive with Jeff.
- **The group (implied — Lynsay/Ryan):** Prepare specific questions/use cases ahead of Jeff's in-person visit next week.
