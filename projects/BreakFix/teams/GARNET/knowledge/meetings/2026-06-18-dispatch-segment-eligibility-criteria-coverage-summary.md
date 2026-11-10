# Release Sync: Dispatch Segment + Eligibility Criteria with Coverage Summary — 2026-06-18

**Date & Time:** Thursday, June 18, 2026 (time not captured in source doc) (~45 minutes)

**Attendees:**
- Ahtasham Billah Himel — Engineer, GARNET (led the demo)
- Lynsay Faucett — PM, GARNET
- Amran Hossain — Engineering Manager (BD teams)
- Bilas Sarker (goes by "Bilas Chandra" in this transcript) — Engineering Lead, GARNET
- Ridi Haque — Engineering Manager, GARNET + MARBLE (meeting organizer/invitee)
- Habib Rahman — Engineer, GARNET
- Emdadul Islam — Engineer, GARNET
- Azizul Hakim — Engineer, GARNET

*Note: Mahim was referenced (drawer testing/copy work) but did not appear as a speaker in the transcript — not counted as a confirmed attendee.*

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1s0vsbyUxjxQQ60Zs1YOOx7dWAKZTUCE01doiUT-FGMo/edit)

---

## Overview

Ahtasham walked the group through Smart Dispatch V3, which builds on V2 by splitting provider sourcing into three optional segments — Service Territory (ST), Talent Pool (TP), and Marketplace (MP) — versus V2 where ST was a mandatory attachment. New in V3: a hard 60-mile distance filter applies across all segments, and a "talent pool" concept is introduced alongside configurable per-segment criteria. The live demo hit a snag partway through when the talent pool summary page failed to load; Habib suspected a permissions issue (staff vs. buyer login), and Ahtasham suspected it might trace back to a dependent team's (DTP) service currently having issues — unresolved by end of meeting.

The group spent significant time aligning on assignment/ranking logic and failure semantics. Ahtasham and Lynsay walked through the pipeline step by step: eligibility filters → work order qualifications → 80/20 tier-based selection → round-robin tie-break within a tier. Bilas surfaced a live bug during the demo — a work order with talent pool providers available still failed when service territory was enabled but had no service attached — and the group agreed this is wrong: smart dispatch should only fail if *all* enabled segments come up empty, not just one. Amran pressed on integration risk given past V1/V2 issues; Ahtasham confirmed the team assessed this and found no required integration changes, since the logic now checks whether a setting is enabled rather than hard-depending on service territory.

The back half covered rollout mechanics and a couple of UI decisions. Lynsay will own migrating buyers from V2 to V3 (DoorDash is the primary active user; others are testing), framing it to account owners as a look-and-feel change rather than a system shift, and will centralize all migration/support communication into the "ask smart dispatch" Slack channel to avoid the fragmented DMs seen in past releases. Bilas and Lynsay aligned that the auto-dispatch button should be hidden whenever smart dispatch is enabled on a project, to prevent both running simultaneously — this closes a gap that existed in earlier versions. The meeting closed on a copy fix (dropping inaccurate "drug screening" language from the qualifications text) and a plan to send a demo video once outstanding bugs and copy are resolved.

---

## Key Notes

- **Hard 60-mile distance filter is new in V3:** In V2, service territory had no distance limitation. In V3, any provider more than 60 miles away is excluded outright — a behavior change buyers should be aware of during migration.

- **Smart dispatch failure criteria redefined:** Bug found live in the demo — dispatch failed when ST was enabled but had no service attached, even though talent pool had eligible providers. Group agreed: failure should only occur if *no* providers are found across *all* enabled segments, not just one empty segment. This needs to be fixed (see action items).

- **Ranking pipeline confirmed:** Eligibility filters → work order qualifications → 80/20 tier selection (fill 80% of quota from Tier 1 before moving to Tier 2) → round-robin only applies when there's a tie among multiple providers within the same tier.

- **No major integration changes required:** Amran asked about integration risk based on past V1/V2 pain. Ahtasham confirmed the team checked and the dependency logic now keys off whether a setting is enabled, not a hard requirement on service territory — so integrations should be unaffected.

- **Auto-dispatch hidden when smart dispatch is enabled:** Bilas and Lynsay aligned that a project with smart dispatch enabled should not also show the auto-dispatch button, closing a gap Bilas noted existed since the initial smart dispatch release. Lynsay floated a longer-term idea of nudging buyers toward smart dispatch via a modal rather than just hiding the button — noted as a future-state idea, not in scope now.

- **Migration approach for V2 → V3:** Lynsay wants to move all buyers to V3 to avoid running two versions long-term. DoorDash is the only heavy active user today; others are testing individual work orders. Migration should read as a look-and-feel change to account owners, not a system change they need to understand.

- **Support channel centralized:** Past releases saw fragmented support requests (DMs to different people). Lynsay is centralizing all smart dispatch migration/support traffic into the "ask smart dispatch" Slack channel, filtering escalations from there into GARNET/PEX-leads channels as needed.

- **Talent pool summary page loading issue — unresolved:** Blocked part of the live demo. Habib suspected a staff-vs-buyer login/permissions issue; Ahtasham suspected a dependency on another team's service (DTP) that may currently be having issues. Cause not confirmed by end of meeting.

- **Copy fix needed:** Current work order qualifications text says "meets all background and drug screening qualifications" — inaccurate, since the qualification in question doesn't test screening. Lynsay wants it changed to "provider meets all work order qualifications."

- **Coverage summary / eligibility criteria demo deferred to production:** Bilas asked to demo eligibility criteria + coverage summary, but the feature wasn't loading properly in the lower environment. Suggested showing it from production instead since the eligibility indicator has already been merged there.

- **Assigned provider drawer nearing release:** Ridi and Emdadul reported the drawer (built with Marble team) was in final testing and expected to release later that day (June 18). Full validation across both teams' pieces wasn't confirmed complete.

---

## Action Items

- **[The group]:** Investigate and fix the talent pool loading issue seen in the demo (possible permissions or DTP-dependency cause — unconfirmed).
- **[Ahtasham Billah Himel / group]:** Fix the failure-criteria bug so smart dispatch only fails when *all* enabled segments have zero eligible providers, not just one.
- **Lynsay Faucett:** Manage the V2 → V3 migration, starting with DoorDash.
- **Lynsay Faucett:** Plan and communicate the platform update timeline to account owners.
- **Lynsay Faucett:** Review drawer copy in the Figma file against actual application text.
- **Ahtasham Billah Himel (implied):** Review past V1/V2 release issues proactively to flag risks ahead of the V3 release (per Amran's request).
- **Mahim:** Test the assigned provider drawer functionality ahead of release.
- **Emdadul Islam:** Send the demo video to the team once outstanding bugs and copy fixes are resolved.
