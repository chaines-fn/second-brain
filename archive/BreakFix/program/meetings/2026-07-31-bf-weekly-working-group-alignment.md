# B/F Weekly Working Group Alignment - Jul 31, 2026

**Organizer:** John Shaw · 11:00 AM CDT
**Invited:** Ann-Bettina Colace, Ryan Schulte, John Shaw, Alyssa Eby, Lynsay Faucett, Chuck Haines, Melanie Payeur, Amran Hossain, Kayle Schiele, Sadat Haque, Tauhedul Shagor, Rick Jundt
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1O4-ruYNkLICSBvtoX7QUOUPLZe4P_dFKFDFgbKf29cc/edit)

> Chuck's first day back from vacation was Jul 31; he was invited but the notes don't record him speaking.

## Key Decisions

| Decision | Detail |
|----------|--------|
| Service territory configuration data will be **ingested directly into the data platform**, not into separate DW tables | The meeting's one firm architectural alignment. |
| Default service territory configuration is a **pre-qualification step** for Smart Dispatch | Similar to project setup; **does not alter existing eligibility requirements**. Consistent with the Jul 17 "no data model change" clarification. |

## Metrics

- **Smart Dispatch success rate is 45%** on the Tableau dashboard. First hard number on SD performance.

## Timeline

- **Third consecutive slip.** Service territory configuration is only now "on track to enter the fit-and-finish phase next week." Against the original Jul 27 target and the Jul 17 "early August" commitment this is roughly two-plus weeks late. Lynsay's action item is still to **finalize the technical design** for standardized service territories.
- **Email service port target now firm: August 24.** Until then outreach is paced across 4–5 days. Active recruitments 5–6, down from 8 the prior week.
- **Interface live launch targeted next week.**

## H3 vs. Zip - Signal, Not Closure

Lynsay described default service territory configuration as utilizing **existing zip-code lookup processes used for auto-applying pay rates**. Clear Captions separately requested their service territories and associated **zip codes** to prep their internal tools for migration. No H3 mention in any of the three July working-group meetings. The mid-August decision deadline was never referenced.

**Reconciliation needed:** this reads zip-forward, while the Jul 28 PEX Lead Sync landed on adding an **H3 resolution-8 column to DW tables**. Best current reading is the standing Jul 9 split (H3 for reporting/visualization, zip for operational computation) rather than a new decision either way - but nobody has said so on the record.

## Scope Cut

- **NetDev stages + provider quizzes split.** Ryan proposed splitting scope to avoid derailing the quarter, deferring **four items** on the cut list including technical design and prep work for the buyer experience. Stages/quizzes are described as "the last core functional features required before automation."

## Risks & Dependencies

- **Territory configuration requires buyer-side setup.** Clear Captions integration depends on their existing service territory setup - a buyer-side prerequisite that is an adoption risk against the "three buyers at volume" OKR.
- **New cross-team dependency:** backend data issues for assets/equipment on the provider profile require work from **Quartz or GARNET**; delivery may be delayed though SLATE's own scope is unaffected.
- Tony's flagged Smart Dispatch **notification and qualification flag** issues still under engineering investigation. Lynsay: keep escalating to engineering even during the current hypercare phase.

## Open Question (new, structural)

**Project/client structure is too granular and the "client" function does not solve the requirement.** Melanie surfaced CS Zones training feedback that buyers are using **custom fields as workarounds**; Lynsay confirmed via Andrew and Laura. The team wants a formalized long-term solution for treating programs consistently, to be evaluated during **Field Week**. The notes call this "a major structural methodology gap." This is arguably the same tension as the territory-customization debate resurfacing from the buyer/CS side.

## Pipeline

- **NCR - ~$1.5M break/fix potential, ~2027 start.** Site lists received for "NCR Voyex," being processed through the tool.
- **High Point Staffing** - previously stalled, now back with a **September 1 go-live** for data center work. Melanie advised starting coverage analysis and service territory setup now.
- CS target account list of **~85 buyers with at least $500K in third-party spend**. Rep partnership kicked off with mid-market reps Joe and Nate - training on break/fix pain pillars, 6sense messaging pillars, buyer archetypes.

## Field Week

Three-hour in-person brainstorms planned on **next-gen dispatch automation, command center improvements, network health automation, coverage assessment reporting**, plus the project/client structure question. Open to NetDev, CX, and data science stakeholders.

## Parts

Core work nearing completion, fit-and-finish early next week. Coordinating **Salesforce and ServiceNow** integration expectations; parts-return features may land on the **Q4 integration team roadmap**.
