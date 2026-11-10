# Field Week: "Hold FW Network Health Automation"

**Date:** Aug 12, 2026, 10:03 AM CDT
**Attendees:** Ryan Schulte, John Shaw, Lynsay Faucett, Chuck Haines, Alyssa Eby, Mobarok Hossain

One of the Field Week working sessions on deferred B/F themes (parallel to the Aug 11 "NextGen Dispatch Automation" session — see `program/meetings/2026-08-11-fw-nextgen-dispatch-automation.md` if created, otherwise `program/overview.md`). Covers recruitment automation, coverage scoring, and service territory geography — reviewed via Gemini notes, not yet fully reconciled against other B/F threads.

## Recruitment / Fairness Metrics

- Target set: **1.5 workers per provider in a service territory**.
- Fairness logic thresholds: **5–10 work orders per week**.
- Low-volume territories (**under 5 calls/week**) excluded from fairness logic.

## Data Platform Integration

- **Coverage score** prioritized as the initial data product for platform integration.
- Historical demand data integration **pending data governance approval** — action item for Mia Mu to submit the coverage score to governance ahead of schedule.
- Requirement finalized to link project demand data directly into the platform.

## Recruitment Trigger Logic

- Recruitment triggers defined by coverage scores + fill rates.
- Logic proposed to manage in-progress providers to prevent over-recruitment.
- New requirement: alert when target demand diverges from actual volume.

## Recruitment Workflow

- Workflow stages: **In-Queue, In-Progress, Approved, Opt-Out**.
- Automated approval flow initiated for the rate-agreement step.
- Automated reminder emails for mid-progress providers.
- Visibility gap identified in current recruitment notifications — manual phone calls are the current engagement driver; text-based notifications discussed as a fix.

## Milestones & Scope

- Q4 milestones adjusted due to engineering resource constraints.
- Q3 prioritization: **back-end reporting over network expansion** (consistent with capacity constraints already tracked elsewhere).
- Provider mapping validated based on work completed in the **last 90 days**.

## Service Territory / Geospatial

- **Zip code polygons identified as the primary method for defining service territory boundaries — over H3 cells.** New data point in the still-unresolved H3-vs-zip tension (see `teams/SLATE/knowledge/knowledge.md` Open Questions) — leans toward zip, consistent with the Jul 31 "zip-code lookup processes" framing rather than the Jul 28 H3-column direction. Not stated as a final, formal decision.
- Site import commitment capped at **50 points**, excluding broader UI requirements.
- Dispatch expansion aims for configurable distance tiers for metro vs. rural service areas.
- Groundwork connecting service territories to geospace needs **1 sprint** before design start.
- Storing historical work order data in the geospatial service **needs re-evaluation** — flagged as high resource cost.
- **M2 milestone** focuses on standardized service territory platform groundwork.
- Strategic direction reaffirmed: **talent pools drive dispatch; service territory associations are retained for reporting** — operationalizes the Jun 1 "talent pool as source of truth" direction (previously aligned but not built). **Migration mandatory** for accounts with existing providers in service territories upon smart dispatch activation. Existing provider-to-territory mapping today relies on manual, error-prone queries; proposed automation would trigger provider association with service territories when a talent pool is added to dispatch settings.

## Next Steps

- Pull workload-breakout data (variation of WOs per provider across service territories) to identify imbalances.
- Load historical demand data into the platform to inform initial recruitment target volumes.
- Mia Mu to request an expedited data-governance review of the coverage score.
- Acquire an engineering resource to build the data products / learn the coverage-score logic.
- Establish a formal process for managing in-pipeline providers (pause/continue recruitment logic).

## Not Applied (out of B/F scope)

- A Google Ads address-integration / InfoSec re-approval item and a GCP scripting/phishing-flag issue were mentioned in the same notes but are unrelated tooling, not Break/Fix scope.
