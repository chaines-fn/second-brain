# Weekly BF Strategy and Deep Dive Working Session — 2026-07-06

**Date & Time:** Monday, July 6, 2026, 11:00 AM CDT (~58 minutes)

**Attendees:**
- Ryan Schulte — PM, SLATE
- Jeff Novotny — Internal NetDev customer (primary stakeholder), SLATE
- Lynsay Faucett — PM, GARNET
- John Shaw — Group PM; Lynsay Faucett's manager; regularly attends cross-cutting BF meetings (invited; did not attend this session — out this week)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1eb27rAC3SM2RuMniZKqkY2yYO4_vjUhhID1rbESASbg/edit)

---

## Overview

This is the recurring weekly BF strategy/deep-dive sync between SLATE and GARNET product. Most of the meeting was a continuation of the service-territory strategy thread that's been running across several program-level meetings — moving default territories off zip codes and onto H3 hex/isoline (drive-time) shapes. This is the important thread going forward: the group didn't land on final answers, but worked through the open problems (how to define shapes, how to handle the "whitespace" gaps between them, how to modify/merge them, and how to keep demographic data attached) so engineering has a clearer starting point when it does its own technical deep dive later this week.

Ahead of the H3 discussion, the team closed out two smaller items: the recruitment tool's service-territory upload bug is fixed, and the L4 label technical design is done and just waiting on Mike's sign-off. They deliberately sequenced open-search work (keyword search across work order descriptions, closing notes, and provider entries) behind the L4 label TD so it doesn't introduce a new technical design mid-sprint. The second half of the meeting was more speculative — how default territories could double as a lightweight, non-disruptive reporting layer for cross-project and cross-buyer comparison, and whether that same idea could improve MSI's CBSA-based geography model.

The planned engineering-focused technical design review for H3/isolines (originally set for July 7) hit scheduling conflicts (an all-company meeting, plus Chuck Haines and Mobarok "Mo" Hossain availability) and is being pushed to later in the week, likely recorded for async review given how hard it is to get the full engineering group together live.

---

## Key Notes

### Service territory / H3 strategy (the main thread — carries forward into next steps)

- **Zip codes are the wrong long-term unit for default territories:** Ryan reintroduced the case for moving default territories off zip codes onto H3 hex cells / isoline (drive-time) shapes — zip boundaries shift over time and don't cleanly match a realistic drive-time radius, leading to oblong or awkward shapes. Both Ryan and Jeff acknowledged neither of them understands H3/isolines well enough yet to fully reason about the tradeoffs — flagged as a gap to close before the team can commit.

- **Whitespace/contiguity is the hard problem, not the shapes themselves:** Today's zip-based defaults are contiguous by construction (unclaimed zips just become "whitespace" but still tile the map). If territories are instead drawn as freeform drive-time shapes, gaps will open up between them. The group didn't converge on a fix — options floated were: bigger sectioned whitespace regions (e.g., splitting the leftover area into quadrants), or defining ad hoc whitespace shapes to fill gaps. Manual effort to make all shapes "touch" was raised as a real concern.

