# Smart Dispatch Priorities Sync — 2026-04-21

**Date & Time:** Tuesday, April 21, 2026, ~7:31 CDT (~30 minutes)

**Attendees:**
- **Lynsay Faucett** — PM, MARBLE (running the meeting; pinch-hitting for Linda on Smart Dispatch priorities)
- **Bilas Sarker** — Engineering Lead, GARNET *(his email is bilas.chandra@fieldnation.com but he goes by Sarker — don't let the email fool you)*
- **Ridi Haque** — Engineering Lead, GARNET
- **Amran Hossain** — Engineering Manager, BD teams
- **Chuck Haines** — Sr. Principal Engineer

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1jLZ3GIaT-4wMUV_Y5xA-GrAzqao6UoYMFAmwKyaBeXQ/edit?usp=meet_tnfm_email)

---

## Overview

A regroup led by Lynsay to align the GARNET team on Smart Dispatch priorities following last week's conversations with Linda. The session covered three things: (1) Amran's demo of the Break/Fix initiative structure in Linear and how status updates should roll up at the project level going forward, (2) the ISO date timestamp for SLA capture — still targeting first week of May, but dependent on a response from Chris Broveleit (TPM, Integrations) about ServiceNow compatibility, and (3) the larger Smart Dispatch V3 workplan, specifically how to sequence hard filters, priority ordering, and eligibility criteria given the team's Q2 capacity.

The prioritized-order direction is locked in: the team is solving via a dedicated priority-ranking path rather than custom selection rules (CSRs) — reinforcing the Apr 20 decision Chuck and Bilas made. Bilas will deliver two technical designs: the first (hard filters, due this week) unblocks engineers and is already in review with Chuck; the second (priority-ordering ranking) will use the existing ranking algorithm but introduce a new concept where the highest score on the buyer's first criterion dominates subsequent tiebreakers. Bilas estimated TD2 + diamond-team alignment wraps within two sprints. Capacity is the real blocker: single-team throughput can't deliver filtering + priority ranking together in Q2, so the team will deliver filtering first and release only when product is ready.

Most of the V3 work (UI, mobile talent-pool storage, default ranking, expansion mechanics, coverage-summary endpoint) can proceed in parallel. Excluding eligibility criteria, most of V3 is projected to land within May (accounting for Eid week). Eligibility criteria is the big open scope question — Lynsay will validate the 5–6 most urgent criteria against customer-pipeline data (early leaders: distance, PSS, work completed, work completed by my company) so Bilas can scope the MRP. Bilas flagged a serious regression risk from V2/V3 sharing code paths; a comprehensive integrated test plan is required.

---

## Key Notes

- **Break/Fix initiative in Linear — updates roll up at project level, not milestone:** Amran walked through the Linear setup: top-level Break/Fix initiative → three sub-initiatives (Network Development, Parts Logistics, Break/Fix Ops) → projects → milestones. Project-level status is what goes to OKR review; milestone-level is detail. P1/P2 label filtering is possible but not yet applied; the current view shows all in-progress B/F work.

- **ISO date timestamp for SLA — May 1 launch target, blocked on ServiceNow integration check:** The proposed custom-field-type solution needs validation against ServiceNow integrations. Chuck hasn't opened the conversation with Chris Broveleit yet; Lynsay committed to chasing Chris today. Bilas confirmed the May launch is feasible provided no higher-priority work displaces it.

- **Prioritized order — dedicated priority-ranking approach, NOT CSRs:** Lynsay reconfirmed the direction. Chuck noted he, Mike, and Bilas are aligned (reinforcing the Apr 20 CSR rejection captured in `2026-04-20-q2-scope-and-ranking.md`). Two technical designs support this.

- **TD sequencing: filtering (TD1) first, then priority ordering (TD2):** TD1 is the easier piece — already in Chuck's review this week, unblocks engineers on the surrounding work. TD2 introduces the new priority-ordering concept: when a buyer sets multiple criteria, ranking primarily sorts by the highest score on criterion #1; subsequent criteria only break ties. **No new ranking algorithm** — Bilas will use the existing Match ranker with a new data/formula shape. Needs diamond-team alignment (they own the ranker). Bilas believes TD2 + alignment wraps in two sprints.

- **Capacity can't deliver filtering + priority ranking together in Q2:** Chuck called this out as the core constraint — single-team throughput isn't enough to ship both in Q2. Engineering will deliver in pieces; product can release when ready. If both shipping together in Q2 is non-negotiable, additional dev capacity is required.

- **Ranking criteria narrowed to four (for now):** Distance, PSS, work completed for same type of work, and work completed for my company. Lynsay will validate against customer-pipeline data and give Bilas the top 5–6. Last active and talent-pool membership are already in default ranking — under the new priority-ranking model, only buyer-configured criteria would influence scoring, though talent-pool membership could remain applied by default behind the scenes.

- **Most V3 work (excluding eligibility criteria) projected within May:** UI work has started. Mobile is doing a TA for storing talent-pool info (next sprint). Talent-pool storage settings + expansion (expansion owned by GARNET, WO publish to talent pool) targets two sprints. Coverage-summary endpoint: ~8 points, UI fast on top — likely end of May. Most non-eligibility V3 work wraps in May accounting for Eid.

- **Eligibility criteria — prioritize what has data:** Bilas's assessment: criteria with existing data (distance, PSS, work-completed) are relatively quick to add one-by-one; criteria without existing data (e.g., equipment) take significantly longer. Lynsay will validate the top 5–6 against customer pipeline; Bilas scopes MRP from there. The priority-ranking solution itself, once finalized, should wrap in about one sprint.

- **High regression risk from V2/V3 shared code paths:** Bilas flagged this as a serious concern — V2 and V3 sit on common code, testing effort will be substantial to avoid breaking existing features. Group agreed a comprehensive integrated test plan is required. No owner assigned yet.

- **Mike meeting stays on the calendar:** Amran wants to walk Mike through Bilas's thinking to lock in one direction across the architects. Group agreed to keep it rather than cancel.

---

## Action Items

- **Lynsay Faucett:** Message Chris Broveleit (TPM, Integrations) today about ISO date timestamp custom field type compatibility with ServiceNow integrations; unblock the May 1 launch target.
- **Lynsay Faucett:** Validate highest-priority eligibility criteria against customer pipeline data; share top 5–6 with Bilas. Meeting with John today will inform this.
- **Lynsay Faucett:** Review existing high-level tickets for V3 eligibility criteria and related work.
- **Bilas Sarker:** Message Lynsay to clarify (a) which criteria will be targeted for ranking, (b) who decides, and (c) how ranking should behave when a buyer sets only one criterion. Lynsay will loop Linda in as needed. Share proposed technical approach afterward.
- **Bilas Sarker:** Align the priority-ranking technical solution with Chuck, Mike, and the diamond team (who own the ranker service).
- **Bilas Sarker:** At-mention Lynsay with outstanding technical-design questions (some went to Linda and weren't picked up).
- **Bilas Sarker + Tine (implied):** Estimate story points ("marble points") for items scheduled in the next two cycles so Lynsay can assess capacity.
- **The group (implied):** Develop a comprehensive integrated test plan to cover V2/V3 compatibility given shared code paths.
