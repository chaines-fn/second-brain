# Talent Pools // Service Territories // Smart Dispatch Cont'd — 2026-06-15

**Date & Time:** Monday, June 15, 2026, 1:00 PM CDT (~60 minutes)

**Attendees:**
- Ryan Schulte — PM, SLATE (meeting organizer)
- John Shaw — Group PM (cross-stream)
- Lynsay Faucett — PM, GARNET
- Chuck Haines — Sr. Principal Engineer (cross-cutting)
- Alyssa Eby — UX, GARNET
- Mia Mu — Data Science

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1r3lyNuw5-qx4kts1rp7IixP6XNX6tW6AhcahuB_Uwd0/edit)

---

## Overview

This was a continuation of the talent pool ↔ service territory alignment conversation that began June 1. The central problem Ryan laid out: Break/Fix currently has two competing ways to attribute providers to a project — direct assignment into service territories, and talent pools via smart dispatch. This creates a confusing mental model for buyers who already use talent pools historically and don't understand why they'd need to divide that talent into service territories for every project. The proposed solution is to make talent pools the single source of truth and have service territories become a downstream view that gets populated automatically based on where talent pool providers are located.

The group aligned on the core architecture: when a buyer links a talent pool to a B/F project, the system auto-maps providers to the relevant service territories based on proximity rules (the "auto-proximity" logic, expected to cover ~95% of cases). The existing service territory database tables remain the storage layer — they continue to be populated and kept current via event-driven updates (talent pool membership changes, provider address changes), so smart dispatch, coverage reporting, and all downstream consumers work without modification. Service territories effectively become a managed view, not a manually curated list.

A significant practical concern shaped the second half of the meeting: Smart Dispatch V3 ships June 23 with talent pool support, but without the talent pool → service territory linking in place, buyers using talent pools will have no coverage scores. Building out the full service territory infrastructure is a Q3 body of work. The group agreed that as an interim measure, auto-proximity proxy coverage scores should be generated in-platform (not at the reporting layer), so the reporting and recruitment tools can inherit the data and buyers have a functional — if not perfectly precise — coverage picture from day one.

---

## Key Notes

- **Architectural model confirmed:** Talent pools are the source of truth for B/F projects. Service territory tables persist as before but are populated programmatically — not by buyers directly. The system listens to talent pool modification events (provider additions/removals) and provider address changes to keep territory rosters current in real time. Chuck flagged that this approach preserves historical data (point-in-time territory snapshots) and requires no changes to existing smart dispatch logic or the downstream reporting surface.

- **Auto-proximity mapping as the core mechanism:** When a talent pool is linked to a project, providers get auto-mapped to territories based on configurable rules (distance radius from territory center, and/or work history in the area). Ryan estimates this covers ~95% of providers correctly. The exact rule set (what "nearish" means) is still TBD — that's part of the technical design work Chuck is scoping.

- **Blacklist / whitelist deferred:** Manual force-include and force-exclude overrides at the territory level were discussed but explicitly deferred to a future phase. These edge cases (the 5% who need explicit mapping) are real — Jeff Novotny's use case was cited — but the group agreed not to let them block the core talent pool linking work.

- **Default Tier 1 for all providers:** Rather than requiring buyers to configure tiers upfront, the group agreed to default all providers linked via talent pools to Tier 1. This aligns with how smart dispatch actually operates (80/20 sends 80% of volume to Tier 1) and simplifies the initial setup experience. Buyers who want more granular tiering can configure multiple talent pools mapped to different tiers, or use future criteria-based membership rules within the pool.

- **UI simplification:** The smart dispatch settings experience would flatten to three options — Marketplace, Talent Pools, and Private Networks — removing the current separate entry point for service territories. Service territories become invisible infrastructure, not a buyer configuration surface. The ranked vs. unranked talent pool choice also disappears; instead, buyers link pools and can assign tiers to them.

- **Private network support:** Chuck confirmed no major rearchitecture is needed. The existing sandbox prototype already mimics the right dispatch flow (work orders go to the private network). There is implementation work remaining to transition the current hack into a fully supported feature, but it's not a blocker for the talent pool architecture.

- **Coverage score gap is the urgent problem:** Without talent pool → service territory mapping, V3 buyers using talent pools will have no coverage scores after June 23. The full service territory infrastructure (with potential technology changes to isolines/hex grids) is a Q3 project for SLATE — a full quarter gap. The agreed interim: implement auto-proximity proxy logic in-platform, so reporting and the recruitment tool can inherit it. Alyssa underscored that doing this at the reporting layer would create divergence across reporting, the platform, and the recruitment tool (each computing proximity differently). In-platform is the right call.

- **Sequencing question unresolved:** John asked the right question: what order do these bodies of work go in? Lynsay's read: in-platform service territory creation (with new geolocation tech) is foundational and goes first in Q3. Talent pool linking and smart dispatch network expansion are then layered on. The group didn't finalize whether proxy logic should ship now vs. waiting for the full service territory build — that estimate work is the next step.

- **Compliance flag on provider-level tiering:** Lynsay flagged a potential compliance risk — a recent conversation about capacity requirements at the provider level may apply to setting tiers per provider as well. Ryan noted this would mean the existing service territory tiering is already in scope for the same question. Group agreed this needs legal/compliance review before building out more sophisticated tier-mapping features.

---

## Action Items

- **Chuck:** Evaluate technical architecture — assess the proposed data storage approach for service territory and talent pool integration, confirm feasibility before development begins.
- **Chuck:** Finalize private network support — complete the work to turn the current sandbox prototype into a fully supported feature.
- **Ryan Schulte:** Hold working session with engineers to review each job to be done and size efforts (small / medium / large).
- **Ryan Schulte:** Create a comparison table showing what each option (proxy logic now vs. waiting for standard service territories) provides, to inform sequencing decisions.
- **The group:** Investigate compliance risks around setting tiers at the provider level before building out advanced tier-mapping functionality.
- **The group:** Estimate the development effort for proxy logic vs. waiting for standard service territories, and evaluate the timeline gap between the two paths.
