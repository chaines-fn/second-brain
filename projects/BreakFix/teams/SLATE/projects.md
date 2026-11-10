# SLATE Projects

## Q3 2026 Capacity & Scope (locked Jul 6, 2026)

**Target: 95 points** — 107 raw capacity, minus 30% of Mobarok Hossain's time for design work, minus a 10% buffer. Locked at the H2 Initiative Planning & Prioritization session (Amran Hossain). **Service territory configuration locked at 27 points, escalated to top priority** (sales/internal urgency surfaced the prior week). **Service territory enhancements + setup-needs bucket capped at 20 points** (down from a broader ask) — frees 12 points reallocated to in-platform service territory creation. Break/Fix reporting gets a 10-pt placeholder pending Alyssa Eby's scope confirmation. See `program/meetings/2026-07-06-h2-initiative-planning-prioritization.md`.

## 1. Dynamic Talent Pools (DTP)

**Service:** `fieldnation/dynamic-talent-pools`
**Status:** Active / In Production

Core service for managing provider networks. Handles:
- Talent pools and talent pool groups (= service territories = coverage groups — naming inconsistency acknowledged)
- Provider attributes for eligibility filtering
- Zip code association for service territory auto-matching
- Bulk CSV upload for zip code management (background processing via RabbitMQ)
- Strategy Pattern for territory matching — zip code today, radius/polygon extensible later

Key tables: `talent_pool_groups`, `talent_pool_group_mappings`, `talent_pool_provider_attributes`, `service_territory_postal_code_history`

## 2. Service Territory Zip Code Association

