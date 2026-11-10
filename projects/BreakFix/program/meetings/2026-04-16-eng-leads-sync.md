# Break/Fix Eng Leads Sync — 2026-04-16

**Date & Time:** Wednesday, April 16, 2026, ~12:00 PM CDT (~37 minutes)

**Attendees:**
- **Chuck Haines** — Sr. Principal Engineer (meeting lead)
- **Ridi Haque** — Engineering Lead, GARNET
- **Mobarok Hossain** — Engineering Lead, SLATE
- **Bilas Sarker** — Engineering Lead, GARNET (appears as "Bilas Sarker" in transcript — middle name)
- *Amran Hossain (invited, not present)*
- *Fayshal Uddin (invited, not present)*

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1pScvPEMKISLljuinUXIVVlpQsT7_Xhg2FBZzQc952lg/edit?usp=meet_tnfm_email)

---

## Overview

The eng leads sync covered three substantive topics: Kafka connector instability, a proposed new custom field type for datetime-with-timezone, and the strategy for delivering smart dispatch technical designs incrementally.

The Kafka connector has been a persistent pain point across all Break/Fix microservices — not just FN Core — with incidents requiring manual pod restarts to resolve. The core issue isn't purely technical; it's an ownership vacuum. Mike Mueller previously pulled in Jasime (a principal engineer on Diamond/platform), but Jasime was unsure whether the platform team or individual feature teams should own the long-term fix. Chuck agreed to take bullet points from Mobarok and Bilas to the principal engineering group to get clarity on ownership and resolution responsibility. The team emphasized that smart dispatch reliability is directly impacted — roughly 7–8 out of every 10 dispatch issues trace back to Kafka message sync failures between Dynamic Talent Pool and Dispatch Automation.

On custom fields, Bilas raised the challenge of adding timezone precision to datetime custom fields without breaking existing integrations and downstream consumers (flight board, activity reports, filtering, data warehouse). Chuck proposed creating an entirely new custom field type — something like "datetime Z" — that requires ISO datetime format with timezone offset, stored as UTC in the database. This avoids forcing changes on existing customers while giving integration customers an opt-in path to true datetime precision. The team aligned on this direction but flagged that product approval (Linda) is needed before proceeding, and a sync with Chris Broveleit and the Integrations team should follow.

Chuck also reinforced the incremental delivery philosophy for smart dispatch V3 technical design. Linda confirmed she won't release smart dispatch until the layered ranking algorithm is in place, but Chuck's position is that engineering should deliver incrementally regardless — filtering first (using default Match ranking), with ranking as a separate follow-on technical design. This prevents analysis paralysis and keeps the team unblocked. Bilas's technical design doc is ready for Chuck's review, and Chuck strongly recommended running it through Claude before submitting to the broader engineering group.

---

## Key Notes

- **Kafka connector ownership is the real blocker, not just the bugs:** The connector has been unstable for two quarters. The SLATE/GARNET teams and Diamond Engineering have collaborated on multiple fixes (adding pod logging, resolving auth issues), and the situation has improved — but it's not cured. Restarts still happen. The root cause that would eliminate manual restarts remains unknown. The critical next step is getting the principal engineering group to assign clear ownership so there's a team accountable for long-term resolution rather than ad hoc collaboration on each incident.

- **Smart dispatch is disproportionately affected by Kafka issues:** Mobarok estimated that 7–8 out of 10 smart dispatch incidents are traceable to Kafka message sync failures between DTP and Dispatch Automation. This makes Kafka stability a de facto dependency for the B/F program's core value prop.

- **New "datetime Z" custom field type proposed:** Rather than modifying the existing datetime custom field (which would have cascading impact on flight board, activity reports, filtering, integrations, and data warehouse), the team aligned on introducing a new field type that requires ISO datetime format with timezone offset. Customers pass time with TZ info, the system stores in UTC. Existing customers don't have to change anything; customers needing SLA capture precision or true datetime accuracy can opt in to the new type and migrate fields as needed. Salesforce already sends ISO datetime; ServiceNow does not — integration team discussion needed.

- **Incremental smart dispatch V3 delivery strategy confirmed:** Chuck directed the team to split the technical design into two phases: (1) filtering with default Match 2.0 ranking, and (2) the layered ranking algorithm as a follow-on design. Linda won't release until ranking is in place, and that's fine — engineering delivers incrementally so it's never the bottleneck. This aligns with the Apr 14 scope review decision to defer priority ordering.

- **Claude as a technical design review tool:** Chuck shared that he uses Claude to review technical designs before they go to the broader engineering group, feeding it the engineering standards repo, public API design philosophy, bounded context, and schema registry. He noted the geospatial technical design was fully Claude-authored. He encouraged Bilas to use Claude Projects or Cowork to build up persistent local context (V1/V2/V3 smart dispatch, provider match codebase, etc.) rather than trying to load everything in-the-moment, which causes confusion and hallucination.

- **Bangladesh visit confirmed:** Chuck, Mike Mueller, Cory Muir, and Travis Emslander are all visiting Bangladesh next month for field day. The team in BD expressed strong excitement — this will be the most senior engineering leadership to visit the BD office together.

---

## Action Items

- **Mobarok & Bilas:** Write up 2–3 bullet points summarizing the core Kafka connector problems (specific failure modes, what's been tried, why restarts are the current fix). Provide to Chuck so he can bring it to the principal engineering group for ownership resolution.
- **Bilas:** Draft a bullet-list proposal for the new datetime-with-timezone custom field type and post it in #temp-bf-ops-pex-leads. Tag Linda Lo with product questions (ramifications of introducing a new field type, impact on existing customers).
- **Chuck:** Schedule a meeting early next week (targeting Tuesday) with Chris Broveleit, Integrations, Bilas, Ridi, and Linda Lo to discuss the datetime custom field plan.
- **Bilas:** Share the smart dispatch technical design document with Chuck today or tomorrow morning for review and feedback before it goes to the broader engineering group.
- **Bilas:** Submit a ConductorOne request to extend Claude access to indefinite (current access was set to one-month trial).
- **Mobarok:** Ping Cory Muir later today if Claude access hasn't been granted yet.
