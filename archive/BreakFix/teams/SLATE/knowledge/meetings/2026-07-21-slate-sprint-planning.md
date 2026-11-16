# SLATE (BF NetDev) Sprint Planning - Jul 21, 2026

**Organizer:** Amran Hossain · 8:12 AM CDT (20:12 GMT+6)
**Attendees:** Lynsay Faucett (PM), Ryan Schulte, Mobarok Hossain, Abu Saleh Matul, Sony Sadi, Nazmus Saqib, Amran Hossain
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1dPiHx68INjODSxoTHgfcDn7Mynb2QAWT_rPDR98rQhw/edit)

## SLATE / GARNET ownership split finalized

Tickets were physically moved between boards - Abu Saleh coordinated with Ridi to move them from the Slate board to the Garnet board.

| Team | Owns |
|------|------|
| **SLATE** | Opt-in service territory feature management (toggle on, save opt-in territories); the **async → synchronous conversion of auto-association**; Kafka flow synchronization and fit-and-finish |
| **GARNET** | Smart Dispatch trigger points and pre-validations across all touchpoints (integrations, mass create, work order create) |

Lynsay's summary: "you're handling the rule and they're handling how it works in dispatch." This confirms the Jul 16 synchronous-ST-association decision is being executed by SLATE.

## Capacity

- Initiative sized at **17 SP SLATE / 21 SP GARNET / 38 SP total**.
- SLATE's normal velocity is 21 SP. Three PTO days this sprint (Mobarok 2, Sony 1) cost ~1.8 SP, mathematically ~19. **Sprint committed at 21 SP with an operating target of 20.**
- One 3-point ticket expected to actually be ~2; will be split if it exceeds 2.
- **KTLO:** 1 SP allocated to investigate the replica-lag incident.

## Scope / Timeline Decisions

- **Service territory input-limit ticket deliberately excluded** from the sprint despite rising urgency. Lynsay to take priority up with John - she called this out pointedly given Ryan's push to prioritize it.
- **Integration documentation update for service territories deferred.** More buyers are onboarding to service territories and need to update their integrations, but Mobarok flagged that the ST create/update/delete integration endpoints built 3–4+ months ago haven't been retested after substantial subsequent changes. **Decision: integration testing first, documentation update after** - likely next sprint. No ticket exists yet.
- A V3-API adoption ticket in To Do wasn't picked up and slips past the upcoming sprint. Not a blocker for the map view release.

## Map View

- Pin opacity set to **50%** (Ryan's call; Sony to push to the existing ticket).
- Filter-bar API optimization needs ~2 more hours before deploy.
- Pin **clustering radius left as-is** - decision deferred until real production data shows whether clustering is too sparse.

## Replica-Lag Incident Root Cause (Abu Saleh)

DB lock contention - another service updating the same work-order row concurrently. **Not** a post-assignment-service defect. Platform team already raised replica IOPS from **8,000 → 12,000**, which is expected to stop recurrence. Mobarok insisted on time to investigate and report findings back since it was an incident action item; Abu Saleh assessed it as not urgent.
