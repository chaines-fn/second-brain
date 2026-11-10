# Define Flightboard Rollout Plan — 2026-08-11

**Date & Time:** Tuesday, August 11, 2026 (~27 minutes)

**Attendees:**
- Andrew Kandels — Engineering lead, UI modernization (day-to-day technical lead)
- Maria Saxton — Linear project lead, WWT Phase 1 / Core-UI portfolio (calendar organizer)
- Chuck Haines — Sr. Principal Software Engineer, Field Nation

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1rcM5QldRi5wLRZV1or6tiC1V4zVzZlGpycMX5VDTaLI/edit)

**Note:** The transcript's speaker labels are generic ("Kuala Lampur 840 (Baker Center, 8)") rather than per-person, so individual lines below can't be reliably attributed to a specific attendee. Notes reflect the group's collective discussion.

---

## Overview

This was a working session to define the rollout plan for the remaining 60% of Flightboard buyers, now that the team can start shifting contractors back to this work after being pulled onto WWT for several months. The group worked through two open technical blockers before turning to sequencing: a column-dropping bug believed to be role-related (affecting heavy power users with manager/admin role combinations more than smaller buyers running admin-only), and a recurring "white screen" deployment issue caused by browsers serving stale cached pages despite no-cache headers. Neither has a reliable reproduction step, so the group agreed the real test of whether they're fixed will be the absence of new complaints once rollout resumes.

The second major thread was the MUI v9 migration. The team decided to freeze the old MUI repository starting Monday, August 17, and swarm to migrate any outstanding changes into the already-upgraded MUI v9 package within roughly one to two weeks — otherwise the team stays permanently behind. Downstream services that depend on MUI updates will need to bump to service version 9 once the freeze takes effect. The group flagged Harness/CI infrastructure work as a wildcard that could add significant time if it turns out to be required, and agreed to announce the freeze via Nge News before it starts.

On rollout sequencing, the group settled on pushing the remaining 60% of buyers in one move (rather than staged increments), holding back roughly 7 identified high-risk buyers and about 15 previously opted-out buyers for last, in that order — splitting those two groups keeps their issues from landing all at once. Coordination with Customer Success Representatives was called out as the next concrete step to get buyer-specific communication moving. The group also revisited (but didn't act on) the idea of a standing "early adopter" or "willing tester" buyer group for future feature rollouts.

---

## Key Notes

- **Column-dropping bug — believed fixed, not confirmed:** Root cause theorized as role-related: heavy power users with manager/admin role combinations appear affected, while smaller buyers running admin-only roles have shown zero issues despite months in the 40% rollout. No reliable reproduction steps exist, so the group can't definitively confirm the fix — validation will come from watching real-world behavior once rollout resumes.

- **White-screen deployment issue — recurring, root cause identified:** Despite an earlier three-part mitigation plan (2 of 3 steps completed), the issue recurred. Browsers are serving a cached version of a page that no longer exists, even though nginx is sending no-cache headers. Planned fix: introduce a CDN to retain old assets a bit longer, paired with existing version-check logic that prompts users to reload. Estimated at about one week, though the group noted that any meaningful Harness/infrastructure involvement could push that out by a month — a caveat raised as a pattern from other teams' infra work, not specific to this fix.

- **MUI v9 code freeze decided:** The old MUI repository will lock for updates starting Monday, August 17. Any team needing to update MUI after that point must bump their service to v9 and consume the new core-ui package instead. The team will "swarm" to migrate whatever's outstanding in the old repo before the freeze, estimated at one to two weeks of catch-up. Two Nge News announcements planned — one this week, one closer to the freeze date — to coordinate with other teams whose services depend on MUI.

- **Feature flag suspicion for Provider Success Score issue — unvalidated:** Working theory is that feature flags aren't wired up correctly in the new Flightboard for this feature. Plan is to validate by comparing old vs. new views as a staff user in production, which bypasses feature flags entirely.

- **Rollout sequencing decision:** Push the remaining 60% of buyers in one wave once confident the two technical fixes hold, then the ~7 high-risk buyers, then the ~15 opted-out buyers last. Reasoning: splitting the two smaller, riskier buckets avoids getting hit with both sets of issues simultaneously. Next step is identifying CSRs per affected buyer and agreeing on a communication cadence (days-after-communication before rollout).

- **Validation approach — indirect, due to no repro steps:** Since neither the column-dropping nor white-screen issue can be reliably reproduced, the group agreed the strongest signal that both are fixed will be ramping from 40% to 100% (excluding the high-risk and opt-out buckets) and watching for whether new issues surface.

- **Early adopter / willing tester group — raised again, not acted on:** One participant noted this idea was proposed months ago (framed at the time as an opt-in flag) and rejected; it resurfaced here as worth revisiting given how long feedback has taken to surface on past issues, but no action was assigned.

- **Cross-reference:** This meeting appears to be the direct source of the Flightboard Rollout milestone and tickets (OBS-480 through OBS-484) that were first surfaced through Linear/Slack sync on 2026-08-12 — the MUI freeze date, phased rollout sequencing, and high-risk/opt-out holdback described here all match what's tracked in Linear.

---

## Action Items

- **[The group]:** Catch up on Flightboard changes accumulated during the pause.
- **[The group]:** Verify the column-dropping fix is fully resolved through testing (no confirmed reproduction steps available).
- **[The group]:** Implement CDN-based fix for the white-screen deployment issue (~1 week estimate).
- **[The group]:** Document rollout requirements/pre-reqs needed before introducing Flightboard at WWT's current scale to other high-volume buyers.
- **[The group]:** Begin MUI v9 code freeze on the old MUI repository starting Monday, August 17; swarm to complete the migration (~1-2 weeks).
- **[The group]:** Announce the code freeze via Nge News (once this week, once closer to the freeze date).
- **[The group]:** Identify CSRs for high-risk and opt-out buyers and coordinate rollout communication and timing.
- **[The group] (implied):** Monitor for new issues after ramping to 100% (excluding high-risk/opt-out buyers) as the primary signal that the fixes hold.
