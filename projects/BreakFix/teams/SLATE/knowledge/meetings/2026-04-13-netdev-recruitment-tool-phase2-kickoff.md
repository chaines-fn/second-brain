# Net Dev Recruitment AI Tool — 2026-04-13

**Date & Time:** Sunday, April 13, 2026, ~2:30 PM CDT (~34 minutes)

**Attendees:**
- Ryan Schulte — PM, SLATE team (meeting organizer)
- John Shaw — Group PM
- Amran Hossain — Engineering Manager (BD teams)
- Chuck Haines — Sr. Principal Engineer
- Mobarok Hossain — Engineering Lead, SLATE team
- Safin Ahmed — Engineer, GARNET team
- Almansur Siddiqui — Engineer, SLATE team (AI-focused)
- Cory Muir — Sr. Director, Engineering (invited, unclear if actively participated)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1QtllftSLVR5jQwALMygN95UZ7_d6rN9o9Mok4mZ8Yrk/edit?tab=t.5y6941ebpgjb)

---

## Overview

The team met to align on Phase 2 resourcing, capacity constraints, and a key technical decision around geolocation/territory handling for the Net Dev Recruitment AI Tool. The central tension was between Amran's need to hold a firm 30-point capacity boundary for Q2 Phase 2 (to protect other SLATE initiatives and provide clear milestones to leadership) and the rest of the group's sense that AI-assisted development could blow past that capacity faster than traditional pointing would predict.

Chuck estimated the existing HTML/JSX prototype could be converted to a working Field Nation UX in less than a day using FN React best practices, which shifted the conversation toward putting the whole team on the next sprint. Ryan committed to sending over the prototype package (HTML + JSX + docs) that day. The group agreed that one person (maybe two) should own the UI while others focus on API shaping and backend work — dividing UI work further would only slow things down.

On the technical side, the group made a significant simplification: CBSA-based territory mapping is being dropped for Phase 1 (and possibly longer). Instead, the tool will use a radius-based approach where staff pick a center-point address on a map and throw a radius around it to represent a territory. A new optional "territory" column in the CSV import will serve purely as a grouping label — no real-world zip code or CBSA definition behind it. Safin confirmed this could leverage the existing geocoding/isoline infrastructure. Ryan noted that depending on team feedback, the temporary radius approach might end up being permanent if it meets user needs.

---

## Key Notes

- **30-Point Capacity is the Hard Boundary:** Amran was emphatic that Q2 Phase 2 has exactly 30 story points of capacity (traditional pointing), and building beyond that means deferring other SLATE initiatives. This applies regardless of how many engineers work on it — more people just burns through the 30 faster. Phase 1 used 32 points for reference.

- **Whole Team on Next Sprint:** The group decided to put the full team on the Net Dev Recruitment Tool starting the next sprint (Wednesday, April 15), unless a compelling reason not to emerges. Phase 2 will technically kick off before Phase 1 is fully wrapped.

- **UI Should Stay with One Person:** Chuck and Ryan agreed that the prototype-to-production UI conversion is a 1-person (maybe 2) job. Additional engineers should be pointed at backend/API work. Dividing the UI would be slower, not faster.

- **Prototype Conversion Estimate:** Chuck estimated less than a day to get a working UX from the current prototype by applying Field Nation React best practices. It won't be fully wired up, but it'll be functional — possibly even with mock data.

- **CBSA Concept Omitted:** The group explicitly decided to drop CBSA-based territory mapping. The complexity of odd-shaped territories defined by zip code groups, center-point calculations for irregular shapes, and potential zip code islands made it too heavy for Phase 1. Safin confirmed this after discussing with Jamu.

- **Radius-Based Geolocation Instead:** Staff will manually pick a center-point address on a map for a territory and use a radius (e.g., 60 miles) to find nearby providers. This is a deliberate "informed tradeoff" — scrappier but fast. Chuck confirmed the existing geospatial/isoline infrastructure (user proximity API) could support this.

- **Territory as a Grouping Label Only:** The CSV will get an optional "territory" column that's purely a label for grouping results. No real-world definition — calling it "Chicago" doesn't mean it maps to specific zip codes. Multiple addresses sharing a territory label get combined into one provider list in the results. Mobarok confirmed this understanding.

- **CSV Scope for Phase 2:** Ryan is adjusting the prototype to only show data available from the provider CSV import (PSS, distance, point-in-time work order count, background check, drug test) rather than data that would require a new Field Nation API integration. This removes a major dependency.

- **OKR/Milestone Uncertainty:** John flagged difficulty in setting milestones beyond Phase 1 since the team might far exceed initial forecasts. Chuck suggested modeling OKRs after other innovation projects (LTE, Herman quoting tool) since this isn't traditional software delivery. John will connect with the LTE PM to compare approaches.

- **Innovation Project Framing:** Chuck reinforced that this project should be run differently than standard FN delivery — "informed tradeoffs" rather than "cutting corners." Ryan is documenting the product vision alongside explicit tradeoff decisions (e.g., what's MVP vs. real product, where security can't be compromised like buyer access/user roles) to set leadership expectations.

- **Talent Pool Expansion TD Not Finalized:** Amran mentioned the technical design for talent pool expansion isn't finalized yet, and the team is exploring what work isn't dependent on it that could be pulled into the next sprint (5-6 points).

---

## Action Items

- **Ryan Schulte:** Send over prototype package today (HTML prototype, JSX files, supporting documents) — adjusted to only show provider CSV data, not FN API-dependent fields.
- **John Shaw:** Connect with the LTE PM to understand her approach to quarterly milestone planning for innovation projects.
- **Engineering team (Safin, Mobarok, Almansur):** Prepare estimation of what can be built within 30 points by Wednesday (April 15).
- **John Shaw:** Schedule a check-in for late Wednesday or Thursday to review Phase 1 status and Phase 2 setup ahead of Friday's leadership sync.
- **Ryan Schulte & Chuck Haines (implied):** Finalize the product vision / informed tradeoffs document that Ryan started and shared on Friday.
