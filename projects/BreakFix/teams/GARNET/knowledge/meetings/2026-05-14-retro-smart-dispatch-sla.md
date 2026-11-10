# Garnet Retro: Smart Dispatch & SLA Capture — 2026-05-14

**Date & Time:** Wednesday, May 14, 2026, 9:00 AM CDT (~59 minutes)

**Attendees:**
- **Ridi Haque** — Engineering Manager, GARNET + MARBLE *(meeting organizer)*
- **Amran Hossain** — Engineering Manager (all BD teams)
- **Bilas Sarker** — Engineering Lead, GARNET
- **Habib Rahman** — Engineer, GARNET
- **Ahtasham Billah Himel** — Engineer, GARNET
- **Azizul Hakim** — Engineer, GARNET
- **Emdadul Islam** — Engineer, GARNET
- **Lynsay Faucett** — PM, GARNET
- **Alyssa Eby** — UX, GARNET
- **Brent Grill** — former PM, GARNET *(attending as project historian)*
- **Chuck Haines** — Sr. Principal Engineer

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1inwCoP_rcaU7bg7GkpI34icGkJ9Ybis9xEnaYkh54ls/edit?tab=t.g35gl4x0cl0n)

---

## Overview

The GARNET team held a retrospective focused specifically on the SLA Capture initiative — spanning its original scope through Smart Dispatch V2 and V3. The retro used a structured format with AI-assisted thematic grouping, which surfaced four main themes: strengthening testing and stability, requiring technical design reviews, clarifying requirements early, and formalizing delivery strategy. The group voted to spend most of their discussion time on technical design, which received the most votes.

The core of the discussion unpacked how the SLA Capture project grew from a "quick win" — a small effort to capture metadata for a timely fill rate metric — into something significantly more complex, without the process guardrails to match the expanding scope. Bilas Sarker laid out the technical consequences: the initial bounded context treated SLA capture as simple metadata storage, so when V2 introduced custom fields and priorities, the architecture didn't hold and architects (including Chuck and Mike Mueller) raised concerns that required substantial rework. The team also surfaced that cross-team visibility was missing — feedback from Mike Mueller and the Buyer Analytics team surfaced late, creating potential blockers that could have been resolved earlier.

The retro closed with two clear consensus action items: (1) mandate proper Technical Design Reviews for all future initiatives regardless of perceived size, and (2) improve how requirements are defined — specifically, making sure the team distinguishes *user needs* from *proposed solutions*. Chuck called out a recurring pattern in this project where solutions ("use custom fields," "use templates") were written as requirements, which locked in direction prematurely and created friction when architects pushed back. The team agreed this needs to change. Lynsay closed by recognizing Habib Rahman for his resilience during a particularly rough stretch of the project.

---

## Key Notes

- **Scope of the retro:** Amran clarified early that the retro should focus specifically on the SLA Capture initiative (not a general team health retro), including Smart Dispatch V2 and V3. The request came from John Shaw.

- **How it started — the "quick win" trap:** Brent provided historical context: SLA Capture was originally scoped to support the timely fill rate reporting metric in a single sprint. As stakeholder conversations expanded the scope, the team kept building without stepping back to declare it a proper initiative with a PRD or solution direction. This left context fragmented across timely fill rate documentation and Smart Dispatch docs, with no central home.

- **The bounded context problem:** Bilas explained that V1 was treated as "just store this form data" — no solution exploration, no detailed end-user goal analysis. When V2 arrived requiring custom fields, priorities, and use cases *outside* Smart Dispatch, the original bounded context no longer held, which is when architectural concerns and delays started stacking up. The team was essentially asked to scale a foundation that wasn't designed to scale.

- **TDRs aren't enough alone:** Bilas argued — and the team generally agreed — that a Technical Design Review (TDR) can't compensate for missing solution exploration and requirements work upstream. The TDR answers "how do we architect this," but it can't answer "what does the customer actually need" or "what are all the use cases." That has to happen in solution exploration documents and UX documents *before* the TDR.

- **Product/UX black box problem:** Alyssa described product and UX feeling like they're outside the TDR process — designs pass early review, then an architect flags something as wrong at fit and finish with no visibility into why it wasn't caught earlier. She suggested PM and UX either help author the technical doc or attend TDR meetings to answer business questions in real time. This framing resonated; the group agreed that TDRs should be more cross-functional forums.

- **Cross-team alignment arriving too late:** The team noted that Mike Mueller and the Buyer Analytics team raised concerns about overlapping scope relatively late in the project. Lynsay flagged this as a structural gap — cross-team dependencies need to surface earlier, ideally through architect-to-architect alignment in the early design phase. (Note: Alyssa clarified she doesn't believe Buyer Analytics is actually stepping on the three reporting targets for Break/Fix — that reconciliation was handled by Linda and Stephen — but said she needs to see Mike's specific feedback to assess impact.)

- **Requirements vs. solutions — Chuck's call-out:** Chuck identified what he sees as the root process failure: requirements in this project were written as solutions. "Use custom fields" is a solution, not a requirement. "Use templates" is a solution, not a requirement. Locking in solutions at the requirements stage removes the team's ability to evaluate alternatives and creates immediate friction when architects disagree. Chuck and Lynsay spent significant time untangling the actual user needs — and after doing so, landed on something simpler, more scalable, and architecturally sound.

- **Habib's resilience acknowledged:** Lynsay gave a specific shout-out to Habib Rahman for handling a rough few weeks on this project and being part of the team that arrived at a more defensible solution in about a week and a half.