- **Three distinct sub-problems identified (Jeff's framing):** (1) how to define a territory's shape, (2) how to define the area that has no shape yet (whitespace), and (3) how to modify a shape when sites or volume change (merge, split, shift center). The group agreed these are separable problems worth tackling in that order.

- **H3 resolution can flex from country-level to city-block level:** Ryan demoed hex resolution ranging from ~14 hexes covering the whole US (resolution 7, too coarse to be useful) down to half-a-city-block-sized hexes (resolution 11). GARNET has already generated a few standard hex resolutions (Lynsay recalled roughly 4, 6, and one other) but hasn't mapped any Field Nation data (volume, demographics) onto them yet.

- **Demographic data isn't attached to hexes yet — new build required:** Jeff has an existing zip-code-based calculator for cost of living, population density, and labor market classification, built from Maptitude data. No equivalent exists for H3 hexes at any resolution. Lynsay confirmed GARNET would own building this, using the existing zip-based service territories as the starting point and overlaying/averaging that data onto whichever hex resolution is chosen. Jeff flagged that whatever gets built needs to be *updatable* over time (e.g., areas north of Dallas shifting from rural to suburban) — a one-time snapshot isn't enough.

- **Virtually every buyer customizes off the defaults:** Jeff said he hasn't yet built a default territory set that didn't need some modification — usually merging a lone site into a neighboring territory, or shifting/splitting centers based on where a buyer's sites actually land. He sees this as a near-100% rate across the ~100 programs Ryan is planning for.

- **Cost of NOT consolidating small/extra territories:** More territories than necessary means more tech pools to manage, more recruiting overhead (recruiting into corners of a territory that don't need it, or missing coverage on the far side), and a "doesn't look logical" visual impression for buyers reviewing their map. Ryan and Jeff agreed dispatch itself isn't really at risk here — better routing/whitelisting logic can absorb most of that pain — but recruiting and territory-count management are real costs.

- **Proposed direction: keep FN defaults consistent, solve complexity in the UI/reporting layer instead:** Rather than letting every buyer's manual customization fragment the underlying defaults, the idea is to keep ~1,200+ FN-defined defaults stable and consistent, then hide that complexity from users — e.g., visually merging adjacent territories in the UI, or auto-folding/grouping them in reporting (region → state → metro/whitespace) so users only see as much detail as they need.

- **Default territories as a temporary/virtual reporting layer:** Ryan's proposal — treat FN default territories as a filter/overlay applied only at reporting time, without touching the actual (manually customized) territory configuration underneath. This would let Field Nation compare coverage, fill rate, cost, and quality consistently across a buyer's multiple projects (e.g., "show me Chicago across all my projects") even when each project's manual territories don't line up 1:1. Jeff was supportive in general but flagged it likely doesn't work well for Unicus, given their point-radius-based model rather than defined territories.

- **Reporting UI feedback:** Jeff reacted positively to a rough dashboard mockup Ryan showed (grouped/bundled/folder-style layout), calling it a clear improvement over the current Break/Fix reporting interface, which he described as swinging between "total rollup" and "hyper local" with no useful middle ground. He suggested Ryan show it to Alyssa Eby and Mia Mu.

- **MSI / cross-buyer angle raised, not resolved:** The group discussed whether MSI's CBSA-based geography could eventually be replaced with H3/default-territory shapes for cross-buyer insight — a smaller lift if territories stay zip-based, more rework if the team moves to H3. Separately, Jeff mentioned Stephen Wald has asked John Shaw why the team is building a new tool instead of extending MSI for this — noted as an open question the group is already aware of and discussing separately, not resolved in this meeting.

- **H3/isoline engineering deep-dive rescheduled, likely to be recorded:** The dedicated technical design review for H3/isolines (aimed at the engineers) was originally set for July 7 but conflicts with an all-company H1-reflections meeting Jeff has to attend. Lynsay moved it toward Wednesday, but that also has conflicts (Chuck Haines needed but has a technical interview scheduled; Mobarok "Mo" Hossain needs to be present to demo). Lynsay's fallback plan: record the session and collect questions/feedback asynchronously rather than trying to get every engineer in one live meeting.

### Other items

- **L4 label TD done, blocked on Mike's approval:** Lynsay confirmed the L4 label technical design was completed the prior week and is waiting on Mike's sign-off — described as "important and urgent" to land the upcoming sprint.

- **Recruitment tool upload bug resolved:** The service-territory upload issue Erin reported in the recruitment tool has been fixed as of this meeting; Lynsay will follow up with her directly.

- **L4 labels prioritized ahead of open search (aligned):** Lynsay pushed back last week on discussing open search because she didn't want to open new questions while finishing the L4 TD — every new capability currently requires its own technical design, and she was worried scope creep would delay the L4 sprint. Ryan agreed with the sequencing. Open search will follow once L4 wraps.

- **Open search need and scope:** The team needs keyword search across work order descriptions, special instructions, and the provider's actual closing notes (what was done on-site) — not just pre-defined labels. Jeff's data engineer can write ad hoc SQL for known cases, but real open search is needed specifically where no label exists, largely because of typos and free-text variation.

- **AI as a future open-search evolution (not scoped now):** Ryan and Jeff discussed layering an LLM on top of open search later — matching typo variants, adjacent technology terms, model numbers, and product-line names to the right label (e.g., "cradle point" → wireless access point). Jeff noted L4 labels already support an association/replace feature for exactly this kind of product-name mapping, which is a nearer-term way to get some of that benefit without AI. Filed as a future direction, not committed work.

---

## Action Items

- **Lynsay Faucett:** Follow up with Erin to confirm the recruitment tool service-territory upload issue is resolved on her end.
- **The group / Jeff's data engineer:** Write SQL queries to support keyword/open search across work order descriptions, closing notes, and provider entries.
- **Ryan Schulte:** Give the team (starting with Jeff) an explainer/walkthrough of H3 and isoline technology and what it can actually do.
- **Jeff Novotny:** Finish the in-progress documentation on service territory use cases; share before the H3/isoline review so it can inform tool design.
- **Ryan Schulte:** Draw/illustrate how drive-time radius shapes and whitespace territories would work visually, to make the concept easier to discuss.
- **GARNET (per Lynsay):** Build the mapping between demographic data and hex resolutions, using the existing zip-based service territories as the baseline; ensure the result is updatable over time.
- **Ryan Schulte:** Show the new reporting interface mockup to Alyssa Eby and Mia Mu for feedback.
- **Ryan Schulte:** Send Jeff/Lynsay the document on default territory classification rules (zip-code classification today, future H3-based rules, and downstream implications like pay rate or dispatch range by territory class).
- **Lynsay Faucett:** Reschedule the H3/isoline technical design review (targeting Wednesday, pending Chuck Haines and Mobarok's availability); record it and share for async feedback if a live session can't be arranged.
