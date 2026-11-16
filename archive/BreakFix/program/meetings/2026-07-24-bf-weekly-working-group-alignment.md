# B/F Weekly Working Group Alignment - Jul 24, 2026

**Organizer:** John Shaw · 11:00 AM CDT
**Invited:** Ann-Bettina Colace, Ryan Schulte, John Shaw, Alyssa Eby, Lynsay Faucett, Chuck Haines, Melanie Payeur, Amran Hossain, Kayle Schiele, Sadat Haque, Tauhedul Shagor, Rick Jundt
*(Linda Lo and Brent Grill dropped off the invite list vs. Jul 17.)*
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1n4lHMqmAUEKGp2cBb0qPZ6EXB298hQLaF14AXODWjTc/edit)

> Chuck on vacation.

## Shipped

- **Cancelling Smart Dispatch is live for all buyers** - "quiet release," positive feedback already from Interface. This piece of the original Jul 27 bundle landed roughly on time.

## Key Decisions

| Decision | Detail |
|----------|--------|
| **Timely fill rate calculated on all work orders**, not only filled ones | Prevents fill problems being masked. Coverage score migrating to DW reporting. |
| **Return trips / revisit initiative paused for the rest of the quarter** | Needs a broader redesign and architectural update; not in Q3 metrics. To sync with **Shadab Ahmed, the new product lead** on it. |
| **Parts logistics engineering concludes end of August**, capacity moves to buyer analytics | Sadat Haque + Tauhedul Shagor shift to support **AVI-SPL**, expected **GA in October**. Material reprioritization away from MARBLE. |
| Parts return completion **moves to end of August** | Knock-on of the reallocation above. Core workflows are built; preparing to sunset active experiments and roll the new flow out to buyers. |
| **Build a production-ready email outreach service**, 2–3 week effort | Blocker being solved: NetDev's 2,000/day email send limit. |
| Guide buyers toward **service territories as the sustainable setup**, not talent pools as a temporary workaround | Also push existing buyers to higher volume rather than chasing low-volume one-offs. Softens the Jul 17 "new buyers over existing buyers" framing. |

## Risks

- **Map view initial-load performance is now an explicit launch gate.** The team is openly weighing whether to launch in its current state or optimize further to avoid a poor UX. Consistent with the known degradation at ~1,200 territories (DoorDash).
- **Service territory config slips again** - now "expected to go live early in the next sprint," softer than the Jul 17 "early August" commitment.
- **PM Luchian begins a 12-week leave at end of August.** Hunari and onboarding specialists may cover PM functions.

## Notable

- **New rationale for the territory work:** territory mapping is now framed as the fix for a Smart Dispatch **reporting gap** - today there's no way to differentiate work orders intended for Smart Dispatch from those using talent pools.
- **NetDev traction:** 27% reply rate across 8 recruitments with 2,200 providers invited.
- **Unicus** - meeting next week with Adam, Erica, Alyssa, John to align on H2 2026 program goals; targeting Smart Dispatch testing/validation for Unicus as a top-3 ICP buyer. **IBM** to be brought on shortly.
- Tooling: migrating **Chorus → Gong** for call recordings (scorecards, break/fix conversation flagging, theme tracking) - POC now, access next week. ClickUp board stood up jointly by Kayle Schiele + Alyssa Eby.
- Onboarding team now live and managing customer intake. IP Pass kickoff is Kayle's team's focus for two weeks and may delay Break/Fix program reviews.
