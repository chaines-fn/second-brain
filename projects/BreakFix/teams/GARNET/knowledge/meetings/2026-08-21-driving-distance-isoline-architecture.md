# Driving Distance — AWS Isoline Architecture 1:1

**Date:** 2026-08-21, ~3:30 PM CDT
**Attendees:** Mike Mueller, Chuck Haines
**Source:** Gemini notes, "Driving Distance - 2026/08/21 15:30 CDT"

## Context

A focused architecture 1:1 on the AWS-isoline-based driving-distance mechanism underlying the Network Expansion TD's drive-time band ladder (15/30/45/90/120 min — see `teams/GARNET/projects.md` §6). Directly relevant to the Network Expansion TDR's active review (Confluence page 6167330821) — several review comments landed the same day and the following Aug 24, including one from Mike Mueller that traces straight back to this conversation (see below).

## Mechanism walked through by Chuck

- Send a central point (work order location) to AWS to generate up to **5 isolines** (e.g., 5/15/30-min driving bands) using Mapbox plus historical traffic data — returns lat/long polygon boundaries.
- Convert provider lat/long into a Postgres **point** GIS data type.
- Use an **indexed point-in-polygon query** in Postgres to find providers falling within a calculated isoline — fast, no need to calculate drive time individually for every provider.

## Current-state gaps identified

- Today's geospatial service is a **lift-and-shift from fn-core** that bypasses isolines entirely.
- **H3 cells are used for territory shape**, not for drive-time computation — a distinct concern from the isoline/driving-distance mechanism discussed here.

## Alignment reached

- **Isoline-based provider matching:** work order creation or a location change triggers an isoline calculation; results get stored in a database; provider matching queries against pre-converted provider point data — avoids per-provider drive-time API calls.

## Open item — not adopted

- Mike Mueller proposed emitting a **Kafka event every time the Geospatial service calculates isolines** (so other services, e.g. Provider Match, can subscribe for schedule-conflict detection or ranking purposes, and so cached results avoid redundant AWS isoline API calls for the same position). **Chuck confirmed this Kafka-eventing pattern is not currently planned** in the design. Mike re-raised the same idea as an inline comment on the Network Expansion TDR later the same evening (Aug 21, ~10:37 PM UTC) — see `teams/GARNET/knowledge/knowledge.md` Open Questions.

## Next steps (Chuck Haines)

- Check whether Elasticsearch has a plugin capable of isoline-style geospatial calculations comparable to the Postgres extension being used.
- Review a geospatial design doc recently shared by "HL" (name as transcribed by Gemini — unconfirmed; likely garbled).

## Related

- `teams/GARNET/projects.md` §6 (Network Expansion)
- `teams/GARNET/knowledge/knowledge.md` Open Questions (Aug 21 & 24 Network Expansion TDR review batch)
- `program/overview.md` (Aug 21/24 Current Status entries)
