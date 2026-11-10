# GARNET Projects

## Q3 2026 Capacity & Scope

**Capacity accounting changed Jul 28, 2026 - now expressed as 85% dev / 15% KTLO, not story points.** The point figures below are stale as a planning instrument but are retained for the scope decisions attached to them. See `program/meetings/2026-07-28-pex-lead-sync.md`.

### Original lock (Jul 6, 2026)

**Target: 110 points against 112 total team capacity.** Locked at the H2 Initiative Planning & Prioritization session (Ridi Haque). Cross-team scope decisions from the same session: PSS score + driving distance display cut entirely from Q3 (Ryan Schulte); public API conversion (3 APIs, ~10-12 pts) deferred to Q4/Phase 2 (Lynsay); Break/Fix reporting gets a 10-pt placeholder pending Alyssa Eby's scope confirmation; Network Expansion (§6 below) confirmed will not complete in Q3. Leadership signal (Shane Kinder, relayed): move away from adding more eligibility-criteria/config options toward smarter, more automated assignment logic — applies directly to the advanced-availability-criteria work in §6. See `program/meetings/2026-07-06-h2-initiative-planning-prioritization.md`.

## 1. Smart Dispatch (Core)

**Service:** `fieldnation/dispatch-automation`
**Status:** In production, v2 feature flag enabled for all B/F accounts (April 2026)
**Stack:** Node/TypeScript

### Core Dispatch Flow
1. WO created → service territory matched (auto via zip code or buyer-specified)
2. Smart dispatch triggered → dispatch run created
3. WO published to service territory providers (tier-based)
4. Wait for provider responses (configurable runtime)
5. Rank responding providers → assign top-ranked
6. Fallbacks: re-dispatch on backout, marketplace expansion, escalate to buyer

### Dispatch Settings Schema
```
project_dispatch_setting:
  dispatch_runtime_minutes: int
  rerun_until_threshold: boolean
  rerun_dispatch_threshold_minutes: int
  dispatch_to_first_available: boolean
  revert_to_draft_on_no_available: boolean
  expansion_settings: [{ type, delay_minutes, config: { max_distance, pss_tier } }]
```

### Ranking Logic
- **ST Pool:** 80/20 tier selection (80% Tier 1, 20% Tier 2+3) + round robin within tier
- **Marketplace Pool:** Provider Match 2.0 standard ranking (PSS, distance, qualifications)
- **Mixed Pool:** ST requests always win if any exist; marketplace only if zero ST requests

### Assignment Transparency
- Immutable Assignment Snapshot stored at assignment time
- `DispatchAssignmentDetailsUpdated.INT` event carries context to Provider Match
- WO detail page shows plain-language explanation (assignment source, tier, expansion triggered, etc.)

## 2. Marketplace Expansion

**Status:** Released Q1 2026 (under contract flag `DA_MARKETPLACE_EXPANSION`)

Allows WOs to expand beyond the curated service territory pool if unfilled after a configurable delay.

- Buyer-configurable delay (X minutes after dispatch start)
- Hard eligibility filters: PSS tier, max distance
- ST providers ALWAYS outrank marketplace providers
- Triggers via SQS delay queue + dedicated K8s worker
- Provider Match service handles marketplace provider ranking

## 3. Smart Dispatch V3 (Q2 2026)

**Status:** In build. Live demo completed June 15, 2026 — talent pool segment working end-to-end in lower env; several UX gaps identified, none blocking launch. See `knowledge/meetings/2026-06-15-smart-dispatch-v3-segments-demo.md`.

