# Core Transform Kickoff Meeting

**Date:** 2026-04-08, 2:55 PM CDT
**Attendees:** Shane Kinder, Travis Emslander, Cory Muir, Chuck Haines, Spencer Moulton, Linda Lo, Brent Grill
**Absent:** Maria Saxton (OOO — family funeral)
**Transcript:** [Google Doc](https://docs.google.com/document/d/1lYJ6f4-cGv8JZ2e9c_If8Ux8ehQ2c49e2DVWwNsi94I/edit?tab=t.s3icbyh2y7ue)
**Slide Deck:** [Google Slides](https://docs.google.com/presentation/d/1w97_RaI0V7Y77KXTpIUUqpnGy-GNKcL_T8LjnKOMKmU/edit)

---

## Mission Statement

> "Transform the product experience leveraging AI to first automate buyer tasks that are manual today, and then work towards a new buyer experience where Field Nation acts as a virtual service company."

---

## Key Concepts: Now / Next / Later

### Now (FN 1.0) — Manual operations
Three stages of a buyer program, all manual today:
1. **Program Planning** — feasibility, cost estimates, designing the program, building the provider network
2. **Program Execution** — ticket-by-ticket work order fulfillment, dispatching, redispatching, fulfillment assistance, check-in, deliverables
3. **Program Management** — weekly/monthly performance monitoring, SLA tracking, budget management, diagnostics, network optimization

### Next (FN 2.0) — Task automation with human-in-the-loop
- Look across the lifecycle and automate tasks one at a time
- Start human-in-the-loop, progressively move toward full automation
- Prioritize by impact × solvability — high impact + reasonably solvable first
- Build foundational platform and consistent patterns so automations aren't all one-offs
- Avoid edge case rabbit holes ("aim for 80/20 automation") — move fast, don't let the long tail block progress

### Later (FN 3.0) — Virtual service company
- All tasks automated, coordinated by super-agents per stage (program planner agent, dispatch agent, program manager agent)
- Fundamentally different buyer experience — not just a chat sidebar on the current platform
- Buyers give Field Nation the work like they'd give a service company — "here's my project, here's the parameters, come back with a proposal"
- Both 2.0 and 3.0 may operate concurrently for a period
- Opens new TAM: large buyers who lack dispatchers/coordinators can now use Field Nation
- Target: virtual service company MVP in 2027 (directional, not a hard commitment)

---

## Q2 Deliverables (The H2 Plan is the Keystone)

1. **Activity shortlist with priority order** — Decompose the buyer lifecycle into automatable tasks, prioritize for H2 execution
2. **FN 3.0 concept vision artifact** — "Magic marker fidelity" — slides, prototype, comic strip, whatever — a shared picture of the end state to ground decisions. Prevents the "data platform 2.0 spiral."
3. **Technical architecture and foundations** — Near-term, mid-term, long-term decisions. Identify one-way door vs two-way door decisions. Always leave an escape hatch.
4. **H2 plan** — Now/next/later structure. This is the Q2 OKR. Everything above feeds into this.

---

## Strategic Decisions & Considerations

### Legal/Compliance
- Get as close to a service company as possible WITHOUT taking on compliance risk or outcome ownership
- "95 yards toward a service company" — buyer must approve the final action (the line that preserves 1099 classification)
- W2 model is NOT on the table
- Need early, ongoing partnership with legal to define explicit buyer actions that maintain the distinction
- Travis: "The defense has been that the buyer assigns the provider. As automation increases, that defense gets harder."

### Co-Creator Program
- Must identify multiple collaborative buyers operating at scale
- Design a structured program: embed them in development, quarterly on-site visits
- Avoid over-rotating to a single buyer's edge cases
- Co-creators must be flexible — willing to accept solutions that solve their problem but may not match their exact spec
- SLT has already floated names

### Team Operating Model
- This team owns: vision, strategy, roadmap, patterns, and technical foundations
- Other teams (fulfillment, buyer analytics, etc.) align their work within this framework
- Team has permission to experiment in other domains — like a "birth team" or the integrations team model
- Long-term: transition ownership of specific agents to domain teams
- Leadership (Shane, Travis, Cory, Mynul Khan) are engaged partners, not drivers
- Regular syncs: 30 min/week with leadership. Mynul Khan wants to be involved as an "accelerant."

### Speed & Focus
- Shane pre-wired the SLT: "If we go through every edge case, it'll take 10 years and be irrelevant"
- Must move faster than past initiatives (every previous initiative took 1+ year to roll out)
- Product marketing should be pulled in earlier than usual to develop internal narrative

### Platform vs. Product
- Travis: "We don't want Agent Force where customers have to build their own agents. We should automate core processes for them."
- Cory: Platform should enable extension — other teams, contractors, or even buyers could build agents, but FN must define the core use cases first
- Interoperability with buyer source systems is critical (e.g., schedule change → buyer's agent coordinates with end client)
- Standards-based implementations over proprietary approaches

### Don't Over-AI It
- Shane: "Don't apply AI where simple AB logic works. But also intentionally find early agent opportunities to build experience."
- Linda & Spencer: Hesitation about going zero-to-agents — start with automation, heuristics, rules to build comfort and fix data quality
- Travis: The end goal MUST remain the virtual service company — that's what opens the new market

---

## Roles Clarified

| Person | Role on Initiative |
|---|---|
| **Spencer Moulton** | Core team leader — owns driving design and implementation |
| **Linda Lo** | Product lead — transitioning from current team, already sending use cases to Spencer |
| **Brent Grill** | Design lead — transitioning from current team |
| **Chuck Haines** | Architecture lead — not 100% dedicated, cross-cutting support, also working other AI projects |
| **Maria Saxton** | Pearl team engineering manager (absent from this meeting) |
| **Kimberly Zito** | Pearl team engineer (not in this meeting but on the team) |
| **O Max Anderson** | Pearl team engineer (not in this meeting but on the team) |
| **Shane Kinder** | Initiative sponsor — VP/SVP level, framed the initiative with Travis |
| **Travis Emslander** | Engineering leadership — engaged partner, not driver |
| **Cory Muir** | Engineering leadership (Spencer's management chain) — engaged partner |
| **Mynul Khan** | CEO — wants to be brought along, sees it as foundational to the business |

---

## Next Steps (from the meeting)

- [ ] Spencer, Linda, Brent: Define the activity shortlist and priority order
- [ ] Team: Create the FN 3.0 concept vision artifact
- [ ] Spencer & Chuck: Articulate near-term, mid-term, long-term technical decisions
- [ ] Linda: Support buffer time in planning for reworking early decisions
- [ ] Team: Design the co-creator program and identify collaborative buyers
- [ ] Team: Define operating model and set up regular syncs with Shane, Travis, Mynul Khan
- [ ] Cory: Look at logistics for in-person workshop in Minneapolis (targeting late May / early June for sign-off)
- [ ] Shane: Share the slide deck with attendees
- [ ] Linda: Set up Slack channel (became #pearl-eng, created by Maria on 2026-03-19)
- [ ] Linda & Brent: Work through transition plans from current teams

---

## Pre-Meeting Discussion (AWS AgentCore Workshop Debrief)

Before Shane and Cory joined, Travis, Spencer, and Chuck discussed takeaways from an AWS AgentCore workshop earlier that day:
- **Outgoing authentication** — interesting for integrating with external systems (e.g., Google Calendar) but unclear on the OAuth flow without a desktop app
- **Memory/tenancy** — hand-wavy answers from AWS on multi-tenant memory management; name-spacing seems to be the answer but wasn't clearly articulated
- **Memory management** — still very explicit (short-term vs long-term), not auto-figured-out
- Travis is using memory for session storage in the "Daryl thing" (stores in S3)
