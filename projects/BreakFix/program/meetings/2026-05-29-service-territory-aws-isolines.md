# Service Territory Geolocation — AWS Isolines & Anchor-Point Model — 2026-05-29

**Date & Time:** Friday, May 29, 2026, 8:51 AM CDT (~10 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer (cross-cutting architecture)
- Lynsay Faucett — PM, author of the Default Service Territory Configuration brief

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/17hPEzDKARHmEQL0Ng_r1N6MHA-NswK69yCEAvgfrTig/edit?usp=sharing)

---

## Overview

A quick working session where Chuck reviewed Lynsay's Default Service Territory brief and delivered the headline: the engineering foundation for the geolocation problem is already built. When the team extracted the geospatial service for provider match, they stood up PostGIS/Postgres and an AWS Location integration that already does efficient "crow-flies" point calculations in-database (replacing the old manual MySQL approach). On top of that, AWS Location's isolines (drive-time boundaries, up to five tiers per point, built on historical traffic) and the route matrix (overlap conflict resolution) map directly onto what the brief was reaching for. So the capability gap is small — mostly configuration and modeling work, not net-new infrastructure.

The conversation then resolved two framing questions in the brief. First, zip codes are out entirely: Lynsay had carried them only as a presumed visualization requirement, and once Chuck confirmed they aren't needed even as a visual aid, the team dropped the zip dependency. Second, Chuck pressed the point that *every* candidate model — isolines, polygons, or an Uber-style H3 hex grid — still reduces to the same core primitive: a territory is one or more **anchor points plus a driving distance**. The model choice (hex vs. polygon vs. isoline) is secondary to defining those anchors. A buyer site list, in this framing, is just a set of anchor points the system snaps territories around.

The most consequential addition was a serviceability nuance Chuck surfaced from Jeff (NetDev): drive time alone is not a sufficient definition of coverage. A provider who regularly does high volumes of work in a city — even one they drive 90 minutes to reach — should count as serviceable there. Historical provider work location is as important a signal as drive time. Lynsay proposed deriving anchor points from the last two years of work-order volume, and agreed to fold the work-history dimension into the brief. Next step is for Chuck to get the AWS solutions architect on a call to confirm exactly which services cover the requirements.

---

## Key Notes

- **Infrastructure already exists (the big unlock):** The geospatial service extracted for provider match already runs PostGIS/Postgres and is integrated with AWS Location. Crow-flies distance now happens efficiently in-database; the old approach was a manual, intensive MySQL calculation. This is the foundation the territory work needs — "we've already invested in the infrastructure to solve this."

- **AWS isolines = the drive-time boundary engine:** Given an anchor point, AWS returns up to five drive-time tiers (e.g., 20/30/45/60 min) as geographic boundaries that can be stored in the DB and used for calculations. Crucially, it supports **historical traffic data**, not just live — this directly answers the brief's "Tuesday 10am" temporal-accuracy concern (it's real modeled traffic at the chosen time, not a static assumption).

- **Route matrix = overlap conflict resolution:** AWS's "calculate route matrix" is the mechanism for deciding which isoline wins when territories overlap — the conflict-resolution capability the brief called for.

- **Zip codes dropped entirely:** Lynsay had included zips only because she assumed a visual aid required them. Chuck confirmed they aren't needed even for visualization. Decision: remove all zip-code dependency; anchor-point + driving-distance replaces it.

- **Everything reduces to anchor points + distance:** Chuck's key framing — isolines, polygons, and H3 hexes all still require a central anchor point and a defined distance. You cannot define a territory (or a site list's geography) without anchors. The model is a representation choice layered on top of anchors, not a substitute for them.

- **H3 is open source but not turn-key:** Lynsay was drawn to the hex model's zoom-out/zoom-in hierarchy (US → regions → metro detail). Chuck clarified Uber's H3 library/model is open source, but Uber doesn't publish anchor points — we'd still have to define our own center points and driving distances regardless of choosing hex vs. polygon. Hex vs. polygon vs. isoline remains undecided.

- **Anchor points from 2 years of WO volume:** Lynsay's proposal — build anchors off the last two years of work-order volume, then expand from there. Demand-driven anchor placement.

- **Drive time is NOT enough — provider work history matters (Jeff's point):** Chuck raised a requirement he's heard Jeff make repeatedly and hasn't seen addressed anywhere: serviceability must also account for where a provider has *demonstrated* work history. A provider doing 100 WOs in a city they travel 90 min to should be considered serviceable there. This is a second signal alongside drive time and needs to be in the model. (Consistent with the prior NetDev insight that provider *work* location beats home location as a recruitment signal.)

---

## Action Items

- **Lynsay Faucett:** Update the project brief — remove zip-code requirement, add historical provider work volume as a serviceability consideration, resolve outstanding open questions, and revise the proposed solution recommendation.
- **Chuck Haines:** Get the AWS solutions architect on a call — frame the problem and confirm which AWS Location services best cover the requirements (isolines, route matrix, storage/serviceability modeling).
