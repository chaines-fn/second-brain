# Service Territory Solution Discussion Cont. — 2026-07-09

**Date & Time:** Thursday, July 9, 2026, ~7:30 AM CDT (per Ryan's comment about the booking) — ~36 minutes

**Attendees:**
- Lynsay Faucett — PM, GARNET (meeting organizer)
- Ryan Schulte — PM, SLATE
- Mobarok Hossain — Engineering Lead, SLATE
- Bilas Sarker (Chandra) — Engineering Lead, GARNET
- Alyssa Eby — UX, GARNET
- Ridi Haque — Engineering Manager, GARNET + MARBLE
- Amran Hossain — Engineering Manager, oversees all three team deliveries
- Chuck Haines — Sr. Principal Engineer, cross-cutting architecture

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1mkSjIgJNsuCLRXZYe4TwXGVo-gh82UQcFB3bZqD0tUM/edit)

---

## Overview

This was a working session between SLATE and GARNET to align on the service territory / H3 solution proposal ahead of a same-day stakeholder demo with Jeff, Mia, Jake, and Reswan. Ryan Schulte framed the core proposal: buyers map a site list to standardized default service territories rather than getting in-platform tools to redraw, merge, or slice shapes; custom shapes stay supported only via off-platform creation and import, preserving today's functionality. Chuck reframed the underlying decision test — if territories are still purely zip-code-defined, H3 buys nothing; if Jeff's team will eventually draw boundaries independent of zip lines, H3 is clearly the right storage model even though the initial defaults will be zip-inspired. The group converged on H3 as baseline.

Mobarok raised the open problem of how future territory updates get collected from Jeff's team in an H3-compatible format, since today's definitions are zip/Mapitude-based with no direct conversion path. Lynsay proposed pulling in Reswan (data team), who has prior experience mapping zip-based territory data onto H3, to help close that gap. On driving distance, Chuck and Mobarok discussed AWS isolines as a more accurate and, despite a one-time cost, more efficient alternative to today's per-provider driving-distance API calls. The session closed with Lynsay setting the plan for the stakeholder demo: keep it short, explain H3/isolines at a high level, and leave most of the time for Jeff/Mia/Reswan's questions.

---

## Key Notes

- **Territory customization scope decision:** No in-platform redraw/merge/slice tools for buyers — only default territory selection via site-list upload, plus the existing off-platform custom-shape import path. Ryan plans to scope the advanced customization tools anyway, specifically to get a concrete point estimate he can show Jeff to justify not building it, rather than asserting infeasibility without data.

- **H3 confirmed as baseline geographic model:** Chuck's framing was the deciding factor — the real question is whether territories remain zip-code-defined going forward. If boundaries will eventually be redrawn independent of zip lines, H3 is the right storage model now even though defaults start zip-inspired.

- **Driving distance via AWS isolines:** Chuck noted isolines are map-data based (similar to the Mapitude approach Jeff uses today) and, despite a one-time generation cost, are cheaper and far more efficient than the current approach of querying every candidate provider individually for exact driving distance.

- **Input-data gap for future territory updates:** Mobarok flagged that Jeff's team currently defines default territories via zip codes in Mapitude/Excel, with no defined process for capturing future updates in H3-compatible format. Lynsay proposed looping in Reswan, who has prior experience converting zip-based territory data to H3.

- **Whitespace/gap coverage flagged as unresolved:** Ryan noted that moving to isoline/H3-derived shapes requires an explicit process for filling gaps between adjacent territories (e.g., inlets) so there's no unclaimed area nationwide — a known hard problem carried over from SLATE's prior deep-dive.

- **Demo framing decided:** Lynsay will keep the same-day stakeholder session short — explain H3 and isolines at a high level using Mobarok's example visualization, then leave the second half for Q&A from Jeff, Mia, and Reswan rather than exhaustive technical detail.

- **Scheduling friction:** A calendar mixup caused an inconveniently early call time for several attendees. Chuck acknowledged ambiguity in his calendar (which events are "agent-only" vs. require his attendance) and committed to clarifying it.

---

## Decisions

**Aligned:**
- Platform customization strategy: restrict in-platform UI customization for service territories; rely on off-platform creation + import.
- Scope advanced customization features (merge/redraw/slice) to quantify engineering cost before finalizing the exclusion decision.
- H3 confirmed as the geographic storage model for service territories, replacing zip-code-only boundaries.

**Needs Further Discussion:**
- Process for receiving/updating standardized service territory data from Jeff's team in an H3-compatible format — unresolved; Reswan's help to be sought.

---

## Action Items

- **Ryan Schulte:** Scope cost/feasibility of extension-based customization tools (merge, redraw, slice territories).
- **Lynsay Faucett:** Prepare same-day demo materials for the session with Jeff, Mia, Jake, and Reswan, focused on explaining H3 and isolines at a high level.
- **Mobarok Hossain:** Consult Reswan on handling input-data challenges for converting Jeff's zip-based default territories into H3 cells.
- **Ryan Schulte:** Trial-run converting zip code shapes into H3 hex cells with current tooling.
- **Chuck Haines:** Update calendar settings to distinguish "agent-only" events from ones requiring personal attendance.
