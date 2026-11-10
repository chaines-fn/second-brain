# Break/Fix Q2 '26 NNL Plan Review

**Date:** 2026-04-09, 9:29 AM CT
**Organizer:** Lisa Sacquitne
**Invited:** Ann-Bettina Colace, John Shaw, Lisa Sacquitne, Bilas Sarker, Travis Emslander, Chuck Haines, Amran Hossain, Cory Muir, Shane Kinder, Ridi Haque, Kelley Manuel
**Source:** [Google Doc (Gemini notes only — no transcript available)](https://docs.google.com/document/d/1pGHpTmSDLA8iE3B-cj07dO9HxjA0Ox0B0Wf_Xug-i3Q/edit)

## Purpose

Leadership-level review of Q2-Q4 Break/Fix plans across all three workstreams (SLATE, GARNET, MARBLE). This is the broader NNL (next-next-level) plan review — the audience includes Travis (CTO), Shane Kinder (EVP Product), Lisa Sacquitne, Cory Muir, and cross-program leads.

## High-Level Q2-Q4 Objectives (Three Pillars)

1. **Unlock additional B/F program use cases** and increasingly enable buyers to self-serve (bulk of self-service in Q3/Q4)
2. **Enable B/F programs with tooling for efficient network builds** — NetDev AI tooling focus
3. **Complete parts logistics experience** and expand to broader use cases — expected to mostly wrap in Q2; MARBLE team potentially repurposed in Q3

## Q2 Specifics by Workstream

### Smart Dispatch (GARNET)
- Enable existing talent-pool-based B/F programs to use V2 features, building on Q1 marketplace expansion
- Expand SD to consider talent pools alongside STs and marketplace — ranking: ST > TP > MP
- Buyers can choose whether to prioritize marketplace or talent pools and set the order
- "CSR-like capabilities" for buyer optimization — **proposing 7 of 10 criteria in first phase**, remaining 3 incremental due to capacity constraints
- Open discussion with Match team ongoing re: visibility of SD ranker in Match 2.0 — **may require adjusting delivery plan if integrated**
- SLA data capture and dispatch duration override work scheduled for early April (Q1 follow-up)
- Metric readiness: John Shaw working with Linda to finalize; determining edge vs. data support needed

### Network Development (SLATE)
- NetDev tooling top-of-funnel + initial end-to-end tool build — 2 engineers dedicated
- Help update talent pool → service territory architecture (enables SD to leverage talent pools)
- ST dashboard enhancements: map view to visualize STs and providers, mass actions, management at scale — targeting 90% completion, possible Q3 carryover
- ST pay rate enforcement: low priority, may be included depending on Q2 cutoff
- **Q3 vision:** Automated/standardized ST creation — platformize the concept, auto site attribution on list upload, geographic concepts for reporting. Buyer could leverage TPs without needing to put providers into STs.

### Parts (MARBLE)
- Schedule coordination, parts exception handling, standardized head-to-site workflow carried from Q1 (Unisys urgency pushed these)
- **Provider added parts (trunk stock)** is highest priority after schedule coordination — flagged by multiple pipeline buyers as required to fully use parts experience
- Return experience may carry into Q3 (UX/design handoffs not finalized)

## Key Discussion Points

### Unisys Assignment Cap — New Complexity
- Unisys flagged a per-provider daily assignment cap (max 5 sites/day) to maintain SLA
- Linda and Chris workshopping solutions — **not currently on the roadmap**
- Underlying problem: multiple tickets at the same physical location can send multiple techs to the same site — SD doesn't handle this today
- **Requires investment in location/geocoding logic** to deduplicate and understand same-site WOs
- Could extend to "across the street" scenarios for same-tech assignment
- Alternative of having Unisys communicate ticket bundles considered too burdensome for them

### Smart Dispatch Criteria: 7+3 Phased Delivery
- Team proposing 7 criteria in phase 1, then 3 more incrementally
- Match 2.0 visibility of SD ranker is an open discussion that **could change the delivery plan**

### Resource Contention — Senior BI Analyst
- Both buyer analytics and this team believe they're allocated the same senior BI analyst resource
- A plan exists to resolve but John Shaw flagged it as a risk — needs follow-up
- Cory and Lisa to review cross-team asks during data Q2 plan review

### Leveraging Prototype Artifacts for Development
- Ann-Bettina's ST dashboard prototype was built entirely in Claude
- Discussion about how to share code for engineering to accelerate development
- Cory suggested exploring: designers using Claude to produce code vs. engineers extracting from Figma via Figma MCP server
- Amran and Cory to explore which path is faster

### In-Platform Reporting Dashboard
- Targeted for Q3 — key component of B/F suite
- Dependencies with data platform; ongoing QuickSite discussions
- Q2 goal: achieve 80% confidence on V1 dashboard, leaving only platform dependency for Q3

## Risks Flagged

1. **Unisys assignment cap** — not on roadmap, could disrupt if it becomes urgent
2. **Early V2 adapter iterations** — may need unplanned iteration cycles
3. **Team transitions in Q2** — (aligns with "Core Transform" team spin-up mentioned in Apr 8 meeting)
4. **Data support dependency** — heavy reliance on Tahsin and Mia for reporting/analytics
5. **BI analyst resource contention** — double-allocated
6. **NetDev tooling pace** — managed via weekly leadership connections
7. **Match 2.0 integration** — if required for Q2, changes the delivery plan

## Action Items

| Owner | Action |
|-------|--------|
| Network Dev team (SLATE) | Help update talent pool → service territory architecture |
| John Shaw | Finalize metric readiness plan with Linda; determine edge vs. data support this week |
| John Shaw | Follow up on BI analyst resource contention |
| Cory Muir + Lisa Sacquitne | Review cross-team resource contention in data Q2 plan review |
| Amran Hossain + Cory Muir | Explore leveraging Claude prototype artifacts to accelerate dev; determine faster path |
