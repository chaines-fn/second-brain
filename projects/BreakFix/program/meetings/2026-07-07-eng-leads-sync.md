# Break/Fix Eng Leads Sync — 2026-07-07

**Date & Time:** Tuesday, July 7, 2026, ~7:00 AM CDT (~35 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Engineer (cross-cutting architecture)
- Amran Hossain — Engineering Manager (all BD teams) — invited, addressed directly by Chuck re: NetDev planning, but not quoted speaking
- Ridi Haque — Engineering Manager (GARNET + MARBLE)
- Mobarok Hossain — Engineering Lead (SLATE, per team roster — presented GARNET's service territory work in this meeting)
- Bilas Sarker — Engineering Lead (GARNET; email bilas.chandra@fieldnation.com)
- Fayshal Uddin — Engineer (SLATE) — invited; not quoted or addressed in the transcript. *Flag: `people.md` notes he stopped attending this sync as of Jun 30, 2026, but he's still on today's invite — worth confirming if that's stale.*

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1yEZqmN7RjZRwpow6EhyBPcGSBEOPtFLlVEjf4Rbv_90/edit)

---

## Overview

The meeting covered two GARNET initiatives Bilas is driving — a lightweight cancel-smart-dispatch design review and a new auto-accept-counter-offers feature — plus a cross-team service territory configuration initiative Mobarok is leading. On counter offers, Chuck pushed back on Lindsay's (Lynsay Faucett's) proposed strategy of auto-accepting the *cheapest* eligible counter offer, arguing the team should instead re-rank eligible providers by quality and assign the highest-ranked one — consistent with Chuck's recurring position on not overriding quality-based ranking for cost. Bilas had already raised the same concern with Lindsay independently; she's now consulting with customer success. The team aligned on the Kafka implementation: extend the existing request event with optional counter-offer properties rather than introduce a new event.

Mobarok then walked through a new service territory configuration initiative aimed at letting buyers (starting with Clear Captions and Interface) adopt Smart Dispatch incrementally within a single project rather than needing a separate project per dispatch mode. Chuck's main pushback was on downstream reporting — today there's no clear signal distinguishing Smart Dispatch, Auto Dispatch, and manual assignment outcomes, and this initiative doesn't yet account for that gap. He also raised whether the current Kafka-based service territory association logic needs to move to a more synchronous, command-based model; Mobarok clarified this initiative is a separate work-order-level prerequisite check and doesn't touch that logic, so the question stays open until the shape of service territories (zip-based or not) is settled. Mobarok's related H3/zip-code hybrid proposal — keeping zip codes as the relational foundation while layering in H3 cell data for mapping and radius accuracy — drew skepticism from Chuck, who questioned why they'd add H3 complexity if the system stays zip-code-grounded, especially since imprecise driving-distance handling was the original reason for moving off zip codes.

On administrative items: Chuck gave two weeks' notice that he'll be on vacation July 19–30 and asked the team to plan around it. He also has a scheduling conflict with tomorrow's NetDev Q3 planning meeting (a principal engineer interview) and will sync with Amran afterward; Ridi asked to fold a NetDev planning discussion into his existing 1:1 with Chuck tomorrow, with "Bash" included, which Chuck agreed to.

---

## Key Notes

- **Cancel Smart Dispatch — no formal design review:** Bilas asked whether last week's TD needed a full technical design review. Chuck said no — the change is small (mostly status handling), and suggested Bilas post it in the engineering TDR discussion channel for optional async feedback instead.

- **Auto Accept Counter Offers — ranking strategy disagreement (unresolved):** Bilas's proposed config lets buyers set an auto-accept threshold (e.g., up to $45 or a percentage) for counter offers when no standard-request provider is eligible. As drafted, the logic would assign the *cheapest* eligible counter offer. Chuck pushed back: take all counter offers within the accepted threshold, re-rank those providers using existing ranking rules, and assign the highest-ranked (not cheapest) one. Bilas had independently suggested the same thing to Lindsay, who is now checking with the customer success team/stakeholder before deciding. Open question, not yet resolved.

