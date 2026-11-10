# Quick Connect — Service Territory Config & H3 Strategy — 2026-07-07

**Date & Time:** Tuesday, July 7, 2026 (exact clock time not stated in transcript — a late add to the calendar; ~52 minutes)

**Attendees:**
- Lynsay Faucett — PM, GARNET (moved from MARBLE, May 2026); meeting organizer
- Mobarok Hossain — Engineering Lead, SLATE
- Chuck Haines — Sr. Principal Engineer (cross-cutting)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1pnO4CO5eXNqJW4Utjqnfztx-TeX_-sEPxjwp-kgTB1A/edit)

---

## Overview

Lynsay set up this working session to align Chuck and Mobarok on two things before Thursday's stakeholder demo: a new sequencing/trade-off philosophy for pulling urgent work into sprints, and the service territory configuration feature that's now SLATE's top Q3 priority. On sequencing, the group agreed that technical design must be complete before work enters a sprint — if something urgent jumps the queue, an already-committed body of work gets explicitly traded out rather than quietly sub-ticketed, so the trade-off is visible in reporting rather than looking arbitrary.

On service territory configuration, Lynsay walked through a new check-in step that lets buyers test smart dispatch against specific service territories instead of committing a whole project at once — driven by buyer hesitation (Clear Captions, Interface) and Q3 onboarding pressure. The group worked through an edge case Chuck raised: projects that use only talent pools/marketplace expansion (no service territories) shouldn't be able to enable the new toggle, and the group aligned that this should be disabled/grayed out rather than silently breaking dispatch — with the hide-vs-disable UX call deferred to Alyssa. Downstream reporting impact (smart dispatch vs. auto-dispatch fallback) came up as a gap Lynsay hadn't fully scoped; she'll loop in Alyssa, and Mobarok's plan is to store the relevant state in the dispatch-automation service to preserve reporting continuity.

The back half of the call was Thursday-demo prep on H3 and isolines, continuing the standing default-service-territory-geolocation thread. Chuck reiterated the key distinction for Jeff's benefit: H3 is just how a service territory gets *stored*, not how it's *created* — you can still build a territory from zip codes or from isolines and convert either into H3 cells. Mobarok demoed converting Jeff's existing zip list into H3 hexes (visible gaps, e.g. parts of Florida, attributed to an incomplete source list rather than a flaw in the method). The group reaffirmed a stance already recorded in the proposal doc: no version control on standardized service territories — changes cascade to everyone — and buyer-level customization requests should be scrutinized, since most turn out to be recruitment/grouping needs (per the Unicus precedent) rather than genuine service-territory definition needs.

---

## Key Notes

- **Sequencing philosophy — trade out, don't sub-ticket:** New rule: don't pull work into a sprint until technical design is done. If something urgent must jump the line anyway, trade out an already-committed body of work rather than quietly sub-ticketing unconfirmed work — keeps the trade-off visible in reporting instead of looking like an unexplained gap. Chuck was explicit he agrees with this framing but *not* with planning technical design and implementation in the same sprint as a general practice — this is a one-off accommodation for the current urgent ST work, not a new norm.

- **Service territory config — check-in/toggle workflow:** New workflow adds a check-in step after project setup: once smart dispatch is enabled and a project has service territories configured, a buyer can toggle "limit to service territories" to test smart dispatch on a subset before rolling out fully. Default state (toggle off) = everything smart-dispatches as today; toggle on = only configured work orders route through the limited territories. Driven by buyer hesitation about full-project rollout (Clear Captions, Interface — Interface already built a clunky manual workaround).

- **Edge case — no service territories on a project:** Projects that only use talent pools/marketplace expansion (no service territories at all, a valid config since the ST requirement was dropped when talent pools/marketplace expansion launched, per Lynsay) shouldn't be able to enable the new toggle — doing so would effectively turn off smart dispatch with no clear signal why. Aligned: disable or gray out the toggle in that case. Whether to hide entirely vs. show disabled-with-explanation is explicitly punted to Alyssa (UX) — Chuck leans toward "let them click it and tell them why," but deferred to her judgment.

- **Downstream reporting gap flagged:** Chuck asked whether toggling this setting changes what shows up in reporting (e.g., assignment method reads "auto dispatch" instead of "smart dispatch" for excluded work orders) — a gap Lynsay hadn't fully scoped with Alyssa. She'll follow up. Mobarok's plan: store the relevant configuration/assignment data in the dispatch-automation service so reporting continuity is maintained regardless of toggle state; he'll double-check for edge cases.

