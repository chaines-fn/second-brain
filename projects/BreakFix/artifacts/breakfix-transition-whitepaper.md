# Break/Fix Program: Architecture & Engineering Whitepaper

**Prepared by:** Chuck Haines, Sr. Principal Software Engineer, Field Nation
**Date:** August 6, 2026
**Purpose:** Get you oriented on Break/Fix's architecture and engineering state fast. This is the stuff that isn't written down anywhere else in one place.

## How to use this doc

This is a snapshot as of early August 2026. Break/Fix moves fast and plenty of what's below will be stale in a few weeks. Where I can, I've pointed at the living workspace docs that get updated continuously, so treat those as the source of truth once this doc starts to feel dated. The full workspace lives at `BreakFix/` with a `CLAUDE.md` file that maps everything out.

Read this straight through once. Then use it as a reference.

## 1. What Break/Fix is and why it matters

Break/fix is reactive field service: something breaks, we send a technician to fix it. High volume, low value per ticket, tight SLAs (1 to 2 hour assignment windows), 14,000+ work orders a month. The Break/Fix program is building an intelligent, automated fulfillment engine for this segment, moving Field Nation beyond simple placement into managed service delivery. That's the competitive bet: buyers want predictable fulfillment at scale, and doing that by hand doesn't scale.

Three workstreams, three teams:

| Team | Workstream | What they own |
|------|-----------|----------------|
| **SLATE** | Network Development (NetDev) | Recruiting and vetting providers into curated Break/Fix networks, plus service territory infrastructure |
| **GARNET** | Smart Dispatch & Fulfillment | The automated dispatch engine: tier-based routing, marketplace expansion, provider ranking |
| **MARBLE** | Parts Management | Parts logistics: tracking, returns, provider-added parts (winding down, see the MARBLE deep dive below) |

Roadmap, in theory: Phase 1 (Q4 2025) was basic tier-based dispatch. Phase 2 (H1 2026, in progress now) is marketplace expansion, predictions, and models. Phase 3 (H2 2026) is continuous learning and cross-program coordination. In practice, the roadmap has been reshaped repeatedly by buyer pressure and by the service territory work described below, so treat phase boundaries as directional, not a schedule.

Beta and live customers as of early August: One Diversified, IBM, RIMG, ClearCaptions, and Zones are live on Smart Dispatch. AVI-SPL is mid-pilot (8/11 to 8/25, GA targeted October). PlayOn, Essential Enterprises, Closed Circuit, and Shift are in the beta pipeline. There's a long tail of prospective buyers (Black Box, ETS, CapsaHealthcare, Otodata, and others) behind that. Smart Dispatch success rate was measured at 45% as of the July 31 Tableau dashboard, the first hard number anyone's published on it.

## 2. Architecture at a glance

### Key services

**fn-core** is the monolith and source of truth for work orders, providers, and the core platform. Smart dispatch is triggered from here via `POST v2/workorders/smart-dispatch`.

**dispatch-automation** (`fieldnation/dispatch-automation`, GARNET, Node/TypeScript) is the orchestration engine for smart dispatch: dispatch runs, publish groups, provider ranking, marketplace expansion.

**dynamic-talent-pools** (`fieldnation/dynamic-talent-pools`, SLATE, aka DTP) manages talent pools and talent pool groups (these are the same thing as service territories and coverage groups under the hood, a naming inconsistency everyone's aware of and nobody's fully fixed).

**provider-match** is the existing event-driven service that matches providers to work orders on skills, location, and attributes. Owns assignment transparency data for all assignment types.

**geospatial** (SLATE, newer) handles postal code validation and is becoming the home for the H3/PostGIS geospatial work described below.

### Event architecture

Everything communicates over Kafka. Naming convention: `.CMD` suffix means "do this," `.INT` suffix means "something happened, FYI." Protobuf schemas live in the `schema-registry` repo. Key topics cover talent pool and service territory changes, dispatch run lifecycle, and work order dispatch/assignment events.

### The thing you need to know on day one: Kafka connector instability

