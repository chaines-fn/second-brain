# SLA Capture Sync — 2026-05-15

**Date & Time:** Friday, May 15, 2026, 3:10 AM CDT (~50 minutes)

**Attendees:**
- **Bilas Sarker** — Engineering Lead, GARNET (primary presenter; led technical discussion)
- **Ridi Haque** — Engineering Manager, GARNET + MARBLE
- **Mike Mueller** — Principal Engineer (architectural guidance)
- **Chuck Haines** — Sr. Principal Engineer (architecture, cross-cutting)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1Z0d2ydR0_RpD_TkRLKUO7GitFhTkkPDDEubPvss8PzQ/edit)

---

## Overview

This was a technical design working session for SLA capture, focused on resolving two open questions: how to capture SLA target data at the right moment in the work order lifecycle, and what the storage model should look like. The meeting started with Bilas presenting the existing historical event log as a potential source for custom field values, and the team quickly aligned that the event log is off the table for any new reporting — it's too large and already degrading buyer reporting freshness rates.

The group converged on an event-based capture approach: a background worker listens to work order lifecycle events (assigned, checked-in, checked-out) and writes a snapshot to a new, purpose-built table — tentatively named something like `project_performance_target_captured`. This is deliberately narrower than SCD Type 2 on the custom field table, which would capture every change. The tradeoff is less historical flexibility in exchange for keeping database volume manageable. MySQL is the right store for now; S3 was considered briefly but deferred.

The meeting also touched on service lines/service offerings (confirming templates as a concept is dead, service offering is the path forward), and opened a new thread around work order priority — specifically whether priority needs to track history and whether it should be lockable after a work order is published. No final decision on priority, but the lean is that it should only be mutable while in draft.

---

## Key Notes

- **Event log prohibited for new reporting:** The historical custom field event log is too large and too slow to query. The prior May 7 principle holds: no new reporting should be built on the event log. This was the opening framing and was not contested.

- **Custom field type changes — use original value:** If a buyer changes a custom field from a date type to a number type after a work order is created, the team agreed to continue using the original value captured at work order creation. No complex type-change logic will be introduced; this is an edge case with low reporting value.

- **Event-based capture over SCD Type 2:** Rather than implementing SCD Type 2 on custom fields (which would track every value change), the team chose a lighter approach: a worker reacts to specific work order events (assigned, checked-in, checked-out) and writes the relevant SLA data at that moment. This limits data volume to only the events that matter for SLA evaluation. Mike raised the S3 / Kafka firehose idea as a future direction, but agreed MySQL is right for now.

- **New table: `project_performance_target_captured` (name TBD):** A single, project-scoped historical table. Does NOT duplicate a separate active/current view as a second table — if a view is needed, it can be a DB view. No SCD needed on this table because we're agreeing to capture at event time, not track every mutation. Table will include work order ID, provider ID, SLA target date, and an enum for SLA outcome (met / not met). Redundant timestamps (e.g., restoring check-in date here) are explicitly excluded to avoid a second source of truth.

- **Upsert strategy for multi-check-in scenarios:** Multi-day jobs and return trips can produce multiple check-in events for the same work order. The table should be an upsert keyed on `(work_order_id, provider_id)`, retaining the **first** check-in. Provider ID is part of the key because the provider can be re-assigned, and we want to track which provider was associated with each check-in for historical accuracy.

- **No capture if value is blank:** If the custom field value is empty at the moment the relevant event fires, nothing is written to the table. This avoids polluting the table with null evaluations.

- **What to store — SLA target date + enum:** The output of the SLA evaluation is the calculated target date (derived from whatever the configured source is — custom field value, business day offset, etc.) plus a boolean-ish enum: "met" or "not met." The current check-in or assignment timestamp already exists elsewhere — don't store it redundantly here.

- **No dedicated capture event:** When the worker writes to the table, it does NOT need to fire a new dedicated downstream event (e.g., "SLA captured"). The worker consumes the existing WO lifecycle events and writes silently. Future Kafka event fanout to S3 is a separate concern for later.

- **UI location and framework:** The project performance targets feature lives under the Smart Dispatch section in the UI, but is not architecturally dependent on Smart Dispatch — it applies to all work orders in the project. Implementation is in the existing UI repo using the Modern UI (MUI) framework, consistent with what the team has been doing for Smart Dispatch.

- **Service offerings vs. templates:** Templates as a concept for sub-project differentiation is gone. The team will use "service offerings" (discussed as "service lines" during the call). Needs to be verified with the cost and quality team to ensure the taxonomy aligns with their project-based reporting requirements. There was brief discussion about whether existing Field Nation "service contracts" might already serve this need — Bilas will surface this when reviewing with Lynsay.

- **Work order priority — open question:** Priority (numeric 1–6) drives SLA calculation and eventually dispatch overrides. The question is whether to allow changes post-creation. The team's lean: priority should be editable while in draft, but locked after publish, since rolling back to draft and re-publishing would pick up the new value naturally. If that's true, a simple `work_order_attribute` table entry is probably sufficient — no SCD needed. This needs Lynsay's confirmation before the TD is finalized.

---

## Action Items

- **Bilas (implied):** Present the service offering concept to Lynsay and confirm alignment on implementation (check whether Field Nation's existing service contracts concept satisfies the need before introducing something new).
- **Bilas (implied):** Confirm with the cost and quality team whether "service types" or "service offerings" will be used for their project-based reporting requirements, so taxonomy is consistent.
- **Bilas:** Update the SLA capture technical design document to specify the event-based capture strategy — listener approach, table schema, upsert logic, and null-value behavior.
- **Bilas (implied):** Consult with Lynsay regarding whether work order priority can be modified after a work order is published/created, to confirm whether a simple attribute table entry is sufficient or SCD/history is needed.
