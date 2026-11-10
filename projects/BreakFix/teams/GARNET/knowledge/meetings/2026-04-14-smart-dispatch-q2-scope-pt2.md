# Review: Smart Dispatch Q2 Scope pt2 — 2026-04-14

**Date & Time:** Monday, April 14, 2026, ~1:14 CDT (~75 minutes)

**Attendees:**
- **Linda Lo** — Principal PM, Dispatch/Fulfillment (meeting organizer)
- **Brent Grill** — PM, GARNET (design walkthrough presenter)
- **Mike Mueller** — Principal Engineer (appeared as "Marrakech 839" — joined from conference room with Jonah)
- **Jonah Hacker** — Product Manager, Marketplace/Platform (joined from conference room with Mike)
- **Chuck Haines** — Sr. Principal Engineer
- **Trent Victor** — Designer/PM, Match
- **John Shaw** — Group PM
- **Lynsay Faucett** — PM, MARBLE
- **Ryan Schulte** — PM, SLATE
- **Cory Muir** — Sr. Director, Engineering (invited, unclear if actively participated)
- **Stephen Wald** — PM, Labor (invited, unclear if actively participated)
- **Spencer Moulton** — *(invited, role unknown — not found in workspace files)*
- **Bilas Sarker** — Engineering Lead, GARNET (invited, unclear if actively participated)
- **Ridi Haque** — Engineering Lead, GARNET (invited, unclear if actively participated)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1JYnq3U72SD6_vy6OgmG7dwFgTSFlY0px6he-jtQve-M/edit?tab=t.w4yyfuaozirh)

---

## Overview

This was part two of the Smart Dispatch Q2 scope review. Part one (April 8) covered the evolution of smart dispatch and provider segments; this session picked up with provider eligibility criteria and ran deep into how ranking and buyer preferences should work alongside the existing Match 2.0 algorithm. Brent Grill walked through the design prototype for eligibility criteria — distance, background checks, drug screens, PSS, experience, capacity, certifications, and tools/equipment — showing how buyers would set minimum floors and apply them selectively to different provider segments (service territory, talent pools, marketplace).

The most consequential discussion centered on whether buyer-defined priority criteria should create a separate ranking pass or work within the existing Match 2.0 algorithm. Mike Mueller pushed hard that the core ranking algorithm should remain independent and owned by the quality team — not fragmented by per-buyer smart dispatch rules. He argued that Custom Selection Rules (CSRs) have led to poor buyer outcomes because they hand too much control to buyers who don't maintain them, and that this new system should replace CSRs rather than replicate their problems. Chuck Haines raised the conceptual difficulty of a "two-pass" approach (Match 2.0 first, then buyer preferences), questioning how a second pass wouldn't just override the first. The group ultimately converged on using the default Match 2.0 ranking out of the box for the initial release and iterating from there, with Mike suggesting the priority numbering/reranking be deferred until the team better understands how buyer preferences would actually influence rank.

The group also aligned on several structural decisions: eligibility criteria are hard filters (not ranking inputs), the filtering process is sequential across segments (service territory → talent pools → marketplace via separate API calls), assignment transparency data should live in the Provider Match service regardless of assignment type, and the system must be built reusably so it's not a one-way door locked into break/fix only. The open question of how Smart Dispatch criteria should surface on the Match page when auto-assignment fails was flagged as critical follow-up work.

---

## Key Notes

- **Eligibility criteria are hard filters, not ranking inputs:** The group clarified that all criteria (background check, drug screen, PSS, experience, capacity, certs, tools) act as post-filtering on requests already received. Distance is the only criterion that gates visibility/notifications. A provider who doesn't meet the other floors can still find and request the work order through marketplace search — they just won't be considered for automatic assignment.

- **Single-tier criteria for V1:** Brent confirmed the first iteration only supports setting each criterion once with segment-level applicability (e.g., background check within 2 years for talent pools and marketplace but not service territory). Buyers cannot set different floors for different segments in V1 — no "6 months for marketplace, 12 months for talent pools."

- **Sequential segment processing with separate API calls:** Chuck and Mike aligned that smart dispatch should query provider match in sequence: service territory first, then talent pools (tier by tier if ranked), then marketplace. If a service territory provider matches, there's no reason to rank anyone else. This translates to potentially three separate API calls to the ranking service, not one batch call.