This has been a known, unresolved problem since at least April 2026. Roughly 7 to 8 out of every 10 smart dispatch incidents trace back to Kafka message sync failures between DTP and dispatch-automation. No root cause has been found despite multiple rounds of collaborative work with the Diamond platform team. The workaround is restarting the affected connector pod, which is not a fix. Ownership of a permanent solution has never been assigned. If you pick one thing to push on early, this is a strong candidate: it's been quietly costing the program reliability for months and nobody owns it.

### Patterns worth internalizing

SCD Type 2 for history tables (`effective_from`/`effective_to`, `9999-12-31` for open records). Outbox pattern for Kafka publishing (though as you'll see in the service territory TDR review referenced below, not everyone's followed it). Split.io feature flags at the company level for rollout. Correlation and causation IDs on v2 events. Strategy Pattern for service territory matching (zip code today, designed to be extensible to radius or polygon).

## 3. Current state as of August 6, 2026

This is the stuff that was live and moving fast at the moment I wrote this. It will be stale soon; check the workspace's `program/overview.md` for the latest.

**Service territory configuration is the single biggest live thread and it has slipped three times.** Originally targeted July 27, it's moved to "early August," then "early next sprint," then "entering fit-and-finish." The most recent tracker entry (August 4) shows a target launch date of August 10 at 90% engineering readiness, but that hasn't been corroborated by an actual working-group meeting yet. The three named causes of the slip: converting service territory association from async to synchronous (also fixes a pay-rate bug), map view load performance at scale (DoorDash has up to 1,200 territories and the map is currently unusable at that count), and the data platform ingestion design. Work is split 21 points GARNET / 17 points SLATE.

**Every technical design now requires a Reporting section.** This came out of the In-Platform Service Territory design, which couldn't answer a basic reporting question, how many work orders were completed in a given service territory over the last year, against a territory definition that changes over time. The fix going forward: new designs need to state up front which reportable questions they support and which data products/tables serve them, rather than leaving reporting to be reverse-engineered later. I did a full architectural review of that specific design on August 3 (`teams/SLATE/knowledge/2026-08-03-tdr-review-in-platform-service-territory-creation.md`); worth reading in full since it surfaces several real risks you'll want to know about, including a dual write from DTP to Kafka with no outbox and no reconciliation on exactly the kind of path that's already unreliable (see the Kafka section above), and an H3 resolution that's never actually stated anywhere in the design.

**Cancel Smart Dispatch shipped to all buyers July 24,** the first delivered H2 item, quiet release, positive feedback so far.

**MARBLE is winding down.** Parts logistics engineering concludes end of August. Parts return and provider-added parts ship together August 31. That engineering capacity moves to buyer analytics, with two MARBLE folks (Sadat Haque and Tauhedul Shagor) reassigned to support the AVI-SPL pilot.

**NetDev's email send cap got resolved.** Google Workspace was hard-capping sends at 2,000/day against a stated need of 10,000/day. Decision: scale the internal email send service permanently (no interim SMTP band-aid), target port date August 24.

**Capacity accounting changed from story points to an 85% dev / 15% keep-the-lights-on ratio** as of July 28. Any story-point figures you see from earlier in the summer are stale as a planning tool.

## 4. The service territory saga (read this one carefully)

If there's one architectural thread that's dominated the last four months and will keep dominating for a while, it's this. Giving it its own section because the history matters for understanding why current decisions look the way they do.

**The core tension:** service territories were originally just zip code lists. That's fragile (zip codes vary wildly in size, don't reflect drive time, and create overlapping/noisy coverage), so the team has spent since May exploring a move to H3 hexagonal cells with AWS Location isolines for drive-time boundaries. This is architecturally the right call in my view (see the proposal at `program/proposals/default-service-territory-geolocation.md`), but it's expensive and the requirements have shifted more than once as the direction was validated with internal stakeholders.

