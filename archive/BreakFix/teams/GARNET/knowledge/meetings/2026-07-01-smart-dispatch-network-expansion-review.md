# Product Review | Smart Dispatch Network Expansion — 2026-07-01

**Date & Time:** Wednesday, July 1, 2026, 12:30 PM CDT (~36 minutes)

**Attendees:**
- Lynsay Faucett — PM, GARNET (meeting organizer)
- Shane Kinder — EVP of Product
- John Shaw — Group PM
- Chuck Haines — Sr. Principal Engineer
- Ryan Schulte — PM, SLATE
- Ann-Bettina Colace — PM, SLATE (invited; no audible participation in transcript)
- Cory Muir — Sr. Director, Engineering (invited; no audible participation in transcript)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1-UG_JIxusm-E8pQh4CGdgOBYpVFV9ikQTZU4o1RRkrU/edit)

---

## Overview

This product review realigned the Smart Dispatch network expansion effort around two hard questions Shane Kinder pushed at the outset: who are the actual buyers driving this investment, and why now. That reframing shifted the conversation away from solution design and toward problem validation — Lynsay walked through recent Smart Dispatch progress (talent pool expansion launch, hard filters) and buyer feedback from RMG Zones and Unicus that's shaping the roadmap: territory-specific launch for A/B testing against auto dispatch, cancel-in-progress, advanced availability criteria, and auto-accept counter offers within a threshold.

The core technical debate centered on the "proximity problem" — rigid service territory shapes forcing buyers into tiered custom selection rules to approximate what should be dynamic, distance-based dispatch. Chuck raised a recurring tension between prioritizing proximity and preserving provider quality (PSS), which the group resolved conceptually by separating dispatch (who gets notified first) from ranking (who gets assigned) — proximity determines notification order, quality ranking still governs assignment. Chuck also flagged that the current "broadcast to entire service territory" behavior was an intentional technical simplification, and that adding granular visibility control would be a much larger lift than it sounds.

Shane pushed hard on making time-to-fill a primary constraint rather than a secondary metric, arguing that without it the team will keep building "anxiety-mitigating" features instead of trustworthy automation — John Shaw agreed, framing the whole effort as being in service of faster fill. The group also aligned on looping in data science (Mia) sooner rather than later to move toward algorithm-driven dispatch instead of an ever-growing set of manual buyer-configurable dials, and on syncing with the Provider Match team (once Ashley returns Aug 3) since the new database-driven distance approach could benefit both efforts. Sequencing: early Q3 ships buyer-requested enhancements, standardized service territories land later in Q3, and full network expansion work is tentatively Q4.

---

## Key Notes

- **Reframing from solution to problem validation:** Shane opened by pushing back on jumping to solution framing — his priority was identifying which buyers need this and why now, separating out the "where is the tech located" data problem as its own separate issue not to be solved here.

- **Buyer feedback driving the roadmap:** RMG Zones and Unicus are in active talks to adopt Smart Dispatch. Feedback surfaced four asks: launch Smart Dispatch to specific territories to A/B test vs. auto dispatch; cancel Smart Dispatch mid-run to preserve manual override trust; advanced availability criteria to prevent double-booking; and auto-accept counter offers within a buyer-set threshold (RMG). None of these are available today with auto dispatch.

- **The proximity problem, not a business-logic customization problem:** Lynsay explained that what looked like a need for full custom ranking logic (from Q2 work) actually decomposed into a narrower proximity problem — buyers build multiple tiered custom selection rules purely to approximate "dispatch closer providers first," not because they need genuinely custom business logic.

- **Proximity vs. provider quality tension (Chuck):** Chuck raised the risk that deprioritizing PSS/quality in favor of proximity could mean assigning a lower-scored but closer provider over a higher-scored, farther one — undermining Provider Match's mission. Resolution: proximity governs *notification/dispatch order* (who hears about the work first), not final assignment ranking — quality ranking still decides who gets assigned once responses come in.