- **Fairness rotation within service territories:** When multiple service territory providers qualify, smart dispatch uses a fairness rotation based on 7-day assignment history — the provider least recently assigned gets the work. This logic lives within smart dispatch, not provider match.

- **Use default Match 2.0 ranking, don't build a parallel algorithm:** Mike was emphatic that the ranking algorithm is owned by the quality team and must remain independent of smart dispatch rules. The company tracks a KPI for assignments to "good or better" quality providers; allowing buyers to fully ignore PSS undermines that. The group agreed: if no criteria are set beyond hard filters, ranking defaults to Match 2.0. The question of buyer-influenced re-ranking was deferred.

- **Deferring priority criteria numbering:** Mike suggested removing the priority numbering from the UI for now. The team couldn't articulate clearly what "background check as priority #2" would actually mean in terms of assignment logic. Linda acknowledged internal stakeholders consider priority ranking critical, but the group agreed to start with hard filters + default ranking and iterate once they have outcome data from Q2 buyers.

- **CSR replacement is the long-term direction:** Mike and Trent both advocated for this system eventually replacing CSRs entirely. CSRs give buyers too much rope — they make poor rules, don't maintain talent pools, and end up with "fastest finger" assignments. The new system should set guardrails while still giving buyers the illusion of control. Chuck clarified there's no proposal for buyers to write their own CSRs; this UI format is the replacement. For Q2, Linda proposed a temporary "behind the scenes" CSR approach while the new re-ranking model is developed.

- **Assignment transparency must live in Provider Match:** Mike pushed that all assignment details (why a provider was chosen, counter offers, other requestors at time of assignment) should be stored in the Provider Match service — not just for smart dispatch but for manual assignments, auto dispatch, and CSR assignments too. This is a bounded context decision: Match owns assignment data regardless of how the assignment originated. The UI side drawer showing eligibility criteria should display all criteria set in the project.

- **Match page reflection is a critical open question:** When smart dispatch fails and a buyer has to manually assign, they'll go to the Match page. Mike asked whether Smart Dispatch preferences should be reflected there. Linda said at minimum, service territory providers need to be identifiable. Mike noted the talent pool filter already supports this in prod. The group agreed this needs follow-up design work — Brent and Linda will develop a proposal, though it may not ship in Q2.

- **Trent's historical context on buyer-controlled match features:** Trent shared that a similar feature (letting buyers define what constitutes a "green check" desired outcome vs. "blue check" acceptable cutoff) was designed for Match 2.0 but never built. The decision to skip it was pragmatic — porting existing CSRs to get buyers onto Match 2.0 faster was prioritized over building the new system first.

- **Build reusably — no one-way doors:** Chuck stressed that even though the immediate focus is break/fix, the team cannot build something that would need to be completely redone to work outside of break/fix. John Shaw confirmed the long-term vision is expanding all break/fix features to the broader platform. Mike agreed reusability is essential.

- **PSS filtering risk for service territory providers:** Linda flagged a concern that filtering on PSS could inadvertently exclude providers that buyers deliberately added to their service territories (e.g., Unisys doesn't care about PSS and doesn't give feedback). This needs testing — Chuck noted it could be examined through provider match to see which service territory providers would get filtered out.

---

## Action Items

- **Linda Lo:** Do a quick write-up summarizing the meeting outcomes and open threads
- **Linda Lo:** Facilitate async alignment on the direction for prioritizing criteria
- **Linda Lo:** Flesh out intentions and example scenarios for buyer priority criteria so the dev team can evaluate what's achievable within provider match constraints for Q2
- **Linda Lo:** Follow up by end of week on how break/fix features should display on the Match page — unblock the technical design team
- **Linda Lo:** Run passes/tests to analyze how Match filters service territory providers and how PSS impacts provider filtering
- **Linda Lo:** Add participants to the Slack channel with open questions
- **Linda Lo & Brent Grill:** Develop a proposal for evolving the Match page to reflect Smart Dispatch criteria
- **Brent Grill:** Work closely with Mike, Trent, and Jonah to bring Smart Dispatch design context into the Match page
- **Jonah Hacker:** Provide reporting data on CSR successes and poor outcomes (Mike wants to demonstrate CSRs lead to bad buyer decisions)
- **Chuck Haines:** Catch up on the afternoon review meeting with the NetDev team (was in AWS meeting)
- **Chuck Haines:** Flag anything helpful back to unblock progress
- **The group:** Determine which provider match capabilities can be unlocked for Q2 within existing constraints