**Where it's landed, as best I can tell:** H3 as the storage and computation layer, confirmed June 17 (MySQL doesn't support H3, so this also means a move to Postgres for the relevant service). Zip codes remain as a buyer-facing input format and a per-work-order pay-rate anchor, not an ongoing parallel computation layer. "Standardize, Don't Customize" is the stated direction: buyers use Field Nation's roughly 1,200 default territories as-is, or bring a fully custom definition built off-platform and imported. No in-platform tool to redraw or reshape default territories.

**What's genuinely still open:** whether the "H3 for visualization, zip for computation" split that's been restated in several meetings is an actual final decision or just an unexamined habit everyone keeps repeating. Worth pinning down explicitly rather than letting it keep being implicitly re-confirmed, since it affects how much you should invest in either representation going forward.

**Adjacent and related, not the same thing:** GARNET has a separate initiative (renamed five times, currently "Smart Dispatch Opt-In Service Territories") that gates *whether Smart Dispatch is enabled* for a given territory, independent of how territories are defined. It's a real and confusing naming collision with SLATE's "service territory configuration" work; they don't have a build dependency on each other, just a shared vocabulary.

## 5. Team deep dives

### SLATE (Network Development)

**Mission:** build tools for the internal NetDev team to recruit and vet providers into curated networks, and own the service territory infrastructure described above.

**People:** Ryan Schulte and Ann-Bettina Colace (PM), Mobarok Hossain (Engineering Lead), Fayshal Uddin and Almansur Siddiqui (engineers). Jeff Novotny is the primary internal stakeholder for NetDev requirements; Laura Middleton owns vetting criteria.

**What's active:**

The **NetDev Recruitment Tool** is a phased build (foundation first, AI later) that's replacing a legacy "Manage Recruitment" tool. It's had several early pivots (HubSpot, then chatbot, then provider sourcing tool) before settling into this phased approach. It recently crossed the line from "innovation project" to a real product with normal technical design rigor, since it now needs platform integration. Adoption numbers are starting to come in (roughly 27% reply rate across recent recruitments, on track for 100% of new recruitments running through the tool).

**In-platform service territory creation** is SLATE's other big Q3 initiative, locked at 27 points as top priority. This is the work behind the TDR reviewed in section 3.

Watch for: the "project/client hierarchy" structural gap flagged by CS in late July (buyers using custom fields as workarounds because the project/client model is too granular), deferred to a cross-team "Field Week" discussion.

### GARNET (Smart Dispatch & Fulfillment)

**Mission:** the automated dispatch engine itself, tier-based routing, marketplace expansion, provider ranking.

