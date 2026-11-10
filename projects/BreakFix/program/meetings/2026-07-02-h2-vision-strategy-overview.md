# BF H2 Vision and Strategy Overview — 2026-07-02

**Date & Time:** Thursday, July 2, 2026 (clock time not stated in transcript; ~65 minutes)

**Attendees:**
- John Shaw — Group PM (organizer/leader)
- Ann-Bettina Colace — PM, SLATE
- Ryan Schulte — PM, SLATE
- Tahsin Jahin Khalid — Data Analyst (invited; no speaking part captured in transcript)
- Mobarok Hossain — Engineering Lead, SLATE
- Ridi Haque — Engineering Manager, GARNET + MARBLE
- Lynsay Faucett — PM, GARNET
- Tauhedul Shagor — UX, MARBLE
- Chuck Haines — Sr. Principal Engineer (cross-cutting)
- Sadat Haque — PM, MARBLE
- Alyssa Eby — UX, GARNET
- Bilas Sarker (invited as "Bilas Chandra") — Engineering Lead, GARNET
- Amran Hossain — Engineering Manager, all BD teams
- Melanie Payeur — GTM Lead (invited; no speaking part captured in transcript)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1XuwjUsnrOESojaekZwrZFF0l8jxSiAZicx7XaYMdBi0/edit?usp=meet_tnfm_email)

---

## Overview

John Shaw called this session ahead of official Q3 planning (kicking off the following Friday, OKRs due the 15th) to walk the full cross-team group through Break Fix mission, history, and the V2 → V2.next → V3 roadmap, and to surface any misalignment before planning locks in. The mission hasn't changed since early 2025: let buyers run break-fix programs at scale, hitting cost and quality targets with minimal overhead, in an increasingly automated experience. V2 (delivered Dec 2025) covered core service territories, smart dispatch, parts logistics, and reporting. V2.next through 2026 has been expanding those use cases — marketplace dispatch in Q1, talent pool dispatch and Smart Dispatch V3 in Q2. V3 is the end-state vision: true end-to-end automation of network health and dispatch, with AI layered in where it helps, so programs can largely run themselves.

A recurring thread was how much of the current suite is still staff-heavy — service territory creation, zip mapping, and reporting (still living in Google Sites, not yet in-platform) all require significant manual lift, and Q3/Q4 work is aimed at cutting that down. Several open structural questions came out of the discussion rather than being resolved: whether the NetDev tool should still be run as an "innovation project" now that it's starting to integrate with the core platform (Chuck raised this twice, no resolution), how reporting productization gets divided across teams once metrics clear prototype stage, and how service-territory-to-work-order association should work as fulfillment increasingly crosses territory boundaries. The Marble team's Q3 focus was also confirmed live in the meeting after Bilas Sarker flagged it was missing from the roadmap slide.

The meeting closed on planning-process logistics: the group agreed the current PEX leads sync (Slate + Garnet only) isn't a substitute for a real cross-team planning session, so Amran Hossain will schedule three meetings — a Marble-only session, a Slate/Garnet session, and an overarching cross-team dependency sync — to get Q3 plans properly sequenced before the leadership review.

---

## Key Notes

- **NetDev "innovation project" status is an open, unresolved question:** Chuck Haines raised twice that the NetDev tool is being run as an innovation project even as it starts integrating with the core platform, and that this bypasses normal product TDR/process rigor. John Shaw agreed there's no defined criteria yet for when it "graduates" to a formal roadmap item, and said it needs to be figured out — not resolved in this meeting. Amran Hossain separately asked whether NetDev resources (Safin, "Jamu") count against the SLATE/BD team headcount or run as a separate initiative; John's answer was to continue as-is through Q3 and revisit resourcing once the tool is more tied into the platform.

