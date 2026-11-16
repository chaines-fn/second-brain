# MARBLE Knowledge Base

## Customer Requirements by Company

| Customer | Key Requirement | Complexity |
|----------|----------------|-----------|
| **Unisys** | PUDOs at part level, multiple WO constraints, ServiceNow integration | Highest — custom solution built |
| **RIMG** | Strict "before provider leaves site" data gathering | High — timing enforcement |
| **DiverseNet** | Tracking numbers must be tied to payment approval | Medium — workflow dependency |
| **Black Box** | RMA + exit survey for Bank of America repairs | Medium — return workflow + custom fields |
| **Spectrotel** | ~90% return rate | Medium — return volume/scale |
| **ETS** | Google ships parts before provider; strict return penalties | Medium — pre-ship + return compliance |
| **CapsaHealthcare** | Bench stock model — parts not WO-specific at creation | Medium — provider-added parts |
| **Otodata** | Trunk stock model | Medium — provider-added parts |

## Key Decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| June 2025 | MVP launch with 4 pilot buyers | Get real feedback before building ETE experience |
| 2025 | Parts suite assumes parts known at WO creation | Original design; now known to fail for trunk stock / pre-ship scenarios |
| Mar 2026 | Custom Unisys solution built | Unisys requirements too specific; custom path unblocks their launch |
| Apr 8, 2026 | Q2 locked: Provider-Added Parts + Return Experience | These are the two commitments; provider-added parts is non-negotiable scope, return experience has UI scope flexibility |
| Apr 8, 2026 | Contract enablement phase 2 deferred to Q3 | Supports entire B/F contract, not just parts — shouldn't eat into Q2 parts logistics time |
| Apr 8, 2026 | Smart Audit kept under 10 points, no enhancements | Not a major adoption driver; keep minimal |
| Apr 8, 2026 | Parts requirements configurability deferred | Don't make configurable unless you can also add parts at template/project level; future VOC item |
| Apr 8, 2026 | Parts editable until assigned status | Consistent with other task-related configuration |
| Apr 8, 2026 | Parts delay message finalized | "A parts delay has impacted the work order schedule. Please coordinate as soon as possible to determine next steps." |
| Jul 2, 2026 | MARBLE's Q3 focus confirmed: parts logistics/returns through July, then provider recruitment + BF work-order visibility | Roadmap slide had assumed Marble might get reassigned to LTE or another large project after parts logistics wrapped — this was a real gap. Actual plan aligned by Ann-Bettina Colace, Lynsay Faucett, and John Shaw in the prior 1-2 weeks; confirmed live at the H2 Vision & Strategy Overview after Bilas Sarker flagged it missing. See `teams/MARBLE/projects.md` §7 and `program/meetings/2026-07-02-h2-vision-strategy-overview.md`. |

## Security

**Critical JWT auth bypass found and fixed (Jun 18 – Jul 1, 2026):** Ridi flagged two high-priority vulnerabilities into the MARBLE triage queue on June 18 — a JWT signing key exposed via a verbose error response on QA1 (work order endpoint), enabling full authentication bypass, and a related path where a production raw ELB (bypassing the Ambassador gateway) exposed the same vulnerable JWT code path. Both closed out by Monjurul Hoque (MARBLE-625 on June 30, MARBLE-626 on July 1). Worth a spot-check that no other services front an ELB directly without going through Ambassador, since that was the exposure vector for the second ticket.

## Known Gaps & Gotchas

**Custom fields workaround:** 55% of barcode scans are being captured via custom fields instead of standard shipment fields. This means data is unstructured and hard to report on — a signal that the standard UX isn't working for these workflows.

**Integration-readiness gap:** Most buyers are not API-integration-ready. The ETE experience needs to serve both integration-ready (like RIMG) and manual-heavy buyers. Bulk import/export is a key need for the latter group.

**Parts MVP scope:** MVP assumed buyer-provided parts at WO creation. Trunk stock, bench stock, and pre-shipped scenarios all require the "provider-added parts" capability that wasn't in MVP scope.

**Unisys complexity:** Unisys has PUDO (pick-up/drop-off) requirements at the part level, not just the WO level. Their ServiceNow integration adds further complexity. The custom solution built for them should be reviewed when thinking about what "standard" parts capabilities need to support.

**Flightboard migration risk:** A previous rollout caused past shipment changes to be overwritten. Any new flightboard rollout needs to account for this — Ridi following up on beta build plan and migration process (as of Apr 8, 2026).

**ServiceNow integration ownership:** As of Apr 8, 2026, MARBLE is pushing ServiceNow integration work back to the Integrations team. Ridi to follow up.

**Delivered status tooltip bug:** No tooltip on "delivered" status — agreed to fix for consistency (Apr 8, 2026).

## Open Questions

- When does the parts solution graduate from pilot to GA? What's the criteria?
- How do we standardize the barcode/shipment capture experience to reduce custom field workarounds?
- What does "integration-ready" actually mean as a platform concept — can we make it easier for non-technical buyers?
- Will Integrations team actually pick up the ServiceNow work? (flagged Apr 8, 2026)
- **Smart Audit / part-return-status VOCC placement unresolved (Jul 2, 2026):** Ridi Haque asked where Smart Audit (adding part-return status) sits — Lynsay confirmed it's a VOCC (Voice of Customer Commitment) item tied to the returns-experience work, but there's no dedicated VOCC bucket; VOCC items are folded into planned initiative work. Lynsay and Sadat Haque to align on backlog placement. See `program/meetings/2026-07-02-h2-vision-strategy-overview.md`. **Update Jul 6, 2026:** Confirmed for Q3 completion at the H2 Initiative Planning & Prioritization session (small, customer-driven scope; Amran to inform Sed) — backlog/VOCC bucket placement itself still not resolved. MARBLE otherwise had no substantive presence in that session; still needs its own dedicated Q3 planning meeting (outstanding since Jul 2). See `program/meetings/2026-07-06-h2-initiative-planning-prioritization.md`.
