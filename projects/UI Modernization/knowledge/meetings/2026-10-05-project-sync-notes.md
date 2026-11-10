# UI Modernization Project Sync — 2026-10-05

**Automated sync run from scheduled task**

---

## Status Overview

### Critical Incident (RESOLVED)
- **Date/Time:** 2026-10-05, 07:59–08:36 CDT (P0)
- **Issue:** UI deployment caused `ui-prod` pods to fail with `CrashLoopBackOff`
- **Impact:** Work-order functionality unavailable; buyers unable to log in; buyer-facing outage
- **Mitigation:** Team rolled back to release 1520
- **Resolution:** Incident resolved at 08:36 CDT
- **Action Items:**
  - Post-mortem scheduled (Mokhlesur Mahin to coordinate)
  - E2E test coverage gap identified — tests did not catch the issue
  - Recommended: Add E2E test for the failure pattern before next release

### Flightboard Rollout Status
- **Phase 1 Complete (2026-10-02):** 40% + 60% of buyers on new Flightboard
- **Next Phase:** High-Risk buyers and ~15 opt-out buyers → Oct 8 (with CS outreach)
- **Build Enhancements:** PSS, Pending Assignment, Messaging all shipped on schedule
- **Historical Data:** ~12 months backfill validated for WWT PNC projects

---

## Active Work

### Immediate Focus (PR Review & Merge)
- **PR #580** (brand refresh logo updates, core-ui) — Final review by Tasnim Ferdous, approved Oct 5, **merge scheduled Oct 7**
- **PR #571** (Project Manage page migration, Ivan Sakhman) — new route `/projects/:id/new/manage-project`, AES mapping added, awaiting CR
- **PR #586, #585, #578** — Multiple PRs in flight from Ivan, Yurii, Miroslav for Messages/architectural work

### Infrastructure & Architectural
- **Brand refresh:** Logo consolidation (Andriy/Miroslav) — CDN path fixes shipped, email logo adjustments in herman#785 pending
- **Project Manage migration:** New route wired, needs VITE_AWS_LOCATION_MAP_URL env var and Harness flag `CORE_UI_MANAGE_PROJECT` (default OFF for company 10)
- **Layered architecture (OBS-493):** Continued implementation via PRs #511/#503 (routes→features→components→foundation)
- **Messages enhancements (OBS-496/497):** API field additions (assigned-message-count), UI drawer scope refinement

### Team Capacity
- **Yurii Dmytriiev:** On sick leave Oct 4–5 (approved)
- **Ivan Sakhman & Andriy Kurenda:** Active on Project Manage and brand refresh work
- **Miroslav Isikiyski:** Backend work on Messages API and brand refresh CDN issues
- **General velocity:** Steady PR review and merge cadence despite Oct 5 incident rollback

---

## Blockers / Open Questions

1. **Post-mortem from P0 incident:** Root cause analysis in progress (Mokhlesur)
2. **E2E test gap:** Pattern that caused Oct 5 crash not covered in test suite
3. **Project Manage env vars:** VITE_AWS_LOCATION_MAP_URL needs documentation on whether it's the same key across QA/stage/prod
4. **Harness flag setup:** CORE_UI_MANAGE_PROJECT needs to be wired; testing should enable it for company 10

---

## Data Quality Notes

**Limitations of this sync:**
- Google Calendar/Gmail connections unavailable — could not retrieve "UI Modernization Sync" or "WWT Phase 1 Daily Sync" meeting transcripts as requested
- Meeting notes likely in Google Docs but inaccessible
- Slack search was primary data source for this run

**Sources used:**
- Slack #proj-ui-modernization channel history (recent 50 messages)
- Slack search for "UI Modernization Sync" and "WWT Phase 1 Daily Sync" (no results)
- Incident channel #incident_8008 for P0 details

---

## Next Sync Priorities

1. Confirm post-mortem completion and action items from P0 incident
2. Track PR #580 merge completion (target Oct 7)
3. Verify Project Manage route deployment and Harness flag setup
4. Follow up on Yurii's return from sick leave and team capacity
5. Check E2E test coverage improvements for deployment stability