- **Auto Accept Counter Offers — budget edge case (unresolved):** Chuck flagged a gap: if the highest-ranked eligible provider's counter offer exceeds available budget (e.g., pre-funded work orders), it's unclear whether the work order should simply fail to assign (today's behavior without Smart Dispatch) or fall back to the next provider — which adds complexity. Bilas will check current behavior for normal (non-SD) work orders and ask Lindsay for the intended outcome.

- **Auto Accept Counter Offers — Kafka design decided:** Per EA team guidance, the team will enhance the existing request Kafka event with optional counter-offer properties rather than create a new event, since counter offers and normal requests share the same request ID. Chuck agreed this avoids unnecessary complexity.

- **Chuck's vacation:** July 19–30. Flagged two weeks out specifically so Bilas and the team can plan work to stay unblocked.

- **Service Territory Configuration — initiative overview:** Mobarok presented a product-driven initiative (not yet shared with Chuck by product directly) to let buyers test Smart Dispatch incrementally within a single project by territory, rather than requiring a separate project per dispatch mode. Motivated by buyers like Clear Captions and Interface who aren't ready to fully commit to Smart Dispatch. Work orders falling inside a buyer-configured territory (within a project where Smart Dispatch is enabled) would route to Smart Dispatch; others fall back to existing platform features (Auto Dispatch, publish/route). This is a prerequisite check layered on top of existing auto-association — it doesn't change how work orders get associated with service territories in the first place. Matul is drafting the technical design; Mobarok is coordinating with Fayshal to avoid downstream Smart Dispatch impacts. *Note: this is the same initiative tracked in `teams/GARNET/projects.md` as GARNET's #1 Q3 priority — Mobarok is presenting it here even though he's rostered under SLATE.*

- **Service Territory Configuration — reporting gap (unresolved, needs follow-up):** Chuck's main concern: if this setting suppresses Smart Dispatch for a territory-associated work order, does reporting still correctly distinguish Smart Dispatch vs. Auto Dispatch vs. manual assignment? Mobarok hadn't considered this — confirmed today's reporting simply doesn't fire if Smart Dispatch never initiates. Both agreed this needs to go to Lindsay/data team: does this new configuration require a downstream reporting update. Assigned to "the group" to verify.

- **Service Territory Configuration — association mechanism unchanged for now:** Chuck asked whether the existing delayed/async Kafka-based service territory association logic should shift toward a more synchronous, command-based approach, given a related conversation last week (Matul, Mike Mueller). Mobarok clarified this initiative is strictly a work-order-level prerequisite check (is Smart Dispatch enabled for this territory in this project) and is separate from — and won't change — the existing association logic. Chuck's lean: wait to revisit the association mechanism until the final shape of service territories (whether they stay zip-code-based) is settled.

- **Integration flag dependency reaffirmed:** Chuck asked whether prior work adding a flag for integration customers reluctant to change their integration to enable Smart Dispatch still needs to be accounted for here. Mobarok and Bilas confirmed a one-time setup already exists on Field Nation's side (no buyer-side integration change needed) and that this new initiative will apply the same "is Smart Dispatch enabled" check at the same existing endpoint — no new touchpoint. Mobarok will still double-check.

- **H3 + zip code hybrid strategy (needs more discussion, per Chuck):** Mobarok's direction: keep zip codes as the foundational/relational data for service territories (buyer-configured and default), but also store H3 cell data alongside for map rendering and radius-based expansion — since zip code size varies enormously (some Alaska zips exceed 20,000–30,000 sq. mi.), making them unreliable for accurate radius dispatching. Chuck was skeptical, noting the original motivation for moving off zip codes was that they don't reflect driving distance — questioned whether staying zip-grounded undercuts the point of adding H3 complexity. Open sub-questions Chuck raised: what size should the H3 hexagons be (zip codes can be ~1 mile, H3 cells might default larger), and whether buyers need the ability to draw arbitrary custom territory shapes rather than combining whole zip codes (Mobarok hasn't heard that requirement yet, but H3 would support it if needed). Mobarok is still prototyping and will bring more detail once the technical design is ready.

- **NetDev Q3 planning conflict:** Chuck can't attend tomorrow's NetDev Q3 planning meeting (conflicts with a principal engineer technical interview) and will sync with Amran afterward instead. Ridi asked to cover NetDev/provider-side-net-solution planning context during his existing 1:1 with Chuck tomorrow, including "Bash" — Chuck agreed. This is for MARBLE's planning purposes (problem framing from their product managers on the provider side).

---

## Action Items

- **Bilas Sarker:** Check existing behavior of normal (non-SD) work orders regarding counter offers and budget constraints.
- **Bilas Sarker:** Ask Lindsay (Lynsay Faucett) for the intended outcome when budget is insufficient to accept a counter offer.
- **Bilas Sarker:** Share the technical design document for the auto-accept-counter-offers initiative with Chuck (small review, not urgent — work starts next sprint or later).
- **The group:** Verify downstream reporting impacts of the new service territory configuration (Smart Dispatch vs. Auto Dispatch vs. manual attribution).
- **Mobarok Hossain:** Share the technical design for the service territory configuration initiative once Matul's draft is ready.
- **Mobarok Hossain (implied):** Confirm the existing Smart Dispatch integration flag/endpoint logic isn't broken by the new territory check.
- **Ridi Haque + Chuck Haines:** Discuss NetDev/provider-side-net planning during their 1:1 tomorrow (Jul 8), including Bash.
- **Chuck Haines:** Sync with Amran on NetDev Q3 planning outcomes since he can't attend the session directly.
