# Technical Design Review - In-Platform Service Territory (SLATE) - Jul 31, 2026

**Organizer:** Mike Mueller · 7:00 AM CDT
**Presenter:** Mobarok Hossain
**Speakers:** Mobarok Hossain, Mike Mueller, Kimberly Zito, Brian Chapeau, Bilas Sarker, Ashraful Fuad, Safin Ahmed, Amran Hossain, Masud Alam
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1cS-4kRsCFNwAQ6T6C_eFInHW-tfchNoO8AVn6yjV5J0/edit)

> Break/Fix scoped. Chuck's first day back from vacation; not recorded as attending. **This is the highest-value item of the sync for Chuck's architecture role.**

## Outcome: TDR declared INCOMPLETE

Mike Mueller blocked on **one item - reporting**. His challenge: "I know we're going to be asked how many work orders were completed in this service territory in the last year." Because territory boundaries change over time, the design must be able to answer that question against *today's* territory definition (territory 123 as defined now, looking back 1–2 years). No solve is present in the doc.

## New org-wide standard (biggest carry-forward)

**Mike has added a Reporting section to the TDR template.** Every team must now fill it out and work with their PM/product rep on reporting needs upfront. His mandate, quoted: *"we have to stop relying on the data team to solve the problems through unique solutions... we need to be building the data and the feature so that it is possible to report on it."* He was explicit that this is an organizational change, not a knock on Mobarok's design.

Amran added that the TDR should also name which geospatial tables / reusable data products land in the data platform. Interim answer: this quarter targets existing DW tables; longer term served from the data platform.

## Decisions

| Decision | Detail |
|----------|--------|
| **Postal-code fallback on geocoding failure rejected** | Mobarok's design auto-fell back to postal code when an address failed geocoding. Mike: if a buyer supplies an address and it's bad, **reject the record** so the human in the loop corrects it. Postal-code fallback is retained **only when no address is supplied at all**. Mobarok to implement. |
| Territories are **polygon-based** (grouped postal codes), not radius circles | Lat/long retained for center-point/radius expansion. Carries market class (rural/suburban/metro). Uber S3 cells mentioned as an alternative mapping mechanism. ~80–90% of the source spreadsheet columns used; `name2`, `ID`, `RPO` excluded. |
| **Customized copies are point-in-time snapshots and never re-sync** | Once a buyer copies and customizes a default territory it stops receiving updates. Schema adds a default-service-territory ID column plus a `customized` flag on the talent pool group / service territory table. Uncustomized copies may still receive updates. |
| Default list is **static, republished by re-running the CLI script** | No partial updates. New list issued by the internal team triggers a full re-run. |
| Geocoding via **AWS Location Service**, reusing the geospatial service NetDev recruiting already uses | Cache: 1-hour LRU → 24-hour Redis. Safin noted the 24-hour TTL traces back to **Chuck's earlier geospatial TDR** (AWS updates distance data frequently). Mobarok to add AWS cost tracking and revisit whether the TTL can be lengthened. |
| API prefix **v2**, matching existing project and service territory endpoints | |

## Risks Raised

- Brian Chapeau flagged staleness risk in the address database for new site buildouts. Mike's answer: a managed AWS service should stay current, with the no-address/postal-code path as the escape hatch.

## Process Note

Mike handed MC duties for the TDR meeting to the presenting teams - "you guys own this meeting, no longer need to rely on me for MC."
