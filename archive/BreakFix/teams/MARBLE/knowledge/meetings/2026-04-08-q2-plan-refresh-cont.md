# Marble: Q2 Plan Refresh (cont.)

**Date:** April 8, 2026
**Attendees:** Lynsay Faucett (PM), Alyssa Eby (PM), Ridi Haque (Engineering Lead)
**Invited but absent:** Chuck Haines, Bilas Sarker (no-show)
**Transcript:** [Google Doc](https://docs.google.com/document/d/1zeNs8A6u7KgXM6RLXs38jl72ogTj2YZPPRew4TLDbZM/edit?usp=meet_tnfm_calendar)

## Summary

Continuation of Q2 planning for MARBLE. The team locked in Q2 priorities — **Provider-Added Parts** and **Part Return Experience** are the two commitments. Contract enablement phase 2 was explicitly deferred to Q3 to protect parts logistics focus. Smart Audit stays small (under 10 points, no enhancements). Several follow-ups assigned to Ridi around estimation, flightboard migration risk, and ServiceNow integration ownership.

## Key Notes

- **Q2 commitments confirmed:** Provider-Added Parts (trunk stock) and Part Return Experience. Provider-added parts is non-negotiable scope; return experience has more flexibility to descope the UI lift if needed.
- **Contract enablement deferred to Q3:** The team decided that contract enablement phase 2 (making contract available in package overview + cleaning up experiments) supports the entire B/F contract, not just parts, and shouldn't eat into Q2 parts logistics time.
- **Smart Audit kept minimal:** Target under 10 story points, no enhancements — not considered a major adoption driver.
- **Part Return Experience design is NOT complete.** Alyssa corrected the spreadsheet — design work is still in progress.
- **Delivered status tooltip bug:** No tooltip appears on "delivered" status, which is confusing. Agreed it needs to be fixed so all statuses consistently show tooltips on hover.
- **Parts delay messaging finalized:** Generic message for when a part delay impacts a WO: _"A parts delay has impacted the work order schedule. Please coordinate as soon as possible to determine next steps."_
- **Parts requirements configurability deferred:** Alyssa advised against making parts requirements configurable (at template/project level) unless they simultaneously add the ability to add parts at that level. Tagged as a future VOC item.
- **Editing constraints:** Parts on a work order can be edited up until the assigned status, consistent with other task-related configuration.
- **Integration requirements confirmed:** Parts need to be available in the integration without changing internal process. Significant UI update needed for new parts requirements data, especially mobile.
- **Flightboard migration risk flagged:** Alyssa raised concern about changes being overwritten during new flightboard rollout (happened before). Ridi to follow up on beta flightboard build plan and migration process.
- **ServiceNow integration → Integrations team:** Lynsay asked Ridi to push the ServiceNow integration work back to the Integrations engineers rather than MARBLE owning it.
- **Parts integration staffing:** Lynsay suggested pulling in a specific person with parts knowledge. Ridi will ask the integration manager to assign them for one sprint.

## Action Items

| Owner | Action |
|-------|--------|
| Ridi Haque | Review spreadsheet — verify high-level notes capture all details |
| Ridi Haque | Add estimation line item for Part Return Experience (possible cut item) |
| Ridi Haque | Ask integration manager to assign parts-knowledgeable engineer for one sprint |
| Ridi Haque | Follow up on flightboard beta build plan + migration process for recent changes |
| Ridi Haque | Push ServiceNow integration work back to Integrations team |
| Ridi Haque | Share revised estimations with group for alignment |
