# Q2 Scope Refinement — April 2, 2026

**Meeting:** Q2 scope refinement
**Date:** April 2, 2026
**Attendees:** Chuck Haines, Linda Lo, Bilas Sarker (Bilas Sarker), Amran Hossain, Ridi Haque, Brent Grill
**Source:** [Gemini Notes + Transcript](https://docs.google.com/document/d/1eR8RiAdoEKW0pGFYZAl21gbXhKZ_9En5P7MkE8Of47w/edit)

---

## Summary

Working session to cover two active production fires (Unisys rollout exclusion + Smart Dispatch provider bugs) and then dig into Q2 Smart Dispatch scope and the Brent/Linda prototype for provider segmentation/criteria.

---

## Topics

### 1. Unisys Rollout Exclusion

Unisys freaked out about the new provider-side feature rolling out too fast on top of the recent "on my way" change management they just finished. The agreed resolution:

- **Exclude Unisys from the rollout for ~2 weeks** using existing Split.io feature flags (both buyer-side and provider-side flags)
- Corey (Unisys) will provide a list of their talent pools to scope the exclusion
- **No code changes needed** — pure flag config
- ~98% of affected providers are Unisys-only, so a consistent provider experience is maintained during the exclusion window
- Max exclusion cap: ~1 month
- **Chuck to post go-forward plan in GTM channel** after Mubar confirms the solution works

### 2. Active Smart Dispatch Production Issues

Two separate bugs are happening simultaneously:

**Issue 1: Kafka event gap → providers missing from dispatch groups**
- Root cause: Kafka producer/consumer incident last week prevented events from being published
- Result: Some service territories have providers who appear in the ST data but never made it into dispatch automation's "published groups" — so when dispatch triggers, those providers aren't included
- A new Kafka connector monitoring alert has been created to catch this going forward
- Fix: Bilas to also address a related status bug where dispatch shows "completed" but the WO never actually got dispatched (validation error early on, then job re-ran and hit a "no provider found" with no notification)

**Issue 2: FN Core endpoint applying unexpected provider filtering**
- Root cause: Team used an existing FN Core endpoint to get provider info, but it applies filters (including marketplace access flags) that silently exclude providers
- Result: Providers with marketplace access turned off were being filtered out — they couldn't be added to service territories
- **Interim fix:** Ryan is creating a dedicated talent pool for the affected customer with all needed providers included
- **Bypass:** Pass `a_=-1` query param to the sticky filter to bypass the filtering (Bilas to inform Mubar)
- **Longer-term fix:** Build a new "phased API" that returns provider info without applying filtering logic

**Cross-cutting observation (Chuck + Linda):** These bugs exposed a gap — Smart Dispatch looks fine internally but receives "faulty input" from the ST ecosystem (provider syncing, filtering). No cross-team oversight is catching this until something breaks in production. Amran suggested more frequent product-side testing using test buyers (e.g., Dunder Mifflin) before real customers are impacted. Linda proposed a **joint "fit and finish" review** for major Slate/Garnet releases going forward.

### 3. Enable Smart Dispatch V2 for All Eligible Customers

Bilas proposed removing the marketplace expansion feature flag and enabling Smart Dispatch V2 for all customers who have the contract flag enabled. Rationale: V2 has better error visibility and workload detail vs. V1's generic messages, and no customers are actively using V1 at this point.

**Decision: Approved** — Linda agreed, provided all features stay gated under the contract flag.

### 4. Q2 Smart Dispatch Scope (Smart Dispatch V3)

Linda outlined Q2 priorities for Smart Dispatch:

- **Timing profiles + SLA capture** — new tab in dispatch settings; should ship as soon as ready, doesn't need to wait for V3
- **Provider segmentation** — introduce a segments layer: service territory pool, talent pools, marketplace
- **Provider criteria** — configurable hard filters + ranking criteria (background check age, min PSS score, etc.)
- **Preview of eligible pools** — buyers can see who would be dispatched before going live

**Prototype walkthrough (Brent Grill):**
- Unranked state: ST providers first → talent pool providers → marketplace providers
- Ranked state: talent pools can be assigned tiers; two pools can be treated equally at the same tier rank
- Provider criteria: each criterion sets a minimum floor (hard filter); surviving providers are then ranked by the criteria's relative weighting
- Criteria can be scoped to specific segments (e.g., apply only to talent pools + marketplace, not ST)
- "Point tippers" (preferences that rank up without excluding) can be handled via criteria with very low floors

**Two open business logic decisions** (need resolution by ~end of week):

1. **ST vs. talent pool priority:** Should service territories always rank above talent pools if both are active, or can they be treated equally?
2. **Tiering model:** Sequential tiering (Tier 1 exhausted before Tier 2 gets any) vs. the current 80/20 percentage-share distribution? CSMs are used to sequential; Bilas says sequential is not technically complex but may require an API call per tier.

**Distance calculation question (Chuck):** Current dispatch uses crow-flies distance. Chuck floated using **AWS ISO lines** (isochrone polygons based on driving time/distance, accounting for traffic) as a more accurate alternative. Adds scope but Linda agreed it's worth evaluating. Not decided.

**Provider criteria bucketing concern (Chuck + Bilas):** Pure granular ranking (e.g., 1 month > 2 months) is a big technical lift. The plan is to define a fixed set of buckets (e.g., 10) that cover the data set rather than dynamic bucket creation. Linda to build hypothetical bucket lists per criterion so Bilas can evaluate volume/performance implications.

**V2 → V3 migration strategy:**
- Current "Advanced Settings" + "General Settings" → consolidated into a new "Dispatch Settings" tab (maintains V2 support)
- New V3 features (Segments Criteria tab) hidden behind a V3 flag
- All existing V2 marketplace configs (distance, PSS score) will need to be migrated to V3 segment/criteria framework once V3 is stable

### 5. Time Zone Issues in Data Integration

Ridi raised that some data sources (e.g., ServiceNow) aren't sending timestamps in ISO format, causing storage and reporting issues. System is currently storing date/time without timezone. Next step: coordinate with integrations team to ensure ISO format consistently from all sources.

---

## Decisions

| Decision | Details |
|----------|---------|
| Unisys excluded from rollout | ~2 weeks using existing flags; Chuck to communicate in GTM after Mubar confirmation |
| Smart Dispatch V2 enabled for all contracted customers | Remove marketplace expansion flag; Bilas to execute |
| Joint fit-and-finish reviews | For major Slate/Garnet releases going forward |
| SLA capture / timing profiles | Ship independently, don't wait for V3 |
| ISO lines for distance | Evaluate — not yet decided |
| Tiering model + ST/pool priority | Open — need decision by ~end of next week |

---

## Action Items

| Owner | Action |
|-------|--------|
| Chuck | Post Unisys exclusion plan in GTM channel (after Mubar confirmation) |
| Bilas | Inform Mubar about `a_=-1` sticky filter bypass |
| Bilas | Enable V2 / remove marketplace expansion flag for eligible customers |
| Bilas | Assess technical complexity of sequential tiering vs. 80/20 distribution |
| Bilas | Review Linda's bucket lists for performance/maintenance viability |
| Linda | Build hypothetical bucket lists for all provider criteria (using Brent's proposed floors) |
| Linda | Write business requirements doc for SLA capture + timing profiles |
| Linda | Create Linear ticket for SLA capture / timing profiles |
| Linda | Send copy suggestions to Brent (avoid "contractually obligated" language) |
| Linda | Ping Chuck re: timezone issue resolution path |
| Brent | Share prototype file path / link in Slack channel |
| Brent | Fix talent pool display error in prototype |
