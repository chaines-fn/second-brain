# Smart Dispatch Q2 Scope Review — Wider Stakeholder Walkthrough

**Date:** 2026-04-08
**Facilitators:** Linda Lo, Brent Grill
**Attendees:** Mike Mueller, Jonah Hacker, Stephen Wald, Bilas Sarker, Ridi Haque
**Absent:** Chuck Haines
**Source:** [Google Doc (Gemini notes + transcript)](https://docs.google.com/document/d/1K07bsqCptV_scfQVlizHm7VnV_zsK0P5RfbFnNflXh4/edit)

## Purpose

Linda walked Mike, Jonah, Stephen, and the GARNET eng leads through the Q2 smart dispatch scope — the same content Chuck has already seen in prior reviews. Goal was to get cross-team feedback (especially from Mike on Match/ranking alignment) before final sign-off the next day (Apr 9).

## What Was Presented

Linda recapped the Q4 → Q1 → Q2 evolution:

- **Q4:** Service territories as primary dispatch channel, publish-to-group paradigm, request-based assignment (not first-to-accept)
- **Q1:** Marketplace expansion as fallback channel (distance, PSS, drug/background check criteria)
- **Q2:** Talent pools as third dispatch channel, configurable provider criteria, coverage count preview, copy settings across projects, change log

Brent walked through the UI designs for:
- **Dispatch Settings tab** — single dispatch duration, 60% ranking window, redispatch on backout
- **Segments & Criteria tab** — toggle ST/talent pools/marketplace on/off, rank/tier talent pools, per-segment criteria application

## Key Discussion Points & Feedback

### Mike Mueller — Match 2.0 consistency (recurring theme)
Mike's core stance: smart dispatch should NOT have its own ranking algorithm. Match owns ranking and filtering; smart dispatch owns automation and B/F-specific rules (like ST priority). He pushed on two things:
1. **Match 2.0 UI must reflect the same ranked/tiered view** — if dispatch expires and a dispatcher manually assigns via Match 2.0, they need to see the same provider ordering. Brent acknowledged he hasn't explored this yet.
2. **Request attributes in Match 2.0** — how incoming requests display their talent pool membership, tier, etc. needs to be worked out. Linda agreed this is must-do for requests received, but displaying attributes for non-responders can come later.

### Mike Mueller — Distance filter cap
Mike recommended hard-capping the distance filter at 250 miles (no free-typing above that). Provider Match today doesn't cap this and people type in absurd values like 10,000 miles. Brent agreed the intent is to cap at 250. This sets up for future driving distance calculation work.

### Mike Mueller — Driving distance calculation
Stephen asked about using actual driving distances. Mike clarified: it's doable but requires (a) defining the right provider set to pre-calculate on, and (b) handling eventual consistency (what happens before calculation completes). They have SWAGs but haven't been given time to build it. Linda confirmed crow-flies is the Q2 approach — noted Chuck is exploring ISO lines as a P2.

### Mike Mueller — Evaluation windows
Mike suggested the ranking evaluation should happen at 60%, 70%, 80%, 90%, 100% instead of just 60% and 100%. Linda confirmed it's only 60/100 for now. Jonah also questioned why users can't choose the evaluation point or base it on request count. Linda: eventual vision is buyer-configurable optimization (speed vs. cost vs. quality vs. fairness), but starting simple.

### Jonah Hacker — Criteria granularity
Jonah probed whether marketplace criteria (distance, PSS) are buyer-configurable vs. system-set. Linda confirmed they're all buyer inputs at the project level. Jonah flagged that the project-level model may not map well to traditional rollout projects where costs vary per WO — Linda acknowledged this is B/F-scoped for now.

### Stephen Wald — Publish-to-group alignment with Labor
Stephen flagged that Labor is also introducing publish-to-group and asked Mike to connect with Andrew to align on the technology approach. Mike agreed to message Andrew.

### Stephen Wald — Rates
Stephen asked about rate handling. Linda confirmed rates are "soft pre-negotiated" — verbally agreed during recruitment but not formalized in the system yet. Ryan and John are working on setting rates at the ST level that inherit into WO details.

### Bilas Sarker — Ranking within criteria buckets
Bilas raised the idea of auto-bucketing within criteria (e.g., background check completed within 1 month ranks higher than within 6 months, even though both pass the 12-month floor). Mike pushed back — said he had issues with that approach and wanted to see the design first. This aligns with the "floor + ranking by freshness" model already captured in V3 scope, but the mechanics need alignment with Match.

### Segment priority — talent pools over ST?
Mike predicted buyers will quickly ask to prioritize talent pool tiers ahead of ST. Linda's response: for now, if you're building an ST specifically for this project, why would you deprioritize it? If a buyer prefers talent pools first, the workaround is to not use STs. Customizable hierarchy is deferred to Q3 (consistent with what's already in projects.md).

### Private networks
Mike asked about private networks. Linda confirmed they're the next channel to unlock — Q3 target after talent pools.

## New Information (Not Previously Captured)

1. **"Core Transform" team** — Brent mentioned a new team being spun up for Q3 focused on "automation and AI." Brent and Linda may move back under the marketplace umbrella or onto this team. Very early/vague — Brent was learning more that day.
2. **Final sign-off scheduled for Apr 9** with a broad cross-functional group (Luke, Todd, Foley, Gina, Laura, Amber, Tony).
3. **Stephen Wald and Jonah Hacker** are now in the loop on smart dispatch Q2 scope — first time seeing these designs.
4. **Follow-up meeting** — Linda scheduling a deeper criteria review by end of week. Noted Bilas and Ridi may not be able to attend due to timing.
5. **Brent sharing interactive HTML prototype** for attendees to click through and surface additional questions before the follow-up.

## Action Items

| Owner | Action |
|-------|--------|
| Mike Mueller | Message Andrew about publish-to-group concept alignment with Labor |
| Stephen Wald | Find time to walk the group through the Labor overall plan |
| Brent Grill | Determine how request attributes display in Match 2.0 for incoming requests |
| Brent Grill | Hard-cap the distance filter at 250 miles (no typing above) |
| Linda Lo | Schedule follow-up meeting to deep-dive criteria — by end of week |
| Brent Grill | Share interactive HTML file for attendees to explore |
