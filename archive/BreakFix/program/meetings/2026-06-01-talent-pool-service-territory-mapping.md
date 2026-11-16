# Talent Pool ↔ Service Territory Mapping — 2026-06-01

**Date & Time:** Monday, June 1, 2026, 1:00 PM CDT (~59 minutes)

**Attendees:**
- **Ryan Schulte** — PM, SLATE / Network Development (meeting organizer)
- **John Shaw** — Group PM (cross-stream alignment)
- **Chuck Haines** — Sr. Principal Engineer (cross-cutting architecture)
- **Lynsay Faucett** — PM, GARNET / Smart Dispatch

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1ynoqHs9C-K0RpEQFrWZjfardfaVEcdXyhTIDNqjtKhA/edit)

**Related:** [Product brief — Break-Fix Projects & Service Territories](https://docs.google.com/document/d/1sik7Wo9k6ZLgcLEJeye4DDSIRhKis5qVe6N-c2Qte5k/edit)

---

## Overview

Ryan called this session to tackle a growing maintenance problem: as Break/Fix has blended talent pools, smart dispatch, and service territories, buyers (and FN's NetDev team) are stuck managing duplicate, near-identical provider lists project by project — with no easy way to reuse a network across projects or update one list in one place. Ryan's pitch was to make the **talent pool the single source of truth** for providers, linked at the project level, with service territories demoted to containers used for filtering, segmentation, and reporting rather than for storing provider lists.

Chuck pushed the idea further architecturally: since a service territory is *already* nothing more than a group of talent pools under the hood, the two concepts can be collapsed. A service territory becomes a project-level container you attach talent pools to, with optional filtering attributes (zip, pay rate, geo boundary). Smart dispatch then simplifies to three concepts — service territory, talent pools, marketplace — while still covering every known use case. The group aligned on decoupling provider lists from service territories and treating dispatch as paired with talent pools, with territories existing only for reporting/segmentation. The key mechanism: a territory's provider list is a **dynamic view** (filter applied to the linked talent pool), not a stored table — so adding/removing someone from the talent pool automatically flows through to dispatch and coverage.

Two hard problems stayed open: **white-space dispatch** (a provider who'll serve a rural area but doesn't live there) and **coverage reporting**. The agreed direction is to expand dispatch *and* coverage logic to use identical rules — physical address **plus** "regularly worked there in the last ~6 months" — plus a manual **whitelist override** at the territory level for recruited providers who've never worked the area (those should still belong to a talent pool, per Ryan's preference, e.g. a floating/one-off pool). On priority, John questioned urgency: nothing here unblocks net-new capability — it's friction reduction and self-serve polish. The group decided to use **DoorDash's upcoming second Break/Fix project** as the real-world test of how painful the friction actually is before committing to the full talent-pool-as-source-of-truth migration. Two near-term, non-throwaway wins were identified: a **non-static distance filter** (AWS Location isolines, e.g. 10/20/60/100 mi) and adding **work-history** as a dispatch/coverage criterion. John asked for a follow-up walkthrough of mockups against the three jobs-to-be-done before development proceeds.

---

## Key Notes

- **Core problem — talent list maintenance:** Buyers increasingly reuse networks across projects (e.g. reusable gold/silver/bronze tiers plus marketplace), but today they'd have to create and populate dozens/hundreds of service territories per project, then edit each one individually to add/remove a provider. This is both an external (buyer self-serve) and internal (NetDev managing networks across projects/programs) pain point. Original assumption — that B/F was low-liquidity and projects would always be unique — turned out wrong.

- **ALIGNED — Talent pool as primary provider source:** Service territories are re-architected as *containers* for talent pools rather than independent provider lists. Talent pools can be linked at the project level (default for the project) or directly at the individual service territory level. Ryan favors linking to the project so pools can operate even without a territory existing.

- **Chuck's architectural insight — collapse the two concepts:** A service territory is already just a group of talent pools that FN decided not to surface as talent pools. So flip the model: a service territory is a project-level container; attach as many talent pools as you want; use its attributes (zip, pay rate, geo boundary) for *optional* extra filtering. This leverages the ranked/unranked talent pool work already happening in Q2 and simplifies smart dispatch to three concepts. "We could build both models" (project-level inheritance vs. explicit per-territory linking) — decision is which solves the bigger use case first, then layer the other on.

- **The "view" mechanism (how membership works):** A territory's provider list is *not* a stored table — it's a dynamic view = (linked talent pool) filtered by the territory/dispatch rules. Chuck's whiteboard example: talent pool has providers 1–5; territory filter = "within 10 mi of zip X"; the view auto-includes only those who match, every time smart dispatch runs. Add someone to the pool who matches → they're instantly eligible for the next WO; remove them → instantly gone. Reduces everything to a talent-pool management problem.

- **White-space dispatch problem:** Common case — provider lives in Minneapolis but serves a rural town 2 hrs away. Pure distance filter misses them. Fix: dispatch logic should match on physical address **OR** where the provider has regularly done work (last ~6 months). Ryan also raised an Uber-style **ever-growing radius** model — keep expanding (e.g. up to 200 mi) until N providers found in rural areas, stop early (~20 mi) in dense areas. Chuck: AWS Location Services returns up to 5 isolines at once (drive times), so the DB can be queried across 10/20/60/100 mi bands in one pass and sorted by distance.

- **Coverage reporting must use identical logic to dispatch:** Chuck was firm — coverage analysis must use the *exact same* filter logic as smart dispatch (work history + distance), or the numbers won't match. This covers ~95% of cases. The remaining case (a recruited provider who's never worked the area and doesn't meet distance) needs the whitelist override.

- **Whitelist / override rule:** Add an explicit "provider ID IN (…)" override so specific recruited providers are included in a territory even if they fail the automated filters. Open question resolved toward: the whitelisted provider should still belong to *a* talent pool (e.g. a floating/one-off pool) rather than existing only as a free-floating ad rule — otherwise lists fragment across projects and become unmaintainable. These overrides are theoretically temporary (once the provider works the area, the work-history filter catches them). The inverse — a **ban rule** to hide a provider from a territory — was also noted.

- **Where rules live — territory level vs. smart dispatch level:** Debated. Lynsay argued for the service-territory level so coverage analysis/reporting can run per territory (and potentially surface in MSI later). Chuck agreed territory-level is more explicit and supports a future where one dispatch rule spans multiple territories. Functionally equivalent either way; the choice is about reporting granularity and explicitness.

- **Unicus migration challenge (Lynsay):** Unicus currently runs ~179–200 separate talent pools, one per service territory / pickup-dropoff location, all equal tier. In the new model they'd need just **one** talent pool with a rule (e.g. 25-mi radius / 25-min drive time around each site, which Ryan estimates covers ~95% of their providers). Migration is scriptable, but the hard part is *convincing the customer* the automated filter logic matches or beats their manual list management. One-time conversion effort to explicitly whitelist the oddball providers who don't meet the rule.

- **Default service territories:** Purely geographic templates; they come first (create territories, then link talent pools — though order is flexible). Mapping logic is identical whether a territory is a default template or a pre-existing custom one — talent pool providers just flow through the rules. Defaults don't change provider-membership behavior.

- **Workflow Ryan sketched:** Setting up a project → pick/link talent pools → set project-level rule (e.g. "live or worked within X mi") → providers auto-map across all territories into the dynamic view → optionally open an individual territory and apply an override (e.g. 100 mi instead of 10, plus a specific provider ID). Both dispatch and coverage then read those rules. Note: territory *membership* (e.g. a manual whitelist of "Zoe") does **not** travel with the talent pool to another project — only the pool does; overrides are re-applied per project.

- **Priority / urgency (John ↔ Ryan):** This is primarily a self-serve + friction-reduction effort — it doesn't unlock anything FN can't do today. SLATE doesn't hit this until ~Q4. Decision: don't commit to the full migration yet. Use **DoorDash's second B/F project** (first buyer with two unique B/F projects) to measure real friction. Short-term, possibly just patch the friction (easier multi-project add/remove). If that becomes unmanageable, prioritize talent-pool-as-source-of-truth.

- **Near-term, non-throwaway wins:** (1) Non-static distance filter via AWS isolines — layers cleanly onto Q2 filtering work, candidate for Q3 or a Q2 swap. (2) Add "has worked in this area" to the distance criterion — likely the more common white-space case (more likely they've worked there than not). Chuck floated taking the work-history criterion to the **Provider Match** team as a possible global default / checkbox, since Match is the source of truth for filtering and FN's services build on it anyway. Ryan jokingly resisted looping in Provider Match ("how much could I bribe you…"), but Chuck's point stands: bring them along rather than build in isolation.

- **Q2 work is validated, not wasted:** The talent pool expansion work currently in flight directly supports this model — if FN adopts the two-layer filtering model (project/dispatch-level + territory-level), all of it is needed anyway. Nothing discussed makes current work throwaway.

---

## Action Items

- **The group:** Model the workflow — illustrate how talent pools, service territories, and smart dispatch interact across all three jobs-to-be-done, and walk the team through it for review.
- **The group:** Develop a Unicus migration + validation plan for consolidating their ~179 pools into one, proving the new filtering logic meets their operational expectations.
- **The group:** Expand smart dispatch filtering logic to include provider work history (in addition to physical address); ensure the coverage analysis tool uses identical logic.
- **Ryan Schulte:** Analyze filter coverage — calculate the % of providers added by a work-history filter vs. a standard distance filter, to determine whether the functionality is worth building.
- **Ryan Schulte:** Schedule a follow-up workflow review (~1 week out) to present a mockup of how the 3 jobs-to-be-done work in the new model.
- **Chuck Haines:** Consult the Provider Match team on whether to update the global distance-filtering logic to include a work-history criterion — explore feasibility of adding it as a standard checkbox feature.
- **(implied) The group / John Shaw:** Use DoorDash's upcoming second B/F project as the test case to gauge real friction before committing to the full talent-pool migration.