- **H3 is a storage layer, not a creation method:** Reiterated for Jeff's benefit ahead of Thursday: H3 only defines how a service territory's boundary is *stored*. Creation can still be zip-code-based or isoline/drive-time-based — either gets converted to H3 cells behind the scenes. This directly addresses Jeff's stated concern that "H3 is just a grid with no driving-distance intelligence."

- **H3 granularity:** Cell size must be consistent nationwide (not variable like zip codes) and smaller than the smallest zip code being mapped, to avoid losing fidelity. Mobarok demoed a zip-to-H3 conversion of Jeff's list; visible coverage gaps (e.g., parts of Florida) were attributed to Jeff's source list being incomplete, not a flaw in the conversion — Lynsay: "his list is not complete and he probably doesn't know it."

- **Update/versioning stance — no version control:** Standardized service territories should not be versioned; any update (e.g., "Phoenix is now a metro area") should cascade to every buyer immediately. Lynsay: version control creates confusion where different buyers/projects are on different definitions. Chuck agreed. This mirrors the existing proposal's direction and ties to Mike Mueller's point (relayed by Chuck) that default service territories could become a platform-wide reporting mechanism eventually replacing CBSAs and zip codes — Lynsay is enthusiastic about pairing this with talent-pool mapping for better forecasting.

- **Buyer customization pushback strategy:** Team aligned that buyer-level customization of standardized service territories should be resisted — FN-level customization (that flows to the whole platform) is fine, buyer-specific customization is not. Precedent cited: when Ryan pushed Jeff on *why* he needed customization, the real answer was a recruitment-side need (merging territories for recruiting purposes), not a genuine service-territory problem — same pattern as the Unicus exception. Plan for Thursday: separate "recruitment needs" from "service territory needs" explicitly, and reframe customization pushback around downstream costs (dispatch, network expansion, reporting) rather than treating it as the easy/path-of-least-resistance option Jeff currently sees it as.

- **Near-term technical hedge (Mobarok):** To reduce risk while the model is still being finalized, Mobarok's current plan is to keep zip code as the primary database relation/buyer-facing feature signal, paired with a converted H3 cell representation for radius/geospatial calculations — i.e., maintain both in parallel rather than committing fully to H3-only yet. He flagged this needs more investigation before finalizing.

- **Alternative to buyer customization — grouping/view layer:** Lynsay floated a possible middle ground for the Thursday conversation: instead of letting buyers modify the underlying standardized territories, give them a "view" that groups existing hexes differently (e.g., a buyer-specific rollup instead of the default state-based grouping) without touching the standardized definitions themselves. Framed as an idea to raise if needed, not a locked decision.

- **Thursday demo, Jul 9 — attendees and plan:** Demo with Jeff, Mia, Jake, and Reswan. Plan: ~5 minutes up front (Mobarok) explaining H3 and isoline technology and how it differs from zip codes, then the demo, then open Q&A — likely covering how to finalize service territory construction (zip-list-based vs. isoline/anchor-based) and the customization-impact conversation.

---

## Action Items

- **Lynsay Faucett:** Revise the service territory config documentation — incorporate her answers to open questions and clarify sections flagged as ambiguous (she noted some of her prior written answers were "stream of consciousness").
- **Lynsay Faucett:** Update functional requirements to document the decision to hide/disable the config toggle when a project has no service territories.
- **Lynsay Faucett:** Consult Alyssa Eby on downstream reporting impact of the service territory toggle (assignment-method reporting for excluded work orders).
- **Lynsay Faucett:** Share/synthesize notes from the prior week's service territory conversation (with Jeff) and send to Chuck.
- **Lynsay Faucett:** Share the recording of last week's call on service territory customization with Chuck.
- **Lynsay Faucett:** Coordinate with Ryan Schulte on strategy for handling the buyer-customization pushback conversation at Thursday's meeting.
- **Lynsay Faucett:** Prepare a slide summarizing H3 and isolines for Thursday, based on what Mobarok shared in Slack.
- **Mobarok Hossain:** Open Thursday's demo with ~5 minutes explaining H3/isoline technology to build stakeholder confidence before addressing Jeff's driving-distance concern.
- **Mobarok Hossain (implied):** Continue investigating the zip-code-to-H3 bridging approach (dual-store zip + H3) — not yet finalized, needs more work.
- **Mobarok Hossain / Lynsay Faucett (implied):** Confirm with Reswan on anchor-point sourcing methodology for isolines ahead of committing to an approach.
