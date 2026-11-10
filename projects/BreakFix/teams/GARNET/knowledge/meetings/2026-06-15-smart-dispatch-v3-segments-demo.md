# Demo: Dispatch Segments & Eligibility Criteria (Hard Filter) — 2026-06-15

**Date & Time:** Sunday, June 15, 2026, ~10:00 AM CDT (~63 minutes)

**Attendees:**
- Lynsay Faucett — PM, GARNET (meeting organizer)
- Ridi Haque — Engineering Manager, GARNET & MARBLE
- Bilas Sarker — Engineering Lead, GARNET
- Ahtasham Billah Himel — Engineer, GARNET (demo presenter)
- Azizul Hakim — Engineer, GARNET
- Habib Rahman — Engineer, GARNET
- Emdadul Islam — Engineer, GARNET

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1eJj8CUNCDkxC7Wvwam-vkARq2adHBIrEqGl_9bNQZ_s/edit)

---

## Overview

Himel ran a live demo of Smart Dispatch v3, which introduces configurable segments (Service Territory, Marketplace, Talent Pool) and moves away from the mandatory service territory requirement in v2. The session covered how the new filtering and ranking pipeline works, walked through a talent pool dispatch demo in the lower environment, and surfaced several UX gaps and edge cases the team needs to address before broader enablement.

The most substantive discussions were around (1) clarifying the correct order of operations in the filtering/ranking pipeline, (2) a W2 provider edge case that generated a notification the provider couldn't act on, and (3) UX problems with how invalid configurations are currently surfaced to buyers. The team landed on removing the service territory requirement for displaying the smart dispatch button and on adding save+validation to the settings page to prevent leaving an invalid state. The Auto Dispatch vs. Smart Dispatch button conflict was identified but tabled.

The demo itself was largely successful despite lower-environment data discrepancies. Provider 25 was assigned via the talent pool segment as expected, and the team now has a shared understanding of how the filtering, qualification, and 8020 logic stack together.

---

## Key Notes

- **Pipeline order of operations:** Provider match criteria runs first as a hard filter (eligibility/criteria config). Then work order qualifications (equipment & insurance) are applied. Then 8020 tier logic runs on the remaining pool. Provider match and 8020 are complementary processes — match filters, 8020 selects. Match is *not* applied again at the end for final selection (Lynsay had initially assumed it might be).

- **Work order qualifications (equipment & insurance):** These were handled mid-pipeline in v2 only for marketplace expansion in certain cases. In v3, they are mandatory for all segments and slot in after the provider match criteria filter but before the 8020/best-match decision. More criteria (background check, last login, etc.) are planned for future phases.

- **Service territory no longer required for smart dispatch button:** Known bug — the UI currently blocks the Smart Dispatch button unless a service territory is selected. Lynsay approved removing this requirement since v3 now supports Talent Pool and Marketplace independently. Himel has this as an action item.

- **Auto Dispatch vs. Smart Dispatch button conflict:** Bilas raised that the Smart Dispatch button currently *replaces* the Auto Dispatch button on the work order detail page. This means a buyer using a break/fix project can no longer fall back to Auto Dispatch. Lynsay tabled the discussion — it's more complex than expected and needs a separate conversation.

- **W2 provider edge case:** Provider 353 received a notification for the demo work order but couldn't request it due to a temporary W2 conflict with the buyer. Lynsay's position: this should be treated as an optimization — filter these providers from notifications proactively (similar to how provider 25 was excluded from visibility). Himel flagged a performance concern since V2 is already slow, but Lynsay said this doesn't block v3 launch. Action: document the edge case and plan a fast-follow.

- **2-minute processing latency window:** The background cron runs every 2 minutes. During that window after publish, a work order is technically accessible before talent pool restrictions are enforced — Habib found provider 30 could access the work order via direct URL during this spike. Bilas noted any such requests would be discarded during ranking anyway, so there's no assignment risk. Himel has a plan to close this gap but deprioritized it given how unlikely the scenario is in practice.

- **Configuration validation — settings page:** Currently, if a buyer disables all three segments, they see only a yellow warning banner on the work order screen when they attempt to smart dispatch. Lynsay pushed for a stronger UX: add a save function and page-level validation so buyers can't leave the Smart Dispatch settings page with all segments disabled. This is the preferred future state, not blocking v3 launch, but should be addressed before GA. The asynchronous/integration dispatch path (non-UI) was noted as a separate concern.

- **Marketplace-only behavior:** When only the Marketplace segment is enabled, the delay dropdown should be locked to "immediately" — there's no meaningful delay when marketplace is the only segment. The proposal is to hide the dropdown in that state and store the previously configured value so it restores when other segments are re-enabled. Lynsay said she'll mock this up.

- **Lower environment data discrepancy:** The demo showed 5 providers visible for the talent pool when only 2 were configured. Attributed to the lower environment pulling provider data from the data warehouse via dynamic services rather than real-time data. Expected behavior, not a bug in v3 logic.

- **Smart dispatch toggle only visible to staff admin:** The buyer view doesn't show the enable/disable toggle for smart dispatch — it's only visible when logged in with a staff admin account. Azizul confirmed. Not a bug; expected behavior.

---

## Action Items

- **Ahtasham Billah Himel:** Remove the service territory requirement for showing the Smart Dispatch button — button should display and function with Talent Pool or Marketplace alone.
- **Ahtasham Billah Himel:** Document the W2 provider edge case (providers who can't request due to temp W2 assignments) and plan how to filter them from notifications to reduce noise.
- **Ahtasham Billah Himel:** Investigate and address the 2-minute spike window where a newly published work order is accessible before talent pool restrictions are enforced.
- **Lynsay Faucett:** Mock up marketplace configuration UX to hide the delay dropdown when only Marketplace is enabled (and store old value for restore).
- **Lynsay Faucett:** Document meeting takeaways and respond to the two questions Bilas added as comments in the shared document.
- **Lynsay Faucett / Ridi Haque:** Schedule a follow-up session (later this week) to walk through additional use cases after current adjustments are made.
- **Bilas Sarker (implied):** Continue in-progress work on the ranking detail section in the work order UI (talent pool request count + criteria display from the match/MARBLE side).
