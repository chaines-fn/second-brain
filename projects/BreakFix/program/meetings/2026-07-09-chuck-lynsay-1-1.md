# Chuck / Lynsay 1:1 — 2026-07-09

**Date & Time:** Thursday, July 9, 2026, 1:00 PM CDT (~33 minutes)

**Attendees:**
- Lynsay Faucett — PM, GARNET
- Chuck Haines — Sr. Principal Software Engineer

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1pznON1qnUjG1NU6MGh28KkvF8DxeiOubFFp4a0ldAEM/edit)

---

## Overview

Lynsay booked this time right after the day's back-to-back service territory meetings (Service Territory Solution Discussion Cont. and the Standardized Service Territory H3 POC Review with Jeff/Rezwan/Jake) to decompress and regroup with Chuck as a thought partner. The H3-vs-zip-code debate had felt resolved 40 minutes into the stakeholder session, then unraveled again in the final ten minutes when Jeff re-raised the need to maintain zip codes — leaving Lynsay frustrated despite otherwise good progress. She and Chuck talked through why: Jeff's argument that "buyers speak in zip codes" conflates a historical/vendor artifact (Mapitude, buyer habit) with an actual technical requirement, and the real gap is that no one has shown Jeff a visualization of what an H3-based territory would look like to a buyer. Plan: Ryan will extend his existing visualization tool to render H3 territories as clean outlines (not raw hexagons) so the team can demo it directly to Jeff, Unicus, and Clear Captions. The decision is a high priority but doesn't block Q3/H2 planning — deadline is mid-August, before the technical design phase.

Lynsay also flagged two roadmap gaps she considers P0 that aren't currently tracked: work orders need better escalation/visibility once Smart Dispatch is running (buyers trust routed work orders more because they land in their own list; SD work orders currently just sit in the general publish state with no urgency signal), and providers need a way to self-identify preferred working areas rather than the platform inferring it purely from history — echoing a point Mike Mueller made in an earlier session about just asking providers directly.

The conversation closed on organizational changes: Chuck said it's likely (not yet finalized — pending a conversation with Cory Muir) that he won't continue as B/F's principal engineer into H2, since the AI Software Factory initiative needs his focus and is currently behind schedule. To offset that, he interviewed a candidate the day before (Jul 8) for a new Principal Engineer role based in Bangladesh, which would give the BD engineering team daily coverage he can't provide today. Both agreed Break/Fix itself is trending toward winding down by Q4 2026 — Chuck noted MARBLE was never expected to survive into H2 in its current form, and floated (with Lynsay agreeing) that MARBLE could end up absorbing the NetDev-to-product transition work as parts logistics winds down, bridging the B/F context into whatever comes next. Chuck was candid that he's skeptical of the "core transform" initiative that leadership wants to pull additional teams into, citing its lack of clear deliverables and repeated pivots so far.

---

## Key Notes

- **H3 vs. zip code decision still open, deadline mid-August:** Despite feeling resolved mid-conversation at the stakeholder demo, Jeff re-raised zip-code maintenance concerns at the very end. Chuck and Lynsay agree the real blocker is visualization, not the underlying data model — Jeff (and buyers generally) need to see what an H3-grouped territory looks like rendered as a clean outline before they'll trust it.
- **Next step: visual mockup via Ryan's existing extension.** Lowest-friction way to get buyer reaction without building new tooling — render standardized territories as hex-grouped outlines and demo directly to Jeff, Unicus, and Clear Captions.
- **Decision doesn't block Q3/H2 planning.** If undecided by the time the team needs to swag technical design work, they'll estimate both models in parallel rather than stall.
- **Two P0 roadmap gaps identified (not yet in the tracked Q3 scope):** (1) escalating/flagging Smart Dispatch work orders for greater buyer urgency/visibility (currently no differentiation from a standard published WO); (2) letting providers self-identify preferred working areas directly, rather than relying purely on inferred history.
- **Chuck likely stepping down as B/F principal engineer for H2 — not finalized.** Driven by AI Software Factory needing his dedicated attention; still needs to be worked out with Cory Muir. No named successor yet.
- **New BD Principal Engineer role in progress.** Chuck interviewed a candidate Jul 8 intended to give the Bangladesh engineering team consistent daily technical guidance — something Chuck's limited bandwidth can't provide. Not yet confirmed/hired.
- **Break/Fix expected to wind down by Q4 2026.** Both expect team structure changes; Chuck notes MARBLE was never expected to persist into H2 as-is, and there's leadership pressure (Shane Kinder) to reallocate teams toward the "core transform" initiative.
- **Marble floated as the team to carry NetDev's innovation-to-product transition forward.** Lynsay's idea, Chuck agreed — MARBLE picking up NetDev as parts logistics winds down would both provide a landing spot for the team and serve as a proof point for how innovation-to-product transitions work generally.
- **Chuck skeptical of "core transform" initiative.** Cites ~30 pivots since it started and no clear deliverables yet, despite leadership wanting to pull four teams into it.
- **Team sentiment check:** Chuck reports no surfaced frustration from the SLATE/GARNET engineering team; Lynsay notes the team is a sprint ahead on technical design and managing scope well despite planning being difficult (moving toward a "now/next/later" model instead of strict sprint planning).

---

## Action Items

- **Lynsay Faucett:** Coordinate with Ryan on updating his visualization extension to render H3 territories for stakeholder demos.
- **Lynsay Faucett:** Work on meeting facilitation — better expectation-setting and conversation management for future stakeholder sessions.
- **Chuck Haines (implied):** Follow up with Cory Muir on H2 role/coverage plans for Break/Fix.