**Status update (June 18, 2026):** Release sync covered a failure-criteria bug (fix in progress — dispatch should only fail if all enabled segments return zero providers, not just one), a full ranking-pipeline walkthrough, and rollout planning. Lynsay migrating buyers V2→V3 starting with DoorDash, framed to account owners as a look-and-feel change; all migration/support communication now centralized in the "ask smart dispatch" Slack channel. Auto-dispatch button now hidden whenever smart dispatch is enabled on a project. Copy fix pending (remove "drug screening" language from work order qualifications text). Talent pool summary page had an unresolved loading issue during the demo (possible permissions or DTP-dependency cause). See `knowledge/meetings/2026-06-18-dispatch-segment-eligibility-criteria-coverage-summary.md`.
**Service:** `fieldnation/dispatch-automation`
**Requirements doc (canonical):** [2025-2026 B/F Smart Dispatch Plan — 26Q2 reqs tab](https://docs.google.com/document/d/1AZhg5kmXWs4H2r0l2eZp02y0k5ETrDrVJsfTkMz2iMg/edit?tab=t.vwqewhx4131#heading=h.iu3msvhv1yuh) (last updated Apr 8, 2026)

### Q2 Workstreams (3 streams, 2 milestones)

| Workstream | Target | SD Version |
|------------|--------|------------|
| SLA capture updates | April 2026 | v2 |
| SD duration override (with SLA capture) | April 2026 | v2 |
| Talent pool expansion + provider criteria | Early June 2026 (latest) | v3 |

**Note:** Tahsin Jahin Khalid is the primary analyst on reporting. Engineering should collaborate with him to ensure data structure works for his reporting needs during V3 transition.

### Q2 Workstream 1: SLA Capture / Reporting Targets (April, v2)

**Status update (June 4, 2026):** Code complete, testing ongoing. **Releasing to production Mon June 8** — Amran blocked any Friday deploy (and refused a Dunder Mifflin production enablement for demo) due to ongoing migration-related incident risk. QA4 resolved; Alyssa's demo date is June 8. See `program/meetings/2026-06-04-pex-lead-sync.md`.

**Status update (May 21, 2026):** Code complete, testing in lower env. 3 PRs pending approval (targeting review by Tuesday May 26). Habib deployed to lower; Alyssa sent to QA; Tahsin (data) verifying tracked data points match requirements. Avoiding Friday production push due to payment processing window. Target: all merged + released to prod by Tuesday May 26. SLA TD has no open technical questions — just needs final communication posted (Bilas's action item).

**Status update (May 7, 2026):** Original custom-field-based design dropped. Three constructs that were being conflated have been separated; phased delivery plan locked in. See [`knowledge/meetings/2026-05-06-sla-data-capture-solution.md`](./knowledge/meetings/2026-05-06-sla-data-capture-solution.md) and [`knowledge/meetings/2026-05-07-sla-data-capture-solution-cont.md`](./knowledge/meetings/2026-05-07-sla-data-capture-solution-cont.md).

**Three constructs (now separated):**
1. **Work order priority** — first-class numeric attribute (1–6), validated, optional (null allowed). Not a custom field. Final WO-model location TBD by Bilas.
2. **Service offering** — optional sub-project / program-within-project differentiator. Renamed from "workstream," which had been renamed from "template." Linda's preferred term; "service line" is the ITSM equivalent.
3. **Dispatch override settings** — separate backend construct in dispatch-automation, keyed off `(priority, service_offering)`. Independent of reporting targets at the data layer regardless of how the UI surfaces them.

**Reporting targets:**
- Project-level construct, lives in fn-core (not dispatch-automation).
- Apply to **all** WOs in a project regardless of routing — manual assignments included. SD is a filter on the report, not a precondition for inclusion.
- Mapping key: `(priority, service_offering)` — both optional. Null/null is the default fallback.
- Phase 2+ hierarchy mapping: most-specific match wins, falls through to default.
- **Platform stores data as it comes in** — no relational FK between WO and reporting target. Marrying happens at the data-platform / reporting layer via SCD on reporting target config + query at read time.

**Phased delivery plan:**

| Phase | Scope | Driver |
|-------|-------|--------|
| 1 | Single default reporting target per project (null/null). One target per project. No priority / service offering concept in code yet. UI restricted to single target. | Unblocks the 95% (Unicus-style buyers) — Alyssa's data-governance push, productizing reporting metrics |
| 2 | Layer in priority (1–6) and service offering. Templates extended to set these on work orders that use templates. Reporting target table key becomes `(priority, service_offering)`. | Blackbox / Laura's case |
| 3 | Extend to integration / work order create payload — buyers can pass priority + service offering directly without using a template. Service offering keying likely follows service-territory dual-ID pattern (FN ID or buyer-provided unique ID). | Integration buyers who don't use templates |

**Sequencing open question:** Phase 2 (templates-first) vs. Phase 3 (integration payload first) — Lynsay/Alyssa to confirm with Linda what Blackbox specifically needs. Lynsay's read: templates-first.

**Snapshotting (parked):** What happens if a buyer changes WO priority mid-flight? Bilas's V1 thinking was snapshot at creation; Alyssa's gut is end-state value. Alyssa to check with Linda — answer affects data model (snapshot field on WO vs. SCD-only).

**UI/UX constraints:** No redesigns this round — design bandwidth is gone (Brent off, no MARBLE designer). Label-only changes are fine (e.g., template → service offering). Backend bounded contexts must still be correct even where UI doesn't reflect it yet.

**Timeline impact:** Chuck's position is the redesign does **not** push SD's June 23 go-live — replaces equivalent custom-field/eventing work with similar effort. Bilas to confirm with an estimate (target Mon 5/11). Bilas updates the existing TD; Chuck + Mike review informally next week in BD (no formal Friday design review).

**Custom field timezone storage gap (still relevant separately):** Timestamp custom fields have no timezone context, breaking SLA evaluation. New `datetime-with-timezone` custom field type approach was aligned Apr 16. See knowledge.md for the decision.

### Q2 Workstream 2: SD Duration Override (April, v2)
- Override SD duration at the SLA capture/reporting target level
- Configurable alongside SLA capture without modifying broader project-level SD settings

### Scope — Talent Pool Expansion + Provider Criteria (Workstream 3)

**Segment toggling:**
- Buyers toggle three dispatch segments on/off: Service Territory (ST), Talent Pool (TP), Marketplace (MP)
- No segment is required, but at least one must be selected. Buyers can run without STs entirely (e.g., TP + MP only, or TP only)
- Fixed cascade priority: ST > TP > MP (only applies when multiple segments active)
- When MP is activated, buyers can choose immediate publish or delayed (delay gives ST/TP time to respond first). No delay = all active segments at once.
- **Smart Dispatch button no longer requires a service territory** (Jun 15, 2026) — old requirement was a bug; button should show and function with Talent Pool or Marketplace alone.
- **Marketplace-only locks to "immediate"** (Jun 15, 2026) — when only MP is enabled, delay dropdown is hidden and locked to immediate. Old delay value stored and restored when other segments are re-enabled. Lynsay mocking up.
- **Settings page validation (post-launch, pre-GA)** — future state: save function + page-level validation prevents leaving settings page with all segments disabled. Currently only a yellow banner surfaces at the work order screen.
- **Round-robin tie-break confirmed tier-only (Jun 18, 2026):** Round robin only applies when multiple providers are tied within a single tier — no cross-tier ranking application.
- **Failure criteria bug — fix in progress (Jun 18, 2026):** Smart dispatch should only fail if all enabled segments return zero eligible providers, not just one (e.g., ST enabled with no service attached shouldn't fail the run if talent pool has eligible providers). No owner/ETA confirmed yet.

**Talent Pool Groups:**
- Buyers create project-specific Talent Pool Groups in two types:
  - **Ranked** — buyer specifies priority order among pools
  - **Unranked** — all pools treated equally
- Slate should contribute to TP Group creation (overlaps with ST Provider List views NetDev has been building)

**Provider criteria (floor-based model):**
- Sits on CSRs in the backend
- Every criterion the buyer sets is a hard minimum floor — binary pass/fail
- **Design principle:** Criteria gate notifications and assignment eligibility, NOT WO visibility. Providers can still search/find WOs in marketplace. Exception: distance gates visibility.
- **ST providers now subject to criteria** — rationale: as programs scale, wide STs aren't precisely curated. Per-segment toggle lets buyers exempt ST from specific criteria.
- v3 limitation: floor value is the same across all segments where a criterion is active (can't set 2yr background for ST and 6mo for MP). Segment-specific floors deferred.
- Buyers add criteria one-by-one, configuring:
  - **Segment application** — toggle per criterion per segment (e.g., exempt ST from PSS check)
  - **Floor selection** — minimum acceptable level (e.g., PSS = "good" filters out everyone below good)

**Ranking strategy (simplified for initial release per Apr 14, 2026 review):**
1. **Layer 1: Eligibility** — floor-based hard cutoff. Pass/fail per criterion. **This ships in V3 initial release.**
2. **Layer 2: Default Match 2.0 ranking** — within the eligible pool, providers ranked by the existing Match 2.0 algorithm (PSS, distance, experience, etc.). No buyer-influenced re-ranking for initial release.
3. ~~**Layer 3: Across-criteria ranking**~~ — **Deferred.** Original design had buyer priority ordering with weight multipliers. Group couldn't define what priority ordering would mean technically; deferred until Q2 outcome data is available.

**Interim CSR approach rejected (Apr 20, 2026):** The Apr 14 meeting floated a "temporary behind-the-scenes CSR approach" as a Q2 interim for priority ordering. Bilas and Chuck revisited this Apr 20 and explicitly rejected it — CSR-based ranking is a separate, category-only ranking path whose implementation isn't reusable for the eventual priority-ranking design, doesn't accelerate Q2 delivery, and would add UI/UX confusion on the Provider Match page. Q2 ships hard filters + default Match 2.0 only. If priority ordering is needed sooner than Q3, PM must cut scope elsewhere (talent pools called out as a candidate). See `knowledge/meetings/2026-04-20-q2-scope-and-ranking.md`.

**TD sequencing + capacity confirmed (Apr 21, 2026):** Two technical designs support the Q2 plan. **TD1 (hard filters)** is the simpler piece — already in Chuck's review this week, unblocks engineers on surrounding work. **TD2 (priority-ordering ranking)** introduces the new concept where the highest score on criterion #1 dominates, subsequent criteria only break ties. No new ranking algorithm — uses the existing Match ranker with a new data/formula shape. Needs diamond-team alignment (they own the ranker); Bilas estimates TD2 + alignment wraps in ~2 sprints. **Capacity reality:** single-team throughput cannot deliver both filtering and priority ranking in Q2. Engineering delivers in pieces; product releases when ready. If both must ship together in Q2, additional dev capacity is required. See `knowledge/meetings/2026-04-21-smart-dispatch-priorities-sync.md`.

**Priority-ranking criteria shortlist (Apr 21, 2026):** Narrowed to four — **distance, PSS, work completed for same type of work, work completed for my company**. Lynsay to validate against customer-pipeline data and confirm top 5–6 for Bilas. Last active and talent-pool membership stay as default-applied signals behind the scenes (not part of buyer-configurable priority ordering in initial release).

**V2/V3 regression-risk — integrated test plan required (Apr 21, 2026):** Bilas flagged that V2 and V3 sit on common code paths; testing effort will be substantial to avoid breaking existing features. Group agreed a comprehensive integrated test plan is needed. **No owner assigned yet.**

*Original 3-layer design (Apr 8) included within-criterion band sorting and across-criteria weighted scoring. These were walked back in the Apr 14 scope review — see `knowledge/meetings/2026-04-14-smart-dispatch-q2-scope-pt2.md` for full context.*

**Priority ordering (from Q2 reqs doc — tension with Apr 14 deferral):**
- Criteria WITH Match 2.0 equivalents (can adjust ranking weights): Distance, PSS, Work completed for same type of work, Work completed for my company. Also in Match but not buyer-configurable: Talent pool membership, Last active.
- Criteria that are FILTER-ONLY (no Match equivalent): Background check recency, Drug screen recency, Licenses & certs, Tools & equipment, Max assignments/day.
- **BLOCKING:** Same-project and same-client work completion need ranking influence but Match only has company-level Buyer Experience. Reqs doc calls this a blocking decision. Options: extend Match, CSR bridge, or other.
- **Note:** Apr 14 meeting deferred priority ordering entirely for initial release. The reqs doc still describes it as part of the design. Alignment needed.

**Dispatch assignment logic at ranking window:**
- ST active + ST requests exist → 80/20 distribution (past 7 days history), then provider criteria ranking on top
- No ST requests + TP active → ranked TP uses 80/20 across tiers; unranked TP skips 80/20, criteria ranking determines assignment
- No ST or TP requests + MP active → criteria ranking, assign highest

**Coverage summary (required for Q2):**
- Shows buyers eligible provider counts by segment before going live
- Can use 24-hour cached data (real-time freshness not needed)
- Counts reflect floor-based criteria — providers meeting all active minimums
- Slate should potentially contribute (similar to ST Provider List views)

**Copy settings:**
- Full config copy from one project to another: segment toggles, TP group config, provider criteria (ordering + floors), all dispatch settings

**Change log:**
- External-facing: who changed what dispatch settings and when
- Focus on segments and criteria setup

**PSS score and distance mapping:**
- No changes — existing behavior carries over as-is to V3
- Engineering should verify existing logic works with expanded segment model

**Observability requirements (all 3 buckets in Q2 scope):**
1. Per-assignment tracking: eligible pool composition, ranking drivers, priority ordering impact vs. default Match, failure reasons
2. Criteria configuration impact: pool size changes on criterion add/remove/change, criteria causing zero pools, ineffective criteria
3. Outcome analysis: reassignment rate, SLA compliance, completion quality, cross-configuration comparison (internal product outcome tracking)

**Cross-ST expansion (stretch goal):**
- Distance-based expansion within buyer's other STs for white-space areas
- Keeps assignment within curated network when primary ST has no coverage
- Open Qs: opt-in vs. automatic, cascade interaction, reporting attribution, travel cost

### Service Company Inheritance Risk (flagged May 21, 2026)

New complexity discovered in the open-source provider model: it has no concept of service companies, which creates data inconsistency scenarios when managed providers are added to or removed from a service company. Marble team is working on a TDD covering schema changes, outbox pattern, and Kafka event production for sync. This is unscoped work — Bilas couldn't give a timeline estimate.

- Insurance sync: planned as real-time
- Equipment sync: considered simpler / largely resolved in discussion
- Specific eligibility criteria causing delay: Bilas to provide list by May 26

**Decision gate (May 26):** Lynsay will decide whether to cut eligibility criteria scope for Phase 1 or push return experience further out, based on Bilas's list.

### Integration Considerations (Q2 — scoping needed)

1. **API-based dispatch triggers** — buyers triggering dispatch from ITSM tools may need awareness of new V3 config model (segments, criteria floors, TP groups). Need to verify existing API contracts still hold.
2. **Inbound data feeds for criteria** — criteria sits on CSR backend; need to inventory which integrations touch CSRs and assess if floor-based model + bucket structure breaks anything.
3. **Outbound notifications/webhooks** — downstream systems may need to know which segment a provider was dispatched from; check if webhook payloads need new fields.
4. **Buyer onboarding/migration** — existing Auto Dispatch (CSR) buyers may need migration path to SD v3 settings. Assess if bulk migration tool needed or one-by-one transition.

### V2 → V3 Migration Strategy
- "Advanced Settings" + "General Settings" → consolidated "Dispatch Settings" tab (V2 compatibility maintained)
- New V3 features (Segments & Criteria tab) hidden behind V3 flag
- All V2 marketplace configs (distance, PSS score) migrate to V3 segment/criteria framework once V3 is stable

### Open Business Logic Decisions
1. **Tiering model for ranked talent pools:** Sequential (exhaust Tier 1 before Tier 2) vs. 80/20 fairness-split distribution. Brent to align with CS team. Sequential is technically feasible but may require one API call per tier.
2. **Adjacent territory spillover:** Not in Q2. Depends on NetDev providing territory-to-territory geographic relationship data (Q3 dependency). Workaround: use a talent pool with the same provider rigor + distance-based filter.
3. **Distance calculation:** AWS ISO lines (isochrone polygons, accounts for roads/traffic) vs. current crow-flies — still evaluating for Q2 fit. Chuck and Ryan Schulte to discuss offline.
4. **Background check & drug test scoring (Apr 20, 2026):** Bilas proposed treating these as boolean (pass/fail) rather than recency-scored, since "within 1 month vs. within 6 months" doesn't reflect a meaningful quality delta. Certs, equipment, and licenses are already boolean. Needs explicit alignment with Linda before locking in.

### Technical Notes
- Provider criteria model: floor-based hard cutoffs (ineligible outside the floor) + automatic ranking within eligible pool by freshness/recency/experience. No separate "required" vs "optional" sections — floor + ranking handles this more cleanly.
- Coverage summary is powered by the NetDev recruitment API — critical dependency, not optional
- "Point tippers" (preference ranking without hard exclusion): handled via criteria with floor set to "anytime" — anyone with any background check ranks higher than someone without
- History/change log UX: engineering logs vs. display to users need to be decoupled. Existing history logs in the platform are confusing and duplicative — Linda to define in Requirements B what surfaces to users
- **CSR-backed ranking needs careful implementation:** weight gap between adjacent criteria must be large enough that buyer's stated priority order is never violated. Band definitions and weight values to be provided in separate document.

### Smart Dispatch Service Territory Configuration (June 30, 2026 - TOP PRIORITY)

**Update (Aug 21, 2026 — GARNET daily scrum/refinement):** Team resolved a sandbox data-sync bug: the platform was pulling local preset data instead of Salesforce records because test accounts lacked proper contract enablement — decided all buyer data must source directly from the contract going forward. Lynsay set an internal go-live target of **Tuesday (Aug 25)** for service territory configuration, pending final sandbox validation. TD-review routing confirmed: internal feedback from Chuck and Mike first, with Bilas recommending GARNET + Split team engagement given dependencies — worth confirming whether Tamvir is included in that first-pass step per his TD-lead role. **Re-checked against the H2 Features tracker (Aug 24):** the core Service Territory Configuration row itself hasn't moved (still "Release Started," 8/10/2026 date, 90% readiness) but two related rows have — **Service Territory Management Enhancements now shows "Delivered"** (actual launch 7/27/2026), and **"Prevent Double Booking" now reads "Completing F&F"** with a new note flagging the need to track how many WOs go unassigned due to the new max-assignments/schedule-conflict logic.

**Update (Aug 19, 2026 — H2 Features tracker):** Notes field now reads "Ready to be enabled for Zones and Interface. Confirming recent error on the SD settings screen is not related before moving forward." First sign this is about to go live for two named buyers (Zones, Interface) since the "Release Started" status flip Aug 18 — gated on confirming an unrelated SD-settings-screen error isn't a blocker. Still 8/10/2026 target date and 90% readiness on record.

**Status (Jul 31, 2026): still not live - third consecutive slip; entering fit-and-finish the week of Aug 3.**

| Date | Target stated | Source |
|------|---------------|--------|
| Jul 10 | Jul 27 release (with SD cancellation + availability config) | Weekly alignment |
| Jul 17 | **Early August** - work "expanded into approximately two and a half sprints" | Working group |
| Jul 21 | Committed to next sprint at **38 SP total** (21 GARNET / 17 SLATE) | PEX Lead Sync |
| Jul 24 | "Early in the next sprint" | Working group |
| Jul 31 | "On track to enter the **fit-and-finish** phase next week"; TD still not finalized | Working group |

**Named causes of the expansion:**
1. **Async → synchronous call conversion** (Jul 17) - the same synchronous-ST-association direction agreed Jul 16 with Mike Mueller, now showing up as schedule cost. It also fixes ongoing issues with auto-applying pay rates. SLATE owns this conversion (see ownership split below).
2. **Map view initial-load performance** (Jul 24) - now an explicit launch-gate decision: launch in current state vs. optimize further to avoid a poor UX. Consistent with the ~1,200-territory DoorDash degradation flagged Jul 14.
3. **Data platform ingestion design** (Jul 31).

**SLATE/GARNET ownership split finalized (Jul 21, 2026)** - tickets physically moved between boards by Abu Saleh in coordination with Ridi:
- **GARNET owns:** Smart Dispatch trigger points and pre-validations across all touchpoints (integrations, mass create, work order create).
- **SLATE owns:** opt-in service territory feature management (toggle on, save opt-in territories), the async → synchronous conversion of auto-association, and Kafka flow synchronization/fit-and-finish.
- Lynsay's summary: "you're handling the rule and they're handling how it works in dispatch." See `teams/SLATE/knowledge/meetings/2026-07-21-slate-sprint-planning.md`.

**Rollout decided (Jul 28, 2026):** releases to **all customers** once fit-and-finish completes, since it's opt-in. Framed as a permanent, necessary feature for buyers to trust Smart Dispatch alongside auto-dispatch. Territory config also **requires buyer-side setup** - Clear Captions' integration depends on their own existing ST setup, an adoption risk against the "three buyers at volume" OKR (Jul 31).

**Scope clarifications (Jul 17 / Jul 31):** no data model changes; it acts as a **pre-qualification step** for Smart Dispatch, similar to project setup, and **does not alter existing eligibility requirements**. New rationale added Jul 24: territory mapping is now also framed as the fix for a Smart Dispatch **reporting gap** - today there's no way to differentiate work orders intended for Smart Dispatch from those using talent pools. Jul 31: territory configuration data will be **ingested directly into the data platform** rather than into newly-created DW tables. *Note the apparent tension with two other Jul 28/31 statements: the Jul 28 PEX Lead Sync decided historical work order data stays in the DW with an H3 resolution-8 column added to **existing** DW tables, and Amran said at the Jul 31 TDR that "this quarter targets existing DW tables; longer term served from the data platform." Best reading is that "existing DW tables" and "new separate DW tables" are different things, and that territory-config data and historical-WO data land differently. Not stated explicitly anywhere. Worth confirming with Amran or Alyssa before designing against it.*

**Status (Jul 17, 2026): TD renamed a fifth time — "Smart Dispatch Opt-In Service Territories."** In a group DM (Amran, Bilas, Lynsay, Mobarok, Chuck, Abu Saleh), Abu Saleh proposed "opt-in" as the official terminology — buyers pilot Smart Dispatch in a limited set of service territories rather than rolling it out program-wide; only WOs in an opt-in-listed ST are eligible for SD. Chuck agreed to rename the API, tables, and columns consistently (not just the endpoint) — proposed endpoint `GET /dispatch-automation/v1/projects/<project_id>/opt-in-service-territories`. The thread also resolved Chuck's Jul 7 gap #1: Mobarok confirmed the opt-in eligibility check is a standalone prerequisite gate, fully independent of which SD segments (ST/TP/MP) a project has active — a WO whose ST fails the opt-in check skips SD entirely even on a TP/MP-only project. See `knowledge/knowledge.md` (Key Decisions, Jul 17).

**Status (Jul 16, 2026): TD renamed again, under active review — synchronous ST association pattern agreed.** The TD (same Confluence page, ID 6046547970) has been renamed a third time, now titled **"Limit Smart Dispatch by Service Territory"** — Chuck flagged Jul 15 that "coverage area(s)" framing was confusing and not what the TD is actually about (selectively enabling STs for SD, unrelated to coverage-area concepts), pushing for better naming. Chuck left extensive review comments Jul 15 (~3:08–3:23 PM): missing "other options considered" section (sparse allow-list vs. dense flag, DA-owned vs. DTP-owned reasoning undocumented), several DDL-standard violations (SCD2 tables must be suffixed `_history`; use `updated_by` instead of the field originally proposed; index plan undefined), and multiple hard pushbacks on assumptions in the doc (marked "No no no" — details in the TDR comment thread). Abu Saleh has been resolving these Jul 15 evening–Jul 16 morning: added ACID transaction handling per Chuck's request, confirmed with GARNET that the **v1 dispatch API will be deprecated** so this TD won't extend it, and renamed the page per the naming feedback.

**Synchronous ST association pattern agreed (Jul 16, 2026, group DM — Chuck, Mike Mueller, Amran, Bilas, Lynsay, Mobarok, Abu Saleh):** Directly resolves Chuck's Jul 15 TD comment questioning whether ST association needs to be synchronous rather than event-driven, given it's critical for other downstream processes (this TD depends on knowing ST association is complete before evaluating SD eligibility). Abu Saleh proposed reusing the existing `PUT /api/rest/v2/workorders/%d/service-territory` endpoint (currently invoked by a sidecar after the async Kafka ST-association command) — mono would call this endpoint's logic internally/synchronously instead. Mike Mueller endorsed reusing the endpoint's underlying logic via a shared service class (not the Kafka/rabbit-triggering v2 wrapper itself), calling it a pattern **"we should be doing everywhere"** — signals this could become a broader synchronous-internal-call standard beyond just this TD. Latency impact expected negligible (internal service call). **This also resolves (in this specific case) the open architectural question in `knowledge/knowledge.md` about moving off the async Kafka ST-association model** — though the general question of the *default* association model (zip-based today) remains separate and still unsettled pending the H3-vs-zip decision.

**New open question (Jul 16, 2026):** Abu Saleh raised with Mike Mueller whether the API should return all eligible Service Territory IDs in a single array response for projects with very large territory counts (e.g., 700+) — not yet resolved, worth watching given DoorDash-scale buyers (up to 1,200 territories) already flagged as a map-view performance risk (see Jul 14 entry above).

**Status (Jul 9, 2026): TD published.** Abu Saleh Matul (new GARNET engineer) shared the technical design — [Smart Dispatch Area Coverage Configuration](https://fieldnation.atlassian.net/wiki/spaces/TDR/pages/6046547970/Smart+Dispatch+Area+Coverage+Configuration) (TDR space) — via Slack DM to Chuck, Amran, Bilas, and Mobarok at 6:16 AM CDT, per Abu Saleh "the initiative Lynsay identified as urgent." Chuck committed to reviewing same day. Note the doc renames the feature **"Coverage Area Limit"** (the reqs doc's "Smart Dispatch Service Territory Configuration" is now this TD's subject).

**TD design summary:**
- New ST-level layer on top of today's project-level Dispatch Settings — additive, not a new response contract. `GET /dispatch-settings?f_st_id=<ST id>` evaluates a 5-case eligibility rule; when the coverage flag + `coverage_eligible` are on and the WO's ST isn't in the buyer's configured allow-list (or the WO has no ST at all), the response reads **as if SD is off** for that WO — it falls through to AD or manual, unchanged elsewhere.
- Buyer-scoped feature flag, disabled by default; buyers pick STs via a searchable modal (resolves Chuck's Jul 7 gap #7 — went with the search-based option, not show-all multi-select).
- New table `project_dispatch_service_coverage_setting` (SCD2, full audit history) + new `coverage_eligible` boolean on `project_dispatch_setting`. Lives entirely in Dispatch Automation; DTP unchanged (DA calls it only to hydrate ST name/region on reads).
- Amplitude events (ST added/removed by name, toggle on/off, adoption) + structured Datadog logs on SD-blocked-by-coverage events (`project_id`, `service_territory_id`, WO id) — **resolves Chuck's Jul 7 gap #6** (no observability existed for the per-WO matched-vs-skipped outcome).
- Snapshot-at-run-start semantics — mid-run coverage edits don't affect an in-flight dispatch.
- Flightboard Auto Assign (AD path) is explicitly out of scope/unmodified; only the Dispatch action and dispatch-settings API enforce the rule.

**Open question this TD raises against Chuck's Jul 7 gap list:** Gap #1 (does the restriction apply to TP-only/MP-only projects with no ST segment?) gets a specific answer here — **a WO with no (opt-in-listed) ST associated is treated as SD-off entirely**, not just ST-segment-scoped. **Resolved Jul 17, 2026:** confirmed intentional in a group DM — Mobarok clarified the opt-in check is a standalone gate independent of active segments. Gaps #2 (fallback vs. ST>TP>MP cascade), #3 (manual WO creation bullet), #4 (zip/territory race condition), #5 ("without project association" for integration WOs), #8 (interim stopgap for Clear Captions/Interface), and #9 (downstream reporting distinguishing suppressed SD) are not addressed in this TD version — worth flagging in review.

**Target release (Jul 10, 2026 weekly alignment): July 27** for territory-based Smart Dispatch (Coverage Area Limit) + Smart Dispatch cancellation, together. Team is pivoting off current filter/pagination work once the TD clears review to hit this date. Availability configuration (see §6) also targets July 27, shipping as a backend experiment flag rather than a frontend change. See `program/meetings/2026-07-10-bf-weekly-working-group-alignment.md`.

**Prior status (Pre-TD, June 30, 2026):** Elevated to #1 GARNET priority for next sprint (June 30, 2026).

High-volume risk-averse buyers (Clear Captions, Unicus, Interface) won't enable Smart Dispatch program-wide. They need the ability to turn on SD for *specific service territories only* while autodispatch continues on everything else. Current workaround (create a separate project) is impractical at scale.

- Trades off against: view-all service territory drawer enhancements, which are deprioritized
- Mobarok can begin reviewing TD this sprint if capacity allows; sprint focus begins next sprint
- Bilas has open questions about counter-offer assignment ranking (lowest offer vs. existing Match ranker conflict) — Lynsay to respond in the shared doc before TD proceeds
- See `program/meetings/2026-06-30-pex-lead-sync.md`

**Requirements doc reviewed by Chuck (Jul 7, 2026):** [Smart Dispatch: Service Territory Configuration](https://docs.google.com/document/d/1nCeyf8flKTxakMJEJ6C2BFotLG_HV368JhA4ErRA22Q/edit?tab=t.0#heading=h.vrj6wlcolyy6) (Product). Opt-in toggle + multi-select of buyer's existing service territories; WOs whose site matches a selected territory get SD, non-matches fall back to auto-dispatch or standard publish. MVP matching is zip-code lookup against the territory's configured zips. Driven by Clear Captions (sensitive relationship, wants single-territory pilot) and Interface (wants it without touching their integration setup).

**Confirmed with Chuck during review:**
- Build against **today's zip-code-based ST matching**, not the talent-pool-as-source-of-truth / H3 direction — ship fast for Clear Captions/Interface, accept the roster model may change underneath this later. Flag this explicitly in the TD so it's not a surprise during the future ST migration.
- This is **fully separate** from SLATE's 27-point "service territory configuration" Q3 priority (in-platform ST creation/setup, see `teams/SLATE/projects.md`) — no build dependency, just a naming collision. Worth a heads-up to whoever maintains the Q3 planning spreadsheet so the two aren't conflated.
- Segment scope (below) is genuinely open — not yet answered.

**Gaps/open questions surfaced (Jul 7, 2026) — need Product/eng alignment before TD locks in:**
1. **Segment scope undefined:** Doc is written entirely in ST terms, but V3 allows TP-only/MP-only projects with no ST segment. Does the territory restriction apply regardless of active segments, or only to ST-segment projects? Unresolved — needs Linda/Lynsay input.
2. **Fallback conflicts with V3 precedence + Jun 18 auto-dispatch-button-hiding decision:** doc says non-matching WOs fall back to "auto-dispatch (if configured) or standard publish." Unclear how this interacts with the fixed ST>TP>MP cascade (does it fall through to TP/MP first?) and with the existing rule that the auto-dispatch button is hidden whenever SD is enabled on a project — would need per-WO conditional UI state, which isn't covered by that decision.
3. **Manual WO creation bullet is ambiguously worded** in Req 2 ("check the zip code and remove Smart Dispatch with 'Auto Dispatch' on the work order create screen") — needs Product to clarify intended UI behavior before it goes into a TD.
4. **Zip-code/territory association race condition likely applies here too** — same class of issue already documented for RIMG's auto-trigger (Apr 10, see `knowledge/knowledge.md`). TD should explicitly reuse that wait-for-association handling rather than rediscovering it.
5. **"Without requiring a project association" (Req 2) is underspecified** for integration/REST-created WOs, given `project_dispatch_setting` is project-scoped today. Needs Product clarification on what's actually meant.
6. **No observability for the per-WO skip decision itself** — Req 3's Amplitude events only cover config changes, not matched-vs-skipped outcomes. Should likely feed the existing V3 "per-assignment tracking" observability bucket rather than being a silent, untracked path — otherwise no way to measure the Clear Captions pilot.
7. **Design not finalized** — Option 1 (show-all multi-select) vs. Option 2 (search-based, like Talent Pool Dispatch segment) — needs a design decision attached during the TD.
8. **Doc's own open question unanswered:** "how do we quickly unblock these two buyers" — worth asking Lynsay directly whether there's appetite for a staff-only interim stopgap (flag-gated, similar to the RIMG project-level toggle pattern) while the full self-serve UI is built.
9. **Downstream reporting gap (flagged Jul 7, 2026 eng leads sync):** If this setting suppresses Smart Dispatch for a territory-associated WO, does reporting still correctly distinguish Smart Dispatch vs. Auto Dispatch vs. manual assignment? Mobarok confirmed today's reporting simply doesn't fire if Smart Dispatch never initiates — no mechanism yet accounts for the new suppression path. Needs to go to Lindsay/data team. Assigned to "the group" to verify. See `program/meetings/2026-07-07-eng-leads-sync.md`.

**Integration flag dependency reaffirmed (Jul 7, 2026):** Chuck asked whether prior work adding a flag for integration customers reluctant to change their integration to enable Smart Dispatch is accounted for here. Mobarok and Bilas confirmed a one-time setup already exists on Field Nation's side (no buyer-side change needed) and this initiative will reuse the same "is Smart Dispatch enabled" check at the same existing endpoint — no new touchpoint expected, but Mobarok will double-check.

### Manual Assignment During Smart Dispatch → Buyer Override Controls (DoorDash request — June 4, 2026)

**SHIPPED - Cancel Smart Dispatch is live for all buyers (Jul 24, 2026).** Quiet release, positive early feedback from Interface. Code complete Jul 21 with Amplitude events; released Jul 23–24 behind the `SMART_DISPATCH_CANCELLED` flag, which was then removed (GARNET-816 cleanup, PRs `ui#8633` + `elasticlist#560`, merged Jul 31). This is the **first delivered H2 item**. Minor UX tweaks remain. See `program/meetings/2026-07-24-bf-weekly-working-group-alignment.md`.

**Status:** Technical design published (Bilas Sarker, Jul 2, 2026) — [Smart Dispatch: Buyer Override Controls](https://fieldnation.atlassian.net/wiki/spaces/TDR/pages/5959090221/Smart+Dispatch+Buyer+Override+Controls) (TDR space). Ready for review; not yet estimated/scheduled. This is also the design behind the "cancel-in-progress" item called out in the Jul 1 network-expansion sequencing (early Q3 buyer-requested enhancements) — see `knowledge/meetings/2026-07-01-smart-dispatch-network-expansion-review.md`.

DoorDash (and buyers generally) are locked out of assignment controls for the full dispatch window once Smart Dispatch is running — the only workaround (revert-to-draft) cancels all active provider requests. The TD scopes this into **two buyer override controls**, not just a cancel:

1. **Assign to Requester** — buyer directly assigns a provider who already submitted a request; terminates the dispatch run and withdraws other pending requests. Eligibility checks (PSS, distance, background checks) are NOT enforced for this buyer-initiated action.
2. **Cancel Smart Dispatch** — buyer stops the run entirely; WO returns to standard published state with all existing requests preserved; SD can be re-initiated immediately, no cooldown.

**Key design decisions (Bilas):**
- New DA REST endpoint `POST /v1/work-orders/:workOrderId/dispatches/:dispatchRunId/cancel`, called directly by the UI and proxied through FN-Core only for integration buyers — chosen over a new Kafka event so the buyer gets a synchronous 200/404 response (rejected the async-event option: EDA has no synchronous response path, which breaks both UI and integration-proxy callers).
- Two new terminal states: `dispatch_status = CANCELLED`, `outcome_status = BUYER_ABORTED`. Requires ENUM migrations on `dispatch_run_status_history`, `dispatch_run_outcome`, and `ext_workorder_dispatch_run`, plus new protobuf values (`DISPATCH_STATUS_CANCELLED`, `DISPATCH_OUTCOME_STATUS_BUYER_ABORTED`) and a Logstash/elasticlist update so the flightboard can filter on "Smart Dispatch Cancelled."
- No new notifications from DA on cancel — FN-Core's standard assignment notification already covers manual assignment; cancel is silent by design.
- Rollout behind new flag `SMART_DISPATCH_CANCELLED`.
- Applies to **all customers**, not just DoorDash.
- Original ~8pt swag estimate (Bilas, unvalidated) is superseded by this TD — no new estimate/date confirmed yet.
- Smart Dispatch warehouse reporting currently mirrors auto-dispatch (basic: providers matched, assignment=true) — intentionally minimal to allow asking more questions later. Josh Regan has time allocated later this month; Lynsay + Alyssa to define which tables/fields to add.

### Auto-Accept Counter Offers (Q3 initiative — TD published Jul 6, 2026)

**Status update (Jul 21 / Jul 28, 2026): deadline will slip.** Ridi confirmed Jul 21 that the auto-accept counter offer deadline slips; schedule-conflict and SD-cancellation milestones still held. Sequencing set Jul 28: **schedule conflicts first, then auto-accept counter offers.** **Shift** has asked for both auto-accept counter offers and territory-level pay rates. Chuck's Jul 7 pushback on the "select lowest-cost qualifying offer" logic (re-rank by Match/quality instead) is still unresolved in any source found.

**Status:** Technical design published (Bilas Sarker, Jul 6, 2026), status Draft — [Smart Dispatch: Auto Accept Counter Offers](https://fieldnation.atlassian.net/wiki/spaces/TDR/pages/6025478150/Smart+Dispatch+Auto+Accept+Counter+Offers) (TDR space, Linear GARNET-730, In Review). Shared alongside the Buyer Override Controls TD as one of two Q3 initiative docs posted to #garnet-eng. This is the "auto-accept counter offers within a buyer-set threshold" item referenced in the Jul 1 network-expansion sequencing (§6, RIMG ask).

**Problem:** Today there's no automated path to accept a pay-based counter offer during a Smart Dispatch run — it sits unreviewed until a buyer manually intervenes, usually after the dispatch window expires. Manual post-expiration acceptance isn't attributed to Smart Dispatch, understating fill-rate metrics. For RIMG, ~46% of work orders are filled via accepted counter offers, so this gap materially limits Smart Dispatch's value for that buyer.

**Proposed solution:** Opt-in, buyer-configured counter-offer threshold (percent above posted pay, or flat dollar amount above posted pay; disabled by default). When a ranking window closes with **no eligible standard requester**, DA evaluates counter offers; if one or more qualify (at/below threshold + meets all non-pay eligibility — background check, distance, PSS, quals, block lists), DA selects the lowest-cost qualifying one, calls the existing fn-core Accept API to assign the provider, and records the run as a Smart Dispatch success. **Precedence never inverts** — an eligible standard requester is always assigned first, even over a cheaper counter offer, so buyers never pay above posted rate when a standard option exists.

**Architecture:** No new service — extends the existing DA ranking loop via a new `CounterOfferAutoAcceptEvaluator` component (mirrors how `CancelDispatchRunService` was added for Buyer Override Controls). New columns on `project_dispatch_setting` (`autoAcceptCounterOfferEnabled`, `counterOfferThresholdType` [PERCENT/FLAT], `counterOfferThresholdValue`), snapshotted per run at run-start so mid-run config changes don't affect an in-progress run. Idempotency relies entirely on the pre-existing fn-core Accept API — DA doesn't re-implement dedup. Rollout flag: `SMART_DISPATCH_AUTO_ACCEPT_COUNTER_OFFERS`.

Not yet estimated or scheduled — TD is in review, open for comments.

**Ranking strategy challenged post-TD (Jul 7, 2026 eng leads sync):** Chuck reviewed the "select lowest-cost qualifying offer" logic above and pushed back — proposed instead taking all counter offers within the accepted threshold, re-ranking those providers by existing Match/quality ranking rules, and assigning the highest-ranked (not cheapest) one. Bilas had independently raised the same point with Lindsay before this meeting; she's checking with customer success/stakeholders. **Not yet decided** — TD may need revision depending on the outcome. See `teams/GARNET/knowledge/knowledge.md` (Open Questions) and `program/meetings/2026-07-07-eng-leads-sync.md`.

**Insufficient-budget edge case (Jul 7, 2026, unresolved):** Chuck flagged that the TD doesn't yet address what happens if the selected provider's counter offer exceeds available budget (e.g., pre-funded WOs) — fail to assign (today's behavior) vs. fall back to the next provider. Bilas to check normal (non-SD) WO behavior and ask Lindsay for the intended outcome.

**Kafka design confirmed (Jul 7, 2026):** Per EA team guidance, the counter-offer max-total-amount will be carried via the existing request Kafka event (as optional properties) rather than a new event, since counter offers and normal requests share the same request ID — avoids extra complexity. Chuck agreed.

## 4. RIMG Integration Pattern

**Status:** Active — temporary solution in build, target availability April 17, 2026

**Temporary solution (approved April 10, 2026):**
- Project-level auto-trigger: if project smart dispatch toggle is ON + new experiment flag is enabled for the buyer, work orders created via Client API (FN REST V2) automatically trigger smart dispatch instead of auto dispatch
- Two config changes required (one staff-facing, one engineering-facing) — no code changes on RIMG's side
- Tradeoff: ~300–500ms additional latency on WO creation, only for integration-created WOs for enrolled buyers
- Latency accepted as tolerable for temporary scope (Linda, John aligned)
- Error visibility: buyer must check fieldnation.com for dispatch errors — no history log for this temp solution
- Lifespan: 3–4 months. RIMG expected to migrate to the separate smart dispatch API once their Fujitsu integration rush is complete
- Testing: Dunder Mifflin demo mimicking RIMG's multi-project auto-dispatch setup before production enrollment

**Implementation detail (Bilas, Apr 10):**
- Feature flag checked in FnCore during WO creation
- Conditions: flag enabled AND project is Break/Fix Smart AND request source = Integration (ClientAPI) AND project dispatch toggle ON
- When triggered: publishes Kafka event to trigger Smart Dispatch via existing EDA pattern
- Auto Dispatch suppressed when SD triggered this way (if `auto_dispatch=true` AND `dispatch.type=SMART_DISPATCH` both sent, AD wins — safety valve)
- Latency breakdown: dispatch-settings API ~10ms avg P99 + Split check + outbox insert ≈ ~500ms total
- **Race condition with zip code association:** DA service can check zip code association progress status (`QUEUED`/`IN_PROGRESS`/`COMPLETED`/`ERROR`) and wait before processing. fn-core fires dispatch start event after ST association event. Needs validation under load.
- **Open question (Amran):** Can existing project SD toggle + Harness flag cover this use case without a new project-level field? Bilas to clarify.

**Timeline:**
- Build: April 14 (Tue)
- Test + release to lower: April 15 (Wed, after BD holiday)
- Production validation (Dunder Mifflin): April 16
- Available for RIMG: April 17 (Thu)

**Original tension (still relevant for long-term):**
- Current pattern: per-WO smart dispatch trigger returns `dispatch_run_id` for status tracking
- RIMG wants: project-level auto-trigger instead of per-WO API call (reduces their integration friction)
- Linda Lo's concern: losing per-WO visibility if we go project-level
- Chuck's concern: project-level means polling for failures vs. getting immediate per-WO response
- Only 2 buyers have significant integrated auto-dispatch volume today — robust long-term solution deferred unless adoption grows

## 5. Unisys WO Assignment Cap

**Status:** Prototype reviewed (Apr 15), pending workshop with Unisys AMs
**Driver:** Unisys wants smarter dispatch assignment limits; original ask was bundled dispatching (location-aware grouping), but team pivoted to operational workflow improvements as a lighter, incremental path.

### Design (Lightweight Prototype — Apr 15)

When all provider requests on a WO exceed the max assignment cap (flat 5/day per project/service territory), smart dispatch stops auto-assigning and brings the dispatcher back into the loop:

1. **Email notification** — lists requesting providers (current assignment count, distance, phone) plus other ST providers who didn't request. Links to WO detail page.
2. **Platform notification** — parallels email; can be used independently if email is turned off.
3. **Confirmation modal** — when manually assigning an over-limit provider, modal shows context (why WO expired/unassigned, current assignments) and requires acknowledgment before override.
4. **Flight board filter (proposed)** — new status filter (e.g., "max assignment exceeded") so dispatchers can find WOs needing manual intervention.

### Scope Boundaries

- **Flat cap only** — no per-provider or per-tier caps (e.g., 2 for new, 5 for experienced). Starting with flat 5, will evaluate tier-based caps based on workshop feedback.
- **Per-day only** — no time-of-day / concurrent schedule conflict resolution. Scooter flagged this as important for Clear Captions (hard-start PSS implications). Deferred to future phase.
- **No bundling logic** — Unisys's ask for location-proximity-aware dispatch grouping is a fundamentally different model FN doesn't support. Framing for workshop: incremental operational improvements while that bigger capability is evaluated for broader buyer demand.
- **Notification noise managed via user settings** — team agreed to include option to disable emails per user while keeping platform notifications.

### Next Steps

- Internal Unisys group review (Apr 16) — Linda presenting slides + prototype walkthrough
- Sign-off from Erica, Kaylee, Scooter
- Schedule dedicated workshop with Matt + 2 AMs (one open-minded, one process-attached)
- Workshop goal: validate whether this approach is incrementally or significantly better than current manual process

## 7. Service Territory Tagging (Jul 2026 - TD now required)

**Status (Jul 28, 2026): formal technical design required.** Direct reversal of the Jul 21 alignment.

- **Jul 21:** approved as a **lightweight single comma-separated DB field**, surfaced as tags in-platform, available to both buyer and internal users. Mobarok confirmed feasible; no TD deemed necessary. Alyssa Eby owned the one-pager.
- **Jul 28:** now requires a formal TD. Alyssa has a light requirements doc plus a prototype in shared Drive.

Sharpest reversal of the Jul 17–Aug 3 window. Worth understanding what changed between the two syncs before the TD is written.

## 8. Reporting Targets Phase 2 - PAUSED (original TD deleted)

**Status (Jul 21, 2026): paused.** Bilas revealed the **original Phase 2 technical design was previously deleted/removed**. The team must decide whether a new TD is needed or whether the existing priority-level foundation can support multiple targets per project via templates. Driven by **Black Box** and **ETS** needs. See `program/meetings/2026-07-21-pex-lead-sync.md` and the phased delivery plan in §3 above.

**Update (Aug 19, 2026 — H2 Features tracker):** Re-flagged as an "emerging priority" with a concrete driver date: **Black Box goes online Sept 1, 2026** and will need a solution "relatively quickly." Tracker status now shows **Solution Exploration** (up from paused/no-owner) with an action item to review solution options with stakeholders. No owner or estimate confirmed yet — worth Chuck checking whether this is being resourced given the Sept 1 date is close.

## 9. Project Budget (New — TDR surfaced Aug 13-14, 2026)

**Status:** In Review. Confluence TDR page 6147309569, owner **Md. Kamrul Talukder** (GARNET engineer). Not yet estimated/scheduled — Tickets section lists ~9 line items (data model, backend APIs, UI, DD logging/dashboard) but no point totals given.

**Problem:** Buyers have no native way to set a budget target on a project and no near-real-time view of spend against it while work is active — today they track it internally and lean on manual WOAR exports or a CSM-built "CVA" deck (6–10 hrs/report, lags field activity by 1–2 weeks). Driven by five discovery calls: DTiQ, NEC, F2Onsite, **Spectrotel** (B/F pipeline buyer), WorldLink.

**Design:** Budget is defined at the project level, broken into per-site targets (labor / travel / materials-additional / out-of-scope buckets); project target and actual are bottom-up rollups from sites. Buyers configure a **linked field** per site (custom field, saved location, or incident ID) so work orders are dynamically matched to sites at read time rather than through a persistent WO-to-site relationship. New fn-core (Monolith V2) tables `project_site` and `project_site_budget` (SCD2 history via `updated_by`/`updated_at`), new REST endpoints for site CRUD + mass import. Feature flag `PROJECT_BUDGET` (Harness, company-ID cardinality, default off), pilot-cohort rollout.

**Possible connection, not confirmed:** the "linked field to identify a site" mechanism reads similarly to the Jul 31 "project/client hierarchy" structural gap flagged in `program/overview.md` (buyers using custom fields as workarounds because the project/client structure is too granular) and to the still-team-TBD third "Break/Fix Management Suite" brief item ("new Break/Fix program build — buyers want multiple clients per project," see `program/references.md`). Worth Chuck confirming directly whether this TDR is that initiative's technical answer or a separate, parallel effort.

**Active review activity (Aug 13-14, 2026):** Atiqul Haque (GARNET engineer — distinct from the BD Principal Engineer candidate of the same first name, see disambiguation note in `program/people.md`) left several review comments: money fields are `FLOAT(7,2)` on both new tables, which risks rounding drift and caps any amount near $99,999.99 — recommends `DECIMAL`; the Mass Import flow (flagged as the highest-risk piece — background processing, retry/idempotency, partial-batch failure) needs a clearer diagram; and a note that the TDR still needs to add the mandatory Reporting section per **Mike Mueller's org-wide TDR-template mandate** (the same Jul 31 requirement that blocked SLATE's In-Platform Service Territory TDR — see `teams/SLATE/knowledge/knowledge.md`). Talukder replied that reporting / post-data-manipulation work is planned as a separate TD next quarter, not this one.

## 6. Network Expansion & Standardized Service Territories (Problem Framing)

**Full technical design published (Aug 19, 2026):** The "[Draft] Smart Dispatch Network Expansion" TDR stub (flagged Aug 17 as an empty template) is now a complete technical design — [Smart Dispatch Network Expansion](https://fieldnation.atlassian.net/wiki/spaces/TDR/pages/6167330821/Smart+Dispatch+Network+Expansion) (Confluence TDR page 6167330821, owner Ahtasham Billah Himel, Status: **DRAFT** — not yet reviewed/approved). This is the resolution of the Network Expansion problem-framing thread open since Jul 1.

**Core design:** Replaces buyer-owned static radius (`max_distance_miles`) with a system-computed **drive-time band ladder** (15 → 30 → 45 → 90 → 120 min) walked outward until a per-work-order supply target (default **15** criteria-eligible providers) is met. The whole plan (settled band, per-segment split, per-provider eligibility) is pre-calculated once at dispatch start via a new `SUPPLY_PLAN_RESOLUTION` event — rejected the alternative of each segment's timer reactively widening on its own, which required a race-condition lock and couldn't see other segments (named "the 200-mile pathology" in the doc).

**Notification vs. visibility now separated** — two new booleans (`eligible_for_notification`, `eligible_for_visibility`) replace the single disposition enum. Service territory members always get **visibility** into a WO (no distance limit), but only those inside the settled drive-time band (or territory-associated) get **notified**. Buyers stop configuring `max_distance_miles`; ST toggle on the settings screen remains the Smart Dispatch eligibility gate. Talent-pool-only buyers see a real behavior change — previously every eligible TP member was notified, now only those in-band, with the rest getting visibility-only.

**Scope/impact:** Major change in `dispatch-automation` (4 new tables, 1 new enum value), moderate in `fn-core` (no schema change — only narrows which provider IDs get passed to the alert path), none in `geospatial`/Provider Match. Assignment/ranking logic (tiered, 80/20, V3) is untouched — this only changes who gets notified. Rollout via new flag `SD_NETWORK_EXPANSION`: single internal buyer → 3-5 pilot B/F buyers → GA (flag-on by default for new projects).

**Active review already underway:** Habib Rahman posted a same-day comment thread scrutinizing the pool-composition formula (`notification set = (Input A territory ∩ criteria-eligible) ∪ Input B drive-time`) — this is a live technical review, not yet resolved/approved. Worth Chuck weighing in given his TD-review role and the "distance vs. territory subscription" precedence choice (drive-time wins when a provider qualifies both ways) is a real design call.

**Status update (Aug 18, 2026, Chuck/Ridi 1:1):** Ridi confirmed **Network Expansion work begins this sprint** - team is establishing the base for the work while the TD (the "[Draft] Smart Dispatch Network Expansion" stub, see Aug 17 entry below) is still under review. Continuing the Aug 4/Aug 6 open question on how to locate providers for expansion, the team is now awaiting data from MIA (coordinated through Lindsay) to run analysis and build **confidence scores** for mapping providers to service territories using home address vs. completed-work-order history - not yet resolved, but the first concrete next step since Aug 6. Same conversation surfaced a new, separately-tracked requirements doc ("Break/Fix Project: Talent Mapping," see `program/references.md`) proposing an auto-association model for this same provider-to-territory mapping problem, driven by Talent Pool coverage-reporting needs rather than network expansion directly - worth Chuck confirming whether these are the same effort or parallel tracks before they diverge. See `program/meetings/` (Chuck/Ridi 1:1 notes not yet filed as a standalone meeting note) and `program/overview.md`.

**New P0 item surfaced on H2 tracker (Aug 18, 2026): "Prevent Double Booking."** In Development, backend experiment without UI. Expands eligibility criteria: max assignments per day by tier, and prevent schedule conflicts (same start time or overlapping schedule blocks) - roughly 25 SP swag, Bilas. This is the named initiative behind the GARNET-802 "schedule-conflict-handling" PR #264 seen as routine chatter Aug 17 - not a new decision, but resolves what that PR was actually for. Likely the next iteration of the "advanced availability criteria" / hard-start double-booking work scoped Jul 14 below.

**Status update (Jul 28, 2026):** **Lynsay owns the dispatch network expansion TD.** Expansion breadth varies by territory type — wider in rural, narrower in metro. A Q3/Q4 delivery roadmap is needed before the TD can start. Separately, **DoorDash and Shift both report the ranking window causing assignment delays and duration-setting problems**, flagged as a potential Q4 item.

**Status:** Problem-framing stage — Product Review held Jul 1, 2026. No solution locked yet; standardized service territories work (pre-req) tentatively lands later Q3, network expansion development Q4.
**Meeting:** See `knowledge/meetings/2026-07-01-smart-dispatch-network-expansion-review.md`

**Problem statement:** Rigid service territory shapes prevent providers in adjacent territories from automatically seeing/being assigned nearby work orders, forcing buyers into multiple tiered custom selection rules (e.g., dispatch within 30mi, then 50mi, then wider) purely to approximate proximity-based dispatch — not because they need genuinely custom business logic. Standardized service territories (linking providers to territories, understanding supply density) are the proposed fix.

**Proximity vs. quality ranking — resolved conceptually (Jul 1):** Proximity governs *notification/dispatch order* (who gets notified first, expanding radius as needed) — it does not override quality-based ranking. Final assignment still runs through existing PSS/quality ranking once responses come in. This avoids the failure mode of assigning a closer-but-lower-quality provider over a farther-but-higher-quality one.

**Buyer feedback shaping sequencing (RMG Zones, Unicus, Clear Captions):**
- Launch Smart Dispatch to specific territories only, to A/B test against auto dispatch (not yet available)
- Cancel Smart Dispatch mid-run without waiting for the ranking window to close (builds trust in automation)
- Advanced availability criteria — see below
- Auto-accept counter offers within a buyer-set threshold (RMG)

**Advanced availability criteria (two distinct buyer needs):**
- **Unicus:** cap the number of work orders a provider can be assigned in a day, to prevent over-assignment/backouts. Coordinating with Reza and Jonah.
- **Clear Captions:** prevent double-booking — don't assign a provider already booked at that time slot.
- **Scoped as a single, non-configurable experiment for phase 1 (Jul 14, 2026 PEX Lead Sync):** Prevents provider assignment only when WO start times conflict (hard-start double-booking) — no buyer-facing UI/configuration yet. Bilas expects it ready to enable early next sprint (targets the Jul 27 release window, see §Q3 capacity above). Metrics (backouts, timeliness) will lag due to client onboarding timelines. Future iterations (drive-time padding, confidence scoring) depend on more data and Mia's involvement. See `program/meetings/2026-07-14-pex-lead-sync.md`.
- **Legal constraint:** caps cannot be set at the individual provider level (misclassification risk). Tier-level caps being considered instead. See also compliance open question in `knowledge/knowledge.md`.
- **Open gap:** same-site multiple work orders (a provider handling several WOs at one location) needs different handling than multiple WOs at different sites — not yet addressed, flagged by Shane Kinder as a separate problem.

**Cross-team alignment planned:** Chuck proposed aligning this proximity/dispatch logic with the Provider Match team — both for platform consistency and because the new approach is 100% database-driven vs. Provider Match's current crow-flies + Google Maps query pattern. Shane agreed; timing tied to Ashley's return Aug 3, 2026 when she resumes broader Provider Match ownership.

**Data science involvement (Mia):** Shane pushed to loop in data science proactively rather than continuing to add manual buyer-configurable parameters for each new scenario (risk: buyers end up with more dials than they can use effectively). Ryan noted the team is already moving toward a "good/better/best" UI to abstract away manual parameter-setting. Untapped data flagged as worth exploring: provider app login history, likely working windows, real-time/periodic location sampling — useful for both immediate dispatch and longer-term coverage planning.

**Open — talent pool vs. territory association model:** Whether providers get associated with service territories via talent-pool "push" or territory "pull" logic is still unresolved (Ryan), adjacent to but not yet decided within this effort.

**Sequencing (as of Jul 1, 2026):** Early Q3 — buyer-requested enhancements (cancel-in-progress, counter-offer acceptance, availability configuration). Later Q3 — standardized service territories + provider-to-territory linking. Q4 — core network expansion development.

**Formal TD stub created (Aug 17, 2026):** A new Confluence TDR page, "[Draft] Smart Dispatch Network Expansion" (page 6167330821, owner Ahtasham Billah Himel), now exists in the TDR space — matches Bilas's Aug 6 mention of "finalizing the TD with Himel." As of this sync it's an empty template (Background/Options/Definitions sections all placeholder, no content yet) — signals the design process has formally started, not that scope/estimate is locked.

**Driving-distance approach confirmed (Chuck/Bilas 1:1, Aug 6, 2026):** Resolves the Aug 4 open question below — Bilas confirmed the team is proceeding with driving-distance/isoline data (sourced from the geospatial service, feeding off the in-platform service territory H3 cell/cluster work) rather than straight-line distance or a radius, and Chuck agreed. No change to existing hard-filter or assignment logic — only how providers are found/notified changes; all changes logged as audit/history in the DB. Technical unknowns deferred to Q4. A matching driving-distance filter is planned for provider-match's "Match 2.2" phase next quarter, for parity with Smart Dispatch. Bilas is finalizing the TD with Himel; **review routing changes** — Tamvir (new BD Principal Engineer, confirmed this same meeting to cover both Break Fix and Granite) reviews first, then Chuck. Chuck's opt-in counter-suggestion from Aug 4 doesn't appear to have been revisited in this conversation. See `program/meetings/2026-08-06-chuck-bilas-1-1.md`.

**Driving-distance provider-location counter-proposal floated (Eng Leads Sync, Aug 4, 2026):** Bilas relayed Lindsay's idea to derive provider location from completed-work-order history plus driving distance for network expansion, rather than home address. Open question Bilas flagged: how to pick a center point for a provider with many disparate completed-work locations. His phase-one proposal: expand purely by driving distance from a work order's service territory center point (may pull in providers technically outside that territory - considered acceptable). Chuck's counter-suggestion: skip the computation and let providers directly opt into which service territories they're willing to work in. **No decision** - Chuck to think it over and give Bilas an answer separately; Bilas refining the proposal either way. See `program/meetings/2026-08-04-eng-leads-sync.md`.

**GARNET commits to owning demographic-to-hex mapping build (Jul 6, 2026):** In the weekly BF strategy deep dive, Lynsay confirmed GARNET will own building the mapping between demographic data (cost of living, population density, labor market class) and H3 hex resolutions — none exists today. Approach: use Jeff's existing zip-code-based service territories/demographic calculator as the baseline and overlay/average that data onto whichever hex resolution is chosen. Must be updatable over time (demographics shift — e.g., areas north of Dallas going rural-to-suburban). Also discussed: a "default territories as a temporary/virtual reporting layer" idea (Ryan) that would let FN compare coverage/fill rate/cost/quality consistently across a buyer's projects without touching manually-customized territory configs underneath — flagged as a candidate input for the Q3 BF-reporting 10-pt placeholder, though it likely doesn't fit Unicus's point-radius model. See `program/meetings/2026-07-06-weekly-bf-strategy-service-territory-deep-dive.md`.
