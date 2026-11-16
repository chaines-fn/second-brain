# Chuck / Ridi / Bilas Check-in — 2026-07-08

**Date & Time:** Wednesday, July 8, 2026, ~7:00 AM CDT (~26 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer (Break/Fix cross-cutting architecture)
- Ridi Haque — Engineering Manager, GARNET + MARBLE (meeting organizer)
- Bilas Sarker — Engineering Lead, GARNET (email is bilas.chandra@fieldnation.com but goes by Sarker)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1U07RQxzAjoy1LrDZ3D3-u8DoQZb81ot5Y3LIraJNFso/edit)

---

## Overview

This was Chuck's regular check-in with Ridi and Bilas, and it landed squarely on an open question Chuck first raised back on July 2: the NetDev recruitment tool (SLATE) is still running under "innovation project" rules even though the team is now talking about integrating it with the rest of the platform. Chuck laid out his reasoning at length — innovation projects are supposed to be self-contained and move fast without the normal technical-design rigor, and the moment anyone starts building reporting or integrations on top of one, it stops being an innovation project and needs to become a real product with a roadmap. Bilas agreed, adding that the team isn't even confident about server stability yet and wants proper vetting before any of this touches real customer features. The three of them converged on a plan: Chuck will draft a DM outline that Ridi and Sadat can refine and send to John Shaw (group PM) via the BFEX leads Slack channel, flagging the architectural gaps and pushing for the tool to go through a proper product transition before further integration.

Chuck also walked through the specific architectural debt he already knows about — the NetDev front end is bundled into a single Go binary with the backend rather than being deployed self-contained via the newer core UI architecture, and provider data is treated as static once imported (no events, no updates), which won't hold up as a real product. He flagged mobile responsiveness as low priority for now since NetDev is internal-only and staff mostly use laptops/tablets. He was candid that this is uncharted territory — no innovation project has "graduated" to a full product before, and he expects possible friction with Ryan Schulte or others who are used to moving fast under the innovation banner, potentially requiring escalation to Cory Muir/Shane Kinder to resolve.

The rest of the meeting was a lighter operational check-in: Ridi gave updates on MARBLE (parts return experience, smart audit) and GARNET (Smart Dispatch based on Bilas's designs, availability configuration next, auto-acceptance two sprints out), plus the still-undefined "network expansion" initiative, which Chuck compared to Uber's model of dynamically expanding search radius to fill coverage gaps. Ridi confirmed the new flight board (on updated MUI) resolves the old Chrome rendering issue. On the administrative side, Chuck reminded the group he's out for two weeks starting in about a week and a half, and the group agreed Sadat (the new MARBLE PM) needs to be looped into the BFEX leads channel and related syncs now that he's ramping up on the recruitment tool work.

---

## Key Notes

- **NetDev "innovation project" status — path forward emerging:** Continues the open question from the July 2 H2 Vision meeting (`program/meetings/2026-07-02-h2-vision-strategy-overview.md`) and tracked in `teams/SLATE/projects.md`. Chuck's position: as soon as NetDev starts integrating with the rest of the platform (reporting, mobile app, etc.), it's no longer in-bounds for the innovation-project exception and needs to go through a real product roadmap and technical design review. Bilas independently agreed, citing unresolved concerns about server stability. The concrete next step is a message to John Shaw via the BFEX leads channel — Chuck is drafting the outline.

- **Specific architectural debt Chuck flagged:** (1) NetDev's front end is bundled into a single Go binary with the backend, rather than deployed self-contained via the newer core UI architecture; (2) provider data is imported as a static snapshot with no event-driven updates — it never refreshes after import. Chuck was clear this doesn't mean a full rewrite, but does mean a real architectural review (bounded context, missing events, whether the tool should replace or coexist with the legacy recruitment tool) before further integration.

- **Mobile responsiveness deprioritized for NetDev:** Since NetDev is internal-only and staff primarily use laptops/tablets, Chuck said mobile responsiveness is "low-hanging fruit" and not an immediate concern — a departure from his normal stance that it should always be in scope.

- **Possible friction/escalation risk:** Chuck expects pushback (specifically named Ryan Schulte) on treating NetDev's integration work as no longer covered by the innovation-project exception, and said this could end up needing resolution at the Cory Muir/Shane Kinder level. No innovation project at FN has gone through this transition before, so there's no precedent to point to.

- **New flight board resolves old Chrome rendering issue:** Ridi asked about a known Chrome rendering/responsiveness incident on the old flight board. Chuck confirmed the new flight board runs on updated Material UI and that issue shouldn't recur there.

- **Network expansion (GARNET) still undefined:** Bilas described the initiative as automatically expanding the service-area search radius (beyond the standard 250 miles) to fill dispatch gaps, rather than a private-network feature. Chuck likened it to Uber's dynamic radius model. No design has been finalized yet.

- **GARNET near-term sequencing:** Availability configuration is next week's priority; auto-acceptance is two sprints out. The team is still waiting on Lindy for answers on open counter-offer design questions — Ridi and Bilas will follow up again.

- **Sadat needs broader channel access:** Sadat (new MARBLE PM) is ramping up on the NetDev recruitment/provider-experience work but isn't yet in the BFEX leads channel or other relevant syncs. Chuck and Ridi agreed he should be added to BFEX leads for sure; the break-fix leads sync is engineering-manager-focused so his inclusion there is less clear-cut (MARBLE doesn't currently have its own engineering manager). Chuck also noted he probably needs to start a regular 1:1 with Sadat.

- **Chuck's upcoming absence:** Reminded the group he's out for two weeks starting in about a week and a half (already flagged in `program/overview.md` / `program/people.md` — vacation July 19–30, 2026) and asked that planning/decisions be wrapped before then.

---

## Action Items

- **Chuck Haines:** Draft a DM outline to Ridi and Sadat laying out the NetDev innovation-project concerns, for them to refine and send to John Shaw via the BFEX leads channel.
- **Ridi Haque, Sadat:** Send the message to John Shaw explaining that NetDev needs to transition from innovation project to a formal product with a roadmap, detailing integration concerns.
- **Ridi Haque:** Forward the NetDev prototype document to Chuck for evaluation (Ridi acknowledged forgetting to do this).
- **Ridi Haque, Bilas Sarker:** Follow up with Lindy again on open counter-offer and availability design questions.
- **Ridi Haque:** Add Sadat to the BFEX leads channel and relevant sync sessions.
- **Chuck Haines (implied):** Start a regular 1:1 with Sadat.
