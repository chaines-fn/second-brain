# Net Dev / Service Territory Regroup

**Date:** April 1, 2026
**Time:** 11:29 AM CDT
**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1q6kHxzNb7xDRrIzniWb8nfKJRU9iPe5zE5IFmqGuidY/edit?usp=meet_tnfm_calendar)

---

## Attendees

| Name | Role |
|------|------|
| **Jeff Novotny** | NetDev internal customer / primary stakeholder |
| **Ryan Schulte** | PM, SLATE team |
| **Laura Middleton** | NetDev team — vetting and fulfillment criteria owner |
| **John Shaw** | Group PM |
| **Chuck Haines** | Sr. Principal Engineer |

---

## Summary

A regroup between NetDev stakeholders and the SLATE team prompted by Jeff Novotny's frustration over lack of documentation and communication around the pivot to an Agentic AI recruitment tool. Jeff didn't understand what had been built, what was committed, or why the strategy had shifted. The team realigned on the current direction (a net-new full-funnel AI recruitment tool replacing the old managed recruitment requirements), agreed to move to weekly syncs, and committed to improved Now/Next/Later documentation. A follow-on Monday meeting (April 6) with broader stakeholders was already scheduled to nail down Phase 1 scope and timeline.

---

## Key Notes

### Strategic Pivot — Old Requirements Are Outdated
- The January Phase 1 requirements (invite experience, automated reminders, message center integration, etc.) were scoped to **updating the managed recruitment tool**. That view is now outdated.
- The new Agentic AI tool is a **net-new, full-funnel** solution that will encompass all of those features and more.
- Jeff was not adequately briefed on this shift — he only heard about a chatbot POC and a coverage analysis feature. The gap in communication caused confusion about what the team was actually building.

### Agentic AI Recruitment Tool — Active, Not Shelved
- This tool is **actively being built** (contrary to what may have been recorded elsewhere). The chatbot-based UI POC was shelved due to hallucination issues, but the team pivoted to a platform-based top-of-funnel approach — that work is continuing.
- Ryan was screen-sharing the **UI for Phase 1** during this meeting — UI is built, backend is actively in development.
- Ryan acknowledged documentation has not kept pace; the driver has been speed-to-value for Jeff's team.

### Phase 1 — Top-of-Funnel Provider Matching
- **Input:** A site or territory list (address-based, CBSAs, or territories)
- **Processing:** Match providers against hard (must-have: PSS, background checks, screenings, distance) and soft (ranking preference) requirements
- **Output:** Exportable filtered provider list for the BD/NetDev team to begin recruitment
- **Distance logic:** Uses crow radius to narrow the list first, then performs driving distance calculation on a smaller subset (full driving distance on all providers is too costly)

### Phase 2 — Email Outreach
- Brings back the outreach and inbound functionality from the chatbot POC
- Enables immediate email outreach to the generated provider list

### Phase 3 — Automated Stage Handling (Critical Dependency on Laura)
- Auto-reply and message parsing to push providers through the funnel with minimal manual intervention
- **Requires** Laura Middleton's team to supply every possible vetting use case and criteria — Jeff emphasized this list needs to be as comprehensive as possible (aiming for ~100% coverage) before this phase can be built reliably

### Provider Work Location as Recruitment Signal
- Jeff and Chuck surfaced an important insight: **where providers actively work** is a stronger recruitment signal than their home address
- For white space solutions (finding providers for underserved areas), home location matters; for active recruitment, work location matters
- This has **not yet been accounted for** in the built tools — flagged as a new requirement

### "Prefers" Ranking — Soft Requirements
- Chuck proposed building "prefers" into the provider ranking algorithm
- Example: providers meeting minimum PSS can be ranked higher if they also have a very recent background check
- This allows nuanced ranking beyond pass/fail hard requirements

### Volume Control and Territory Scale
- Jeff raised concern about the sheer volume: Door Dash has 600+ service territories, Clear Captions has 747
- NetDev BD bandwidth can't recruit for all simultaneously — may need a strategy to prioritize high-volume territories first
- Ryan confirmed the tool is factoring in a "noise reduction" approach — target a smaller number of high-quality providers per territory, scale outreach up as needed

### Terminology (Jeff cares about this)
- **"Recruitment"** = buyer-specific, tied to a specific buyer's need — this is the term to use for work tied to a client
- **"Campaign"** = broad marketplace development (e.g., driving background check % network-wide, not tied to a specific buyer)
- **Never use "campaign" for buyer-specific work** in the UI or documentation
- "Supply request" also acceptable as an alternative to "recruitment"

### Service Territory and Reporting Coupling
- Jeff stressed that service territories and reporting need to be developed **almost simultaneously** — reporting outcomes will influence how territory tooling is constructed
- This also feeds into white space strategy and decisions about defaults vs. custom zip code storage

### Service Territory Visual Editor
- Jeff will document the key visual requirements (territory shape, volume inside/outside territory, RIMG footprint visuals to demonstrate manual build difficulty)
- Ryan confirmed the plan is to **platformize** this — edits made live in the platform without needing Jeff's time or external tools (short-term plan)

### Process and Communication Commitments
- **Weekly sync:** NetDev team meeting upgraded from bi-weekly to weekly
- **Documentation:** Ryan to create Now/Next/Later docs covering accomplishments, commitments, roadblocks, and future path for all tools
- **Visual progress matrix:** Jeff wants a matrix with columns for: deployment status, lessons learned, team testing/approval, branch plans, capacity implications
- **Jake and Daryl:** John Shaw to determine if they should join weekly syncs given reporting, tables, and ticket creation needs

### Upcoming Activities
- **Monday April 6 meeting:** Travis, Chuck, Corey, John Shaw, Shane, Lisa, Ryan — Phase 1 scope and timeline discussion
- **Minneapolis (week of April 7):** Jeff and Laura both in town — Ryan to coordinate 1:1 or workstream sessions for concentrated collaboration
- **Table read exercise:** Walk through realistic buyer scenarios (e.g., a 10,000-site drop) to assess recruitment capacity and strategy end-to-end
- **John Shaw** to schedule walkthrough of Match 2.0 / PSS platform functionality for Jeff, Laura, and others
