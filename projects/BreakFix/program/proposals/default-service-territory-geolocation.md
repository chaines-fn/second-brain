# PROPOSAL (not adopted) — Default Service Territory Geolocation

> **Status: OUTSTANDING PROPOSAL — under exploration, not finalized.**
> This captures working-session thinking and a proposed direction. It is **not domain knowledge** and should not be treated as a decision until the team aligns and the AWS solutions-architect review is done. Do not promote into `knowledge.md` / `architecture.md` as settled until then.

**Origin:** Working session, Chuck Haines + Lynsay Faucett, May 29, 2026 → `program/meetings/2026-05-29-service-territory-aws-isolines.md`
**Relates to:** Default Service Territory Configuration brief (Lynsay) · full detail in `artifacts/default-service-territory-tech-considerations.md`
**Owner:** Chuck Haines · **Last updated:** May 29, 2026

---

## Problem (why we're exploring this)

Building service territories today is manual and offline (spreadsheets, Maptitude), depends on fragile zip codes and buyer site lists, and yields overlapping, noisy coverage that's hard to report on. Goal: a platform-native, nationwide default that a buyer can snap a site list onto with no Field Nation hand-holding.

## The proposal in brief

- **A territory is a named set of H3 hex cells, not a shape.** Same logical concept as today's set of zip codes — just a better membership unit. Isolines/work-history select the cells; the resulting set *is* the territory. Sets can be non-contiguous (a metro plus an outlying area a provider services).
- **Built on infra we already have.** The geospatial service (PostGIS/Postgres + Amazon Location) stood up for provider match is the foundation; we extend it.
- **Engine = AWS Location.** `CalculateIsolines` (drive-time tiers, historical traffic) generates the reach; `CalculateRouteMatrix` resolves overlaps. HERE-grade data already in our AWS footprint. Generated offline/batch → runtime is a cell lookup, so cost is bounded.
- **Two-signal serviceability.** Coverage = drive-time reach **or** demonstrated provider work history (Jeff's point). Anchors seeded from ~2 yrs of WO volume.
- **Storage/representation.** PostGIS polygons for the boundary/display; H3 cell sets for membership, supply/demand aggregation, and the editor — both in the same Postgres. Can stage PostGIS-first, add H3 with the work-history layer.
- **Cells.** We don't define a grid — H3 defines a fixed global grid; we pick a resolution (proposed start: res 6, ~2-mile cells; consider mixed res for rural) and derive cell IDs via `polygon_to_cells` and `lat_lng_to_cell`.
- **Visualization.** Extend the in-flight SD map view with toggleable layers (territory cells, supply/demand, drive-time reach, anchor, boundary); vector tiles (`ST_AsMVT`) + zoom LOD to scale to ~1,000 territories; H3 backs the visual editor.

Figures: `artifacts/service-territory-concept.png` (concept), `artifacts/service-territory-map-detailed.png` (map view).

## What it would unlock (maps to brief goals)

Self-service territory creation with zero manual mapping · site lists snap in automatically · no "No Territory Found" white space (unblocks smart dispatch) · clean non-overlapping reporting · faster, more accurate program launches.

Territory hierarchy (~50 parent regions → ~300 metro + ~500–700 fill territories) for rollup reporting and mass attribute management (e.g., update rural pay rate across all fill territories at once).

Account-level territory library with cascade-with-override: account-level masters push down to linked projects; project-level tweaks stay local. **Scoped as future state / "Could have"** — separate from Q3 FN defaults work.

## Aligned in the session

- Drop zip-code dependency in favor of anchor + drive-distance.
- Anchors from ~2 yrs of WO volume.
- Serviceability must include provider work history, not drive time alone.

> **Related (Jun 1, 2026) — talent pool ↔ territory mapping:** A follow-on session (Chuck/Ryan/Lynsay/John) reinforced this proposal's **two-signal serviceability** (drive-time reach **or** provider work history) and agreed to bring work-history into both dispatch *and* coverage logic, plus a non-static distance filter (AWS isolines, 10/20/60/100 mi). It also aligned on a **complementary** direction: talent pool as the single source of truth, with service territories as project-level *containers* of linked pools and a territory roster that's a dynamic view. **This proposal defines the geographic boundary; that one defines provider membership** — default territories remain purely geographic templates. See `program/meetings/2026-06-01-talent-pool-service-territory-mapping.md` and the "Talent Pool as Source of Truth" section in `program/architecture.md`.

> **Update (Jul 7, 2026 — Quick Connect, Mobarok/Lynsay/Chuck):** Near-term hedge floated by Mobarok — keep zip code as the primary DB relation/buyer-facing feature signal in parallel with a converted H3 cell representation for radius/geospatial calculations, rather than committing fully to H3-only yet; still needs more investigation before finalizing. Mobarok also demoed converting Jeff's existing zip list into H3 hexes ahead of the Jul 9 stakeholder demo — visible coverage gaps (e.g., parts of Florida) were attributed to Jeff's source list being incomplete, not a flaw in the conversion method. See `teams/SLATE/knowledge/meetings/2026-07-07-service-territory-config-h3-strategy.md`.

## Still open (must resolve before this is "real")

- AWS solutions-architect review — confirm services + details (**next step, Chuck**).
- Result storage / caching rights for AWS-derived data (HERE-backed).
- Route-matrix limits/batching at ~1,000 anchors.
- H3 resolution: res 6 vs. mixed — calibrate against WO density.
- Work-history overlay data pipeline (anchor placement + per-provider work-history cells).
- Coverage / white-space metric redefinition (now two-signal).
- Anchor definition & QA — product/data call.
- Hex vs. polygon for the buyer-facing boundary (trade-off; Chuck's call).
- **How does the standard ST model handle Jeff's three coverage types?** Bound Area (predefined zones by zip/county/CBSA/radius — no defaults needed), Unbound Area (large areas with WO density islands + white space), and Physical Locations (site list with exact addresses). Needs an explicit answer per coverage model.

## Next steps

1. Chuck — get AWS solutions architect on a call.
2. Lynsay — update the brief (zip removal, work-history signal, revised recommendation).
3. Revisit and, once aligned + validated, promote the settled parts into `architecture.md` / team `knowledge.md`.
