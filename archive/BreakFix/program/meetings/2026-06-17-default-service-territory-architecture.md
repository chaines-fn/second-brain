# Default Service Territory Architecture — 2026-06-17

**Date:** June 17, 2026
**Attendees:** Chuck Haines, Bilas Sarker, Mobarok Hossain

---

## Agenda

1. Why this exists — the problem we're solving (10 min)
2. What a service territory is (and isn't) (10 min)
3. Requirements for default territories (15 min)
4. Current architecture and the gap (10 min)
5. Proposed approach + key design decisions (20 min)
6. Sequencing and Q3 scope (10 min)

---

## 1. Why this exists — the problem we're solving

Context for Bilas and Mobarok who haven't been in the prior planning sessions.

**The operational problem today:**
- Building service territories for Break/Fix is entirely manual and offline — buyers or FN staff use spreadsheets and Maptitude to define coverage areas, then manually upload them
- The territory definitions use zip codes, which are noisy, overlapping, and don't reflect how providers actually travel
- "White space" — a work order lands in a zip with no territory match — blocks smart dispatch entirely
- Territories are rebuilt from scratch for every new project, even for repeat buyers. No reuse, no defaults
- Result: high FN staff cost to onboard new customers, slow project launches, and smart dispatch gaps in rural areas

**What we want to build:**
- A platform-native, nationwide default territory layer that FN maintains
- When a buyer sets up a Break/Fix project, territories snap in automatically — no manual mapping required
- Buyers can customize if needed, but shouldn't have to

**Why now:** V3 smart dispatch ships June 23 with talent pool support. Buyers using talent pools will have no coverage scores because talent pool → service territory linking doesn't exist yet. That's a Q3 build, and defaults are the foundation it sits on.

---

## 2. What a service territory is (and isn't)

This sounds obvious but has caused confusion — getting the definition right drives the architecture.

**A service territory is a geographic boundary.** That's it. It defines where — not who.

- It is *not* a provider list. Provider membership is a separate concern, derived from talent pools and filtering rules
- It is *not* a smart dispatch setting. It's the geographic lens dispatch uses
- Think of it as a named region with attributes (center point, radius or polygon boundary, pay rates, tier defaults)

**The four jobs it needs to serve:**
1. **Recruiting** — geographic boundary for NetDev to source and recruit providers for a given area
2. **Dispatching** — constraint for smart dispatch: send this WO to providers in this territory
3. **Reporting** — group work orders and performance metrics by geography
4. **Coverage analysis** — show buyers how well a territory is staffed before and during a project

**Important:** the geographic definition may need to behave differently per job. Reporting wants strict containment (WO either is or isn't in the territory). Dispatch and recruiting want expanding logic (grow the radius until you find enough providers).

---

## 3. Requirements for default territories

These are FN-owned, nationwide defaults — not buyer-defined territories.

**Must have:**
- Full national coverage with no gaps — every potential WO location falls in at least one territory
- Non-overlapping (or cleanly hierarchical) so reporting isn't double-counted
- Platform-native creation and management — no spreadsheets, no Maptitude
- A buyer can link their project to default territories without FN hand-holding
- Territory boundaries auto-update when the underlying model data changes (we don't want stale zip codes)
- Supports the two-signal serviceability model: a provider is eligible if they're within drive time of the territory center **OR** they've regularly worked in the area (work history ~last 6 months)

**Should have:**
- A hierarchy (~50 parent regions → ~300 metro territories → ~500–700 fill/rural territories) so mass attribute management works (e.g. update a rural pay rate across all fill territories at once)
- A visual map view for buyers to understand and interact with their territories

**Out of scope (later):**
- Buyer-defined custom territories
- Account-level shared territories that cascade across projects
- Blacklist/whitelist overrides (deferred to future phase)

---

## 4. Current architecture and the gap

Quick tour of relevant existing services so we have shared context before designing.

**`dynamic-talent-pools` (DTP) — SLATE**
- Manages talent pools and talent pool groups (= service territories today = coverage groups — naming is a mess, acknowledged)
- Already handles zip code association via Strategy Pattern (extensible to radius/polygon)
- Fires Kafka events on talent pool / territory changes
- MySQL

**`geospatial` — SLATE**
- Postal code validation, Redis-backed, monthly refresh from zip-codes.com
- PostGIS/Postgres in-database distance calculations (replaced old MySQL approach)
- Integrated with AWS Location for provider match
- **Proposed (not adopted):** extend this to power default territory generation — AWS Location `CalculateIsolines` to build drive-time boundaries, store as PostGIS polygons and/or H3 cell sets

**`dispatch-automation` — GARNET**
- Reads service territory assignments to know which providers to dispatch
- MySQL

**The gap:**
There is no bounded context today for FN-owned default service territories. DTP manages territories, but it was designed around *buyer-configured* territories with *providers directly attributed* to them. Default territories — which are purely geographic templates that exist independently of any buyer project — don't fit cleanly in DTP, especially if we want to move to a modern geospatial model. New service infrastructure is required regardless of which data model we choose.

---

## 5. Proposed approach + key design decisions

This is what I want to walk through together and get your input on. Nothing is locked yet.

### Approach overview

- **Default territories built from anchor points + drive-time boundaries.** We have a list of driving-radius centers already (Jeff Novotny's list from NetDev — ~50 metro anchor points to start). AWS Location `CalculateIsolines` generates the drive-time boundary from each anchor (e.g. 30-min, 60-min, 90-min drive). That polygon becomes the territory. Generated offline/batch — runtime is just a lookup.
- **Storage: PostGIS polygons for the boundary/display; H3 hex cells for membership, supply/demand aggregation, and the map editor.** Both in Postgres. H3 gives us a clean way to ask "which territory does this lat/lng fall in?" without a full polygon intersection every time.
- **Two-signal serviceability.** A provider is in a territory if they're within drive time of the center *or* they've historically worked in the area. Work history is roughly 2 years of WO data. This closes the white-space gap — rural providers who travel to serve a market get correctly attributed even if they live far away.
- **Service territories become a managed view, not a buyer-maintained list.** When a buyer links a talent pool to a project, providers auto-map to relevant territories via proximity rules. The territory roster is computed at dispatch time, not stored and maintained.

### Key design decisions to make today

**Decision 1: Where does this service live?**
Options:
- (a) Extend `geospatial` — already has PostGIS/Postgres and AWS Location integration
- (b) New microservice — cleanest bounded context, no coupling to existing services
- (c) Extend `DTP` — already owns the territory concept, but the MySQL/Postgres mismatch is a problem

My lean is (a) or (b). What do you think? The MySQL vs. Postgres split matters here — H3 requires Postgres, and DTP is MySQL.

**Decision 2: Zip codes vs. H3/ISO lines — which data model?**
I asked Mobarok and the team to spike this (June 2 meeting). What did the spikes show?
- ~80-85% of the infrastructure is shared either way (creation APIs, event publishing, storage service)
- Main deltas with H3: rework today's zip-based WO auto-association and a Postgres migration for the territory service
- My position: build it right from the start. Doing zip codes now means we redo it in 6 months.

**Decision 3: What events does the territory service consume and produce?**
- Needs to consume: talent pool membership changes, provider address changes
- Needs to produce: territory updated events (so dispatch-automation and coverage tooling stays current)
- How does this plug into the existing Kafka topology?

**Decision 4: Proxy coverage scores (short-term gap)**
V3 ships June 23. No TP→ST linking until Q3 = no coverage scores for talent-pool buyers.
- The proposal: implement auto-proximity proxy coverage scores *in-platform* (not at the reporting layer), so the recruitment tool and reporting inherit the same number
- What's the simplest thing we can ship fast? Probably: radius from territory center against talent pool providers
- Is this a few days of work or a sprint? That answer affects sequencing.

---

## 6. Sequencing and Q3 scope

Proposed sequence (validate with Bilas and Mobarok):

1. **Proxy coverage scores** — small, can ship quickly, closes the June 23 gap for buyers. Non-throwaway: the proximity logic is a subset of what the full build needs
2. **Service foundation** — standing up the territory service/context (wherever it lives), storage model, basic CRUD
3. **Territory generation** — anchor data ingestion, AWS Location integration, batch generation of nationwide defaults
4. **Event integration** — Kafka wiring so talent pool changes flow through to territory rosters
5. **Talent pool linking** — buyers link pools to projects, providers auto-map to territories

Account-level shared/cascading territories are explicitly out of Q3 scope.

**Open questions I want to walk out of this meeting with answered (or at least sized):**
- Which service owns this?
- Zip vs. H3 — decision or do we need one more spike?
- Are there AWS SA concerns we need to resolve first? (Route matrix at ~1,000 anchors, data caching rights for HERE-backed data)
- Rough effort for proxy coverage scores
- What are the dependencies that set the critical path?
