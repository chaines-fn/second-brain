# SLA Data Capture Solution Cont'd — May 7, 2026

**Meeting:** SLA Data Capture Solution Cont'd
**Date:** Thursday, May 7, 2026, 08:01 CDT (~64 min)
**Attendees:**
- **Chuck Haines** — Sr. Principal Engineer (drove the proposal)
- **Lynsay Faucett** — GARNET PM
- **Alyssa Eby** — MARBLE PM (joined for reporting/data perspective)
- **Bilas Sarker** (Bilas Chandra) — GARNET Engineering Lead
- **Ridi Haque** — Engineering Manager (GARNET + MARBLE)
- **Habib Rahman** — Engineer (UI/reporting target page — likely GARNET, role to confirm)
- **Amran Hossain** — Engineering Manager, BD teams *(invited; not heard speaking)*

**Source:** [Gemini Notes + Transcript](https://docs.google.com/document/d/1hnGiK4ERDgV7VNdBGtp_wQz5tI7lqOPmBflIQoZD54Y/edit?usp=meet_tnfm_email)

> **Continuation of:** [2026-05-06 SLA Data Capture Solution](./2026-05-06-sla-data-capture-solution.md). Yesterday Chuck pushed back on custom-field priority and the template-required constraint. This meeting is where the group landed on a phased redesign and the terminology that goes with it.

---

## Summary

Chuck walked the group through a clean restatement of the three constructs that have been conflated under "SLA capture": (1) work order **priority** (a numeric 1–6 first-class attribute, not a custom field), (2) **service offering** — formerly "workstream," formerly "template" — as an optional sub-project differentiator, and (3) **dispatch override settings** keyed off priority + service offering, which belong in dispatch-automation as a separate bounded context regardless of how the UI surfaces them. Reporting targets are keyed off `(priority, service_offering)` with both optional, and live at the project level (likely fn-core), independent of Smart Dispatch — manually-routed work orders still count toward targets. Snapshotting concern (what happens if a buyer changes a WO's priority mid-flight) was raised but parked — Alyssa to check with Linda on whether snapshotting is needed.

The group aligned on a **phased delivery plan** to unblock the 95% use case fast: Phase 1 ships a single project-level default reporting target with no priority and no service offering — null/null key, one target per project. Phase 2 layers in priority + service offering, first via templates (Blackbox is the driver). Phase 3 extends to integration payloads / work order create. Naming: priority is a plain numeric 1–6 (not "P1–P6"); "workstream" is renamed **service offering** (Linda's preferred term); "template" terminology is being walked away from for this concept. Crucially, the platform stores data as it comes in — no snapshotting, no business decisions on default fallbacks. Marrying work orders to reporting targets is a data-platform / reporting concern handled at query time, which simplifies the data model significantly (no SCD/historical tables needed in platform).

UI is explicitly out of scope for redesign in this round — design bandwidth is gone (Brent's off, no replacement). Label changes (template → workstream) are fine; major redesigns wait. Chuck doesn't believe the new approach pushes Smart Dispatch's June 23 go-live; the redesign replaces equivalent custom-field/eventing work that was already going to consume similar effort. Bilas owns updating the existing technical design doc to reflect the new model; Chuck and Mike will review informally next week (Chuck is in BD). Habib raised a UI concern — if priority is set inside templates, why does the reporting-target page still need a template selector? — Lynsay confirmed it stays for now (phase-2 concern), and Bilas redirected the rest of those UI questions to Alyssa.

---

## Topics

### 1. The Three Constructs (Restated and Aligned)

Chuck re-walked yesterday's three-construct framing and the group confirmed it:

1. **Work order priority** — buyer's urgency expression. Numeric 1–6, optional (null allowed). First-class attribute on the WO model (final location TBD — Chuck's not sold it lives on the WO itself, but that's the proposal). Validated, foreign-key-able.
2. **Service offering** (renamed from "workstream," which was renamed from "template") — optional sub-project / program-within-project differentiator. Drives the Blackbox case (multiple programs in one project). Linda has been calling this **service offering**; Chuck had been calling it workstream; everyone agreed to land on **service offering**. Chuck noted "service line" is the ITSM term for this same concept.
3. **Dispatch override settings** — independent backend construct in dispatch-automation, keyed off `(priority, service_offering)`. UI may surface it next to reporting targets, but the data model stays separate. Bilas had already proposed dropping this from the SLA/reporting-target object — Chuck confirmed this is the right move at the data layer regardless of UX choices.

### 2. Reporting Targets — Where They Live and How They Map

- **Project-level construct, not Smart Dispatch.** Lynsay re-confirmed reporting targets apply to all WOs in the project regardless of whether SD routed them. So they live at the project level (likely fn-core), not in dispatch-automation.
- **Mapping key:** `(priority, service_offering)` with both optional. Null/null is the default fallback target.
- **Hierarchy mapping (phase 2+):** WO with priority=P1 + null service offering looks first for a target with `priority=P1, service_offering=null`. If none, falls through to default. Chuck flagged this is a tiered fall-through and may need more thought with Alyssa as cases get layered in.
- **No relational marriage in platform.** This was a key clarification. The platform stores priority + service offering on the WO and stores reporting targets independently. It does **not** persist a `work_order_id ↔ reporting_target_id` relationship. The marrying is handled at the data-platform / reporting layer via query (using SCD-style history on reporting target config). Habib pressed on this; Chuck and Bilas confirmed the approach.
- **Snapshotting (parked):** What happens if a buyer changes WO priority from P1 to P2 mid-flight? Bilas's V1 thinking: snapshot at WO creation so historical reporting still works. Alyssa's gut: edge case — use the end-state value. **Alyssa to check with Linda**, since the answer affects the data model (snapshot field on WO vs. SCD-only).

### 3. Phased Delivery Plan

The unlock for the 95% use case (Unicus-style, no priority, no service offering) is much simpler than the full design. Plan:

| Phase | Scope | Driver |
|-------|-------|--------|
| **1** | Single default reporting target per project (null/null). One target per project. No priority, no service offering concept yet in code. UI restricted to single target. | Unblocks the 95% — Alyssa's data-governance push, productizing reporting metrics |
| **2** | Layer in priority (1–6) and service offering. Templates extended to set these for work orders that use templates. Reporting target table key becomes `(priority, service_offering)`. | Blackbox / Laura's case |
| **3** | Extend to integration / work order create payload — buyers can pass priority + service offering directly without using a template | Integration buyers who don't use templates |

Chuck's open question for Lynsay/Alyssa: between phases 2 and 3, which is more important to ship first? Defer to Alyssa to confirm with Linda what Blackbox specifically needs — Lynsay's read is templates-first.

### 4. Priority Field — Numeric, Not "P1–P6"

Group settled on a plain numeric 1–6, not "P1" through "P6" string values. Reasoning: Salesforce and ServiceNow both use numeric. Drop-down or numeric field reduces buyer error vs. free-text. Foreign-key-friendly.

Priority is **optional** (null allowed) — required for the 95% case where buyers don't set priority at all. That's how the default null/null target works in phase 1.

### 5. UI/UX Constraints — No Redesign This Round

Lynsay was explicit: design bandwidth is essentially zero (Brent off, Linda transitioning, no MARBLE designer). Decisions:

- **Keep the current UI as-is.** Even goofy bits like dispatch override settings being visually attached to reporting targets stay for now.
- **Label-only changes are fine** — e.g., renaming "template" to "workstream"/"service offering" in copy.
- **No major redesigns** of the reporting target page in this round. Phase 2/3 may revisit.
- **Backend gets done right anyway** — bounded contexts must be correct (dispatch override ≠ reporting target at the data layer) even if the UI doesn't yet reflect it.

### 6. Habib's UI Question (Worth Flagging for Phase 2)

Habib raised a sequencing concern: if priority is set inside templates (phase 2), why does the reporting-target page still need a template selector? Feels redundant. Lynsay's answer: still required because the workstream concept needs to be configured at the target level — multiple P1s differentiated by template. Bilas reframed Habib's question as a pure technical-relationship one (do we need to programmatically store WO ↔ project ↔ template ↔ priority links, or query at the data layer?) — group landed on "query at the data layer." Habib's UX redundancy concern was redirected to Alyssa for phase-2 design discussions.

### 7. Smart Dispatch Timeline Risk — Chuck's Position

Lynsay flagged concern that the redesign pushes the June 23 SD go-live. Chuck's position: this is **not net-new scope**. The custom-field + eventing work the team would have had to do anyway likely takes similar effort to the new model, and the new model eliminates a lot of fuzzy mapping. Bilas to estimate after TD update. **Outcome expected: timeline holds.** If it doesn't, Lynsay has MARBLE resources committed but is trying to avoid pulling them in.

### 8. Integration Payload — Service Offering Keying

For phase 3, buyers passing service offering via integration (e.g., Blackbox) will likely need the same dual-key pattern FN uses for service territories: accept either the FN ID or a buyer-provided unique name/ID. Lynsay needs to explore how Blackbox's system maps service offerings to FN's. Parts-consumption-style buyer-unique-ID pattern is a candidate.

### 9. Technical Design Document Path

- Bilas updates the existing TD doc with the new model.
- Chuck + Mike review informally next week (Chuck is in BD that week).
- **No formal Friday design review needed** — this is iteration on an in-flight TD, not a new TD.

---

## Decisions

| Decision | Details |
|----------|---------|
| Priority is a numeric 1–6 first-class attribute | Optional (null allowed). Validated. Not a custom field. Drop-down/numeric field, not "P1–P6" strings. Final location on WO model TBD by Bilas. |
| "Workstream" is officially **service offering** | Linda's preferred term. "Template" terminology is being walked away from for this concept. "Service line" noted as ITSM equivalent. |
| Reporting targets live at the project level (fn-core, likely) | Independent of Smart Dispatch. Apply to all WOs in project, including manually-routed. Keyed `(priority, service_offering)` — both optional. |
| Dispatch override settings — separate backend construct | Lives in dispatch-automation. Independent of reporting targets at the data layer regardless of UX. |
| Phased delivery | Phase 1: single default target per project (null/null), no priority/service offering in code. Phase 2: priority + service offering via templates. Phase 3: priority + service offering via integration payload. |
| Platform stores data as it comes in — no snapshotting business logic | Reporting target ↔ work order marrying is a data-platform/reporting concern, handled via SCD on reporting target config + query at data layer. No relational FK between WO and reporting target in app DB. |
| UI is **not** being redesigned in this round | Label changes only. Dispatch override staying visually attached to reporting target on UI is acceptable for now. |
| Reporting target → template association stays for phase 2 | Habib's redundancy concern noted but parked. |
| TD review path | Bilas updates existing TD; Chuck + Mike review informally next week in BD. No formal Friday design review. |

---

## Action Items

| Owner | Action |
|-------|--------|
| **Bilas** | Update existing technical design doc with new model (priority as first-class numeric attribute, service offering, separate dispatch override, phased data model). Get to Chuck + Mike for informal review next week in BD. |
| **Bilas** | Define how customers pass priority and service offering in the work order create model (phase 3 design). |
| **Bilas** | Send open questions on priority field naming/values to Lynsay via the **#garnet-pex** Slack channel (originally said "Garnett channel" — likely #garnet-pex). |
| **Alyssa** | Check with Linda on whether priority snapshotting at WO creation is required (impacts data model — SCD-only vs. WO-level snapshot field). |
| **Alyssa + Bilas** | Address UX implications of separating dispatch override from reporting targets (UI surface decision). |
| **Lynsay** | Explore how Blackbox's external system keys service offering / service line to FN's (dual-ID pattern à la service territories or parts consumption). |
| **Lynsay + Ridi** | Set up a sequencing review for phases 2 and 3 once the phase-1 TD is unblocked. Target early next week (Mon 5/11). |
| **Lynsay** | Confirm with Alyssa/Linda whether Blackbox's templates-first path is the right phase-2 priority over integration payload. |
| **Habib** | Send remaining template configuration / UI redundancy questions to Alyssa (Bilas redirected him). |
| **Bilas** | Provide an estimate (target Monday 5/11) on phase-1 unblock timeline so Lynsay can confirm SD June 23 go-live still holds. |
| **Chuck** *(implied)* | Brief Mike Mueller next week in BD on the bounded-context decisions before the informal TD review. |

---

## Open Questions

- **Snapshotting:** does priority change mid-WO require a snapshot field, or is end-state the right reporting answer? *(Alyssa → Linda)*
- **Priority storage location:** Chuck proposed work order model but isn't sold — could it live elsewhere given it's primarily a reporting/dispatch-override key? *(Bilas to weigh in as part of TD update)*
- **Integration keying for service offering:** what's the ID strategy for buyer-passed service offerings? *(Lynsay to explore)*
- **Phase 2 vs. Phase 3 sequencing:** templates-first or integration-first? Driven by what Blackbox actually needs. *(Alyssa → Linda)*
- **Reporting target fallback behavior when no match found in phase 2+:** Alyssa's instinct is "fail / no target assigned, sorry." Confirms platform stores raw data and reporting handles fallback logic — but the UX behavior on reports needs to be specified.
- **Habib's role/title** — confirm where he sits (GARNET engineer? UI specialist?). Not in current `team.md` — possibly worth adding.
