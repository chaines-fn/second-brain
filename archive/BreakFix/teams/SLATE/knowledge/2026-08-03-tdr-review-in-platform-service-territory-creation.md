# TDR Review — In-platform Service Territory Creation

**Reviewed:** 2026-08-03 · **Reviewer:** Chuck Haines
**TDR:** [In-platform Service Territory Creation](https://fieldnation.atlassian.net/wiki/spaces/TDR/pages/5993988099/In-platform+Service+Territory+Creation) · Owner: Slate Team / Mobarok Hossain · Status: in review
**Method:** lightweight ATAM (utility tree → scenario testing → risk/non-risk/tradeoff synthesis) + FN operational review lenses + `fn-api-design` / `fn-ddl-design` delegation

**Caveats on what I could read:** the Overall Architecture diagram and sequence diagrams 2–4 are embedded images and are not reviewable from the page export — flow review below is based on the prose descriptions only. The Database Design section exports as table names + one-line purposes with **no column definitions**, so index/constraint naming, data types, and partitioning could not be checked (existing reviewer comments on `alt_name`, `sort_order`, `region_code`, `default_territory_id` suggest column detail exists somewhere I can't see). Ask for DDL text or a text schema description.

---

## 1. Utility tree (quality attributes pulled from Background / Core Problems)

| Quality attribute | Where it comes from |
|---|---|
| **Reusability / common geographic foundation** | "provides a common geographic foundation for Smart Dispatch, provider search and reporting"; "no common geographic model for reporting" |
| **Backward compatibility** | "remaining compatible with existing postal code workflows"; NFR + Assumptions both restate it |
| **Scalability & performance at nationwide volume** | "Support nationwide territory data, large site uploads, and high work order volume" |
| **Cross-service data integrity** | "Territory relationships must remain consistent across services"; FR 4 |

Secondary: **cost efficiency** (the AWS isoline section is a real, quantified attribute — unusual and good).

### Scenarios, rated (business importance × technical risk)

| # | Scenario | Type | Rating | Depth |
|---|---|---|---|---|
| S1 | Buyer edits postal codes; the DTP→Kafka command is lost or the connector stalls; geometry never recomputes | Exploratory | **H × H** | Deep |
| S2 | Buyer copies 1,211 default territories into a project; map + cell volume at DoorDash scale | Growth | **H × H** | Deep |
| S3 | Reporting asks "all work orders in current default territory #123 over the past year" across buyers holding different point-in-time snapshots | Use case | **H × H** | Deep |
| S4 | Smart Dispatch expands 45→60→120→…→480 min; AWS isoline behavior/cost above the documented 180-min ceiling | Exploratory | **H × M** | Deep |
| S5 | Buyer uploads a 10,000-row site list of street addresses; geocode volume, cost, and unmatched handling | Use case | **H × M** | Deep |
| S6 | Ops re-imports the master territory list (CLI) while a buyer copy job is mid-flight | Exploratory | **M × H** | Deep |
| S7 | Someone calls `clone-from-defaults` or `copy-from-defaults` for a company/project they don't own | Exploratory | **H × M** | Deep |
| S8 | AWS Location unavailable during provider search → straight-line fallback | Exploratory | **M × L** | Light — answered |
| S9 | Geospatial unavailable during a copy → job stays queued | Exploratory | **M × L** | Light — answered |
| S10 | Future country / territory model added (FR 9) | Growth | **L × M** | **Deliberately skipped** — no stated business driver behind FR 9; `country_code` on the tables is a reasonable hedge. Naming it a deferred non-risk rather than a gap. |
| S11 | Amplitude UI event plumbing | Use case | **L × L** | **Deliberately skipped** |
| S12 | Health/readiness endpoints | Use case | **L × L** | **Deliberately skipped** — reuses existing, correct answer |

---

## 2. Structural completeness (against the FN TDR template)

**Filled and genuinely answered — worth saying out loud:** both Service Drill Down blocks have real content in **every** NFR cell (Observability, Health check, Idempotency/Out-of-Order, Fault Tolerance). That is the single most common gap in FN designs and this doc doesn't have it. Options Considerations contains two real decisions with both sides compared, not just a rejected alternative. The AWS cost section with per-search pricing, cache-key design, and a hit-rate example is better than most TDRs get.

**Present but unfilled placeholder text (hard gaps, not nits):**

- **Reporting & Data Platform** — the section body is still the template's instructional prose ("_What things would internal or external people want to be able to report on?_", "_Are there new tables that need to be CDC synced…_") and the Reusable Data Product table is **empty**. Mike's own comment on this page already resolved the direction: *"we will need to solve for Reporting from Day 1; and build the Data Platform data products necessary to enable this reporting."* That instruction has not landed in the doc yet.
- **Tickets** — still "_Links to any EPIC tickets_".
- **Last Presented / Recordings** in the header table are blank.

**Missing entirely:**

- **Authorization / Access Control** — no statement anywhere, for any of the seven endpoints. Details in §4.
- **Out of Scope / phase boundary** — Background states goals but never says what's deferred. Real deferrals are currently only recoverable from the comment thread (default-territory versioning removed; Provider Match drive-time deferred to the SD network-expansion TD; reporting model deferred to a product thread). A reader can't review scope without them in the doc.
- **Database Design columns** — see caveat above.

---

## 3. Six-part scenario testing (the H×H items)

### S1 — Lost postal-code-change command → permanently stale geometry

| | |
|---|---|
| Stimulus source | Buyer (single edit, mass import, or backfill) |
| Stimulus | Postal codes change in DTP; DTP publishes `UpdateServiceTerritoryPostalCodesCmd` |
| Environment | Steady state, but on the DTP↔Kafka path that our own incident history says is unreliable |
| Artifact | DTP MySQL write + Kafka publish (dual write); `territory_cell` / `service_territory_boundary` in geospatial |
| Response in the doc | Retries and backfills are "simple by replaying the command"; `UPDATE_REASON_BACKFILL` exists |
| **Response measure** | **None.** No detection, no reconciliation, no freshness SLO. |

This is the highest risk in the design, for two reasons the doc doesn't acknowledge:

1. **It's a dual write with no outbox.** DTP commits postal codes to MySQL and then publishes to Kafka. If the publish fails, DTP is correct and geospatial is silently wrong — and geospatial is what Smart Dispatch and reporting read. Nothing in the doc defines the half-failed state or a reconciliation path.
2. **We already know this path breaks.** Per `program/architecture.md`, roughly 7–8 of every 10 smart dispatch incidents trace to Kafka message-sync failures between DTP and dispatch-automation, with no root cause found in two quarters. This design puts geometry correctness on that same class of path and treats replay-by-hand as the answer.

The author's reply to Mike's related comment ("we can perform the recompute synchronously within that job… as a fallback if we run into any issues") is, I'd argue, the better primary design, not the fallback — the postal-code import is already a background job in DTP, and doing the recompute inline removes the dual write entirely. If Kafka stays, the doc needs an outbox and a staleness reconciliation job with a stated detection window.

### S2 — 1,211-territory copy at DoorDash scale

The DTP example response literally shows `requestedTerritoryCount: 1211`, and the geospatial clone example shows 752 cells for 2 territories (~376 cells each). Extrapolated, one buyer project is ~450k `territory_cell` rows — and that per-territory figure implies roughly H3 res 6, while History Tracking proposes adding **res 8** to `dw.work_order`. **The H3 resolution is never stated anywhere in the design.** At res 8 a rural western territory is tens of thousands of cells and the row count goes into the millions per buyer.

No numbers on: cell count per territory by market class, total `territory_cell` rows, copy-job duration for 1,211 territories, or map render budget. We already know from the Jul 14 PEX sync that the map view is effectively unusable for DoorDash at ~1,200 territories, and the earlier geolocation proposal specifically called for vector tiles (`ST_AsMVT`) + zoom LOD to handle that. Neither appears here, and `service_territory_boundary` / `default_service_territory_boundary` are described as existing "for map rendering" with no rendering strategy attached. Performance NFR says "low latency" with no number.

### S3 — Reporting across divergent point-in-time snapshots

Mike raised this and the author took it to product; Mike then set the bar: solve reporting Day 1, build the data products, don't build the reports. It's still open, and I'd frame it more sharply than a reporting gap: **"buyers copy defaults as a point-in-time snapshot" is the central architectural decision of this design and it is not in Options Considerations.** Copy-snapshot vs. reference-with-override is precisely the tradeoff that produces the reporting problem, and no alternative is recorded. Related: FR 4 says "maintain the relationship between default and buyer territories" and Mike's open comment asks *why* — the answer is presumably "so cross-buyer reporting can roll up to a canonical territory," but if that's the why, a bare `default_territory_id` FK to a snapshot doesn't deliver it. `default_territory_id` should probably also be `default_service_territory_id` to match the entity, and the open comment about how it survives re-import is unanswered.

### S4 — Expansion to 480 minutes

The cost table stops at Premium ≤180 min / ≤300 km. Smart Dispatch is specified to expand to **480 minutes**. What bucket is that, is `CalculateIsolines` even supported at that threshold, and what does an 8-hour drive-time polygon cost and take? Also: cache keys are origin-rounded to ~111 m, which is a great hit-rate assumption for repeated searches near the same site, but expansion bands are per-work-order-location and each band is a distinct threshold — so a single dispatch that walks 45→60→120→240→480 is 5 uncached isolines, not 1. The ~97% hit-rate example assumes repetition that the expansion flow may not have. Worth a sentence on expected cost per *dispatch*, not per *search*.

Two smaller things in the same area:

- The band filters are written `filter=drivingTimeMins:gte:45,drivingTimeMins:lte:60`. Per the API philosophy §2.4 rule 5, **duplicate filter fields resolve last-occurrence-wins** — so as specified, the `gte` is discarded and every band returns the full 0→N set. Either the standard needs a range operator or these need to be separate params.
- The provider search example still sorts `distanceInMiles:asc` even though the straight-line ordering language was removed after Mike's comment. Probably just a leftover, but the mismatch between a drive-time filter and a crow-flies sort is the exact thing he flagged.

### S5 — 10,000-row site list

Three connected gaps:

- **The address path isn't actually specified.** Sequence diagram 3 and FR 5 both promise address matching with postal-code fallback, but the CSV schema accepts only `postalCode` (required) and `POST /v1/default-service-territories/match-sites` accepts only `{ "postalCode": "..." }`. There is no request shape anywhere that carries an address. Either the address flow is out of scope (say so) or the contracts are incomplete. This also moots Mike's open comment about rejecting unmatchable addresses — there's nothing to reject yet.
- **No limits.** No max file size, max rows, or max `sites[]` array length on `match-sites`. Compare the reviewer history on other TDRs: unbounded upload sizes are a standing FN catch, and the multipart path needs a stated ceiling that the file server actually supports.
- **Geocoding cost is unmodeled.** The AWS section covers isolines carefully and says nothing about geocoding. If address matching lands, a 10,000-site list is 10,000 geocode calls per upload. Worth the same treatment the isolines got.

### S6 — Re-import while a copy job is in flight

The doc says "existing buyer copies are point-in-time snapshots, so they are not affected by the new default territories." That covers *completed* copies. It doesn't cover a copy job that's mid-flight when the CLI re-import lands — that job can read cells written by two different revisions and produce a territory that matches neither. `default_service_territory` has a `revision` field, which suggests the mechanism to fix this already exists: pin the job to the revision it started with. Worth stating.

Related, and I think more important than it looks: **Personas says the Field Nation Operations Team "imports, maintains, and publishes default service territories," but the flow says "Engineering runs a CLI script."** Those don't match. If Ops owns this, an engineer-run CLI is a support burden and a permanent handoff; if Engineering owns it, the persona is wrong. Also unanswered from the comments: is the master list static, and what's the process when a value needs to change?

### S7 — Authorization

Nothing in the doc states an authorization rule for any endpoint. Specifically:

- `POST /v2/company/{companyId}/projects/{projectId}/service-territories/copy-from-defaults` — needs company **and** project ownership, not just authentication, and should use the Centralized Resource Authorization Service rather than a local check or a call back into mono.
- `POST /v2/company/{companyId}/service-territories/copy-from-site-list` — same, plus it's scoped to company while the sibling endpoint is scoped to project. Where does the project come from? (See §4.)
- `POST /v1/service-territories/clone-from-defaults` — takes arbitrary `serviceTerritoryId` + `defaultServiceTerritoryId` pairs. If this is internal-only service-to-service, say so and say how that's enforced; as written, nothing stops cross-tenant cloning.
- `GET /v1/users` — returns provider IDs and obfuscated coordinates. Existing endpoint, but this design is a new caller; who's allowed to call it, and is obfuscation sufficient for the new callers?
- The CLI script — who can run it, in what environment, with what audit trail?

---

## 4. API review (delegated to `fn-api-design`)

Grouped by severity. Several of these are cosmetic-looking but they're cheapest to fix now, before an OpenAPI spec is generated from these examples.

**Contradicts the standard:**

| Spot | Finding |
|---|---|
| `GET /v2/default-service-territories`, `GET /v2/default-service-territories/{id}/postal-codes` | Pagination returned as `"_meta": { "nextCursor": "..." }`. Standard collection envelope is `{ "data": [...], "cursor": "..." }` — `cursor` at root, omitted when there is no next page. No `_meta`. |
| Same doc, three different pagination shapes | `_meta.nextCursor` (new v2 endpoints), `_relations.next` (existing `GET /v1/users`), and the standard `cursor`. Pick the standard for anything new and note the legacy shape as legacy. |
| `GET /v2/macro-regions?...&include=regions` | `_included` is nested inside each item of `data`. Per §3.6 `_included` is a **root** object keyed by include name. Worth confirming what the intended shape is for per-item sideloading on a collection endpoint — the standard is genuinely thin here, so this may be a standard gap rather than a doc error. |
| `GET /v2/default-service-territories` response | Duplicate JSON key: `"regionCode": "MOUNTAIN"` and `"regionCode": "AZ"` in the same object. The first is `macroRegionCode`. Invalid JSON as written. |
| `POST /v2/company/{companyId}/...` (both DTP endpoints) | `company` should be plural: `/v2/companies/{companyId}/...`. Resource collections are plural kebab-case. |
| `GET /v2/company/{companyId}/background-jobs/{job_id}` | snake_case throughout the response: `job_id`, `meta_data`, `skipped_existing`, `skipped_conflicts`, `skipped_existing_ids`. JSON fields must be camelCase. Also the path segment `{job_id}`. Flagged as "existing endpoint" — if it's legacy, label it legacy explicitly so it doesn't get copied; if it's being touched, fix it. Note the inconsistency: geospatial's job returns `id`, DTP's returns `job_id`. |
| `POST /v1/service-territories/clone-from-defaults`, `POST /v1/default-service-territories/match-sites` | Non-standard response envelopes — `data` + `summary` (+ `unmatchedSites`) as sibling root keys. The standard envelope is `{ data, cursor }`. If a summary block is genuinely needed for bulk operations, that's a standard amendment worth proposing rather than a one-off. |
| All endpoints | No error responses shown anywhere. RFC 9457 `application/problem+json` with `type`/`title`/`status`/`detail`/`code` and granular `errors[]` is required. The `503` mentioned in the Reliability paragraph needs a body shape. Also note: **422 is never used** at FN, and validation failures are `400` + `VALIDATION_ERROR`. |
| `POST` endpoints (all four) | No `Idempotency-Key` support stated. §10.2 requires it for POST, with 24h retention and `409` + `RESOURCE_CONFLICT` on same-key/different-payload. Especially relevant here: double-clicking "Add to project" is the obvious real-world case, and the design leans on "background jobs are idempotent" without saying what the dedupe key is. |
| `GET /v1/users?filter=addressLine1:eq:1 E Main St,locality:eq:Phoenix,...` | Two problems. (a) §2.4 says no free-form strings in query values; a street address is exactly that, and the unencoded space will bite. (b) Semantically this isn't filtering users by a user field — it's passing a geocode input through `filter`. Consider a distinct parameter or a POST search body for the address-based variant. |
| `?limit=100`, `limit (<=1000)` | `limit` isn't in the standard query-parameter set (`filter`, `sort`, `cursor`, `include`, `fields`). It's used in two places here. Either it's an approved param and the philosophy should say so, or these should drop it. Worth raising with the API standard owners rather than silently diverging. |

**Consistency / clarity questions:**

- **v1 vs v2 in the same feature.** New geospatial endpoints are `/v2/macro-regions` and `/v2/default-service-territories`, but the two new POSTs are `/v1/...`. A reviewer already asked why v2; the answer needs to cover why the same feature spans both.
- **`selection.scope: "REGIONS"` with `regionCodes: ["MOUNTAIN"]`.** After the region/macro-region rename, `MOUNTAIN` is a *macro* region. This should be `scope: "MACRO_REGIONS"` / `macroRegionCodes`. Same class of confusion Mike flagged originally, just moved into the request body.
- **`copy-from-site-list` is company-scoped, `copy-from-defaults` is project-scoped**, yet both create project territories. Where does `projectId` come from on the site-list path?
- **`POST /v2/.../copy-from-defaults` doesn't state a status code** while the site-list sibling correctly says `202 Accepted`. Both are async job starts; both should be 202.
- **`status: "PUBLISHED"`** on default territories implies other states. Does the list endpoint default to published-only? Per the safe-defaults lens this should be an explicit allow-list, not "whatever's in the table."
- **`scope: "ALL"`** copies every territory in a country — 1,211 in the example. Is there a cap, a confirmation step, or a rate limit? Big blast radius for one API call.

---

## 5. DDL review (delegated to `fn-ddl-design`)

Only table names and purposes are visible, so this is partial — ask for the DDL.

**Compliant:** all new tables are singular snake_case (`macro_region`, `default_service_territory`, `default_service_territory_postal_code`, `default_service_territory_boundary`, `territory_cell`, `service_territory_boundary`, `service_territory_geometry_history`). `ext_user_location` correctly carries the `ext_` prefix for externally-sourced data, and adding `h3_index` to it rather than creating a parallel table is the right call.

**Contradicts the standard:**

- **`service_territory_boundary` + `service_territory_geometry_history` is a current-table/history-table split.** §4.6 is explicit: an SCD Type 2 table holds *both* current and historical records, and "there should not be a separate table for the current record." Either fold current into the history table and query `WHERE effective_to = 'infinity'`, or drop the SCD framing and call `..._geometry_history` what it actually is.
- **No SCD Type 2 columns are shown.** If it stays SCD2 it needs: surrogate `..._sk` (likely BIGINT, auto-increment), the natural key, `effective_from` / `effective_to` (inclusive/exclusive, default `'infinity'` on PostgreSQL), `updated_by`, plus `deleted_at`/`deleted_by` if soft delete is possible — and explicitly **not** `created_at`/`updated_at`/`created_by`. Also needs a stated lifecycle/archival strategy (default: purge non-active rows older than 3 years).
- **Why does buyer geometry get SCD2 history when default-territory versioning was just removed?** Mike asked "why do we care about changes over time?" and versioning history came out of the design. Keeping full temporal history on the derived buyer geometry while the authoritative default has none is backwards — and derived data that can be recomputed from DTP's postal codes on demand is a weak candidate for a history table at all. Worth justifying or dropping.

**Questions:**

- **`territory_cell` holds cells for both default and buyer territories** — what's the discriminator, what's the PK, and what indexes support the H3 lookup? At the volumes in §3-S2 this table is the one that decides whether the feature performs. Partitioning strategy?
- **H3 resolution is never stated.** It needs to be, in one place, and it needs to reconcile with the res-8 column proposed for `dw.work_order` and with the res-6 starting point in the earlier geolocation proposal.
- **`default_service_territory_postal_code.region_code`** — Mike's open comment is correct and worth resolving in the schema, not the thread: 42223 spans KY/TN, 97635 spans CA/OR. A single `region_code` per postal code is a wrong-by-construction assumption. Same question for `region_macro_region_map` (is region→macro region really 1:1, and per country?).
- **`talent_pool_group.default_territory_id`** — rename to `default_service_territory_id` for consistency with the entity. It's a cross-service reference with no FK possible (geospatial Postgres → DTP MySQL); what happens on re-import or delete of the referenced default? (Open comment, still unanswered.)
- **`dw.work_order` res-8 column** — Redshift/DW standards supersede this one, and this is a Data-team-owned model. Confirm the column name matches the existing `h3_N_resolution` pattern and that the change is coordinated, not assumed.

---

## 6. Kafka / EDA review

**Sound:** the claim-check shape (command carries only identity; geospatial pulls authoritative postal codes from DTP and recomputes) is the right call — it keeps DTP as source of truth, keeps payloads small, and makes the operation naturally convergent rather than order-dependent. Keying on `service_territory_id` gives per-territory ordering. `UpdateReason` including an explicit `BACKFILL` value for observability is a nice touch.

**Questions:**

- **Is this an event or a command, and does it need Kafka at all?** It's named `...Cmd` and it's a directed instruction to one consumer. Given the author's own comment that the recompute could run inline in the existing DTP background job, the simpler design is a synchronous call (or an outbox event) from that job — no new topic, no dual write, no lost-message class of bug. What does Kafka buy here that the background job doesn't? If the answer is "decoupling and replay," say so; if not, the simpler path is better and removes S1 entirely.
- **Outbox.** If Kafka stays, the publish needs to originate from the DB (outbox), and the architecture diagram's event arrow should reflect that.
- **Topic creation.** New topics must be created in the schema-registry repo — auto-creation isn't supported in Prod. Not stated.
- **Tracing metadata.** Does `common.v2.EventMetadataV2` carry `correlation_id` / `causation_id`? Observability is a stated NFR and this is where it's earned.
- **Consumer concurrency.** With one consumer per partition, per-key ordering holds. If concurrency per key is >1, two rapid postal-code edits can interleave writes to `territory_cell`. State the guarantee.
- **7-day TTL, no compaction.** Fine for a command, but it means a >7-day outage is unrecoverable by replay; the `BACKFILL` reason is the manual escape hatch. Say that explicitly.
- **Missing events, the other direction.** Nothing is emitted when a default territory is published/re-imported, when buyer geometry finishes recomputing, or when a copy job completes (DTP is described as consuming completion events — from where?). Is there a reason we wouldn't introduce those in the GA state, given reporting and dispatch-automation both consume this data?

---

## 7. Synthesis

### Risks

| # | Risk | Business driver threatened |
|---|---|---|
| R1 | **DTP→Kafka dual write with no outbox and no reconciliation**, on a path with a known two-quarter reliability history. Lost command = silently stale geometry feeding Smart Dispatch and reporting, with no detection. | Cross-service data integrity; the "reliable geographic layer for Smart Dispatch" goal |
| R2 | **H3 resolution unstated and volumes unmodeled**, with an internal contradiction (per-territory cell counts implying ~res 6 vs. res 8 for the DW column). Decides `territory_cell` size, copy-job duration, and map performance. | Scalability at nationwide volume |
| R3 | **Map rendering at 1,200+ territories has no strategy**, despite a known unusable-at-DoorDash-scale problem and a prior proposal that specified vector tiles + LOD. | Scalability / performance |
| R4 | **Reporting section is unfilled template text** and the point-in-time-snapshot model makes cross-buyer rollup to a canonical territory unsolved. Mike's Day-1 requirement isn't reflected in the doc. | Common geographic foundation for reporting |
| R5 | **No authorization story for any endpoint**, including two bulk-write endpoints and one that takes raw cross-tenant ID pairs. | Security (unstated but non-negotiable) |
| R6 | **Copy-snapshot vs. reference-with-override is absent from Options Considerations** — the design's most consequential decision has no recorded alternative, and it's the direct cause of R4. | All four; it's the load-bearing decision |
| R7 | **Address-based site matching is promised but has no request contract**; only postal codes are actually specified end to end. | FR 5; "buyers snap a site list on with no hand-holding" |
| R8 | **Provider search expands to 480 min, beyond the documented isoline ceiling and cost model**; per-dispatch cost (5 uncached bands) isn't modeled, only per-search. | Cost efficiency; Smart Dispatch reliability |
| R9 | **No limits anywhere** on site-list file size/rows, `match-sites` batch size, or `scope: ALL` blast radius. | Scalability |
| R10 | **Ops-vs-Engineering ownership mismatch** on default territory import (persona says Ops; flow says an engineer runs a CLI). Permanent support cost and an unaudited write path. | Reusability/operability; also feeds R5 |
| R11 | **No phase boundary / out-of-scope statement**; real deferrals live only in comments. | Reviewability of scope itself |
| R12 | **SCD2 history split into current + history tables**, contradicting the DDL standard, on derived data that arguably shouldn't be historized at all while the authoritative default has no versioning. | Data integrity; maintainability |

### Non-risks (checked, sound, don't re-litigate)

- **Postal codes + H3 together**, with postal codes as the buyer-facing business representation and H3 as the internal operational unit. Genuine two-sided comparison; correct call for the backward-compatibility driver.
- **Geospatial owns default territories, DTP stays system of record for buyer Service Territory business objects.** Right boundary, and it's defended against the DTP alternative.
- **Reusing the existing geospatial service, PostGIS, AWS Location, and the existing `GET /v1/users`** rather than building parallel infrastructure. Textbook platform fit.
- **AWS Location Service ToS compliance** — §82.2, `IntendedUse=STORAGE`, 1h/24h cache TTLs well inside the 30-day allowance, with a link to the existing implementation. Raised, researched, answered.
- **Vendor choice** — settled in the Geocoding & Location Strangle TDR; link it from Options Considerations so it stops being re-asked.
- **Claim-check message design** (identity only, consumer pulls authoritative state). Right pattern; convergent under retry and out-of-order delivery.
- **AWS Location unavailable → straight-line fallback; geospatial unavailable → job stays queued and retries.** Both stated, both reasonable.
- **All Service Drill Down NFR cells actually answered for both services.** Explicitly confirmed, since this is the usual gap.
- **Split flag `DEFAULT_SERVICE_TERRITORIES` at buyer-company level, internal → pilot → GA.** Real phased rollout with a real kill switch. (See tradeoff T4 for what it doesn't cover.)
- **Multi-country readiness (FR 9)** — `country_code` present; deliberately not reviewing further. Deferred non-risk.

### Sensitivity points

| Decision | Attribute it drives |
|---|---|
| **H3 resolution** | Everything: `territory_cell` volume, match accuracy, copy duration, map performance, DW join grain. Highest-leverage single number in the design and it's not written down. |
| **Isoline cache key rounding (~111 m) and TTLs (1h / 24h)** | AWS cost. The 97% hit-rate example is entirely a function of these. |
| **`limit <= 1000` on postal-code paging** | Number of DTP round-trips per recompute. |
| **Expansion band sequence (45→60→120→…→480)** | Both dispatch cost and time-to-assignment. |

### Tradeoff points (where the real conversation is)

| # | Tradeoff | Attributes in tension |
|---|---|---|
| T1 | **Kafka command vs. inline recompute in DTP's existing background job.** Kafka buys decoupling and replayability; inline buys transactional consistency and deletes R1. | Cross-service data integrity ↔ service decoupling / job latency |
| T2 | **Copy-as-snapshot vs. reference-with-override.** Snapshots give buyers stability and preserve the DTP model; references give canonical cross-buyer reporting. Currently chosen implicitly, in favor of the first. | Backward compatibility / buyer stability ↔ common geographic foundation for reporting |
| T3 | **H3 resolution: finer = better matching and finer reporting grain; coarser = fewer rows, faster copies, faster maps.** | Accuracy ↔ scalability & cost |
| T4 | **Split-flag rollback covers the buyer-facing surface but not the state behind it** — in-flight background jobs, buyer territories already created (do they disappear from the UI or persist orphaned?), the Kafka consumer (is it flag-gated, or does it keep recomputing for everyone?), the `ext_user_location.h3_index` backfill, and the DW column. Fast reversibility for the UI; unclear reversibility for the data. | Reversibility ↔ implementation cost |
| T5 | **Address match falls back to postal code vs. rejecting the row.** Fallback maximizes match rate; rejection maximizes accuracy and surfaces bad data. Mike's open comment. Currently a convenience default rather than a decision. | Match rate / buyer effort ↔ data accuracy |

### Open questions for product (not architectural risks — route them, don't guess)

1. **Why must the default↔buyer territory relationship be maintained?** (FR 4; Mike's open comment.) The answer determines whether a snapshot FK is sufficient or whether T2 needs to flip. This is the one that unblocks the most other decisions.
2. **Cross-buyer reporting semantics.** "All work orders in current default territory #123 over the past year" — when buyers hold divergent snapshots, is the answer computed against the current default geometry, against each buyer's geometry as it was at the time, or something else? Product needs to state the semantics before the data model can serve them.
3. **Unmatchable addresses: fall back to postal code, reject the row, or flag for buyer review?** (T5.)
4. **Who owns default territory import and publishing — Ops or Engineering?** (R10.) Drives whether an admin UI is in scope.
5. **Is the master territory list static?** What's the change process, and what should happen to existing buyer copies when a default changes? (Open comment, unanswered.)
6. **Is `scope: ALL` (1,211 territories in one call) an intended workflow, or should there be a cap or confirmation?**
7. **Should default territories be published-only to buyers by default,** and what other statuses exist?
8. **Drive-time in Provider Match** — deferred to the SD network-expansion TD per the thread. Confirm that's product's position, and note the deferral in this doc's out-of-scope section so it stops resurfacing.
9. **Should AWS Location usage be tracked as a business metric with feature granularity?** (Open comment.) Related: what's the monthly cost ceiling before someone is alerted?

---

## 8. Suggested next steps for the author

Tightest path to "ready to circulate":

1. Add an **Out of Scope / Phase** statement capturing the deferrals already agreed in the comment thread.
2. **State the H3 resolution** and add volume estimates for `territory_cell` at 1,211 territories.
3. Resolve **T1** — decide Kafka vs. inline recompute, and if Kafka, add outbox + a reconciliation/staleness check with a stated detection window.
4. Add an **Authorization** subsection covering all seven endpoints, using the Centralized Resource Authorization Service.
5. Fill in **Reporting & Data Platform** per Mike's Day-1 direction, and delete the instructional placeholder text.
6. Add **copy-snapshot vs. reference** to Options Considerations, even if the conclusion doesn't change.
7. Fix the API items in §4 — pagination envelope, `regionCode` duplicate key, `companies` plural, camelCase on the job response, error shapes, `Idempotency-Key`, the `gte`+`lte` band problem.
8. Post the **DDL as text** (or a text schema description) so the schema can actually be reviewed, and resolve the SCD2 current/history split.
9. Add **limits**: site-list file size and row count, `match-sites` batch size, `scope: ALL` behavior.
10. Extend the **AWS cost model** to 480-minute isolines, per-dispatch (not per-search) cost, and site-list geocoding.
11. Extend the **rollback plan** to cover in-flight jobs, already-created territories, the Kafka consumer, and the backfill.
