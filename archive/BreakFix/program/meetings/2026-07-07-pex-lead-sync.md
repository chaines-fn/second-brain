# B/F PEX Lead Sync — 2026-07-07

**Date & Time:** Tuesday, July 7, 2026, 8:30 AM CDT (duration not stated in transcript)

**Attendees:**
- Lynsay Faucett — PM, GARNET (meeting organizer)
- Ryan Schulte — PM, SLATE
- John Shaw — Group PM (cross-stream)
- Amran Hossain — Engineering Manager (all BD teams)
- Ridi Haque — Engineering Manager (GARNET + MARBLE)
- Mobarok Hossain — Engineering Lead, SLATE
- Bilas Sarker — Engineering Lead, GARNET (email: bilas.chandra@fieldnation.com)
- Alyssa Eby — UX, GARNET
- Chuck Haines — Sr. Principal Engineer (cross-cutting)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1Kiv2UvlLj-oOqwF7gBwkDu4C-7NESSGN7PN4l3n0K4o/edit)

---

## Overview

The meeting opened with Alyssa crediting Chuck for turning around a planning deck and clearing a GitHub access blocker quickly, using the right templates — a short aside that fed into a broader "ship it, don't wait for perfect conditions" agreement between the two of them.

The bulk of the sync continued the standardized-service-territory thread that's been running since the May 29 AWS isolines proposal: zip codes vs. the Uber H3 hex model. Ryan opened by framing the foundational architecture question — should service territories be permanent objects linked back to defaults, or independent copies once customized? Chuck flagged that live links between customizations and defaults introduce real technical complexity and edge cases, which pushed the group to question how much customization is actually needed. Ryan's take: buyers likely don't need open-ended polygon drawing — most customization requests are really about reshaping a territory to match a specific site list. Mobarok then laid out the technical contrast directly: H3 gives uniform cell sizes everywhere, unlike zip codes, which vary wildly in size and shape. The group aligned on standardizing at H3 resolution 4, and Ryan and Mobarok agreed H3 is the more sustainable long-term model for accurate radius-based definitions.

From there the discussion moved to how to manage customization without exploding internal support burden. Ryan proposed "smart consolidation" — automated merging tools to fix imbalanced territories — instead of manual redrawing, and suggested dispatch logic could be solved separately from reporting/dashboard views. On cascading updates, Ryan noted H3's comprehensive coverage could reduce the need for cascade logic entirely, since it avoids the "gap" problem that plagues zip-code-boundary updates. Alyssa raised the internal-support-cost concern directly — every unique customization is something the team has to maintain — while Ryan countered that NetDev genuinely needs customization to recruit for specific project needs (Jeff is the primary voice on this). The group's working resolution: solve NetDev's actual pain points (reporting noise, recruiter workflow) through better reporting/dashboard tooling rather than opening up territory customization itself. Data granularity came up as a hard requirement either way — Ryan emphasized attributes like Labor Market Index and density classification need to live at the lowest object level (H3 cell or zip code) so they can be accurately re-averaged if territories get merged or reshaped; Chuck and Lynsay worked through the mechanics of that averaging. Ryan also floated versioning defaults (e.g., v1.03 vs. v1.07) so updates don't force changes onto live projects, and Chuck noted this intersects with historical reporting — changing a WO's territory after the fact means needing something like SCD Type 2 to preserve point-in-time accuracy. Ryan closed with a longer-term view: as automated recruiting matures, territory *shapes* become less load-bearing since the system will increasingly just prioritize provider-to-site distance directly.

Lynsay wrapped by pointing to Thursday's stakeholder meeting (Jeff, Mia, and Jake Reswan) as the forcing function: Ryan will present the solution proposal addressing Jeff's customization concerns, and Mobarok will demo the H3 model directly to Jeff. The zip-code-vs-H3 decision itself is still open pending that session.

---

## Key Notes

