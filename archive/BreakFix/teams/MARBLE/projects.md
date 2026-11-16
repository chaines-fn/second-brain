# MARBLE Projects

## ⚠️ Wind-down and reallocation (Jul 24–30, 2026)

**Parts logistics engineering concludes end of August 2026**, after which that engineering capacity moves to **buyer analytics**.

| Item | Date / detail |
|------|---------------|
| Parts return + provider-added parts ship **together** | **Aug 31, 2026** - internal work complete **Aug 25**; includes sunsetting active experiments. Suggestion to dip into the KTLO bucket to hit the date. |
| Parts logistics engineering ends | End of August 2026 |
| Sadat Haque + Tauhedul Shagor reassigned | To support **AVI-SPL**, expected **GA in October**. AVI-SPL pilot runs 8/11–8/25. |
| Remaining parts-return integration scope | May land on the **Q4 integration team roadmap**. Coordinating Salesforce and ServiceNow integration expectations. |
| RabbitMQ removed from the parts and shipment service | Event consumption converted to Kafka. Bilas scoped as a minor task, bundled into the Aug 31 release. |

**Two signals that the team itself is moving on, not yet confirmed:**
1. The Jul 28 PEX Lead Sync quick notes record a **"Marble team project departure scheduled for August 31"** - reads as the team rolling off the project, lining up exactly with the parts release date.
2. Mike Mueller, in a Jul 30 thread on principal-engineer allocation: **"Marble is also transitioning to Data Platform work, too."**

**Open question:** this conflicts with §7 below, MARBLE's stated Q3 focus shift to provider recruitment and Break/Fix work-order visibility (confirmed Jul 2). Worth confirming with Ridi or Amran whether §7 survives the reallocation or was superseded.

**Related:** the **return trips / revisit initiative is paused for the rest of the quarter** (Jul 24) pending a broader redesign and architectural update; **Shadab Ahmed** is the new product lead. The Jul 22 OKR Review separately flagged current revisit models as unreliable and needing architecture leadership, with no owner named. **Black Box parts integration is on hold** - Black Box won't use parts until advanced data capture and the Shift return experience ship (Jul 21); open whether they proceed on current runbooks or wait.

See `program/meetings/2026-07-24-bf-weekly-working-group-alignment.md`, `program/meetings/2026-07-28-pex-lead-sync.md`, `program/meetings/2026-07-21-pex-lead-sync.md`.

**Still shipping new feature work despite wind-down (Aug 21, 2026):** MARBLE-672 ("allow presite pickup location on pre-assignment workorders," `parts-and-shipments` PR #272, Monjurul Hoque Chowdhury) adds a three-level pickup-location visibility model (FULL/PARTIAL/NONE) so unassigned providers can see partial pickup-location info before assignment. Not previously tracked by name — net-new scope, not part of the named Aug 31 parts-return/provider-added-parts release bundle. Worth confirming whether this fits inside the "internal work complete Aug 25" deadline above or is separate.

## 1. Parts Schedule Coordination

**Status:** Solution Exploration phase

**Problem:** 20% of site incompletes caused by parts not on-site when provider arrives.

**Solution direction:**
- FedEx/UPS webhook integration for real-time shipment tracking
- Enhanced flightboard visibility for buyers/ops

**Goal:** Reduce parts-related incompletes from 20% to <10%

## 2. Part Return Experience

**Status:** In Design (design NOT yet complete as of Apr 8, 2026; confirmed Q2 commitment with UI scope flexibility)

**Problem:** Buyers can't communicate return requirements (RMA instructions, labels) in-platform.

**Key stats:**
- 7% of WOs require returns; 1/3 not trackable in-platform
- 55% of barcode scans captured via custom fields instead of standard shipment fields

**Goal:** Reduce return time from 4 days to <24 hours; increase trackable shipments to 100%

**Customer context:**
- **Black Box** — RMA + exit survey required for Bank of America repairs
- **Spectrotel** — ~90% return rate; highest volume returns customer
- **ETS** — Google ships parts; strict penalties for late/missed returns

## 3. Provider-Added Parts

**Status:** In Design (confirmed Q2 commitment — non-negotiable scope as of Apr 8, 2026)

**Problem:** Parts suite assumes parts are known at WO creation — fails for trunk stock and pre-shipped scenarios.

**Impacted customers:**
- **Black Box** — parts already on-site
- **ETS** — Google ships before provider
- **CapsaHealthcare** — bench stock model
- **Otodata** — trunk stock model

**Goal:** Allow providers to add parts data to WOs (not just buyers at creation time)

## 4. End-to-End (ETE) Parts Management Experience

**Status:** Problem Framed

**Problem:** Scaling requires "integration-ready" buyers; most aren't.

**Stats since MVP launch (June 2025):**
- 86,000+ parts added
- 4 pilot buyers active
- Only RIMG was integration-ready quickly

**Needs identified:**
- Bulk import/export
- Manual creation improvements
- In-platform reporting

## 5. Unisys Solution

**Status:** Wrapping up (as of March/April 2026)

Custom solution for Unisys's complex requirements: PUDOs at part level, multiple WO constraints, ServiceNow integration. Web demo shared March 2026. Communication plan prepared for launch.

## 6. Contract Enablement Phase 2

**Status:** Deferred to Q3 (as of Apr 8, 2026)

**Scope:** Make contract available in package overview + clean up all associated experiments. Supports the entire B/F contract, not just parts logistics. Team decided this shouldn't take Q2 time from parts work.

## 7. Q3 Focus Shift: Provider Recruitment & Break Fix Work Order Visibility

**Status:** Confirmed live at the July 2, 2026 H2 Vision & Strategy Overview (was missing from the roadmap slide; Bilas Sarker flagged the gap in the meeting). Aligned in the prior 1-2 weeks between Ann-Bettina Colace, Lynsay Faucett, and John Shaw.

**Plan:** MARBLE finishes parts logistics — including the Part Return Experience (#2) — through July, then shifts focus to provider recruitment and Break Fix work-order visibility. Sadat Haque, Tauhedul Shagor, Ryan Schulte, Ann-Bettina Colace, and John Shaw are framing the problem and solution direction.

**Context:** Improving fill rates likely involves mobile app changes and possibly push notifications (not in use today for published work). Ann-Bettina Colace noted this isn't purely a UX/visibility gap — Break Fix opportunities need to be marketed as distinct from standard 1099 work orders, tying into recruitment funnel work already underway. Alyssa Eby flagged that relevant wireframes/concepts already exist from a prior off-site ("the Dallas trip," May 2025) and will share them with Tauhedul Shagor to pass to Sadat, John, and Ryan.

See `program/meetings/2026-07-02-h2-vision-strategy-overview.md`.
