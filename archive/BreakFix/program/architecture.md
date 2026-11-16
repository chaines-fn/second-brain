# Break/Fix Technical Architecture

## Key Services

### fn-core (Monolith)
- Source of truth for work orders, provider data, core platform
- Handles WO creation, publish, assignment, notifications
- Key tables: `ext_workorder_dispatch_run`, `workorder_service_territory_history`
- Smart dispatch triggered via `POST v2/workorders/smart-dispatch`

### dispatch-automation (GARNET — New Microservice)
- GitHub: `fieldnation/dispatch-automation`
- Orchestration engine for smart dispatch
- Manages dispatch runs, publish groups, provider ranking, marketplace expansion
- Stack: Node/TypeScript
- Key tables: `dispatch_run`, `dispatch_run_status_history`, `publish_group`, `publish_group_member`, `project_dispatch_setting`, expansion settings
- Key APIs: project dispatch settings CRUD, work order dispatches, publish group members
- Feature flags: `ENABLED_AUTOMATED_DISPATCH`, `DA_MARKETPLACE_EXPANSION`
- SQS delay queues for expansion timer; separate K8s worker for expansion processing

### dynamic-talent-pools (SLATE — DTP)
- GitHub: `fieldnation/dynamic-talent-pools`
- Manages talent pools, talent pool groups (service territories/coverage groups), provider attributes
- Extended for zip code association: bulk CSV upload, background processing via RabbitMQ
- Service territory matching: Strategy Pattern (zip code now, radius/polygon later)
- Kafka events for talent pool and service territory changes
- Key tables: `talent_pool_groups`, `talent_pool_group_mappings`, `talent_pool_provider_attributes`, `service_territory_postal_code_history`

> **Open architectural gap (Jun 2, 2026):** There is **no bounded context today for FieldNation-owned default service territories.** The existing dynamic-talent-pools context doesn't fit cleanly once defaults exist *without* providers, so new service infrastructure is required regardless of the data model chosen (zip codes vs. H3/ISO). A geospatial (H3) path further implies **Postgres** — MySQL doesn't natively support H3. Where this lives (possibly its own service) is to be determined via Q3 spikes. See `program/meetings/2026-06-02-standardized-service-territories.md`.

### provider-match (Existing)
- Event-driven service matching providers to WOs based on skills, location, attributes
- Extended for ranked-providers API: `POST /work-orders/:id/ranked-providers`
- Stores assignment details consumed from dispatch-automation events
- Uses OpenSearch index

### geospatial (SLATE — New)
- Postal code validation service
- Redis-backed with S3 source data (zip-codes.com)
- `POST /api/v1/postal-codes/validate` — batch validation up to 5,000 codes
- Monthly automated updates via K8s CronJob + Kafka
- **PostGIS/Postgres point calculations** — in-database crow-flies distance (replaced the old manual/intensive MySQL approach); integrated with AWS Location, originally for provider match

> **Proposed (not adopted):** extending this service to power default service territories — anchor + drive-time via AWS Location isolines/route matrix, stored as H3 cell sets and/or PostGIS polygons, with two-signal serviceability. Under exploration, not settled architecture. Full detail + open items: `program/proposals/default-service-territory-geolocation.md`.

## Proposed Model: Talent Pool as Source of Truth (direction aligned June 1, 2026 — not yet built)

Architectural direction aligned in the June 1 talent-pool ↔ service-territory session (Chuck/Ryan/Lynsay/John). **Direction only — build deferred pending the DoorDash friction test and a mockup walkthrough.** See `program/meetings/2026-06-01-talent-pool-service-territory-mapping.md`.

- **Talent pool = single source of truth.** Service territories are re-architected as project-level *containers* that link one or more talent pools, not independent provider stores. A service territory is already nothing more than a group of talent pools under the hood (`talent_pool_groups`); the proposal collapses the two concepts. Pools can be linked at the project level (default) or directly to an individual territory.
- **Territory roster = a dynamic view, not a stored table.** A territory's providers = (linked talent pool) filtered by the territory/dispatch rules, evaluated every dispatch run. Add/remove a provider in the pool → instantly reflected in dispatch and coverage. Reduces membership maintenance to a single talent-pool management problem; this is talent-pool expansion (Q2 work) rolled up into the territory.
- **Dispatch + coverage share identical filter logic.** Coverage analysis must use the exact same rules as smart dispatch or the numbers diverge. Both expand from physical address to also match **work history** (provider regularly worked the area, ~last 6 months) — covers the ~95% white-space case.
- **Non-static distance (white-space dispatch).** Uber-style ever-growing radius via AWS Location `CalculateIsolines` (returns up to 5 drive-time bands per call, e.g. 10/20/60/100 mi) — keep expanding until N providers found in rural areas, stop early in dense ones. Query DB across bands in one pass, sort by distance.
- **Whitelist / ban overrides.** Explicit per-territory override ("provider ID IN (…)") to force in a recruited provider who fails the automated filters (temporary — the work-history filter eventually catches them), and a ban rule to hide a provider. Open lean: whitelisted providers should still belong to *a* talent pool (e.g. a floating/one-off pool) rather than free-floating ad rules, to avoid list fragmentation.
- **Open: where rules live** — service-territory level (favors per-territory coverage reporting; Lynsay) vs. smart-dispatch level. Functionally equivalent; choice is about reporting granularity and explicitness. A two-layer model (project/dispatch-level rules + per-territory overrides) is on the table; the in-flight talent-pool expansion work supports it either way.
- **Relationship to the default-territory geolocation proposal:** complementary, not competing. That proposal defines the *geographic boundary* (anchor + drive-time + work-history → H3 cells); this defines *provider membership* of a territory. Both converge on the two-signal (distance **or** work history) serviceability model. Default territories remain purely geographic templates — they don't change provider-membership behavior.