- **Service territory architecture — copy vs. linked, still unresolved:** Ryan raised whether customized territories should be permanent objects linked to defaults or independent copies. Chuck's position: live links create real technical complexity and edge cases. This is logged as "Needs Further Discussion" in Gemini's decision tracking, alongside the H3-vs-zip-code technology choice and the permissible level of customization — none of the three are settled yet.

- **H3 resolution 4 confirmed as standard:** The one item Gemini logged as "Aligned" — H3 cell resolution is set to 4 for consistent territorial definition. This matches the resolution 4 direction referenced in the June 30 PEX sync notes.

- **Customization scope reframed as site-list matching, not free-form drawing:** Ryan argued buyers don't need open-ended polygon tools — most requests are really about reshaping a territory to fit a specific list of sites. This reframes the customization problem as narrower than previously assumed.

- **H3 vs. zip codes — technical case made directly:** Mobarok explained H3 (Uber's Hexagonal Hierarchical Spatial Index) gives uniform cell sizes across all locations, unlike zip codes which vary significantly in size/shape. Mobarok and Ryan agreed this makes H3 the more sustainable long-term model for accurate, radius-based territory definitions and reduces the "gap" problem inherent to zip-code boundary updates.

- **Smart consolidation proposed over manual redrawing:** Instead of letting users manually redraw territories, Ryan proposed automated "smart consolidation"/merging tools to fix territories with imbalanced site counts. Chuck suggested dispatch logic could be handled separately from reporting/management dashboard views — i.e., don't force one solution to serve both use cases.

- **NetDev's actual need vs. the proposed fix:** Alyssa flagged the internal support burden of managing many unique customizations. Ryan countered that NetDev (Jeff) genuinely needs customization to recruit for specific project needs. Working resolution discussed: address the underlying pain points (reporting noise, recruiter workflow friction) through better tooling rather than opening up territory customization itself.

- **Attribute granularity is a hard requirement regardless of the model chosen:** Ryan stressed that attributes like Labor Market Index and density classification must be tracked at the lowest object level (H3 cell or zip code), so metrics can be accurately re-averaged if a buyer merges or reshapes territories. Chuck and Lynsay discussed the mechanics of that averaging for custom territories.

- **Versioning proposed for default territory updates:** Ryan suggested versioning defaults (e.g., v1.03 → v1.07) so changes don't automatically force updates onto existing live projects. Chuck flagged this intersects with historical reporting — territory changes mid-flight complicate point-in-time reporting and likely require an SCD (slowly changing dimension) Type 2 pattern to track accurately.

- **Automated recruiting could eventually make territory shapes less critical:** Ryan's longer-term framing — as automated recruiting matures, the system can prioritize provider-to-site distance directly, reducing reliance on precisely drawn territory boundaries.

- **Unicus examined as the hyper-local customization example:** The group looked at Unicus's use of very specific, hyper-local territories to manage cost and technician relationships. Lynsay suggested some of these needs might be better solved by consulting on the benefits of larger, more automated provider pools rather than manual territory customization.

- **Thursday stakeholder meeting is the decision-forcing event:** Ryan will present the solution proposal to Jeff (framing how reporting/dispatch tooling addresses his concerns without manual merging); Mobarok will demo the H3 model directly. Attendees include Jeff, Mia, and Jake Reswan. The zip-code-vs-H3 decision and the cascading-updates approach are both pending this session.

---

## Action Items

- **Ryan Schulte:** Present the solution proposal to Jeff — the specific combination of tools and platform changes that solve current service territory management pain points, addressing his customization concerns by showing how reporting/dispatch can be handled without manual merging.
- **Mobarok Hossain:** Present the H3 demo to Jeff during Thursday's meeting (Jul 9), explaining how the model handles service territory mapping compared to the current zip-code approach.
- **The group:** Finalize the zip-code vs. H3 cell model decision for service territory definitions, evaluating long-term scalability and the requirement to prevent coverage gaps.
- **Lynsay Faucett:** Schedule a follow-up session to continue the talent pool mapping / network expansion discussion.
- **Mobarok Hossain:** Present H3 technology research and findings at the Thursday stakeholder meeting.
