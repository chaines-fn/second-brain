# Obsidian Refinement/Planning — Projects Page — 2026-04-28

**Date & Time:** Tuesday, April 28, 2026, 09:30 CDT (~48 minutes)

**Attendees:**
- Lacey Selvagn — Product Design Lead *(meeting driver — walked the team through the design)*
- Andrew Kandels — Engineering lead, UI modernization
- Yurii Dmytriiev — Contractor, Frontend Engineer
- Ivan Sakhman — Contractor, Frontend Engineer *(present; spoke briefly at end)*
- Maria Saxton — Sr Software Engineering Manager *(taking on PM role for this project)*
- Chuck Haines — Sr. Principal Software Engineer
- Isikiyski Miroslav (Miro) — PHP backend contractor *(invited; did not speak)*
- Laura Quist — Sr. Customer Success Executive *(invited; did not speak — repeatedly cited as the decision-maker on open questions)*

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1N41bd3d4PN3_Kg6KmtGjYf2D06LCGUlAC55iduLuUhE/edit?usp=meet_tnfm_calendar)

---

## Overview

Lacey walked the team through her design for a redesigned project page — an embedded, project-scoped flightboard meant to give WWT (and eventually other high-volume buyers) a place to triage daily work. The current page is essentially unused, which makes it a good test bed for new patterns before they touch the main flightboard. Lacey's "end vision" mockup includes streamlined header/actions, new tabs (Today / Upcoming / Past / All), grouping by Site ID, lifecycle stages, an exceptions suite, and a bunch of column/filter/export improvements. The point of the meeting was to figure out how to break that vision into phases that engineering can actually start on this week.

The biggest engineering shift came from Andrew: because projects rarely exceed ~10,000 work orders, the team can pull the full project dataset in one API call and let MUI's data grid handle grouping, sorting, filtering, multi-sort, pivoting, and export *client-side* — rather than building custom server-side implementations like Flightboard has. That single decision unlocks a lot of the "later phase" features almost for free, as long as every column has a scalar value defined for grouping/sorting. The team also aligned on moving stickiness from user-level to project-level so configuration (columns, sort, filters, grouping) is shared across everyone at the buyer's company — Laura's intent is for her to set it up once for WWT during onboarding.

Phase 1 was scoped down to the must-haves: the new tabs based on service date plus status conditions, default grouping by Site ID (configurable, since Site ID is a WWT custom field), project-level column configuration with stickiness, and lifecycle stages. Exceptions management, PSS, tasks-as-separate-data, real-time message indicators, and saved filters were all deferred. Maria committed to drafting 3–4 Phase 1 tickets from the transcript and scheduling a follow-up tomorrow with Laura present to nail down the open question on tab-level vs. universal stickiness. Phase 2 (exceptions suite) was punted to a future session.

---

## Key Notes

- **Projects page is the strangler test bed, not Flightboard:** The redesign is scoped to the project-specific page — not the main Flightboard. This is intentional: the page is currently unused, so the team can experiment with patterns (project-level config, client-side processing, MUI-native features) without disrupting the high-traffic Flightboard. Successful patterns can graduate to Flightboard later. Other tabs on the page (templates, etc.) keep working as-is. The "Planner" tab will be hidden — nobody uses it and budget-setting isn't actually wired up.

- **Client-side data processing is the architectural unlock:** Andrew proposed pulling the entire project work order list in one API pass and letting MUI handle grouping/sorting/filtering/pivot/multi-sort on the client. This works because few (if any) projects exceed 10K work orders. It's the opposite of Flightboard's approach (server-side because of tens-of-thousands of rows). The cost is that every column needs a scalar value defined alongside its visual rendering — e.g., a schedule column shows "Today at 8:30" but stores `2026-04-28 08:30` as the scalar for grouping/sorting accuracy. This is required for sort anyway, so it's not net-new work.

