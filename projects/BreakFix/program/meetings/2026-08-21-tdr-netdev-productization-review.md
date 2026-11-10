# Technical Design Review — NetDev Productization TD

**Date:** 2026-08-21, ~7:00 AM CDT
**Attendees:** Safin Ahmed, Corbin Munger, Al Hasan, Mike Mueller, Chuck Haines (per notes; standing `technical-design-review` distribution)
**Source:** Gemini notes, "Technical Design Review - 2026/08/21 06:53 CDT"

## Context

Follow-through on Safin Ahmed's Aug 13 "Netdev Innovation to Product Transition: Architectural Review" TD (Confluence page 6113329156). This session reviewed the technical design for productizing/consolidating the NetDev recruitment tools.

## Decisions

- **TD split in two:** one design for tool consolidation (merging Phase 1 `netdev-ai-poc` and Phase 2 `netdev-recruitments-tool` into a new "golden go" application), one for productization — reduces document length and reviewer confusion.
- **Email service migration:** Gmail SMTP → AWS ACS (SES). Ties into the broader NetDev 2,000/day send-cap saga (see `teams/SLATE/projects.md` §5 and the Jul 24 NetDev Tooling Connect decision to scale Andrew Kandels' email-send service).
- **Database standardization:** deprecate SQLite; standardize on MySQL across all environments.
- **Drop SQS:** tied to the legacy Python-based sourcing tool being retired.
- **API usage:** call qualification and certification APIs directly instead of through redundant wrapper APIs.
- **Sync pattern:** the recruitment app fires Kafka events to synchronize provider destinations to downstream services (consistent with the Jul 9 "event-driven for backend-driving data pulls" standard).
- **Reporting:** Mike Mueller — in-application analytics are insufficient for long-term reporting; export data to the platform for use with BI tools (Tableau). Extends his org-wide TDR reporting mandate to this design.
- **Legacy data:** CSV import tables from the old tool will not be maintained in the new application structure.

## Open risk raised by Chuck

Free-text pay rates entered in the recruitment app could create discrepancies with in-platform service territory data once default service territories exist. The design needs to account for how it interacts with future in-platform default-service-territory changes. Safin's follow-up: consult Product on (a) the pay-rate conflict and (b) how in-platform service territory defaults should be reflected in this tool.

## Next steps (Safin Ahmed)

- Define/document a schema registry for the new Kafka events.
- Align all database schemas with platform data types; fix the status field to use MySQL enum instead of free text.
- Rename the S3 document-path column to reflect the presigned-URL approach actually used.
- Capture business-metrics reporting requirements with product/dev teams.
- Update the email-engagement-tracking metrics section; share with Chuck and Mike.
- Consult Product on the pay-rate/service-territory discrepancy and on in-platform ST defaults.
- Add the provider-recruitment-events schema to the documentation.
- Schedule a separate TDR session specifically for the productization design.
- Replace provider events with user events in the design.
- Refactor wrapper-API calls for qualification/certification into direct service calls.
- Publish all TDR updates to the engineering discussion channel.

## Related

- Same-morning "NetDev recruitment standup" (Ryan Schulte, Almansur Siddiqui, Safin Ahmed, Chuck, Amran Hossain, Lynsay Faucett) covered adoption/rollout planning for the same split — see `teams/SLATE/projects.md` §5.
- `program/overview.md` (Aug 21/24 Current Status entries)
- `teams/SLATE/knowledge/knowledge.md`