**Status:** Technical design in review (April 2026). **Note (May 29, 2026):** a proposal under exploration (see #6) would move the nationwide *default* territory model off zip codes; this zip-association path remains for current/custom-zip territories regardless.

Allows auto-matching a work order to a service territory based on zip code. "White space" = WO with no territory match, which blocks smart dispatch.

- Self-hosted zip code registry (zip-codes.com), monthly updates
- Bulk CSV upload via RabbitMQ background jobs
- geospatial service handles validation (`POST /api/v1/postal-codes/validate`, up to 5,000 codes/batch)
- Redis-backed, S3 source, K8s CronJob for monthly refresh

## 7. In-Platform Service Territory Creation & Talent Pool Mapping (Q3 2026)

**Status:** Scoping in progress as of the July 6 H2 Initiative Planning & Prioritization session. This is SLATE's largest Q3 initiative alongside service territory configuration (§ above / capacity summary). See `program/meetings/2026-07-06-h2-initiative-planning-prioritization.md`.

**In-platform service territory creation** — two pieces:
1. **Standardized service territories** — backend default territory definitions a buyer can copy from, rather than building from scratch. Ryan Schulte: must-have by end of Q3.
2. **In-platform creation workflow** — a "setup wizard" (Ryan's term, though he notes it's really something touched at project start or when adding new territories mid-project). Front-end scope deliberately kept light since staff will be the first users, not buyers.

Ryan and Mobarok agreed both pieces "could be done very simply and could be done in a very complicated way" — Ryan is writing a doc breaking out major vs. minor features so the team can draw an explicit line on what ships in Q3 vs. later. Mobarok's back-end starting point: ability to define a default service territory list that buyers can copy from.

**Update (Jul 9, 2026 — Service Territory Solution Discussion Cont., SLATE + GARNET prep for Jeff/Mia/Reswan demo):** Customization scope decided — no in-platform redraw/merge/slice tools; buyers map a site list to standardized default territories, with custom shapes still supported only via off-platform creation + import (today's model, unchanged). Ryan will scope the advanced customization tools anyway to get a concrete point estimate to show Jeff. H3 confirmed as the baseline geographic storage model (see `program/proposals/default-service-territory-geolocation.md` and §6 below) — Chuck's framing: if territories will eventually be drawn independent of zip lines, H3 storage is correct now even though defaults start zip-inspired. Open: no defined process yet for capturing future territory updates from Jeff's team in H3-compatible format (Reswan, data team, to be consulted). See `program/meetings/2026-07-09-service-territory-solution-discussion.md`.

**Update (Jul 31, 2026 - TDR declared INCOMPLETE by Mike Mueller):** Mobarok presented the In-Platform Service Territory TDR at the Jul 31 Technical Design Review. **Blocked on one item: reporting.** Mike's challenge - "I know we're going to be asked how many work orders were completed in this service territory in the last year" - requires answering that question against *today's* territory definition even though boundaries change over time. No solve in the doc. Mike has **added a Reporting section to the TDR template org-wide**; every team must now fill it out and align with their PM upfront. Amran added that the TDR should name which geospatial tables / reusable data products land in the data platform (this quarter targets existing DW tables; longer term served from the data platform).

**Design decisions from that review:**
- **Postal-code fallback on geocoding failure rejected.** If a buyer supplies an address and it's bad, reject the record so the human in the loop corrects it. Postal-code fallback retained **only when no address is supplied at all**.
- Territories are **polygon-based** (grouped postal codes), not radius circles. Lat/long retained for center-point/radius expansion; carries market class (rural/suburban/metro). ~80–90% of source spreadsheet columns used; `name2`, `ID`, `RPO` excluded.
- **Customized copies are point-in-time snapshots and never re-sync.** Schema adds a default-service-territory ID column plus a `customized` flag on the talent pool group / service territory table. Uncustomized copies may still receive updates.
- Default list is **static, republished by re-running the CLI script** when the internal team issues a new list. No partial updates.
- Geocoding via **AWS Location Service**, reusing the geospatial service NetDev recruiting already uses. Cache 1-hour LRU → 24-hour Redis; the 24-hour TTL traces back to **Chuck's earlier geospatial TDR**. Mobarok to add AWS cost tracking and revisit whether the TTL can be lengthened.
- API prefix **v2**, matching existing project and service territory endpoints.

See `program/meetings/2026-07-31-tdr-in-platform-service-territory.md`.

**Update (Aug 19, 2026 — H2 Features tracker):** Tracker now reads **"Technical design complete. Eng has started, first action item is to make associate providers to H3 cells to resolve dependency for NetDev Expansion."** Status moved to **In Development**. This is the first sign the Jul 31 Mike Mueller reporting blocker (see Aug 12 update below) has actually cleared — but that's an inference from the tracker note, not a confirmed sign-off from Mike; worth Chuck verifying directly rather than assuming the reporting objection is closed. Notable: the first engineering task is **associating providers to H3 cells**, framed as unblocking **NetDev Expansion** — ties this TD directly to the GARNET Network Expansion TD published the same day (see `teams/GARNET/projects.md` §6), which also depends on H3/geospatial data reaching resolution 8.

**Update (Aug 12, 2026 — Sony Sadi responds to Mike Mueller's Aug 10 reporting comments):** Obfuscated lat/long confirmed not needed; point-in-time territory tracking rejected per Product's feedback (reporting always evaluates a WO's location against the *current* default territory definition, not the one active at time of assignment); example queries added under the TDR's reporting section. Not yet confirmed by Mike as closing the Jul 31 reporting blocker — see `teams/SLATE/knowledge/knowledge.md` Open Questions.

**Update (Jul 28, 2026):** **In-platform territory editing explicitly ruled out of the current TD.** Territories continue to be imported via CLI script from CSV; an in-platform update tool was discussed and rejected for now - "Standardize, Don't Customize" holding by default. Also decided: with **Jeff Novotny OOO, the team moved forward on the enclosing-zip-code and service-territory-list placeholders rather than wait for his input**, generalized into a principle that blocked items proceed on placeholders. Jeff never re-engaged in any source found through Aug 3 - whether he accepted the Jul 14 "Standardize, Don't Customize" pitch remains unconfirmed.

**Update (Jul 21, 2026 - sprint planning):** the **service territory input-limit ticket was deliberately excluded** from the sprint despite rising urgency; Lynsay to take priority up with John. **Integration documentation update for service territories deferred** - the ST create/update/delete integration endpoints built 3–4+ months ago haven't been retested after substantial subsequent changes, so integration testing comes first and documentation follows, likely next sprint. No ticket exists yet. More buyers are onboarding to service territories and need to update their integrations, so this is a real gap. See `knowledge/meetings/2026-07-21-slate-sprint-planning.md`.

**Talent pool mapping to service territories** — the logical next step, but **out of Q3 scope**:
- Long-term vision (Ryan Schulte): talent pools become the single source of truth for providers; service territories layer on whitelist/blacklist overrides on top of talent-pool membership (aligned with the broader "talent pool as source of truth" direction — see `program/architecture.md` and the June 1 talent-pool/service-territory alignment).
- Today's model is folder-style manual assignment (providers manually added to a territory, like grouping into "my Minnesota folder"). Target state: automatic provider-to-territory attribution based on location, zip code, and work history.
- Bilas Chandra flagged open technical questions on how automatic geographic attribution interacts with talent pool data storage/mapping — needs a sync with Mobarok (action item from the July 6 meeting).
- Explicitly confirmed out of scope for Q3: whitelist/blacklist override logic and the full talent-pool-as-source-of-truth build. Standardized service territories is the Q3 prerequisite; this follows in Q4+.

**Update (Aug 12, 2026 — Field Week "Network Health Automation" session):** Direction reaffirmed and now has a milestone — talent pools drive dispatch, service territory associations retained for reporting. New: **migration mandatory** for accounts with existing providers in service territories upon smart dispatch activation; an **M2 milestone** targets standardized service-territory platform groundwork; proposed automation would trigger provider-to-territory association when a talent pool is added to dispatch settings (today's mapping relies on manual, error-prone queries). Also from that session: zip code polygons reaffirmed as the primary ST boundary method over H3 (new data point in the standing H3-vs-zip tension); coverage score prioritized as the first data product for platform integration, pending data-governance approval. See `program/meetings/2026-08-12-fw-network-health-automation.md`.

## 6. Default Service Territory Configuration — PROPOSAL (not adopted)

**Status:** Outstanding proposal, under exploration — not committed. Targets the Chuck/Ryan Q3 architecture session.

Proposed platform-native nationwide default territory layer to replace manual/offline (spreadsheet/Maptitude) builds: anchor + drive-time model (AWS Location isolines/route matrix on the existing geospatial service), territories stored as H3 cell sets / PostGIS polygons, with two-signal serviceability (drive-time reach or provider work history). Full summary, what's aligned vs. open, and next steps in `program/proposals/default-service-territory-geolocation.md`.

**Update (Jun 2, 2026 — Platform Arch meeting):** No final approach chosen. Team to run **technical spikes comparing zip codes vs. H3/ISO lines** to size the complexity delta before committing (Mobarok/Mubarak/Amran/Chuck). Chuck leans toward "build it right" from the start: ~80-85% of the infrastructure (creation APIs, cascade logic, storage service) is shared regardless of approach; the main deltas are reworking today's zip-based work-order auto-association and a likely move to **Postgres** (MySQL doesn't natively support H3). There's **no bounded context today for FN-owned default territories**, so new service infrastructure is required either way. Anchor points will reuse **Jeff's existing driving-radius center list**; AWS Location generates the drive-time boundaries. A **map-based buyer view** is needed regardless of the underlying data model. Scope confirmed as FN defaults (Q3); account-level shared/cascading territories remain a separate ~1-yr-out effort. See `program/meetings/2026-06-02-standardized-service-territories.md`.

**Update (Jul 6, 2026 — Weekly BF Strategy deep dive, Ryan/Jeff/Lynsay):** Working session on H3 mechanics ahead of engineering's own technical deep dive. Framed the problem as three separable sub-problems: (1) how to define a territory's shape, (2) how to define the "whitespace" area that has no shape yet, (3) how to modify a shape when sites/volume change (merge, split, shift center). **Whitespace/contiguity flagged as the hardest open problem** — today's zip-based defaults tile the map by construction (unclaimed zips just become whitespace); freeform H3/isoline shapes will leave gaps between territories with no agreed-on fix yet (options floated: larger sectioned whitespace regions, e.g. quadrants, vs. ad hoc whitespace shapes — manual effort to make shapes "touch" is a real concern). H3 resolution confirmed flexible from country-level (~14 hexes, res 7) down to city-block level (res 11); GARNET has generated a few standard resolutions but hasn't mapped any FN volume/demographic data onto them yet. Both Ryan and Jeff acknowledged neither fully understands H3/isoline tradeoffs yet — a knowledge gap to close before committing. Directionally, the group leans toward keeping ~1,200+ FN defaults stable/consistent under the hood and hiding buyer customization complexity in the UI/reporting layer instead (visual merging, folder-style rollup) rather than letting manual customization fragment the underlying defaults — not a final decision. See `program/meetings/2026-07-06-weekly-bf-strategy-service-territory-deep-dive.md`.

## 3. Service Territory Visual Editor

**Status:** Planned (weeks to months out as of Apr 2026)

Jeff Novotny wants in-platform visual editing of zip code shapes without needing external tools. Ryan Schulte confirmed platformization plan.

Jeff will provide documentation on key visual elements needed: territory shape, volume within territory, volume outside territory.

## 4. Smart Dispatch Config + Map View (Q2 2026)

**Status (June 30, 2026):** Service Territory Map View released to production, behind the `ST_MAP_VIEW` split flag — ready to enable per buyer. Demo video shared by Sony Sadi in #slate-pex.

**Status (May 21, 2026):** Configuration work complete. Currently provisioning Redis and background jobs in the display automation service. "Talent pool coverage counts" ticket deferred to next sprint — does not threaten the June 23 delivery target.

**Map View capacity planning:** Two remaining sprints at ~29 total story points (4 engineers at 85% after vacation).

| Scope | Target |
|-------|--------|
| Map view + metric view + territory side drawer + fit/finish | June (above capacity line) |
| PSS score display, driving distance, additional interactions | July (below capacity line) |

Amran flagged the timeline as tight overall. Ryan and Lynsay reviewing to potentially deprioritize PSS score + driving distance (~10 pts) to buy headroom. The map itself is already built (static AWS map rendering in service page); remaining work is interactions, data layers, and metric view below the map.

**Auto apply pay rates:** Follows smart dispatch config work; already in progress.

## 5. NetDev Recruitment Tool (formerly "Agentic AI Recruitment Tool")

**Status:** Active — 4-phase build confirmed; leadership aligned Apr 7 (Travis/Shane/Lisa)

### Email send limit — the dominant NetDev thread of late July 2026 (now decided)

**Problem (surfaced Jul 21):** Google Workspace hard-caps sends at **2,000/day**. Ryan's stated safe operating need is **10,000/day**. The workaround of spinning up multiple Google accounts was called "unsustainable." Escalated to Sr. Director level at the Jul 22 OKR Review (~1 month to implement a fix, **Andrew Kandels owns the TD**).

**Decisions (Jul 24 NetDev Tooling Connect - Travis Emslander, Shane Kinder, Lisa Sacquitne, Amran, Ryan, John, Lynsay, Ann-Bettina):**
- **Scale Andrew Kandels' new email send service** to permanently remove the cap. Its TDR passed the morning of Jul 24 with little rework; Travis expects an even simpler solve than Andrew originally scoped. **2–3 weeks of SLATE engineering.**
- **No interim SMTP solution.** SendGrid and second-Gmail-account options rejected — inbound reply parsing would need DNS/MX changes plus a new webhook endpoint, roughly a week of effort against a 2–3 week permanent fix.
- **Mitigation is operational:** batch outreach to ~500/day, stop dev-side bulk test sends.
- **Schedule hit scoped narrowly by Amran:** only the innovation-to-product TDR / architectural review slips. The main implementation does not. (This narrows John Shaw's opening framing that NetDev OKRs and roadmap were at risk for 2–3 weeks; he'd asked Cory about contractors.)
- **Port target firmed to Aug 24** (Jul 31 working group). Until then outreach paced across 4–5 days.

The underlying **"Email Receiving" TDR was not approved** — Travis pushed the design off new data persistence entirely toward a Kafka-publish model where each consuming service owns correlation and retention. See `program/meetings/2026-07-24-tdr-email-receiving.md` and `program/meetings/2026-07-24-netdev-tooling-connect.md`.

### Adoption metrics (first real numbers, Jul 24–31, 2026)

- **8 active recruitments across 5–6 buyers** as of Jul 24 (Zones, Connections, DoorDash overlap); down to 5–6 active by Jul 31.
- **Zones:** 357 providers in funnel, 20% conversion to approved, **27% reply rate**.
- **Connections:** 368 total, 315 still in progress.
- Aggregate (Jul 24 working group): 27% reply rate across 8 recruitments, 2,200 providers invited.
- Ryan: on track for **100% of new recruitments in the tool**, no remaining blockers to that goal.
- Analytics tab now shows per-territory filled-vs-target, % coverage, and **estimated time-to-fill** — new capability.

### Scope changes (Jul 17 → Jul 31, 2026)

- **NetDev AI roadmap item removed** from the current schedule (Jul 17) so MARBLE and Core Transform can establish internal AI patterns/tooling this quarter first, rather than integrating a new AI feature prematurely.
- **Stages + provider quizzes scope split (Jul 31).** Ryan proposed deferring **four items** on the cut list, including technical design and prep work for the buyer experience, to avoid derailing the quarter. Stages/quizzes are "the last core functional features required before automation."
- Next two weeks of feature work (from Jul 24) is **stage gating only** - provider-submitted forms/files route direct to a stage, replacing the Google Form swivel-chair. Email attachments deliberately left undecided; Ryan doesn't want to over-invest in email when stages plus the Q4 mobile provider experience will carry the load.
- **Q4 mobile launch scope now includes provider onboarding that captures work preferences** (short notice, overnight, travel, W2 vs. 1099), stored initially as plain text, to feed double-funnel search and eventual algorithmic recruitment matching. Shane pushed for a raw-text provider attribute field sooner so data isn't lost; Ryan held the line on it being part of provider-experience v1 in Q4.
- **Amran restructured NetDev OKRs (Jul 21):** P1/P2 separated; now two milestones for tool development and two for architectural review.

### Open gaps (Jul 24, 2026)

- **Priority designation is hardcoded** into Alyssa's reporting artifact for apples-to-apples comparison, not built into the platform, so the data can't flow both ways (John Shaw). High/medium/low priority is staff-set, not progress-derived — Shane confirmed that matches the B/F need to sequence territories by incoming volume.
- **No territory ID join.** Shane wants aggregate time-to-fill and response-rate benchmarks per standard territory so NetDev can tell a buyer up front which territories fill fast. Doable manually today only via **name-text-matching CSV exports** — a real data-model gap if this becomes a product commitment.
- **New cross-team dependency (Jul 31):** backend data issues for assets/equipment on the provider profile require work from **Quartz or GARNET**; delivery may be delayed though SLATE's own scope is unaffected.

Originally explored as a full-funnel AI recruitment tool. Went through several pivots (HubSpot → managed recruitments → chatbot POC → provider sourcing tool). Now reframed as a phased build that establishes a usable foundation first, then layers in automation and AI.

**Priority framing:** (1) usable tool that makes NetDev team more efficient; (2) AI and automation. This is an *automated* tool where AI may be the right solution for some parts and regular logic handles others — not an AI-first tool.

### Phase 1 — Provider Sourcing *(in progress, ~sprint ending Apr 21)*
Internal tool for NetDev staff to search providers by basic criteria. Output: CSV with provider data (PSS screening, nearby territory/site info). No outreach yet.

### Phase 2 — MVP End-to-End Recruitment Tool *(kicking off Apr 15 sprint; overlaps with Phase 1 wrap-up)*
Bare minimum for a complete recruitment cycle:
- Bulk invite via provider ID or CSV
- Branded email invites (AI for writing/timing is optional)
- Opt-out management
- Pass/fail vetting — providers reply to email; replies thread into tool; staff reviews and clicks pass/fail
- Staff can send follow-up questions
- Basic CSV export (email, PSS, invite date, responses)
- No auto-placement into talent pools/service territories — CSV import handles that
- **Success criteria:** NetDev can run a full recruitment end-to-end without Google Sheets or ad-hoc tools

**Phase 2 execution decisions (Apr 13):**
- **Capacity:** 30-point cap (traditional pointing) — hard boundary for Q2; building beyond defers other SLATE initiatives
- **UI approach:** Convert existing HTML/JSX prototype using FN React best practices; 1-2 person job, estimated <1 day to working UX. One person owns UI; others focus on API shaping and backend.
- **Data scope:** CSV-only for Phase 2 — show provider data available from CSV import (PSS, distance, point-in-time WO count, background check, drug test). No new FN API integration dependency.
- **Geolocation:** CBSA omitted; using radius-based approach with center-point address + radius. Optional "territory" column in CSV serves as grouping label only (no zip code mapping).
- **Resourcing:** Whole team assigned starting Apr 15 sprint. Engineering estimation of what fits in 30 points due Apr 15.
- **Prototype package:** Ryan sending HTML + JSX + docs for team to begin work.

### Phase 3 — Staff & Buyer Tooling
Recurring automated reminders (every 3–5 days, ~5 sends), internal stages with sub-statuses, coverage targets per geography, flexible stage types (quiz, deliverables, manual), enhanced metrics (time-to-complete, territory conversion rates), buyer access to review progress and approve manual stages.

### Phase 4 — Provider Experience & Advanced AI
Mobile-responsive provider UI + dashboard, integrated message center with escalation path, shared staff inbox, service company recruitment workflow, conversational AI screening (reads responses/docs), auto-reply/FAQ handling, multi-channel drip campaigns.

**Weekly sync:** John Shaw to set up regular weekly cadence with Travis/Shane/Lisa/Ryan/Chuck to track delivery progress and surface risks early. (Apr 7)

**Open question — "innovation project" status: path forward emerging (Jul 2 → Jul 8, 2026):** Chuck raised (twice, in the H2 Vision & Strategy Overview) that NetDev is still being run as an "innovation project" even as it starts integrating with the core platform, bypassing normal product TDR/process rigor. John Shaw agreed there's no defined criteria yet for when it "graduates" to a formal roadmap item. Amran separately asked whether NetDev resources (Safin, "Jamu") count against SLATE/BD headcount or run as a separate initiative; John's answer: continue as-is through Q3, revisit resourcing once the tool is more tied into the platform. See `program/meetings/2026-07-02-h2-vision-strategy-overview.md`.

**Update (Jul 8, 2026 — Chuck/Ridi/Bilas check-in):** Chuck laid out the case at length: innovation projects are meant to be self-contained and move fast without normal TD rigor, and the moment reporting/integrations get built on top of one, it needs to become a real product with a roadmap. Bilas agreed independently, adding the team isn't even confident about server stability yet. Specific architectural debt flagged: (1) NetDev's front end is bundled into a single Go binary with the backend rather than deployed self-contained via the newer core UI architecture; (2) provider data is imported as a static snapshot with no event-driven updates. Mobile responsiveness deprioritized for now (internal-only tool, staff use laptops/tablets). Plan: Chuck drafts a DM outline, Ridi + Sadat refine it and send to John Shaw via the BFEX leads Slack channel. Chuck expects possible friction from Ryan Schulte and said this could need Cory Muir/Shane Kinder-level resolution — no innovation project has "graduated" to a full product before, so there's no precedent. See `program/meetings/2026-07-08-chuck-ridi-bilas-checkin.md`.

**Resolved (Jul 9, 2026 — Innovation to Product Transition Plan Planning Session):** Group aligned on a path forward. **Productization threshold:** an innovation project stays one as long as it's self-contained; crossing into "product" happens when it needs platform integration (mobile app access, or pushing/pulling data with the rest of the platform in a way that requires ongoing maintenance/infrastructure). Below that line, work continues at innovation speed. **Decomposition approach:** strangler pattern — incrementally replace bespoke components (starting with the bespoke email sender) with standard architecture while innovation work continues in parallel; not a rebuild. **Data integration standard:** one-way pushes into talent pools/service territories are fine to build now; on-demand pulls used only for UI display are fine via direct API; pulls that drive backend decisions should be event-driven (same pattern as dispatch-automation). **Sunset plan confirmed:** legacy "Manage Recruitment" tool gets no further investment and will eventually be sunset once the new tool is ready to fully replace it; the two coexist until then. **First architectural step:** assess feasibility of decoupling the NetDev UI from the current Go binary into core UI. Buyers will never access the new tool directly — buyer interaction stays inside the FN platform via data push/pull (Ryan mapping out the specifics). Architecture review targeted for July/August; Safin's capacity shifting accordingly. See `program/meetings/2026-07-09-innovation-to-product-transition-planning.md`.

**Next steps assigned:** Shafi drafts a current-state architecture diagram; Chuck reviews the resulting architecture proposal; Safin produces a formal technical design for broader engineering review; Ryan plans the buyer-experience push/pull strategy; Safin + Bilas coordinate on the Marble-side provider experience roadmap for Q3.

**Update (Aug 21, 2026 — Technical Design Review + NetDev recruitment standup):** Safin's productization TD (from the Aug 13 architectural-review TD) was reviewed and split into **two separate TDs** — tool consolidation (merging the Phase 1/2 tools into the new "golden go" app) and productization — to cut document length/confusion. Architecture decisions from the TDR session (Corbin Munger, Al Hasan, Mike Mueller, Chuck): migrate the email service from **Gmail SMTP to AWS ACS/SES**; deprecate **SQLite** in favor of MySQL everywhere; drop **SQS** (tied to the legacy Python sourcing tool); call qualification/certification APIs directly rather than through wrapper APIs; use Kafka events to sync provider destinations to downstream services; legacy CSV import tables won't carry into the new app. Mike Mueller's reporting mandate extends here too — in-app analytics aren't sufficient long-term, data needs to export to the platform for BI/Tableau. **New open risk (Chuck):** free-text pay rates in the recruitment app could create discrepancies with in-platform service territory data once default service territories exist — Safin to consult Product on both this and how in-platform ST defaults should interact with the tool. Separately, at the same-morning NetDev recruitment standup (Ryan, Almansur, Safin, Chuck, Amran, Lynsay): **the NetDev email-cap permanent-fix port target slips from Aug 24 to Aug 31** (email service integration testing delayed); buyer-facing experience stays inside the buyer-facing tool; legacy "Manage Recruitment" tool sunset delayed until the new buyer-facing experience ships; only 6 of 200 historical recruitments are still active, so the team leans toward starting fresh in the warehouse rather than migrating legacy data; platform sync worker (12-hr cron) targets test completion Aug 25; keyword search scope narrowed to title/description fields only. New meeting note: `program/meetings/2026-08-21-tdr-netdev-productization-review.md`.

**Update (Aug 25, 2026 — tool-consolidation TD published):** The tool-consolidation TD split off Aug 21 above is now published on its own Confluence page — "Netdev Recruitments Provider Sourcing Pipeline Migration and Workflow Consolidation" (Safin Ahmed). Confirms the plan to merge Phase 1 (`netdev-ai-poc`, Python/FastAPI provider sourcing) and Phase 2 (`netdev-recruitments`, Go/Fx outreach/funnel) into one app — recruiters currently bridge the two manually via CSV export/import. Not yet reviewed by Chuck as of this sync.

**Resolved (Aug 13, 2026 — TD published):** Safin Ahmed published "Netdev Innovation to Product Transition: Architectural Review" (Confluence TDR page 6113329156) — the formal technical design assigned above. Confirms NetDev has grown, as an innovation project, into two production-deployed tools (Phase 1 `netdev-ai-poc`, Phase 2 `netdev-recruitments-tool`) further along than "POC"/"innovation" implies. This is the concrete deliverable that resolves the "innovation project" status question raised repeatedly since Jul 2. Not yet reviewed by Chuck as of this sync.

**Estimation notes (Apr 6):**
- Campaign build engine: 10 pts (wizard-style creator 5pt + rich media from scratch 5pt)
- Dynamic stage/workflow builder: 5pt drag-and-drop + 8pt advanced stages + 3pt automated if/else logic
- Untimed assessment: removed from scope (no timed assessments in new tool = nothing to build an "untimed" version of)
- ~90% of prior estimation work still applicable; needs reorganization into phases
- Next step: Safin to provide detailed Phase 2 breakdown; Phases 3–4 can be loose estimates for now

**Background context:**
Phase 1 (original) intent was provider matching against hard requirements (PSS, background check, screenings). Key insight still holds: provider *work location* is a stronger recruitment signal than home address.

**Terminology Jeff cares about:**
- "Recruitment" = buyer-specific, tied to a specific buyer's need (NOT "campaign" in buyer-facing UI)
- "Campaign" = broad marketplace development (e.g., driving up background check % network-wide)
- Note: the tool internally uses "campaign" for the recruitment workflow — watch for friction here as it matures
