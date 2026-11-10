# NetDev Initiative Follow-Up — Apr 7, 2026

**Meeting:** Hold for NetDev Initiative Follow Up
**Date:** April 7, 2026, 2:00 PM CDT
**Source:** [Google Doc / Gemini Notes + Transcript](https://docs.google.com/document/d/1UHIqbzOLpf2dsNn4jBi_e2BU_1ClUTWHNKXHG07-kxA/edit?tab=t.k1apqk71pdk6)

## Attendees

Ryan Schulte, John Shaw, Lisa Sacquitne, Travis Emslander, Chuck Haines, Cory Muir, Shane Kinder

---

## Purpose

Leadership alignment on the NetDev Recruitment Tool approach, delivery speed, and priorities. Shane and Travis had open questions/concerns about build velocity and whether the team was trending toward a "big build" rather than an iterative MVP.

---

## Key Decisions

**Stick with iterative MVP — no big build.** Shane and Travis aligned that the team should not pivot to a long-horizon foundational build. The approach stays: get the most basic admin console in place, then layer automation progressively from there.

**Phase 2 needs to be scoped down significantly.** Ryan will go offline with the team to determine the most stripped-down version of Phase 2 and bring it back to the group with a timeline estimate. The goal is to get something into NetDev's hands as fast as possible so they can start using and iterating.

**Weekly sync cadence.** John Shaw to set up a regular weekly quick sync to track progress, surface learnings, and prevent the drift that happened in Q1 (findings weren't escalated, misalignment wasn't caught until a follow-up meeting weeks later).

---

## What Was Discussed

### Phase plan overview (John Shaw)
- Phase 1 (provider sourcing list) — targeting end of April; replaces today's data team ticket workflow with a self-service tool; estimated 5% efficiency gain
- Phase 2 (MVP end-to-end recruitment tool) — same-speed as today but unified; foundation for future automation; ~70% of recruitments covered
- Phase 3 — automated reminders, flexible stages, enhanced metrics; bumps to ~30% total efficiency gain
- Phase 4 — agentic AI, mostly touchless recruiting; ~90% total efficiency gain

The efficiency numbers are cumulative, not additive. Phase 2 delivers no gain over current workflow — it just consolidates the tooling — and is the necessary foundation before AI can be layered on.

### The tension: speed vs. scalability
Shane and Travis pushed hard on why the current plan felt too slow. Shane's framing: this is an internal tool for a team of 2 running ~10 recruitments/month. A startup would have had something running in weeks. The build should be messy, iterative, and scrappy.

Chuck's counter: we're not a startup. There are 8,000 microservices with tech debt and data scattered everywhere. The recruitment problem is essentially rebuilding a system that was 90 story points to fix. And there are real API gaps — e.g., provider success score doesn't exist in the same API as provider coverage, so you can't just "strap existing APIs together."

Travis argued the two-engineer tiger team shouldn't be investing to solve platform-level problems for other teams. Chuck acknowledged that framing, and realized there's been a genuine disconnect: he'd been approaching this as a scalable long-term platform build, not a fast-and-dirty MVP. That's the core of the misalignment.

### The chatbot POC retrospective
Ryan walked through why the chat POC was abandoned: it was too ambitious, failed in usability, and was riddled with hallucination and security issues (e.g., using user JWTs to call back into APIs — risk of leaking one user's data to another). The pivot to provider sourcing was motivated by wanting to ship something fast and internal before tackling the harder reliability problems.

Shane pushed back that these risks weren't escalated properly — he and Travis weren't aware how serious the issues had gotten until shortly before this meeting. Ryan acknowledged the Q1 pattern: "We found risks and avoided them rather than found risks and mitigated them." Committed to escalating earlier going forward.

### External communication risk and the eval problem
The group agreed that the highest-risk surface for any AI-driven feature is external communications to providers — sending non-compliant language, saying someone is "hired," etc. Chuck flagged that eval frameworks to prevent email hallucination are a significant unknown. Spencer is the person at FN with that expertise; will need to borrow his time or have Safin and Chuck go figure it out.

Travis proposed a pragmatic mitigation: AI queues up emails for a human to review and send. Builds confidence before full automation. The group aligned on this approach as the right pattern — it's essentially the Phase 2 → Phase 3 transition: humans in the loop until the tool earns trust.

### ROI check from Shane
Shane's closing point: a 9–12 month project for a team of 2 doing ~10 recruitments/month has a 4-year payback period. That math doesn't work. Going scrappy isn't just about speed — it's about building confidence in the trajectory before making larger investments. This framing closed the conversation.

---

## Action Items

| Owner | Action |
|-------|--------|
| Ryan Schulte | Go offline with team to scope down Phase 2 to the most minimal viable version; bring back estimated timeline to the group |
| John Shaw | Write bullet summary of discussion outcomes; socialize recap with all participants |
| John Shaw | Set up weekly quick sync cadence to track delivery progress and surface learnings/risks early |

---

## Open Questions / Risks

- **Phase 2 scope:** What's the true minimal Phase 2? Needs Ryan + team's offline discussion.
- **Eval framework:** How do we ensure AI-generated email content doesn't hallucinate before we automate sends? Spencer is the key resource; needs to be engaged.
- **NetDev team buy-in:** They'll need to be set up for a messier iterative tool — not the polished thing they may be expecting. Ryan noted they've been flexible and are comfortable testing work-in-progress builds.
- **Agentic AI timeline:** Still valid as the Phase 3–4 approach, but needs eval infrastructure before it can be trusted. Currently deferred.

---

## Context / Notes

- "Safin and Jimu" in transcript = Safin Ahmed + Almansur Siddiqui (now added to SLATE team.md). Both brought in mid-Q1 as AI-savvy engineers dedicated to this initiative.
- "Unicus" in transcript = Unisys (per prior transcript correction).
- Referenced doc: [Network Development Tool](https://docs.google.com/document/d/1B2yqc2azdMtz9s3JGXhrhn1Kv6QaqSf6eNnNr1FIVzo/edit?tab=t.0)