## Event-Driven Architecture

### Kafka Topics

| Topic | Purpose |
|-------|---------|
| `<env>.marketplace-administration.talent_pools` | Talent pool CRUD events |
| `<env>.marketplace-administration.talent_pool_groups` | Coverage group events |
| `<env>.marketplace-administration.service-territories` | Service territory events |
| `<env>.marketplace-administration.publish_groups` | Publish group sync (DTP ↔ dispatch-automation) |
| `<env>.matching.work_order_dispatch_automation` | Dispatch run lifecycle |
| `<env>.matching.work_order_dispatches` | WO publish/request events |
| `<env>.matching.work_order_assignments` | Assignment events (v1 and v2) |
| `<env>.work-order-management.match-service-territory` | Zip code auto-matching |

### Event Naming Convention
- `.CMD` suffix = command (tell a service to do something)
- `.INT` suffix = integration event (notify about state change)
- Protobuf schemas in `schema-registry` repo under `/proto/`

## Known Issues

### Kafka Connector Instability (as of Apr 2026)

Kafka connector instability affects **all** B/F microservices — FN Core, Dynamic Talent Pool, and Dispatch Automation — not a single-codebase issue. Symptoms include message sync failures that require manual pod restarts to resolve. No definitive root cause has been identified despite multiple collaborative fixes with Diamond Engineering over the past two quarters (added pod logging, resolved auth issues — improved but not cured).

**Impact:** ~7–8 out of every 10 smart dispatch incidents trace back to Kafka message sync failures between DTP and Dispatch Automation. Smart dispatch reliability is directly gated by connector stability.

**Ownership status:** Unresolved. Mike Mueller engaged Jasime (Diamond/platform principal engineer), but Jasime was unsure whether platform or feature teams should own the long-term fix. Chuck escalating to principal engineering group for ownership clarity (Apr 16). Mobarok and Bilas preparing summarized bullet points of specific failure modes.

**Workaround:** Restart the affected Kafka connector pod. Not a sustainable fix.

## Key Patterns

| Pattern | Usage |
|---------|-------|
| **SCD Type 2** | All history tables — `effective_from`, `effective_to` = `9999-12-31` |
| **Outbox pattern** | Kafka event publishing |
| **Split.io feature flags** | Company-level for gradual rollout |
| **Correlation/causation IDs** | Event metadata (v2) |
| **Convenience views** | `*_vw` views for querying current records |
| **Strategy Pattern** | Service territory matching (extensible to radius/polygon) |

## Key Technical Decisions

| Decision | Detail |
|----------|--------|
| Dispatch status protobuf schema | Chuck designed — separates status vs. outcome vs. outcome action |
| WO location events | Mike Mueller pushed for `WorkOrderLocationSet` (state change) over `MatchWorkOrderServiceTerritoryCMD` (specific command) — ongoing debate |
| Naming inconsistency | Talent Pool Group = Service Territory = Coverage Group — recognized; integration API can rewrite paths but not payload field names |
| ESLint `no-explicit-any: off` | In dispatch-automation; Chuck flagged as concerning |
| "Enhanced features" one-way door | Chuck's recommendation on project-level features; accepted |
| Reporting targets — bounded context (May 7, 2026) | Project-level construct, lives in fn-core. **Independent of Smart Dispatch** — applies to all WOs in a project regardless of routing (manual or SD). Keyed off `(priority, service_offering)` with both optional; null/null is the default fallback target. |
| Reporting target ↔ work order linkage (May 7, 2026) | **No relational FK in app DB.** Platform stores priority + service offering on the WO and stores reporting target configs independently (SCD-style for history). Marrying happens at the data-platform / reporting layer via query at read time. Eliminates need for SCD/historical tables on the relationship itself. |
| Dispatch override settings — bounded context (May 7, 2026) | Distinct backend construct in dispatch-automation, keyed `(priority, service_offering)` like reporting targets but stored separately. UI may surface them adjacent to reporting targets — that's a UX choice; the data model stays separate. |
| Work order priority — first-class numeric attribute (May 7, 2026) | Numeric 1–6, optional, validated. Replaces buyer-defined custom field. Aligns with Salesforce/ServiceNow conventions. Final WO-model location TBD. |
