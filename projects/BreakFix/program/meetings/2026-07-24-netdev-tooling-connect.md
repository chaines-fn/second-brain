# Weekly NetDev Tooling Connect - Jul 24, 2026

**Organizer:** John Shaw · 10:30 AM CDT
**Attendees:** Ryan Schulte, John Shaw, Lisa Sacquitne, Travis Emslander, Shane Kinder, Amran Hossain, Ann-Bettina Colace, Lynsay Faucett
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1Ee6KzMFQID8xUgaRItLztrlkY3zaJK4-epqC0_oaTFk/edit)

> Chuck on vacation. Notable for having Travis Emslander (CTO), Shane Kinder (EVP Product), and Lisa Sacquitne all in the room.

## Key Decisions

| Decision | Detail |
|----------|--------|
| **Scale Andrew Kandels' new email send service** to permanently remove the 2,000/day cap | Its TDR passed the morning of Jul 24 with little rework. Travis expects an even simpler solve than Andrew originally scoped. Estimated **2–3 weeks of SLATE engineering**. |
| **No interim SMTP solution** | Third-party (SendGrid) and second-Gmail-account options rejected - inbound reply parsing would need DNS/MX changes plus a new webhook endpoint, roughly a week of effort against a 2–3 week permanent fix. |
| Mitigation is operational | Batch outreach to **~500/day** and stop dev-side bulk test sends. |
| Only the **innovation-to-product TDR / architectural review slips** | John Shaw flagged that absorbing this work put NetDev OKRs and roadmap at risk for 2–3 weeks and had asked Cory about contractors. Amran took it and scoped the hit narrowly - main implementation does not slip. |
| Next two weeks of feature work is **stage gating only** | Provider-submitted forms/files route direct to a stage, replacing the Google Form swivel-chair. Email attachments deliberately left undecided - Ryan doesn't want to over-invest in email when stages plus the Q4 mobile provider experience will carry the load. |

## Adoption Metrics (first real numbers)

- 8 active recruitments across 5–6 buyers (Zones, Connections, DoorDash overlap)
- **Zones:** 357 providers in funnel, 20% conversion to approved, 27% reply rate
- **Connections:** 368 total, 315 still in progress
- Ryan: on track for **100% of new recruitments in the tool**, no remaining blockers to that goal
- Analytics tab now shows per-territory filled-vs-target, % coverage, and **estimated time-to-fill** - new capability

## Open Items / Gaps

- **Priority designation is hardcoded** into Alyssa's reporting artifact for apples-to-apples comparison, not built into the platform, so the data can't flow both ways (John Shaw). High/medium/low priority is staff-set, not progress-derived - Shane confirmed that matches the Break/Fix need to sequence territories by incoming volume.
- **No territory ID join.** Shane wants aggregate time-to-fill and response-rate benchmarks per standard territory so NetDev can tell a buyer up front which territories fill fast. Doable manually today only via **name-text-matching CSV exports** - a real data-model gap if this becomes a product commitment.
- **Q4 mobile launch scope now includes provider onboarding that captures work preferences** (short notice, overnight, travel, W2 vs. 1099), stored initially as plain text, to feed double-funnel search and eventual algorithmic recruitment matching. Shane pushed for a raw-text provider attribute field sooner so data isn't lost in the meantime; Ryan held the line on it being part of provider-experience v1 in Q4.
