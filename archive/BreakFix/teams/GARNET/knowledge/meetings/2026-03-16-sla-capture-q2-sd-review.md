# Review Updated SLA Capture + Latest Rev of Q2 SD — March 16, 2026

**Meeting:** Review updated SLA capture + latest rev of Q2 SD
**Date:** March 16, 2026, 08:00 PDT / 10:00 CDT (~71 min)
**Attendees:** Linda Lo (organizer), Brent Grill, Bilas Sarker, Ridi Haque, Amran Hossain
**Not present:** Chuck Haines (invited, declined/struck-through)
**Source:** [Gemini Notes + Transcript](https://docs.google.com/document/d/11wlEM5GFEe8f3jR6ylsZH9ONgG96oWORflSwVNBMBOw/edit?tab=t.zgtvqsrbcznx)

---

## Summary

Linda walked the GARNET team through two non-trivial pivots driven by late-stage stakeholder feedback. First, Laura Quest's review of the live SLA capture feature flagged "non-starters" that hadn't surfaced in the prior CSM feedback rounds — the "service level" concept and the assumption that buyers would create FN-specific reporting target titles in their integrations. The redesign drops both: priority + work order template now drive the mapping, and "assign by / arrive by / complete by" each get a custom field option so buyers can pass pre-calculated SLA timestamps from their FSMs. Net result is buyers create four custom fields (priority + the three target timestamps) rather than any new fields on the work order object. Bilas pegged the rebuild at ~8 points (5 UI, 3 DB/backend). Feature stays dark for all real buyers until this lands.

Second, Brent walked through the latest Smart Dispatch prototype reflecting feedback from solutions (Luke/Todd) and CSMs (Gina/Laura/West/Amber/Tony). Big additions are smart dispatch history (change log piggybacking on Ryan's ST work), copy-settings-to-project for scaling, and segment-aware required/optional criteria. Linda was explicit about the ranking philosophy: this is *not* a new ranking system — Match 2.0 still ranks; optional criteria are CSR-style overrides on top. She wants to avoid friction with Mike at TD. Two real complications surfaced: (1) max-assignments-per-day might reuse the existing "currently assigned to less than" CSR with the "schedule same day" option, but the three schedule types each need use-case validation (Amran), and (2) CSMs aren't seeing enough daylight between SD and current auto dispatch to justify change management — Linda is exploring travel-cost-threshold-based auto-accept of counters as the "step change" value-add.

The big strategic ask: Linda floated handing off project-specific talent pool group creation and ranked/unranked modeling to **Slate** — they need it anyway for service territory management visualizations. If that frees enough GARNET capacity, it could let Q2 ship sooner. Bilas and Ridi to provide hand-wavy t-shirt sizes per Q2 segment before company holiday so Linda/Brent can identify what to descope. Priority sequence is clear: finish marketplace expansion → SLA capture rework → Q2 SD.

---

## Topics

### 1. SLA Capture Redesign — Laura Quest Non-Starter Feedback

The SLA capture feature shipped to prod, then Laura Quest's review surfaced two non-starters that earlier CSM sessions missed:

- **"Service level" was unclear** — Laura saw it as a confusing proxy for "complete by" and a menu of SLA types (same-day, next-business-day) more than a service level
- **Buyers won't create FN-specific reporting target titles** to send in integrations

**Redesign (reviewed with Jeff Natne last Thursday, signed off):**

- **Priority + work order template** now drives mapping. Buyers can have multiple P1 definitions in one program — template ID disambiguates. Removes reporting target title from work order create payload entirely.
- **Templates drop-down** at the reporting-target row pulls from the project's existing templates.
- **Each timing target (assign by / arrive by / complete by)** now offers either a custom field source OR a calculated/manual fallback. This reflects Laura's input that buyers' FSMs already have these timestamps pre-calculated — no need to recalculate on FN.
- **Buyer-side fields needed:** four custom fields total — priority, assign by, arrive by, complete by. No net-new fields on the work order object.

**Validation philosophy:** The feature still trusts buyer input — no guard rails preventing mixed configurations (e.g., custom field for arrive-by but calculated for complete-by). Reporting is manual today, so Tossa catches misconfigurations downstream. Guard rails come when in-product reporting lands.

**Bilas estimate:** ~8 points total — 5 for UI (add/edit/display list), 3 for DB restructuring + backend payload (multi-template association, per-target custom fields). Plus 2 backend cleanup tickets to remove reporting target association from work order create + validation.

**Bilas-to-Chuck alignment needed** on the database design changes (multi-template + per-target custom fields don't fit current data model).

**Activation:** Feature stays disabled for all real buyers until this redesign ships. Higher priority than Q2 SD, lower priority than marketplace expansion.

**Solutions team sign-off pending:** Linda + Brent to confirm the 4-custom-field approach with Todd Foley / Luke / Scooter before engineering kicks off. Laura and Jeff are on board; just need to validate with implementation folks given Bilas's flag that buyers often resist creating things on their side.

### 2. Smart Dispatch Prototype Updates

Brent walked through the latest SD prototype reflecting Luke/Todd + Gina/Laura/West/Amber/Tony feedback.

**New top-level features:**
- **SD history (change log)** — piggybacks on what Ryan is building for service territories. Shows who changed what dispatch settings and when.
- **Copy settings to another project** — for buyers running same config across multiple projects/offerings.

**Segments (talent pools):**
- Buyers add talent pools as ranked or unranked
- **Unranked:** all talent pools treated equally. Cascade still: ST > TP > MP.
- **Ranked:** order determines assignment priority within TPs.

**Required criteria (hard filter) — segment scoping under exploration:**
- Currently applies to all three segments
- Feedback: some buyers want criteria scoped to specific segments (e.g., apply distance to MP/TP only, exempt ST)
- Bilas's read: filter is most needed for MP and TP (ST is curated). But Linda noted Blackbox is creating very large STs, and "vanilla territory" use cases (nationwide/regional) need distance filtering. So ST needs to remain in scope.
- Largest TP today is ~70k providers — distance filter is non-negotiable there

**Ranking philosophy (important — Linda was emphatic):**
- **NOT creating a new ranking system.** Avoid Mike Mueller TD friction.
- Match 2.0 continues to rank profiles using its algorithm (PSS, distance, etc.)
- Required criteria = hard filter only
- Optional criteria = CSR-style overrides applied on top of Match 2.0 ranking
- Bilas had asked whether both required and optional criteria would feed ranking — answer is no, only optional get the override layer. PSS handled by Match 2.0; if a required PSS floor is set, that's pure filter.

**Max assignments per day — reuse existing CSR?**
- Linda would prefer to use the existing "currently assigned to less than" CSR with the "schedule same day" option rather than build a new one
- Bilas explained the existing logic: matches on work order schedule date (not provider schedule), validates against assigned WOs whose schedule overlaps
- **Amran flagged validation gap:** three schedule types — fixed schedule, arrival window, business hours — each need use-case walkthroughs to confirm the existing CSR works as expected. Not a blocker but needs flushing out.

**Auto-accepting counters (NEW exploration):**
- Real CSM feedback: current SD isn't dramatically better than auto dispatch. CSMs won't push buyers through change management for incremental gain.
- Existing pain point: auto dispatch automation stops when a counter is received — that's the gap.
- Linda + Brent exploring auto-accept-counter behavior gated by a travel cost threshold as the "step change" value-add
- May not make Q2 scope, but worth exploring to drive adoption

### 3. Slate Contribution Opportunity (Strategic)

Linda raised whether Slate could take on the talent pool portion of SD V3 — specifically:
- Project-specific talent pool group creation
- Pulling talent pools into the ranked/unranked model

**Why it makes sense:** Slate needs this for their own provider management visualizations (Ryan's design shows clicking a provider to see which territories AND which talent pool tiers they're in). They'll have to build it anyway.

**Asks for Bilas/Ridi:**
1. Would handing this off free up meaningful GARNET capacity?
2. Is the talent pool work medium/large lift? (If XS, not worth involving another team.)

Linda already gave John Shaw and Ryan Schulte a heads-up that this conversation is happening.

### 4. Q2 Scope T-Shirt Sizing

Linda confirmed Q2 = different timing profiles under reporting targets + talent pools expansion. "This is it" per Chuck's earlier scoping.

**Ask:** Bilas + Ridi to give hand-wavy XS/S/M/L/XL sizes per segment of the Q2 design before company holiday (by ~March 18). Non-committal, just enough to identify XL items that need descoping.

**Segments to size:**
- Copy to project
- Smart dispatch history
- Coverage summary
- Provider segments (ranked/unranked)
- Specific CSRs (moving to optional/required model)
- Reporting targets housing timing profiles
- Assignment transparency (separate Figma — Brent to share link in #breakfix-fulfillment-ops)

### 5. Calendar / Holiday Visibility (Process)

Linda raised that the US-side team can't see Bangladesh holidays or BD team OOO on calendars. Brent recalled a legacy "Bangladesh holidays" calendar but it's no longer maintained. Linda asked Bash and Ridi to reflect OOO and company holidays directly on their calendars going forward — calendar is the primary scheduling signal.

Demo scheduling for marketplace expansion progress — pushed to next week given the BD holiday window (Wed–Mon) and Linda's calendar.

### 6. Retro on CSM Feedback Process

Linda flagged she'll run a retro on why Laura's non-starter feedback didn't surface earlier in Brent's CSM sessions. Goal: figure out the loop so this doesn't repeat.

---

## Decisions

| Decision | Details |
|----------|---------|
| SLA capture redesign approved | Priority + template mapping replaces service level + reporting target title. 4 buyer-side custom fields (priority, assign by, arrive by, complete by). |
| No new fields on work order object | Buyers create custom fields instead. Integration validation will enforce. |
| No validation guardrails on SLA config (yet) | Trust buyer input; manual reporting catches issues. Guardrails added when in-product reporting ships. |
| Feature stays dark until rebuild | No real buyer activation on current SLA capture. |
| Priority sequence | Marketplace expansion → SLA capture rebuild → Q2 SD |
| SD ranking strategy | Match 2.0 stays as ranker; optional criteria are CSR overrides on top. NOT a new ranking system. |
| Max-assignments-per-day | Will attempt to reuse existing "currently assigned to less than" CSR + "schedule same day" rather than build new |
| Q2 scope frozen at | Different timing profiles under reporting targets + talent pools expansion |

---

## Action Items

| Owner | Action |
|-------|--------|
| Ridi, Bilas | Reflect OOO and company holidays on calendar going forward |
| Linda | Reschedule next week's group meeting to a longer slot |
| Linda | Run retro on why Laura's product feedback wasn't caught earlier |
| Bilas | Align with Chuck on DB design changes for new SLA configs (multi-template + per-target custom fields) |
| Bilas | Create 2 cleanup tickets — remove reporting target work order association from create flow + validation |
| Linda, Brent | Confirm 4-custom-field SLA approach with Solutions team (Todd, Luke, Scooter) |
| Linda | Fully flush out Max Assignments Per Day requirements + use cases for each schedule type |
| Linda, Brent | Explore travel-cost-threshold auto-accept counters as Q2 step-change feature |
| Brent | Post Figma link + assignment transparency mock link in #breakfix-fulfillment-ops |
| Bilas, Ridi | Distribute estimation across Q2 segments; identify heavy items + Slate cooperation candidates. T-shirt sizes due before company holiday (~March 18). |
| Bilas | Walk the design end-to-end and produce the high-level point estimates |
| Ridi | Determine SLA capture completion timing based on current pacing (after marketplace expansion completes) |

---

## Open Questions

- Will Solutions (Todd/Luke/Scooter) sign off on the 4-custom-field approach? Buyer-side resistance to creating fields is the historical concern.
- Can the existing "currently assigned to less than" CSR + "schedule same day" cover all three schedule types (fixed, arrival window, business hours) without new logic? Amran to validate.
- Does handing talent pool work to Slate free meaningful GARNET capacity? Depends on T-shirt sizes — TBD by March 18.
- Will auto-accept counters on travel cost make Q2 scope, or stay exploratory?
