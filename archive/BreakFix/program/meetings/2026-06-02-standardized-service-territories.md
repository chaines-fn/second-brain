# Platform Arch: Standardized Service Territories — 2026-06-02

**Date & Time:** Monday, June 2, 2026, 9:31 AM CDT (~43 minutes)

**Attendees:**
- **Lynsay Faucett** — PM (meeting organizer; drives service territory work)
- **Chuck Haines** — Sr. Principal Engineer (architecture)
- **John Shaw** — Group PM *(in the Baker Center / "Venice 837" conference room — the primary unattributed "Venice 837" speaker, along with Ryan)*
- **Ryan Schulte** — PM, SLATE *(also in the Baker Center room; gave the zip-code feedback in the solution doc)*
- **Amran Hossain** — Engineering Manager (BD teams)
- **Ridi Haque** — Engineering Manager (GARNET + MARBLE)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1l8zRDLKweoKHUTmg_kfyQYsX4pFIQ9IEDUCNjLFiOt0/edit)

---

## Overview

This was a Q3-planning conversation that pivoted from the originally scheduled clients/projects-hierarchy topic to standardized service territories, which Lynsay flagged as the current P1. The trigger was Ryan's feedback in the solution doc questioning the reliance on zip codes. The group spent most of the time aligning on what a "service territory" actually *is* and whether to build the near-term solution on zip codes or invest up front in a more modern geospatial approach (H3 / ISO drive-time lines via AWS Location Services).

The key alignment was definitional: a service territory is a **geographic boundary**, distinct from the list of providers associated with it. Today technicians are directly attributed to territories, but the architecture should treat the geographic construct and provider membership as separate things — which Chuck noted will likely require unwinding some of what's already been built. Lynsay validated four jobs-to-be-done (recruiting, dispatching, reporting, coverage analysis), and Chuck pushed that the geographic definition may need to vary by job (strict boundaries for reporting vs. expanding radii for recruiting/dispatching). The team also scoped FieldNation-defined defaults (the Q3 target) separately from account-level territories (a ~1-year-out body of work involving sharing/cascading territories across projects, like talent pools).

On the build approach, Chuck argued that doing it "the right way" (H3/ISO lines) isn't materially more work than a zip-code system — ~80-85% of the infrastructure (APIs, storage, service) is needed either way, with the main delta being rework of zip-based work-order auto-association and a likely move to Postgres (MySQL doesn't support H3). No final decision was made; the team will run technical spikes to size the complexity difference. Chuck also used the meeting to advocate a broader resourcing change: team leads should cap sprint-delivery work at ~30% so they can do the look-ahead/architecture work the program keeps falling behind on.

---

## Key Notes

- **Definition aligned: service territory = geographic boundary, not a provider list.** The group agreed a service territory is a geographic concept with attributes, and that provider membership is a *separate* construct (buyer- or project-owned). Chuck stressed this distinction materially changes the architecture and noted some already-built work (where technicians are directly attributed to territories) will likely need to be unwound.

- **Scope split: FieldNation defaults (Q3) vs. account-level territories (~1 yr out).** Q3 target is FieldNation-defined default territories *in the platform*. Account-level territories — where a company defines/modifies its own territories, shares them across projects, and cascades changes (add a zip at the account level → propagates to 10 projects, like talent pools) — is explicitly a separate, longer-term body of work. There's expected overlap between the two.

- **Four jobs-to-be-done validated:** recruiting (geographic boundary for NetDev sourcing), dispatching (constrain smart dispatch to the right providers/location), reporting (trends by geography), and coverage analysis (preview provider coverage). John noted dispatching will evolve with talent-pool expansion — service territory becomes *one* input to dispatch rather than the sole driver.

- **Geographic definition may vary by job (Chuck).** Reporting may need strict boundaries (only work orders inside the area); recruiting/dispatching may use expanding radii (e.g., 5/10/50/100 mi from a center point, growing outward until a provider threshold is met). Worth designing the model to support more than one notion of "boundary."

- **Zip codes vs. H3/ISO lines — leaning toward building it right.** Chuck's position: the near-term zip approach and the modern approach share ~80-85% of the work (APIs to create defaults, cascade logic, storage service). AWS Location Services can generate the ISO drive-time boundaries; points-in-polygon become essentially the same kind of DB query as a zip equality check. Main deltas: (1) rework of today's zip-based work-order auto-association, and (2) a likely move to **Postgres** since MySQL doesn't natively support H3.

- **No bounded context exists today for FieldNation-owned default territories.** Whatever the approach, new service infrastructure has to be built — it doesn't fit cleanly in the existing dynamic-talent-pools context, especially once defaults exist without providers. Where this lives (possibly its own service) is an open architectural question, made sharper by the likely future need for Postgres/H3.

- **Anchor data: reuse Jeff's existing list.** Jeff already mapped driving-radius centers across the country (e.g., South Minneapolis, St. Paul). AWS hasn't pre-divided the country, so it needs anchor points — the plan is to use Jeff's centers as anchors, with AWS drive-time boundaries possibly yielding more accurate territories than current zip models. Territories could be defined at different gradients for metro vs. rural.

- **Visualization is required regardless of data model.** Buyers think in zip codes / metro areas, not lat-long. A map-based view is needed for buyers to understand and interact with territories (Chuck referenced a Claude-generated mockup; potential to overlay provider-availability heat maps). A zip "translation layer" is a known pattern other companies use, but maintaining it is its own body of work.

- **Resourcing model — Chuck's pitch: cap leads at ~30% delivery.** Chuck strongly advocated that team leads should spend no more than ~30% of bandwidth on sprint delivery, with ~70% on look-ahead/architecture, because over-allocating leads to delivery keeps the program perpetually behind on the hard upstream work. John suggested reserving capacity (~10-15%) during quarterly planning. Amran agreed to work on it starting next quarter, pending engineer feedback.

- **Spike will impact the service map view.** Amran flagged that a 3-5 point spike on service territories will impact current service-map-view work — capacity to be worked out offline (Chuck, Amran, Ridi).

---

## Action Items

- **Mobarok Hossain, Mubarak, Amran, Chuck:** Run technical spikes comparing complexity of a zip-code model vs. ISO-line/H3 mapping for service territories; get a rough magnitude on the delta (2x? 10x? 100x?).
- **John Shaw:** Map out potential requirements for service-territory hierarchies and future cascading-update workflows (parent/child territories, synchronization). *(implied — "someone in Venice 837" committed to drafting requirements; likely John)*
- **Lynsay Faucett:** Update the solution exploration doc to add the discussed use cases and jobs-to-be-done, including parent-child relationships and sync.
- **Amran Hossain:** Revise team-lead bandwidth allocation for next quarter to prioritize look-ahead/architecture over sprint delivery; separate Shafin/Jimu into their own team so SLATE focuses on service territories.
- **Chuck Haines:** Schedule the consultant session on service-territory industry best practices for the week after returning from leave (out ~1 week starting next week).
- **Lynsay Faucett:** Reschedule the client/project-hierarchy architecture conversation (deferred so the team can focus on service territories as P1).
- **Chuck, Amran, Ridi:** Talk offline about spike sizing and its impact on the in-flight service map view work. *(implied)*
- **Leads sync (Thu):** Amran, Ridi, Lynsay to cover this with Mobarok and Bilas — delivery first, then planning. *(implied)*