- **Reporting productization ownership is unresolved:** Reporting (Alyssa Eby's team, built off data-warehouse tables, still surfaced via Google Sites) is still prototype-stage and needs buyer validation before being productized. Amran proposed splitting ownership by feature team (Garnet owns dispatch metrics, Slate owns territory metrics, Marble owns parts metrics) to make resourcing easier on the analytics side. John pushed back that it's not that clean — he expects dedicated reporting engineers to support productization across teams rather than a strict per-team split. Also noted: Chandini is still evaluating whether Quicksite is the right direction before Amran's team builds multi-dashboard support on it.

- **Smart Dispatch is stable; network expansion is the big Q3 item:** Lynsay Faucett and John Shaw characterized Smart Dispatch (post-V3 release) as stable, with only feedback-driven tweaks planned — network expansion is the one substantial piece of work. Chuck flagged that service-territory model changes under discussion (e.g., a single-talent-pool-with-auto-distribution model) could still force smart dispatch changes later, so "stable" may not hold indefinitely.

- **Network expansion / runtime gates — conceptual, not yet designed:** Ridi Haque asked what data would validate the need for distance-based dispatch expansion. Lynsay clarified the goal is about *who* gets dispatched to, not provider ranking — ranking is a separate problem. Chuck sketched a possible runtime-gate model: dispatch within a tight radius for a fixed window, expand radius incrementally if unfilled, re-rank and redispatch — with time-to-assignment protected as the key guardrail. Lynsay confirmed this direction but said it's still being explored, not designed.

- **Service territory association timing — creation vs. fulfillment (Chuck's open question):** Chuck raised whether a work order's service-territory association should be locked in at creation (current zip-based approach) or updated based on where it's actually fulfilled, especially as adjacent-territory fulfillment becomes more common. This has downstream implications for reporting and auto-association logic, particularly as the team considers moving away from zip-based territory definitions (Uber-model/isoline approaches referenced). John agreed it's worth answering but deferred it to the upcoming technical design phase for standardized service territories — not resolved here.

- **Mass-edit for service territory attributes:** Amran asked about mass-editing attributes like pay rate per territory. Ryan Schulte confirmed mass-edit already exists for current attributes, but new attributes coming out of Jeff's modeling work (market type, labor market index, cost of living) will need a different, rule-based approach — currently a rough prototype, not built.

- **Marble team's Q3 focus confirmed live:** Bilas Sarker pointed out Marble's outcomes weren't reflected on the roadmap slide. John Shaw acknowledged this was a real gap — the slide had been built assuming Marble might get reassigned to LTE or another large project after parts logistics wrapped. The actual plan, aligned in the prior 1-2 weeks between Ann-Bettina Colace, Lynsay, and John: Marble finishes parts logistics (including the returns experience) through July, then shifts to provider recruitment and Break Fix work-order visibility, with Sadat Haque, Tauhedul Shagor, Ryan Schulte, Ann-Bettina Colace, and John Shaw framing the problem and solution direction.

- **Work order visibility is a marketing problem too, not just a UX one:** Discussion of improving fill rates covered likely mobile app changes and possibly push notifications (not in use today for published work). Ann-Bettina Colace added that this isn't only a visibility gap — Break Fix opportunities need to be marketed as distinct from standard 1099 work orders, tying into recruitment funnel work already underway. Alyssa Eby noted relevant wireframes/concepts already exist from a prior off-site (referred to as "the Dallas trip," last May) that haven't been revisited since — she'll share them with Tauhedul Shagor to pass to Sadat, John, and Ryan.

- **CSR dependency flagged as the biggest blocker to retiring Auto Dispatch:** On the plan to replace Auto Dispatch with Smart Dispatch by Q1 2027, Bilas Sarker asked about private-network overlap; John said there hasn't been much diligence yet on remaining gaps and expects significant buyer change management. Chuck and Lynsay both flagged Customer Service Reps (CSRs) as a major blocker — CSRs are heavily relied on for auto dispatch today and are also used as a ranking filter on the match page, so removing that dependency is non-trivial. John noted that converting a CSR-heavy buyer like Clear Captions to Smart Dispatch without CSRs would be a strong proof point for everyone else.

- **No major AI work planned for Q3:** Amran asked about AI/agentic solutions in the roadmap. John confirmed nothing beyond NetDev tooling is planned for Q3; Q4 may bring agentic AI use cases for smart dispatch. Reporting already includes some AI-generated insights today.

- **Smart Audit / VOCC backlog placement unresolved:** Ridi Haque asked about Smart Audit (adding part-return status), a Marble initiative. Lynsay confirmed it's slated as a VOCC (Voice of Customer Commitment) item tied to the returns-experience work, but the team doesn't currently have a dedicated VOCC bucket — Amran confirmed VOCC items are just folded into planned initiative work. Lynsay and Sadat to align on where it lands in the Marble backlog.

- **Planning meeting structure is being reorganized:** Alyssa Eby argued that per-team planning risks missing upstream/downstream dependencies that will "come to a head" this quarter. Lynsay clarified the existing PEX leads meeting is scoped to Slate + Garnet only (supporting Smart Dispatch) and was never meant to be a planning session — just an update forum. Chuck noted there's real ambiguity in where Slate ends and Garnet begins given the current roadmap, making it unclear where new work will land. The group agreed to split into a Marble-only planning meeting, a Slate/Garnet planning meeting, and one overarching cross-team meeting for dependencies; Amran Hossain volunteered to schedule all three.

- **NetDev (Net tooling) Q3/Q4 planning is a separate track:** Amran confirmed the NetDev initiatives (Safin/Jamu's work) sit outside this Q3 planning bucket and will need dedicated resourcing conversations with Ryan Schulte. Ryan said most of these items are already documented and roughly estimated ("half swagged").

---

## Action Items

- **The group:** Define the criteria for when NetDev tooling transitions from an "innovation project" to a formal part of the product roadmap
- **The group:** Plan resourcing for the NetDev team as its tooling integrates into the core platform
- **The group:** Define the specific Q3 Break Fix reporting requirements so engineering resourcing can be allocated across teams
- **John Shaw:** Schedule a technical design meeting for standardized service territories, including Chuck's open question on service-territory association timing (creation vs. fulfillment)
- **John Shaw:** Distribute the confirmed Marble team Q3 focus (parts logistics/returns through July, then provider recruitment + work order visibility) to the broader team
- **Lynsay Faucett + Sadat Haque:** Align on where the Smart Audit / part-return-status VOCC item sits in the Marble backlog
- **Amran Hossain:** Schedule a dedicated Marble planning meeting
- **Amran Hossain:** Schedule a dedicated Slate/Garnet planning meeting
- **Amran Hossain:** Schedule a final cross-team synchronization meeting to align dependencies
- **Alyssa Eby (implied):** Share the prior off-site provider-experience wireframes/concepts with Tauhedul Shagor for Sadat, John, and Ryan to review
