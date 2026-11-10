# Break/Fix Eng Leads Sync — 2026-08-04

**Date & Time:** Tuesday, August 4, 2026, ~7:00 AM CDT (~40 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Engineer (organizer)
- Amran Hossain — Engineering Manager (BD teams)
- Bilas Chandra (Sarker) — Engineering Lead, GARNET
- Mobarok Hossain — Engineering Lead, SLATE
- Ridi Haque — Engineering Manager, GARNET + MARBLE

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1e1-4UDInylQWtMFNiN6zeP4MoJThnTrZE9mvXZs-yH4/edit)

---

## Overview

Most of the working agenda was Mobarok walking Chuck through feedback on the default service territories TD, specifically a late-breaking ask from Mike Mueller to formalize reporting requirements before implementation — the same mandatory-reporting-section push already tracked in the Aug 3 sync. The two worked through what "historical reporting" actually requires: work order-to-territory association is easy since it's stored at creation time, but provider-to-territory reporting is hard because provider location is only ever stored as current state, not historical. Chuck pushed hard for adopting Slowly Changing Dimensions on the service territory definitions themselves (separate from the provider-location problem) purely for audit purposes, even though product's stated requirement is current-data-only — Mobarok agreed to incorporate it.

Bilas raised a second technical thread on Lindsay's network-expansion proposal (locating providers by completed-work-order history and driving distance rather than home address); Chuck suggested a simpler alternative — let providers directly opt into service territories — and asked Bilas to think through both. Ridi Haque was present but didn't speak substantively in the recorded portion.

The last stretch covered operational housekeeping (Amran pushing for agendas shared a day ahead, Chuck reiterating his async-first, Slack-over-meetings preference) and status items: the provider flight board migration is still paused pending Andrew (out this week), and Bilas is planning query optimization on the "Completed" tab for performance issues affecting high-volume providers and large service companies.

---

## Key Notes

- **SCD adopted for service territory definitions, for auditing:** Chuck argued that even though product (via Lindsay/Elisa) only asked for current-state reporting, service territory boundaries will shift over time, and without historical tracking there's no way to say what territory a work order or provider fell into at a given point in the past. Mobarok agreed this is low-additional-cost from an engineering standpoint and will incorporate SCD into the default service territories TD, explicitly ruling out full versioning (Lindsay had already said that's not needed).

- **Historical provider reporting is a separate, harder gap:** work order-to-territory association is straightforward because the territory is stored on the work order at creation time regardless of later boundary changes. Provider-to-territory historical reporting is not solvable the same way today — provider location is only synced as current data, with no change-tracking on address updates. Both agreed this is effectively a distinct, unscoped body of work; for now the plan is track-forward-only (if product later wants historical provider data, it'll only be available from whenever tracking starts, not retroactively).

- **H3 resolution 8 confirmed as the mechanism for reporting columns:** Mobarok's plan is a new H3-resolution-8 column in `dw.work_order` and in the geospatial ext table for providers, alongside the resolution 5/6 columns already used for other BI purposes.

- **Mike Mueller wants the data product / "landing zone" platform question settled before implementation:** per Mobarok, Mike's position is to define this on the reusable data platform's landing-zone Redshift cluster first, then sync outward, rather than building the H3 columns directly. Not yet resolved — Mike is out this week, so Mobarok will take it to the product team (Lindsay/Elisa) rather than Mike directly.

- **Network expansion counter-proposal on the table:** Bilas relayed Lindsay's idea to derive provider location from completed-work-order history plus driving-distance expansion (rather than home address) for network expansion. Bilas flagged an unresolved question (how to pick a center point for a provider with many disparate completed-work locations) and proposed, for phase one, expanding purely by driving distance from a work order's service territory center point — accepting that this may pull in providers technically outside that territory. Chuck's counter-suggestion: skip the computation entirely and let providers self-select which service territories they're willing to work in. No decision — Chuck to think it over and give Bilas an answer separately; Bilas to refine the proposal either way.

- **Provider flight board migration still on hold:** paused on unspecified complications Andrew flagged previously; he's out this week so no update until he's back. Current focus remains the buyer-facing regular flight board rollout, with "rest of projects" and then work order details next in the migration queue. Bilas confirmed his planned backend query optimizations (on the "Completed" and "Available" tabs, addressing slow pagination for large service companies/high-volume providers) are independent of the front-end migration question since the API surface isn't changing.

- **Reinforced preference for async-first communication:** Amran proposed sharing agenda items a day ahead of meetings (citing a pattern used on another team); Chuck agreed but added his broader preference is to skip waiting for meetings altogether — tag him in Slack as soon as a question comes up, expect a response within a day or so, and follow up if he goes quiet.

- **Scheduling note:** Chuck will be traveling to Minneapolis for field week next week and expects to be in the air during this meeting's normal slot — floated moving it later in the week (e.g., Wednesday) for that one occurrence.

---

## Action Items

- **Mobarok Hossain:** Address Mike and Kimberly's TD feedback on the default service territories design.
- **Mobarok Hossain:** Take the reporting requirements/trade-offs discussion to the product team (Lindsay/Elisa) since Mike is out this week, and clearly communicate the current-data-only vs. historical trade-off.
- **Mobarok Hossain:** Incorporate SCD into the default service territories TD.
- **Chuck Haines:** Give Bilas a decision on the driving-distance network expansion proposal.
- **Chuck Haines:** Follow up with Andrew on provider flight board migration status/complications once he's back.
- **Bilas Chandra (Sarker) (implied):** Refine the network expansion proposal, including how to handle provider coverage/center-point selection.
- **Chuck Haines:** Research halal-friendly dining options in Minneapolis for the upcoming trip (personal/logistics, not project-related).

---

## Source

Gemini notes + transcript: [Break/Fix Eng Leads Sync — Aug 4, 2026](https://docs.google.com/document/d/1e1-4UDInylQWtMFNiN6zeP4MoJThnTrZE9mvXZs-yH4/edit)
