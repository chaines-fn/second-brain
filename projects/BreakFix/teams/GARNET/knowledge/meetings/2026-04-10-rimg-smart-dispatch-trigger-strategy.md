# Aligning Smart Dispatch Trigger Strategy for RIMG (Product) — 2026-04-10

**Date & Time:** Thursday, April 10, 2026, 11:30 AM CDT (~14 minutes)

**Attendees:**
- John Shaw — Group PM (meeting co-lead)
- Linda Lo — Principal PM, Dispatch/Fulfillment Lead (meeting co-lead)
- Bilas Sarker — Engineering Lead, GARNET
- Ridi Haque — Engineering Lead, GARNET
- Chuck Haines — Sr. Principal Engineer
- Brent Grill — PM, GARNET
- Ryan Schulte — PM, SLATE

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1wC-rC1l-qI4e6zc0Wt93eFFUrg6JIpbgbBejhpDfCo8/edit)

---

## Overview

The team aligned on a temporary approach to enable RIMG to use smart dispatch without requiring them to update their API integration. RIMG is deep in integration work for a large Fujitsu project (~1.5M) and can't prioritize adopting the separate smart dispatch API right now, but the team wants them on smart dispatch as soon as possible to support the V2 suite rollout.

The proposed solution works at the project level: if a project's smart dispatch toggle is enabled *and* a new experiment flag is turned on for the buyer, any work order created through the Client API for that project will automatically trigger smart dispatch instead of the existing auto dispatch. This avoids any code changes on RIMG's side — it's two configuration changes (one staff-facing, one engineering-facing). The tradeoff is ~300–500ms additional latency on work order creation, but only for RIMG's integration-created work orders, which the group agreed is acceptable for a temporary solution.

The solution is scoped to 3–4 months. After that, RIMG is expected to migrate to the separate smart dispatch API. Linda noted this really only solves for 1–2 buyers with significant integrated auto-dispatch volume today, so a more robust long-term solution isn't warranted unless adoption grows. Bilas estimated the work wraps up by Tuesday (April 14), with testing and release Wednesday, Dunder Mifflin production validation, and availability by Thursday (April 17).

---

## Key Notes

- **Connector vs. Client API paths:** Connector-based integrations already support smart dispatch by setting the smart dispatch flag to `true` in the connector config — no code changes needed. The discussion focused on the Client API (Field Nation REST V2) path, which RIMG uses, where the buyer currently passes auto dispatch flags but not smart dispatch data.

- **Experiment flag as guardrail:** A new experiment/feature flag will control which buyers can auto-trigger smart dispatch via the Client API. This prevents other buyers from accidentally triggering it. The flag check is what adds the ~500ms latency — it's a single additional call per work order creation.

- **Smart dispatch overrides auto dispatch:** When both the project smart dispatch toggle and the experiment are enabled, work orders created via Client API will *always* trigger smart dispatch, never auto dispatch. This is the intended behavior for RIMG's use case.

- **Error visibility limitation:** For this temporary solution, if smart dispatch errors occur, the buyer must come into fieldnation.com to see them. There's no comprehensive history log planned — that's a long-term consideration only.

- **Limited buyer footprint:** Linda confirmed only 2 buyers have significant volume through integrated auto dispatch (a few hundred to ~1,000/month per John's earlier data pull). The temporary solution is really just for RIMG; a robust project-level auto-trigger would only be warranted if many more buyers wanted it.

- **Testing approach:** Bilas plans to demo in Dunder Mifflin with a setup mimicking RIMG's environment (multiple projects with auto dispatching) before enrolling in production. John emphasized high confidence is required given RIMG runs total automation.

- **Zip code upload confidence:** Ridi confirmed that zip codes were successfully uploaded via CSV with correct settings for Dunder Mifflin, working with the SLATE and GARNET teams. John can proceed with re-uploading zip codes for DoorDash (and Clear Captions?).

---

## Action Items

- **Bilas Sarker:** Build the temporary smart dispatch auto-trigger solution (target: wrap by Tuesday April 14). Test in lower environments Wednesday, release, then validate in production via Dunder Mifflin. Demo for Dunder Mifflin before production enrollment. Target availability: Thursday April 17.
- **John Shaw:** Re-upload zip codes and connector settings for DoorDash (and Clear Captions?).
- **Ridi Haque (implied):** Available to support John on zip code uploads if needed.
