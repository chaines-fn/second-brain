# Innovation to Product Transition Plan Planning Session — 2026-07-09

**Date & Time:** Thursday, July 9, 2026, 7:30 AM CDT (~33 minutes)

**Attendees:**
- John Shaw — Group PM
- Ridi Haque — Engineering Manager, GARNET + MARBLE
- Lynsay Faucett — PM, GARNET (organizer)
- Chuck Haines — Sr. Principal Software Engineer
- Amran Hossain — Engineering Manager, BD teams
- Sadat Haque — PM, MARBLE
- Ryan Schulte — PM, SLATE
- Bilas Chandra (Sarker) — Engineering Lead, GARNET

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1uxb-0lf9cLTfI9sR3IOUiLZWemI-4wgzYBX0X9AzycU/edit)

---

## Overview

Direct follow-through on the July 8 Chuck/Ridi/Bilas check-in, where Chuck first laid out the case that the NetDev recruitment tool can't keep running under "innovation project" rules once it starts integrating with the rest of the platform. Lynsay set up this session to get the wider group — John Shaw, Ryan Schulte, Amran Hossain, Sadat Haque, Bilas — aligned on when and how that transition happens without slowing down the team's current pace.

Chuck framed the productization threshold: an innovation project stays an innovation project as long as it's self-contained (small blast radius, no external dependencies). The line gets crossed the moment it needs platform integration — the mobile app being the clearest example, or pushing/pulling data with the rest of the platform in a way that requires ongoing maintenance and infrastructure investment. Below that line (e.g., NetDev's current spreadsheet/email-outreach replacement), the team can keep moving at innovation speed. Above it, formal technical design and engineering review kick in.

The group settled on a "strangler pattern" approach — decompose the current monolith piece by piece into proper architecture (starting with the bespoke email sender, which everyone already knew was unsustainable) while innovation work continues in parallel. Chuck was explicit this doesn't mean a rebuild, and that some rework is an accepted cost of having moved fast under the innovation banner. On the specific push/pull data questions Ryan raised (placing approved providers into talent pools/service territories, pulling territory data into the tool), Chuck's guidance: one-way pushes are fine to build now; on-demand pulls used only for UI display are fine via direct API calls; but pulling data to drive backend decisions should be event-driven, the same pattern used for dispatch-automation.

The existing "Manage Recruitment" tool will eventually be sunset once the new tool replaces it — no work is being done on the old tool, and the two will coexist until the team is confident enough to cut over. Buyers will never access the new tool directly; buyer interaction stays inside the Field Nation platform (Ryan to map out the push/pull data points for that).

---

## Key Notes

- **Productization threshold defined:** Crossing into "product" happens when the tool needs platform integration — mobile app access or pushing/pulling data with the rest of the platform that requires ongoing maintenance/infrastructure. Below that line, work can continue under the faster innovation-project process.
- **Strangler pattern adopted for monolith decomposition:** Rather than a rebuild, the team will incrementally replace bespoke components (starting with the email sender) with standard architectural implementations, while continuing to innovate on the rest in parallel.
- **Existing recruitment tool sunset plan:** "Manage Recruitment" (legacy) is not receiving further investment and will eventually be sunset; the new NetDev tool runs in parallel until it's ready to fully replace it.
- **Engineering standards split:** Platform-integrated features must follow formal TD/engineering review; internal-only innovation work can continue at its current speed.
- **Data integration standard going forward:** Future platform data integrations should be event-driven rather than direct API polling, for scalability and architectural alignment — even if it takes a little longer up front.
- **UI decoupling identified as first architectural step:** The NetDev UI is currently bundled into the same Go binary as the backend. Assessing the feasibility of separating it into core UI is a logical first move toward productization.
- **Buyer experience stays platform-native:** Buyers will never access the new tool directly — their interaction happens entirely within the Field Nation platform via data push/pull (e.g., provider counts, territory progress, deliverable approval).

---

## Action Items

- **Shafi:** Draft a current-state architecture diagram of the NetDev recruitment tool to identify scalability gaps.
- **Chuck Haines:** Review the architecture proposal once drafted and provide input to ensure it meets engineering standards.
- **Safin Ahmed:** Produce a formal technical design for the product transition and present it to broader engineering.
- **Ryan Schulte:** Develop the buyer-experience strategy — map out the necessary push/pull data points within the platform.
- **Safin Ahmed, Bilas Chandra:** Coordinate on the Marble-side provider experience roadmap and technical requirements for Q3.
