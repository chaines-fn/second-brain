# Default Service Territories — 2026-06-17

**Date & Time:** Tuesday, June 17, 2026, 10:29 AM CDT (~43 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Engineer (meeting organizer)
- Mike Mueller — Principal Engineer
- Amran Hossain — Engineering Manager (BD teams)
- Mobarok Hossain — Engineering Lead, SLATE
- Bilas Sarker — Engineering Lead, GARNET (attends as bilas.chandra@fieldnation.com)
- Ridi Haque — Engineering Manager, GARNET + MARBLE (invited; did not speak)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1NpXsBGlvQsmsf84PFWHsvFjF4OuHFFC246JfaiexSrA/edit)

---

## Overview

Chuck convened this meeting to align Mobarok, Bilas, and Mike on the high-level architectural direction for default service territories before Mobarok and Bilas begin writing the technical design. The goal wasn't to produce a solution spec, but to reach consensus on a few key principles so the technical design work has guardrails.

The problem Chuck framed: current service territories are manually defined by buyers using zip codes, aren't working in practice, and essentially nobody uses them except buyers FN set up by hand. The plan is to shift to FN-defined default geographic territories using the Uber H3 hex model (driven by ISO/drive-time lines rather than zip codes), giving buyers a ready-made national territory layer they can link talent pools to. Jeff Novotny has already mapped the entire US into territory definitions. The initial scope is purely geographic — no pay rates, quotas, or provider curation.

The meeting converged on most of the key architectural choices: non-overlapping boundaries, H3/Postgres over MySQL, geospatial as the bounded context, and provider opt-in as the mechanism for work location before layering in historical automation. One important organizational point came from Mike: smart dispatch team ownership stays in Diamond (the matching domain) regardless of where the service territory service ends up living. The group also surfaced a meaningful naming concern — "service territories" already carries specific behavioral meaning in the platform (quotas, pay rates, provider lists) and the new concept may need a different name like "service boundaries" or "service regions."

---

## Key Notes

- **Scope strictly geographical for initial rollout:** Default service territories will only define geographic boundaries. Pay rates, quotas, curated provider lists, area managers — all deferred. Chuck noted these could be future layers on top, potentially with three tiers of customization (FN default → buyer level → project level), but nothing beyond geography is required now.

- **Non-overlapping by design:** The entire US will be covered with non-overlapping territories. This is architecturally important for reporting — you need to be able to say definitively which territory a work order belongs to. Jeff has already done this work at a conceptual level.

- **H3/Postgres architecture confirmed:** The team agreed that Uber H3 hex cells + PostGIS in Postgres is the right stack. MySQL doesn't support the H3 or PostGIS plugins needed. This also means the territory service needs its own Postgres store (or lives in an existing Postgres service like `geospatial`). Building on H3 from the start avoids a redo in 6 months.

- **Bounded context: Geospatial:** After discussion, Chuck and Mike both landed on geospatial as the right home — it already has PostGIS/Postgres, AWS Location integration, and the concept maps cleanly. Mobarok had been leaning that direction as well. This is not finalized but is the leading option heading into technical design.

- **Smart dispatch team ownership stays in Diamond:** Mike was explicit — the smart dispatch / matching logic lives in the Diamond (matching) bounded context regardless of where the territory service lives. Someone apparently floated moving ownership to MARBLE or SLATE; Mike pushed back clearly.

- **Provider opt-in before historical automation:** Mike argued (and the group aligned) that before using historical job data to automap provider work locations, the platform should first give providers an explicit opt-in mechanism to designate where they want to work. Historical automation can be layered on top of that mechanism. Product (Ryan/Lynsay) had been hesitant toward manual input and favored historical data — Amran flagged this tension. The team's position: build the opt-in mechanism first because it's cleaner and enables the automation as an extension.

- **Buyer workflow: talent pool → automap:** When a buyer sets up a project, they'll link one or more talent pools. The system automaps providers from those pools into the relevant territories based on geography. No more manual provider attribution. Smart dispatch then applies its normal filter criteria on top (PSS, performance filters, etc.) — an automapped provider who doesn't meet buyer criteria still won't get dispatched.

- **Inactive provider filter when automapping:** Both Bilas and Mobarok raised the "unmanaged talent pool" problem — buyers sometimes add thousands of providers including people who haven't logged in for years. The team agreed automapping should include an activity filter (e.g., logged in within the last year or two) to avoid polluting territory rosters with dead accounts.

- **Naming conflict is real:** Mike pressed that "service territories" already has canonical behavior attached to it (quotas, pay rates, assigned providers). Jamming a new geographic-only concept into the same name will cause API and UX confusion. He suggested "service boundaries" or "service regions." Amran noted product calls it "in-platform service territory" but the rationale is weak. Not resolved in this meeting — needs a product conversation.

- **Zip code → H3 translation layer required:** Jeff's US territory definitions were built using zip codes. There will need to be a conversion step to move those definitions into H3 cells. Not considered a major blocker, just something that needs to happen.

- **Work order ↔ territory association (short-term):** Today, work orders are auto-associated to service territories via zip code. Mobarok asked how this changes. Ryan's guidance was to stick with zip code association short-term, with a longer-term path through location standardization (normalizing WO addresses to lat/lng) then mapping to H3 cells. Chuck agreed: fix the location data first, then the hex mapping becomes straightforward.

- **Long-term vision — ISO lines everywhere:** Chuck flagged a broader goal: eventually replace drive-distance calculations across all provider matching contexts (not just service territories) with ISO line–based proximity. The idea is to pre-compute H3 cells at various drive-time bands and store them, eliminating runtime AWS Location calls entirely. He plans to start nudging the provider match team in this direction.

- **Buyer customization deferred:** No buyer customization in the initial rollout. Chuck's position: give buyers the FN-defined defaults first; if 90% of buyers are satisfied, the customization ask may not materialize. Product has floated a "revert to default" feature — Chuck wants to validate the actual need before building it.

---

## Action Items

- **Mobarok Hossain + Bilas Sarker:** Create the technical design document for the new service territory system
- **Mobarok Hossain + Bilas Sarker:** Sync daily with Lynsay Faucett to get requirements clarity and unblock technical design questions
- **Mobarok Hossain + Bilas Sarker:** Translate Jeff's existing zip code territory definitions into the Uber H3 hex cell model
- **Mobarok Hossain + Bilas Sarker:** Conduct technical spikes to validate PostGIS and H3 hex model for regional boundaries
