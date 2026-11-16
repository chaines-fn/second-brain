# B/F Weekly Working Group Alignment - Jul 17, 2026

**Organizer:** John Shaw · 11:00 AM CDT
**Invited:** Ann-Bettina Colace, Ryan Schulte, John Shaw, Linda Lo, Alyssa Eby, Lynsay Faucett, Chuck Haines, Brent Grill, Melanie Payeur, Amran Hossain, Sadat Haque, Tauhedul Shagor, Kayle Schiele, Rick Jundt
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1Z_52R5_6Y2qeTjH7LRG95P4RMO-5LHKxbiWw4rglX3w/edit)

> Chuck did not attend (vacation Jul 19–30 began the following weekend; he was invited). This is John's business/program-level forum, not the engineering deep dive.

## Key Decisions

| Decision | Detail |
|----------|--------|
| Service territory config target moves to **early August** | Lynsay: the work "has expanded into approximately two and a half sprints of effort." Supersedes the Jul 27 release target set at the Jul 10 alignment. |
| Work split across **GARNET and SLATE in parallel** | Both teams assigned to the same initiative to pull the date in. New cross-team coordination risk. |
| **NetDev AI roadmap item removed** from the current schedule | Let MARBLE and Core Transform establish internal AI patterns/tooling this quarter first rather than integrating a new AI feature prematurely. |
| Smart Dispatch **fit-and-finish prioritized over Amplitude tracking** | Protects the release date; trades instrumentation for schedule. |
| **No data model changes** from standardized service territories | Answers Alyssa Eby's question about impact on coverage score models. The work "unlocks the ability to smart dispatch to specific service territories" - nothing more. TDD goes to Alyssa and Mia for review. |

## Notable

- **Root cause of the 2.5-sprint expansion:** a required **transition from asynchronous to synchronous calls**, needed both to improve the infrastructure and to fix ongoing issues with auto-applying pay rates. This is the same synchronous-ST-association direction agreed Jul 16 with Mike Mueller, now showing up as schedule cost.
- **H2 OKR stated more precisely than before:** onboard **three buyers with service territories at significant volume**.
- **GTM positioning shift:** messaging pivots toward competitive share gain from *new* buyers rather than migrating existing buyers, who are "already scaled within their current workflows." (Partially softened the following week - see Jul 24.)
- **New buyer activity:** Clear Captions now in play with post-meeting Smart Dispatch rules questions, working toward program implementation. **Luxer One** ran a successful break/fix demo. Three new service ops closed.
- **Program setup is still fully manual** across CX/NetDev/Product (John acknowledged). Alyssa Eby standing up project-management foundations in **ClickUp**, targeting streamlined site-list + service-territory creation by end of Q3.
- **NetDev scale:** 5 active recruitments across 3–4 buyers. Recruitment bugs attributed to the volume of simultaneous provider adds.

## Open Questions

- No mention of the H3-vs-zip-code decision or the "Standardize, Don't Customize" pitch to Jeff Novotny. Jeff was not an attendee - that outcome remains unconfirmed by any source.
