# Smart Dispatch V3 — Technical Design Review — 2026-05-01

**Date & Time:** Friday, May 1, 2026, 7:00 CDT (~64 minutes)

**Attendees:**
- **Bilas Sarker** — Engineering Lead, GARNET (presenter; his email is bilas.chandra@fieldnation.com but he goes by Sarker)
- **Chuck Haines** — Sr. Principal Engineer
- **Mike Mueller** — Principal Engineer
- **Safin Ahmed** — Engineer, GARNET
- **Abu Saleh** — Engineer (likely GARNET — asked detailed questions about logging/storage; not yet in `teams/GARNET/team.md`)
- **Mahmudur Rahman** — Attended; role not on file
- **Julius Iqbal** — Attended; role not on file

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1DmbHOUp75Eaf36r5HKRGUlurehnBK88pHFN8ze5q6gk/edit?usp=meet_tnfm_calendar)

---

## Overview

Bilas walked the group through the technical design for Smart Dispatch V3 — the talent pool + provider criteria expansion. The core shift in V3: the three dispatch options (Service Territory, Talent Pool, Marketplace) become independent — no more required ST prerequisite, and the expanded provider eligibility criteria now apply as a hard filter across all three segments at both ranking and service-motivation time. Chuck framed the business "why": existing customers want to leverage their nationwide talent pools inside SD projects, and break/fix workloads with tight SLAs sometimes need to trade quality for speed.

The bulk of the discussion converged on three areas. First, immutability and real-time sync — the group confirmed SD runs stay immutable for the duration (configuration and provider eligibility captured at run start), with the caveat that real-time provider-mass sync handles talent-pool / ST membership changes mid-run by filtering ineligible providers out at ranking time. Second, the "reporting target" / "priority level" naming and placement — Chuck and Mike pushed back hard: it isn't a reporting target, it's a dispatch override, and bolting priority onto a generic custom field on the work order muddies the public API and conflates SD-only configuration with the work order itself. Bilas agreed to take this back to product. Third, future scope — Mike asked whether project ID still needs to be a hard requirement now that TP and MP can run independently, opening a thread about moving SD configuration up to the buyer level. The group agreed it's a worthwhile follow-up, not in scope for V3.

A retention policy for the SCD type 2 configuration log table is also a confirmed gap — the team needs to define this with product/CX. Chuck noted that two years of granular config history is unlikely to have customer value, and the most recent changes are what support actually needs.

---

## Key Notes

- **V3 makes the three dispatch segments independent — ST is no longer a prerequisite:** In V2, ST was required to publish to the marketplace. V3 lets buyers configure ST, Talent Pools, and Marketplace independently with no prerequisites. Provider eligibility criteria (currently PSS + distance, marketplace only) expand to all three segments and act as a hard filter at both ranking and service-motivation time.

- **Smart Dispatch run settings stay immutable for the run duration — confirmed:** Mike asked whether buyers can change settings mid-run; today they can't, and product hasn't heard a single customer ask for it. Future roadmap may include cancelling an in-progress run (e.g., to manually override and assign a specific provider), but it's not implemented. The group agreed immutability is the right behavior for V3.

- **Real-time sync handles provider/talent-pool removal mid-run — provider-mass is the source of truth at ranking time:** Safin and Abu Saleh probed the edge case where a provider is removed from a talent pool (or the talent pool is deleted) after a dispatch run starts. Bilas confirmed the ranking step relies on the real-time-synced provider-mass dataset, so removed providers fall out of the eligible pool naturally via hard-filter rules. Logging captures provider added/removed events on the dynamic talent pool, plus the assignment log if/when a provider is assigned. Abu Saleh raised the audit scenario: if the provider-mass sync lags (10–30s) and a removed provider gets included, can we explain it after the fact? Answer today is partial — assignment timing and pool-membership timing can be cross-referenced, but it depends on what's already logged.

- **Dispatch hierarchy: ST > TP > MP, segment by segment (not all at once):** Bilas clarified ranking does not work across all requests in one pool. Each segment is processed in priority order. If a provider belongs to all three segments, ST priority wins. Tier rules within TP (e.g., tier 3 ranking) apply within the segment.

- **"Reporting target" vs. "dispatch override" — terminology and placement need rework (Chuck + Mike pushback):** Chuck argued the so-called "reporting target" is functionally a dispatch override, not a reporting concept. They should be separated: one configuration for reporting/SLA expectations, a distinct configuration for dispatch override behavior. Bilas acknowledged the ask and agreed to discuss offline. **Action: Bilas to revisit naming + structure with product.**

- **Priority level should not be a generic custom field on the work order — it's a Smart Dispatch concept:** Chuck challenged the design choice of putting priority level into the work-order custom-fields grab bag, since priority level only has meaning inside SD. Mike reinforced from a public-API consistency angle: customers complain that custom fields don't reveal what they affect; putting SD-specific fields with their related SD configuration improves developer-friendliness. Bilas explained the integration-customer driver — they want the value to come from a template instead of being passed in by integrators — but the group consensus was that the current design conflates concerns. **Needs further discussion with product.**

- **SCD type 2 config-history table needs a retention policy — currently unbounded:** Bilas proposed a view (rather than a new table) over the existing SCD2 configuration tables to surface change history for internal staff audit. Mike and Chuck both flagged that without a retention policy these tables grow indefinitely. Chuck noted that customer-support value past ~6 months is likely zero; max useful window is probably 3 years, certainly no more than 5. Needs alignment with product and CX. The history view is currently staff-only; could go customer-facing in the future.

- **Project ID is still required for SD in V3 — but Mike opened the buyer-level question:** With TP and MP now independent of ST, Mike asked whether SD still needs to be project-scoped. Bilas confirmed V3 still requires a project ID — all configuration is project-level. Chuck and Mike both agreed it's worth exploring buyer-level SD configuration in a future iteration, since it would naturally support cross-project copy-of-settings and treat project ID as just one configuration option rather than a requirement. Open question: are all customers wanting talent-pool SD also using projects? Believed yes today, but data verification is needed.

- **Experiment flag toggled mid-run = errors at next checkpoint:** Bilas explained that turning off the experiment flag during an active dispatch run causes failures at the next flag-check point (e.g., near the 60% ranking window). If the flag flips after the assignment has already happened, the assignment stands.

- **Travel logistics:** Mike (and Chuck, joining via a shorter Kansas → Minneapolis leg) heading to Bangladesh on Monday — 19h flight via Seoul + Bangkok layover, arriving Wednesday. Routing west to avoid Middle East airspace.

---

## Action Items

- **Bilas Sarker:** Discuss configuration separation with product — specifically the "dispatch override" vs. "reporting target" terminology and whether priority level should live in SD configuration instead of as a work-order custom field.
- **Chuck Haines + Bilas Sarker:** Define the data retention policy for the SD configuration log (SCD type 2 tables + history view) with product and CX. Set a max age (Chuck's gut: 3 years, max 5).
- **Bilas Sarker:** Explore moving SD configuration scope from project-level to buyer-level in a future iteration; assess whether this solves existing project-setup pain.
- **Bilas Sarker + Chuck Haines:** Verify whether customers wanting talent-pool SD are all also using projects — believed yes, but needs data confirmation.
- **The group:** Continue reviewing the TD document and adding inline comments / suggestions.
- **Bilas Sarker:** Reply to all comments on the TD with implementation feedback.