- **Stickiness moves from user-level to project-level (shared across the buyer's company):** Today, column config / sort / filters are sticky per user per tab on Flightboard. The new model is project-level — Laura configures once, every WWT user sees the same view. Open question deferred to Laura: should stickiness be per-tab (different columns on Today vs. Past) or universal across the page? Andrew flagged this matters for ticket scope.

- **Phase 1 scope locked:** (1) Tabs by service date + status conditions — Today / Upcoming / Past / All, with nuanced rules (drafts excluded from upcoming, Past defaults to last 90 days but configurable, etc.). Andrew noted today's Flightboard "Today" tab has ~12 conditions because of all the sub-statuses — same complexity applies here. (2) Default grouping by Site ID, configurable via MUI's column-header menu (because Site ID is a WWT custom field, this *has* to be configurable, not hardcoded). (3) Project-level column configuration with stickiness. (4) Lifecycle stages. Header/action restyling is *not* required for Phase 1 — it can remain as-is.

- **Use MUI defaults wherever possible:** The team explicitly aligned on using MUI's native UI for grouping (three-dot menu on column headers), multi-sort, filtering, pivot, column manage panel, and even the toolbar — instead of building custom equivalents. Lacey's mockups generally match the MUI defaults. Implication: the existing custom column-configuration dialog they built for Flightboard should be replaced on the projects page with MUI's native column panel. Flightboard itself keeps its current custom implementation.

- **Native MUI export resolves a known pain point:** Today's export doesn't match the configured column view, so users have to re-format every export manually (some do this twice a day). MUI's built-in export respects the current column configuration, so swapping to native export gets that for free.

- **Mass actions must work with grouping (and with mixed-status tabs):** Yurii confirmed selecting a group via its header checkbox should activate the existing mass-actions panel (which Lacey will mock up under the tabs). Mixed-status tabs (Past contains assigned + done + approved) mean some actions won't apply to all selected work orders. Resolution: show all actions, let the action work on applicable items, surface a single error for inapplicable ones — i.e., don't hide actions, don't break, just degrade gracefully. This is how Flightboard's "All" tab already behaves.

- **What's NOT in Phase 1:** Filter bar rebuild (keep existing one as-is — Andrew flagged a rebuild as "several weeks of Yurii's time"). Saved filters at company level. Exceptions suite (Phase 2). PSS column and tasks as separate data fields (Phase 3 — these come from other services and require data-pipeline changes Flightboard hasn't done yet). Message indicators (Phase 2-ish). Column reordering by individual users on top of project defaults (Phase 4). Pivot/charts (Phase 4 — but enabled "for free" by the client-side decision).

- **Permissions on column config — admin-only for now, gracefully degrade for non-admins:** Concern raised that any user could trash the project's config. Resolution for Phase 1: lock column-management changes to admin users; non-admin changes work in-session but revert on refresh. Down the road, an "admin made a change — save for everyone?" banner could be added. Andrew confirmed this doesn't change architectural scope, just permission/UX.

- **Site ID is a WWT-bespoke custom field — design must stay reusable:** Chuck pushed back on hardcoding Site ID grouping. Andrew agreed: "let's not write code that says if WWT." The architecture allows any column to be the default grouping; Laura sets Site ID for WWT during onboarding via the project-level config. Other buyers can group by whatever makes sense for them.

---

## Action Items

- **Lacey Selvagn:** Review the meeting notes/transcript and leave comments where the design needs adjustments — including removing the "return to staff" item from the triple-dot menu (it lives in the blue bar and can't be moved there).
- **Lacey Selvagn:** Finish a Phase-1-only version of the design (separate from the end-vision mockup) so engineering has a clean target to ticket against.
- **Lacey Selvagn:** Mock up the mass-actions panel placement underneath the tabs.
- **The group (with Laura):** Get Laura's call on stickiness configuration — should column/sort settings differ between tabs (Today vs. Past) or be universal across the project page?
- **Maria Saxton:** Draft 3–4 Phase 1 tickets from the transcript with full detail. Targeting today during her hour of free time before her next meeting.
- **Maria Saxton:** Schedule a 30–45 minute ticket-review meeting tomorrow (2026-04-29) — ideally with Laura present to answer open questions.
- **Maria Saxton:** Set up future refinement sessions to walk through Phase 2+ once Phase 1 tickets are in motion.
- **Andrew Kandels:** Add technical detail to Maria's tickets once she's drafted them (Maria will ping him).
- **Chuck Haines (implied):** Stay engaged on the Site-ID-as-bespoke-field point during ticket review to ensure the implementation stays generalized rather than WWT-hardcoded.
