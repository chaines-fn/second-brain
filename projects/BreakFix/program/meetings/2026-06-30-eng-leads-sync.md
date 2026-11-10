# Break/Fix Eng Leads Sync — 2026-06-30

**Date & Time:** Monday, June 30, 2026, ~12:00 PM CDT (~38 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Engineer (cross-cutting architecture)
- Amran Hossain — Engineering Manager (all BD teams)
- Ridi Haque — Engineering Manager (GARNET + MARBLE)
- Mobarok Hossain — Engineering Lead (SLATE)
- Bilas Sarker — Engineering Lead (GARNET; email bilas.chandra@fieldnation.com)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1dz_VTFSoKnMeLZe1cLoWB714-W9hMzpC4LCby9Jf0JY/edit?usp=meet_tnfm_email)

---

## Overview

The meeting covered two substantive topics: the geospatial infrastructure POC and a production integer overflow emergency.

Mobarok demoed his proof of concept using AWS Location Service and Uber H3 for rendering zip code polygons — a step forward that no longer relies on Google Maps. The demo showed a working polygon for a Minneapolis zip code (55401) rendered via PostGIS + the H3 extension. Lindsay (Lynsay Faucett) and the product team are still thinking in terms of zip codes, which is compatible with the H3 approach. Two data inputs are still needed: centroid data per zip code and anchor points across service databases; Lindsay has shared some data but Mobarok hasn't fully explored it yet. Chuck used the demo as a springboard to push the team toward an **isoline-first model** for driving distance — rather than making a per-provider API call to get exact driving distances via Google Maps, the platform should precompute isoline polygons (5/10/15-minute driving areas) and do pure PostGIS database queries. This is dramatically more efficient. Mobarok confirmed PostGIS functions support both radius and driving distance isolines natively. The team agreed this approach should be standardized platform-wide and eventually replace Google Maps everywhere — Chuck asked Mobarok and Bilas to reinforce that message with Lynsay.

On data storage architecture: after some back-and-forth, the team aligned that default service territories (pure geographic definitions with no provider associations) belong in the Geospatial Service. Buyer-specific service territory attributes and pool references live in Dynamic Talent Pools, with a foreign key reference to the geographic record in Geospatial Service rather than duplicating the geographic data. "Service Territory" is locked as the final term — confirmed by Chuck, no further changes planned.

The second topic was urgent: Bilas flagged a production error in the `row_cell` table where the primary key has hit the INT max limit (~2.1B), causing all inserts to fail. The table constantly inserts and deletes during mass imports, so the PK auto-increments indefinitely even though the row count stays low (there is a cleanup cron, but it doesn't reset the sequence). The immediate fix is converting the PK to BIGINT. Chuck's broader concern is whether other tables — particularly the fn_core work order table — are also using INT and approaching rollover. He pushed for a global sweep and a proposed update to the engineering standards DDL/SQL doc to make BIGINT the default for primary keys going forward.

---

## Key Notes

- **Meeting cancellation policy:** Amran requested (and the group agreed) that if no agenda topics are added to the calendar doc ahead of this meeting, it should be canceled. Chuck noted this meeting may become less frequent as the B/F Pex Leads meeting covers similar ground, but wants to preserve the space for technical questions.

- **Parts return discussion deferred:** Bilas noted a separate calendar event was created to discuss parts returns. Chuck hasn't had a chance to dig in yet; they'll try to cover it in 15 minutes in that dedicated session.

- **Geospatial POC status:** Mobarok has a working demo using AWS Location Service + Uber H3 for zip code polygon rendering. Product (via Lynsay) is still zip-code-based, which is compatible. Two data needs still open: centroid data per zip code, and anchor points across service databases. Lindsay said she'll provide the anchor point data; Mobarok needs to review what she shared.

- **Isoline strategy confirmed:** Chuck formally proposed shifting from exact driving distance (one API call per provider) to isoline-based queries — precomputed polygon cells at 5/10/15-minute driving boundaries, queried via PostGIS. Eliminates external API calls for matching and is dramatically more efficient. Mobarok confirmed PostGIS functions support both radius and driving distance isolines natively. Full group aligned. Chuck has already been nudging product in this direction and wants Mobarok/Bilas to reinforce the message with Lynsay — specifically that this should replace Google Maps across the entire platform, including provider match.

- **Provider location complexity (context):** Chuck noted that "provider location" will evolve beyond a simple home address — likely to include historical work areas and/or self-specified service areas. Jeff Novotny and Mike Mueller have differing takes on which signal comes first. The isoline model handles this gracefully since you can query from any address or polygon.

- **Service territory data ownership finalized:**
  - **Default service territories** (pure geographic definitions, no provider associations) → **Geospatial Service**
  - **Buyer-specific service territory attributes** → **Dynamic Talent Pools**, with a foreign key reference to the geographic record in Geospatial Service (not duplicated)
  - **"Service Territory"** is the confirmed, locked term — no more renaming.

- **Network growth / work order proximity (future):** Bilas raised that for the network growth initiative, the team will need to query closest work orders for providers and vice versa using the same isoline logic. Mobarok noted the existing geocoding in the Geospatial Service can convert an address to lat/long → H3 cell ID → radius/isoline query. Chuck confirmed this is the right direction; that functionality may need to be added or extended in the Geospatial Service.

- **Production PK overflow — `row_cell` table:** INT primary key has rolled over; max is exceeded (~2.1B), new inserts are failing in production only. Root cause: table constantly inserts/deletes during mass imports, driving PK up even as row count stays low. A cleanup cron does delete old records but does not reset the PK sequence. There may also be a separate data hygiene issue — Mobarok noted 9M rows in a related table that may not be getting cleaned up. Immediate fix: convert PK from INT to BIGINT.

- **Global PK rollover risk:** Chuck flagged that the fn_core work order table may also use INT for its PK — a potentially critical time bomb. He wants teams to: (1) fix `row_cell` now, (2) do a global sweep of all tables to find others approaching INT max, and (3) propose a BIGINT-as-default standard in the existing engineering DDL/SQL standards doc. Mobarok seconded this — they already have an engineering standards document and this is a good candidate addition.

---

## Action Items

- **Bilas Sarker:** Convert `row_cell` table primary key from INT to BIGINT to resolve the production insert failure. (urgent)
- **Bilas Sarker + Mobarok Hossain:** Conduct a global analysis across all database tables to identify other primary keys approaching INT max — find the ticking time bombs before they hit production.
- **Bilas Sarker + Mobarok Hossain:** Propose an update to the engineering standards DDL/SQL doc recommending BIGINT for primary keys, based on findings from the global analysis.
- **Mobarok Hossain:** Investigate the 9M-row table that may not be getting cleaned up by the existing cron — determine if it's a separate data hygiene issue.
- **Mobarok Hossain:** Review the anchor point data Lindsay shared; finalize the polygon data source for H3 cells.
- **Mobarok Hossain, Bilas Sarker, Ridi Haque:** Add agenda topics to the calendar doc before future meetings; cancel the meeting if no topics are added.
- **Mobarok Hossain + Bilas Sarker (implied):** When talking with Lynsay, reinforce that the isoline/geospatial approach should be adopted platform-wide and that Google Maps should be deprecated everywhere — Chuck has already planted this seed with her.