- **Dispatch runtime implications (Chuck):** If providers are notified in expanding radius tiers rather than all at once, Chuck flagged open questions about whether ranking windows need to pause and wait for the localized tier to respond before expanding — with real implications for total dispatch runtime. Lynsay clarified current behavior: Smart Dispatch already dispatches to everyone in the territory simultaneously and only delays for marketplace expansion; standardized service territories would enable real supply-density-based dispatch decisions instead.

- **Broadcast-to-all was an intentional technical simplification (Chuck):** Publishing work to every provider in a service territory (rather than controlling visibility per provider) was a deliberate technical shortcut — techs who won't rank get filtered out anyway. Chuck flagged that adding granular visibility control beyond distance would be "a much larger technical lift" that was intentionally deferred.

- **Time-to-fill as a core metric, not secondary (Shane):** Shane argued strongly that without treating near-instant fill as a primary constraint for break-fix work, the team will be pushed into building a growing set of "anxiety-mitigating" features for buyers instead of solving the underlying trust/speed problem. John Shaw agreed, framing all of this work as in service of faster time-to-fill, citing Clear Captions' current 9–13 hour fill time as the kind of problem this should solve. Lynsay acknowledged she had this as a secondary metric in the problem-framing doc and agreed to elevate it.

- **Ranking window as a fill-speed lever:** Lynsay noted the current 60% ranking-window delay contributes to buyer frustration and is part of why buyers want the ability to cancel Smart Dispatch mid-run. Optimizing the number/length of ranking windows is a related lever for improving time-to-fill.

- **Cross-team alignment with Provider Match (Chuck):** Chuck proposed aligning this dispatch/proximity work with the Provider Match team, both for platform consistency and because the new approach would be 100% database-driven (vs. today's crow-flies-plus-Google-Maps query for Provider Match). Shane agreed this is a good call and noted Ashley (back Aug 3) will be focused on Provider Match more holistically at that point.

- **Advanced availability criteria — two distinct buyer needs:** Unicus wants a daily assignment cap per provider to prevent over-assignment and backouts (coordinating with Reza and Jonah); Clear Captions wants double-booking prevention (don't assign a provider already booked at that time). Legally, caps can't be set at the individual provider level (misclassification risk) — a tier-level cap is being considered instead.

- **Same-site multiple work orders is a separate, unaddressed problem (Shane):** Shane flagged that a provider handling multiple work orders at the *same* site should be treated differently than multiple *different* sites — current availability criteria work doesn't yet account for this nuance. Lynsay confirmed it's a distinct problem not yet in scope; John noted Chris is leading deeper discovery with the Unicus team on their batching vs. one-at-a-time submission patterns.

- **Push to bring in data science sooner (Shane):** Shane argued that continuing to add manual configuration dials for every new buyer scenario will outpace buyers' ability to use them effectively, and pushed to involve Mia (data science) to define the range of dispatch scenarios and move toward algorithm-driven solutions grounded in operations-research approaches. Ryan noted the team is already moving toward a simplified "good/better/best" UI to abstract away manual parameter-setting.

- **Untapped data sources for dispatch and coverage planning:** Shane and Ryan discussed using currently-uncaptured data — provider app login history, likely working windows, and real-time/periodic location sampling — to improve both immediate dispatch decisions and longer-term coverage planning (predicting availability before work is even posted).

- **Talent pool ↔ territory association still unresolved (Ryan):** Whether providers get associated with service territories via talent-pool "push" or territory "pull" logic is still being worked through and is adjacent to, but not yet resolved within, this network expansion effort.

---

## Action Items

- **Lynsay Faucett:** Update the problem-framing document to elevate time-to-fill as a primary metric for work order assignment (currently listed as secondary).
- **Lynsay Faucett:** Loop in Mia (data science) to discuss the range of dispatch scenarios and explore data-driven algorithmic approaches.
- **Chuck Haines (implied):** Set up time with the Provider Match team to align proximity/dispatch logic once Ashley is back (Aug 3) — Chuck raised this as a "not a today problem" but wants it on record.
