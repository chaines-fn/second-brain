# SD Q2 Scope Walkthrough

**Date:** April 2, 2026
**Source:** [Google Doc (Gemini notes + transcript)](https://docs.google.com/document/d/18viHf6mEKCxdCWgvAzX0AvvP-LsKIHVXmdMz-j3a7HM/edit)
**Facilitator:** Linda Lo
**Attendees:** Linda Lo, Brent Grill, John Shaw, Chuck Haines, Ryan Schulte, Ann-Bettina Colace, Alyssa Eby, Rick Jundt, Melanie Payeur, Kayle Schiele, Chris Broveleit (Lynsay Faucett - invited, not present)

---

## Purpose

Align the core B/F initiative group on the Q2 smart dispatch approach. Linda and Brent have been closely collaborating with auto dispatch and CSR folks (Gina, Luke, Tony, Todd Holey, Laura, Amber) through multiple review rounds. One final signoff discussion with that group was scheduled for the following week.

---

## Q4/Q1 Recap

- **Q4:** Unlocked ability to dispatch a WO to a specific provider group (vs. marketplace-only)
- **End of Q1:** Added tier-based load balancing within service territories + marketplace expansion as a fallback channel (flag removed for all B/F accounts as of ~April 2)

---

## Q2 Problem Statement

The current smart dispatch ecosystem only really supports two extremes:

- **Very low liquidity:** Locked to whoever's in your service territory. No fallback. Dead end if no one responds.
- **Very high liquidity:** Marketplace with minimal filters (background check, drug screen, max distance, min PSS tier). Essentially open.

Everything in between isn't supported. Additionally, buyers who think nationally (not territorially) or have existing curated talent networks can't use smart dispatch at all because service territory is currently required.

---

## Q2 Scope: Key Features

### 1. Talent Pools as a Dispatch Channel

Talent pools will be introduced as a first-class dispatch channel alongside service territories and marketplace. Service territory is no longer required as a prerequisite for smart dispatch.

Buyers will be able to activate any combination: service territories only, talent pools only, marketplace only, or all three simultaneously.

Talent pools can be ranked or unranked. If ranked, buyers assign tier levels (e.g., Platinum + Emergency Response = Tier 1, Gold = Tier 2, Silver = Tier 3). Unranked treats all pool providers equally.

### 2. Dispatch Hierarchy (Fixed for V3 Initial Release)

When all three channels are active, the assignment logic at the end of the window follows this priority order:

1. Service territory providers (highest priority)
2. Talent pool providers (by tier if ranked)
3. Marketplace providers

All three channels are published simultaneously. Marketplace timing is still controlled by the delay setting. Assignment decision is made at the end of the window based on who requested and who passes criteria.

**Key clarification from Chuck:** Service territories are always project-scoped — they can't exist outside a project. If a provider is in both a service territory and a talent pool for the same project, they get treated as an ST provider (higher priority).

### 3. Segments & Criteria Tab

New tab in smart dispatch settings. Replaces the existing approach with a more unified model:

- **Segments:** Toggle which dispatch channels are active (ST, talent pools, marketplace). Select which talent pools to include and set tier rankings.
- **Provider Criteria:** Configurable hard filters and ranking criteria. Currently planned: distance (required), background check age, drug screen age, max assignments/day, PSS score, etc.

**Criteria model:** Floor-based hard cutoffs (anyone outside the floor is ineligible) + automatic ranking within the eligible pool by freshness/recency/experience. Criteria are stack-ranked by the buyer to set relative weight. This replaces the required/optional split that was explored and abandoned — the simpler mental model is: set a floor, and within that floor, fresher/closer/more experienced ranks higher.

Criteria can be scoped to specific segments (e.g., "apply background check requirement to marketplace and talent pool providers, but exempt service territory providers").

### 4. Preview Count / Coverage Summary

Buyers will see a count of eligible providers by segment before going live, based on the active criteria and segments. This helps set expectations — if you've set 15 criteria and your eligible pool is 5 providers, you know manual assignment is likely.

Ryan Schulte noted that an overlay of this on a map view (territory + pool providers visualized geographically) would be very useful — he has work in progress to show this. Chuck flagged this coverage count is powered by the NetDev recruitment API — reinforcing why building that API is critical.

### 5. Notifications: Qualify Before Notify

**Chuck's proposal (not previously discussed publicly):** Dispatch will only send notifications to providers who are actually eligible based on the criteria. The WO remains visible to all marketplace providers — just no push notification for ineligible ones. Goal is to reduce noise without gating visibility.

Kayle raised concern about new providers missing out on awareness. Chuck clarified: WO is still visible, we just don't notify. The incentive to improve their platform standing remains intact.

### 6. Copy Settings Across Projects

Buyers will be able to copy all smart dispatch settings from one project to another — reducing setup friction when they have multiple similar projects.

### 7. Change Log

New audit trail tracking who changed which settings and when. Addresses the recurring blame-game problem where buyers claim "Field Nation must have changed something" when their own team made the edit. Strong support from Kayle — CX team will be relieved.

John Shaw raised a concern that historical change logs in the platform (smart dispatch run history, WO event history) have been confusing and duplicative. Linda acknowledged: the logging for engineering purposes should be massaged before displaying to users. This will be addressed in requirements documentation.

---

## Decisions Made

| Decision | Notes |
|----------|-------|
| Dispatch hierarchy for V3: ST > Talent Pool > Marketplace | Fixed for initial release; customizable priority is a future consideration (Q3 candidate) |
| Adjacent territory spillover NOT in Q2 scope | Depends on NetDev providing territory-to-territory relationship data. Workaround: use a talent pool with the same rigor + distance-based fallback |
| Criteria don't gate visibility, only assignment ranking | Providers can still see and request WOs; criteria applied at assignment decision point |
| Only notify providers who pass criteria (engineering proposal) | WO still visible on marketplace; reduces notification noise |
| No separate required/optional criteria sections | Floor-based cutoffs + ranking within eligible pool handles this more cleanly |
| Change log + copy settings features confirmed for V3 | Both included as "surprise and delight" additions |

---

## Open Questions / Unresolved

- **Tier strategy for ranked talent pools:** Should tier 2+ still get the 80/20 fairness split (like current ST tier model) or should it be true cascading (exhaust tier 1 before tier 2)? Brent to align with CS team.
- **Customizable channel priority** (e.g., talent pool first instead of ST first): Not in Q2. Kayle flagged this will come up especially during adoption phase. Linda agreed to revisit if/when strong use cases emerge, and when ST reusability across projects ships.
- **Distance calculation:** AWS ISO lines (isochrone polygons, accounts for traffic/roads) vs. current crow-flies. Still evaluating whether this fits Q2. Chuck and Ryan to discuss offline.
- **History log UX:** How to surface the change log cleanly without creating the same noisy, duplicative log experience that exists elsewhere in the platform. Linda to define in requirements doc.

---

## Adjacent Territory Discussion (Summary)

John raised adjacent territory spillover for low-liquidity networks. Current state: no. The concern is that service territory A might have no available providers, but nearby territory B does — and today there's no automatic expansion.

Linda's position: smart dispatch won't build territory-to-territory geographic relationship logic. That's NetDev's domain (Q3 dependency). But the workaround is: if the talent pool includes providers with the same rigor as a nearby territory, a distance-based filter effectively accomplishes the same thing. Ryan noted this might not even be a workaround — it might be the better primary pattern (manage coverage via talent pools + distance rather than trying to link territories).

---

## Action Items

| Owner | Action |
|-------|--------|
| Brent Grill | Align with CS team on tier strategy (80/20 fairness vs. cascading) |
| Brent Grill | Model ClearCaptions and Shift CSR setups in the new V3 framework for stakeholder meeting next week |
| Linda Lo | Define all major changes needing logging in Requirements B doc; define how change log displays to users |
| Linda Lo | Review WO change log UX — reduce duplicative/confusing history entries |
| Linda Lo | Send requirements plan link to full group for feedback |
| Linda Lo | Send current CS meeting contact list to Kayle Schiele and Rick Jundt |
| Kayle Schiele + Rick Jundt | Review contact list and identify missing attendees; ensure Professional Services team is included |

---

## Notes from Chuck

- Reinforced the coverage summary / preview count is gated on the NetDev recruitment API being built — this is why that work matters.
- Proposed notification filtering (notify only qualified providers) as an engineering-side proposal not previously discussed.
- Clarified dispatch publishing timing: ST and talent pools publish simultaneously at dispatch start; marketplace follows the configured delay.
- Agreed to discuss ISO lines offline with Ryan — good timing since there's now a solution looking for the right problem.