**People:** Lynsay Faucett (PM), Ridi Haque (Engineering Manager, also covers MARBLE), Bilas Sarker (Engineering Lead, goes by Sarker despite an email that says Chandra, don't "fix" it), plus a sizeable BD engineering team (Safin Ahmed, Kamrul Talukder, Monjurul Hoque, Sony Sadi, Atiqul Haque, Habib Rahman, and others). Abu Saleh Matul joined in July and has been carrying the opt-in service territory TDR.

**What's active:**

**Smart Dispatch V3** is the core in-flight work: talent pool segments, buyer-configurable eligibility criteria (floor-based hard cutoffs, not weighted ranking, for the initial release), and the ST/TP/MP segment model. Ranking uses default Match 2.0 within the eligible pool; true buyer-configurable priority ranking was explicitly deferred after the team couldn't agree what it would even mean technically.

**Network Expansion & Standardized Service Territories** is the bigger, slower-moving problem: rigid territory shapes prevent providers in adjacent territories from being dispatched to nearby work automatically. Time-to-fill has been elevated to a primary Smart Dispatch metric (not secondary) specifically to force this problem to get solved rather than papered over with more configuration options. Sequencing: early Q3 buyer-requested fixes (cancel-in-progress, counter-offer acceptance, availability config), later Q3 standardized territories, Q4 the actual network expansion build.

**Auto Dispatch retirement** has a stated target of Q1 2027 for the first time (previously undated). Customer Service Reps (CSRs) are the biggest named blocker: they're heavily relied on for today's auto dispatch and also used as a ranking filter, so removing that dependency is real work, not just a flag flip.

Watch for: the still-unresolved question of whether the Auto-Accept Counter Offers design should select the cheapest qualifying counter offer or re-rank qualifying offers by quality first (not yet decided), and the API response size question for very-high-territory-count buyers (700+) that's been open since mid-July.

### MARBLE (Parts Management)

**Mission:** parts logistics (schedule coordination, shipment tracking, returns, provider-added parts). Winding down, as covered in section 3.

**People:** Sadat Haque (PM), Ridi Haque (also EM for GARNET). Alyssa Eby and Tauhedul Shagor have moved through UX roles here on their way to GARNET and AVI-SPL respectively.

**What's active, on the way out:** Part Return Experience and Provider-Added Parts ship together August 31, closing out the team's core Q2/Q3 commitments. RabbitMQ is being removed from the parts and shipment service in favor of Kafka as part of that final release.

**One thing worth knowing regardless of the wind-down:** MARBLE found and fixed a real JWT authentication bypass vulnerability in June, exposed via a verbose error response and also via a production load balancer bypassing the API gateway. Both were closed, but it's worth a spot-check that nothing else fronts a load balancer directly without going through the gateway, since that was the actual exposure vector.

**Also unresolved:** the return trips / revisit initiative is paused pending a broader redesign, and current revisit models are flagged as unreliable with no architecture owner named. This is a real, scoped gap if you're looking for a concrete area to dig into.

## 6. Key people

| Name | Role | Why they matter to you |
|------|------|------------------------|
| **Cory Muir** | Sr. Director, Engineering | My manager |
| **Mike Mueller** | Principal Engineer | Architectural guidance across event-driven patterns and Match 2.0; owns the org-wide TDR reporting standard mentioned in section 3 |
| **John Shaw** | Group PM | Owns cross-team program alignment, reports to Lisa Sacquitne |
| **Amran Hossain** | Engineering Manager | Oversees delivery across all three BD teams |
| **Ridi Haque** | Engineering Manager | GARNET and MARBLE |
| **Mobarok Hossain** | Engineering Lead, SLATE | Owns the service territory TDR and most of the H3/geospatial work |
| **Bilas Sarker** | Engineering Lead, GARNET | Owns most of the Smart Dispatch V3 and network expansion technical designs |
| **Lynsay Faucett** | PM, GARNET | Your most frequent product counterpart on dispatch work |
| **Ryan Schulte** | PM, SLATE | Your most frequent product counterpart on NetDev/territory work |
| **Jeff Novotny** | Internal NetDev stakeholder | Primary source of requirements on service territories; worth building a direct relationship with |

## 7. Where things live

The full context lives in a Cowork workspace at `BreakFix/`, structured like this: `program/` for cross-team overview, people, architecture, and references; `teams/SLATE`, `teams/GARNET`, `teams/MARBLE` for team-specific projects and knowledge bases; meeting notes nested under each. Start with `program/overview.md` for the running status log, then `program/architecture.md` and `program/people.md`.

**Recurring meetings worth getting on your calendar:** Break/Fix Eng Leads Sync (Fridays, architecture-focused), B/F PEX Lead Sync (product/eng/UX coordination), the Weekly B/F Working Group Alignment (broader stakeholder check-in), and the Friday morning cross-team Technical Design Review (not B/F-specific, shared with other BD teams).

**Slack:** #breakfix_tech_leads is the cross-team technical channel; #bf-pex-leads and #breakfix-weekly-alignment cover product/program coordination; each team has its own `#<team>-pex` and `#<team>-eng` pair.

## 8. Where I'd focus first

Read the In-Platform Service Territory TDR review in full (section 3 links it), since it's the clearest existing example of the kind of architectural scrutiny this program needs and touches almost every team. Get time with Mobarok and Bilas individually, they're the two engineering leads actually building the hard parts. The Kafka connector instability (section 2) and the unowned revisit/return-trip architecture problem (section 5) are both real, scoped gaps if you're looking for concrete first targets.

Any feedback on this doc is welcome.
