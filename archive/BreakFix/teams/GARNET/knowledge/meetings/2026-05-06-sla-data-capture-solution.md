# SLA Data Capture Solution — May 6, 2026

**Meeting:** SLA Data Capture Solution
**Date:** May 6, 2026, 12:35 CDT (~59 min)
**Attendees:** Lynsay Faucett (MARBLE PM at time of meeting, in transition to GARNET PM, organizer), Chuck Haines (Sr. Principal Engineer), Ridi Haque (GARNET Engineering Manager — joined late, was commuting)
**Source:** [Gemini Notes + Transcript](https://docs.google.com/document/d/1XQbKcH9TZcx2nEHnNKNMhVMVR4cALa_FmtufOSl7W0E/edit?usp=meet_tnfm_email)

> **Org context (May 2026):** Linda Lo and Brent Grill are moving off B/F entirely. Lynsay is moving from MARBLE to GARNET PM (replacing Linda). Ridi Haque is now Engineering Manager for both GARNET and MARBLE. New PM Sadat (and a new UX TBD) are taking over MARBLE. This meeting is one of Lynsay's first GARNET-context working sessions.

> **Chuck's post-meeting position (added May 6):** Custom fields was the wrong approach from the start. Templates are likely also wrong as the differentiator. Further discussion pending — don't lock in a redesign yet beyond what's captured in this note.

---

## Summary

Chuck pushed back hard on the current SLA capture implementation approach. The crux: priority is being represented as a buyer-defined custom field, which gives FN no validation control (buyer can pass any value) and forces fuzzy mapping logic between custom field values and reporting targets. Chuck's proposal — make priority a first-class work order attribute/field with a validated numeric range (1–6, aligning with how Salesforce and ServiceNow already model this). That eliminates the custom field entirely and lets us cleanly map (priority + optional template) → reporting target, with a null-template target as the fallback when no template is passed. Lynsay walked through the visual UX showing how two reporting targets can both be "P2" if differentiated by template (the Blackbox use case where one project houses multiple programs because service territories are too painful to set up for each). Chuck flagged that the real underlying issue is service territories — using templates to differentiate clients/sub-projects within a project is a band-aid — but agreed to solve one problem at a time.

The conversation also untangled three distinct concerns that have been conflated: (1) work order priority itself, (2) smart dispatch override settings keyed off priority, and (3) reporting targets at the project level. Reporting targets must apply to *all* work orders in a project regardless of whether they went through Smart Dispatch or were manually assigned — Smart Dispatch becomes a filter, not a gate. This raised an unresolved bounded-context question Chuck wants Bilas to mull: where should priority and reporting targets actually live? Probably not the work order; possibly dispatch-automation; Ridi suggested a generic reporting solution decoupled from break fix and Smart Dispatch entirely, similar to the conversation happening around service territories.

Net effect: SLA capture development is on hold. The team can't roll out the current implementation (template is required, custom-field-driven). GARNET will pivot to service territories work for the rest of this week while Chuck, Lynsay, and Bilas work through the redesign. Chuck believes the unblock is fast and the resulting solution will be technically sound and still meet business needs — it just won't be the fastest path. Lynsay accepts the schedule slip and will manage Linda/Alyssa expectations.

---

## Topics

### 1. Untangling the Three Concerns

Chuck framed the work as three distinct constructs that have been getting blurred:

1. **Work order priority** — buyer's estimate of how fast a WO needs to be fulfilled (P1–P4 today, proposing 1–6 to align with Salesforce/ServiceNow). Lives on the work order.
2. **Smart Dispatch overrides** — when SD is enabled for a project, priority drives override of dispatch settings (timing, etc.). Lives in dispatch settings.
3. **Reporting targets** — timing-based metrics (assigned-to-completed) tracked at the project level. Apply to all WOs in the project, not just SD-routed ones.

**Key clarification from Lynsay:** Reporting must include all work orders within a project regardless of routing — manual assignments included. Smart Dispatch is treated as a filter on the report, not a precondition for inclusion.

Chuck flagged "SLA" is probably the wrong term for what's being built. Needs renaming. Lynsay noted Salesforce and ServiceNow both use numeric priority levels.

### 2. Why a Custom Field? Lynsay's Take

Lynsay didn't have full historical context but offered her read:

- Custom fields and templates already exist on the work order today, so adoption is easier than adding a new configuration section.
- Unicus is the canonical "no template / no custom field" buyer — they want to set targets at the project level and have them apply universally.
- Templates were introduced specifically to handle Blackbox: they want one Break/Fix project housing multiple programs (e.g., Dell + Lenovo) and need to differentiate targets within that project. The custom field can be the same value (P1) but the template differentiates which target applies.

Chuck's reaction: "which is laughable — why aren't we fixing service territories then trying to slap another band-aid on top of it?" Lynsay agreed in principle but pushed for solving one problem at a time.

### 3. The Custom Field Problem (Chuck's Pushback)

Two core issues with the custom field approach:

- **No validation control** — buyers can put anything in a custom field. Today FN sets the values, but that's not scalable as more buyers self-serve.
- **Fuzzy mapping** — the custom field value has to map to a reporting target via heuristics rather than being a known, validated input. Smart Dispatch then has to do that same fuzzy lookup.

Lynsay also clarified that the current implementation requires a template — there's no path for buyers who don't use templates. That's the immediate blocker.

### 4. Chuck's Proposal: First-Class Priority Field

Drop the custom field entirely. Make priority a first-class work order attribute or field:

- Validated numeric range (e.g., 1–6).
- Field name TBD — definitely not "SLA." Look at what Salesforce/ServiceNow call it.
- Mapping logic at project level: lookup is `(priority, template)` → reporting target. If no template is passed, match to the reporting target with `template = null`. Uniqueness constraint becomes `(priority, template)` per project.
- This decouples Smart Dispatch overrides cleanly — SD just reads the validated priority field; no fuzzy mapping needed.
- Doesn't change buyer behavior fundamentally — they still pass a priority and optionally a template — but eliminates custom field friction and gives FN validation control.

Lynsay: "Okay, so that solves our immediate problem." Confirmed direction subject to validation with Linda.

### 5. Where Should This Live? (Bounded Context — Unresolved)

Chuck put on his architect hat and surfaced an open question: where do priority and reporting targets actually live in our domain model?

- **Smart Dispatch overrides** — clearly belong in dispatch-automation, keyed by priority.
- **Priority on work order creation** — accepted as part of the WO create payload, but the WO may not be the right bounded context for storage (since priority is really a project-reporting and dispatch-override concept).
- **Reporting targets at project level** — currently lives in the SD project settings UI, but Lynsay confirmed reporting applies even without SD enabled. So calling it "Smart Dispatch reporting" is wrong.

**Ridi's suggestion (echoing Mike Mueller):** Build a generic reporting solution decoupled from Break/Fix and Smart Dispatch. Could later be customized for various project types — not tied to SD or B/F specifically. Lynsay drew the parallel to the conversation happening with service territories (similar abstraction concern).

Chuck wants Bilas to think on this and propose: attribute vs. work order field, and where reporting targets/priority should be stored.

### 6. Templates — Still Useful, But Maybe Not the Right Differentiator

The Blackbox case (multiple programs in one project) needs *something* to differentiate two reporting targets that share a priority level. Today that something is "template," but Chuck challenged whether template is the right abstraction:

- Could be a sub-project ID, program name, or other unique identifier.
- Lynsay's pushback: it can't be `project` because the whole point is multiple programs under one project.
- Chuck clarified he means a sub-project / program-within-project concept, not the top-level project.

Open for tomorrow's follow-up: do we actually need to bind to templates, or is there a cleaner unique identifier?

### 7. Lynsay's UX Vision (End State)

On the work order page, similar to Smart Audit:

- Buyer can see the applied reporting target at the WO level.
- Edit affordance to change the priority (e.g., P1 → P2) — this updates which target applies.
- Target details (assign by / arrive by / complete by) displayed inline so it's clear what the target means.
- Reporting itself (historical compliance metrics) lives at the project level, not per-WO.

Chuck noted parts of this implementation he hasn't worked out yet — needs more thought before locking in.

### 8. Schedule and Sequencing

- **Current SLA work is blocked.** Won't roll out as-is — template-required + custom-field-driven are both unacceptable.
- **Ridi confirmed:** GARNET will hold SLA development and pivot to service territories for the rest of the week.
- **Chuck:** "Bilas is not going to be happy with me" — accepting the slip in service of a cleaner technical solution.
- Service territories work targeted to wrap by Friday.
- Lynsay will manage Alyssa/Linda expectations on timeline.

---

## Decisions

| Decision | Details |
|----------|---------|
| Drop custom field for priority | Replace with first-class work order attribute/field. Validated numeric (likely 1–6). |
| Reporting target mapping | `(priority, template)` lookup. Null-template target is the fallback when no template is passed. Removes template-required constraint. |
| Reporting scope | Reporting includes all WOs in a project, regardless of routing (SD or manual). SD becomes a filter, not a gate. |
| Current SLA capture work — held | Won't roll out template-required + custom-field-driven implementation. |
| Pivot in the short term | GARNET moves to service territories for the rest of this week. SLA capture resumes once redesign is locked in. |
| "SLA" terminology | Likely wrong — needs a better name. Look at what Salesforce/ServiceNow call this. |

---

## Action Items

| Owner | Action |
|-------|--------|
| Bilas (Chuck to brief) | Mull priority field implementation. Propose whether it should be a work order attribute vs. an actual work order field. Also weigh in on where priority/reporting targets should live (bounded context). |
| Lynsay | Schedule time with Linda today. Confirm the proposed technical direction (drop custom field, first-class priority field). |
| Chuck | Schedule follow-up with Lynsay + Ridi, targeting tomorrow (May 7) at 7 AM CDT. Re-examine template linkage and whether a unique sub-project ID can replace template as the differentiator. |
| Lynsay | Work with Linda to clarify project goals/requirements without using the terms "template" or "custom field" — separate the requirements from the implementation choice. |
| Lynsay | Loop Alyssa and Linda in on the technical details and timeline impact. |
| Lynsay | Update the group if anything she conveyed in the meeting turns out to be incorrect. |

---

## Open Questions

- Where should priority and reporting targets live in our bounded contexts? (Not work order; possibly dispatch-automation; possibly a generic reporting service per Ridi/Mike's suggestion.)
- Do we actually need to bind reporting target differentiation to templates, or is there a cleaner unique identifier (sub-project ID, program name)?
- What should the priority field actually be called? "SLA" is probably wrong — Salesforce/ServiceNow naming conventions worth investigating.
- Is the priority range 1–6 the right standard, or something else?
- Will Linda sign off on this direction when Lynsay meets with her today?
- Does fixing service territories properly (rather than working around them via templates within a project) make some of this configuration complexity go away? Acknowledged but explicitly deferred.
