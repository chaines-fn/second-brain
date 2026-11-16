# OKR Review Meeting - Jul 22, 2026

**Organizer:** Cory Muir · 10:00 AM CDT
**Speakers:** Cory Muir, Alyssa Eby, Andrew Kandels, Chandhini Eyler, Abdullah Reza, Travis Lang, Woody Woods, Shane Kinder, Maria Saxton, Stephen Wald, Linda Lo, Rashedul Kabir, Arefin Hossain, Amran Hossain, Shadab Ahmed
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1IwjeIM8JuCnbvwYifbEbmDfA28wTX3Xe7t6V5dG0jtM/edit)

> Mostly PEX-wide rather than Break/Fix-specific. Chuck on vacation. Captured here for the cross-program items that touch B/F.

## Break/Fix-relevant

- **NetDev email limit escalated to the Sr. Director level.** Amran reported the 2,000-email/day cap, roughly one month to implement a fix; **Andrew Kandels owns the TD**. Amran + Cory to align on infrastructure so the work isn't redone during data platform integration.
- **New technical risk with no named owner:** current "revisit" models are unreliable. Shadab Ahmed to model a solution; **needs architecture leadership** for potential data refactoring. This is the closest thing in these notes to a gap left by Chuck's transition - worth a look.
- **Q3 B/F OKR language (buyer adoption + standardized service territories) was not discussed or locked** in this meeting.
- **OKR tracker restructured:** P0 items pinned to the top for standing review, P1 next, P2 only as time allows.

## Cross-program decisions worth knowing

| Decision | Detail |
|----------|--------|
| **WWT prioritized over Flight Board rollout** | Shane Kinder justified on projected **$15M+ spend**. Flight Board refinement/estimation slipped to first week of August; two-week hypercare for WWT after rollout; **resourcing/contractor decision hard-dated 8/11**. |
| **DataDog LLM Observability** selected as the standard AI observability tooling | |
| **Orchestration framework scoped exclusively to the public API** | Explicitly excludes mass work order create. Proven via work order create; remaining excluded paths documented by **mid-September**, then a resourcing decision on whether product engineering picks it up. |
| **AI enablement OKRs repivoted** | To an engineering readiness playbook plus a metrics scorecard (cycle time, throughput, code quality). |
| **Brand refresh approved** | Light rebrand - modernized logo, refined palette - **launching at US Field Week**. Product teams get a logo-swap punch list afterward. Relevant to any customer-facing asset produced between now and then. |
| **SMS end-of-life effective Jul 23** | Negative Verizon-user feedback expected; feature flag cleanup to follow. |
| AVI-SPL pilot start now **8/11–8/25** | Slipped on competitive bidding, ratings/reviews complexity, and design resource changes. Two contractors retained through the pilot. |
| Cory: **Q4 buyer migration is unlikely** (buyer busy season) | Need a diverse buyer list, targeting standard integrations (Salesforce, ServiceNow) rather than a single committed buyer. IP Pass internal TDR targeted 7/28. |
| **Co-creator program now formal, requires buyer agreements** | Monthly checkpoints in Q3, bi-weekly value shipping in Q4. Pre-Site readiness: M1 base path defined, agent in production, suggested-task API being estimated. |
