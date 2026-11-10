# Quick Connect on AI Software Factory Blueprint — 2026-04-10

**Date & Time:** Thursday, April 10, 2026, 10:30 AM CDT (~31 minutes)

**Attendees:**
- Joe Hoppesch — Principal Engineering Consultant (co-led discussion)
- Redwan Shahzada — Director of Engineering, Bangladesh team
- Chuck Haines — Sr. Principal Software Engineer / Architecture Lead (joined late)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1BlwL6nYGdApBYipPNovB2dvkj09Iap7qYIJKY7pwNfk/edit)

---

## Overview

The team met to align on two key things ahead of the Tuesday planning meeting with Cory: the problem statement for the AI Software Factory initiative, and what the first concrete deliverable should be. Joe and Redwan had been discussing foundational standards before Chuck joined, and the three worked through some productive disagreements to reach alignment.

The biggest clarifying moment was around problem framing. Redwan initially proposed positioning the problem as "teams working in silos building their own AI factories." Chuck pushed back — the real problem to solve isn't organizational coordination, it's accelerating engineering delivery by automating tasks that would otherwise distract engineers or never get prioritized (bugs, VOCCs, etc.). Joe agreed the factory scope should stay focused, while noting the silo problem is real but a separate leadership conversation. The group converged on this framing.

On scope, the team agreed the core factory should focus on the "Linear to done" workflow: take a bug from Linear, produce a PR ready for human review and merge. Everything upstream of Linear — Salesforce intake, Datadog log monitoring, OPCop alerts — would be handled by specialized intake agents built by domain teams, feeding into Linear in a standard format. Chuck explicitly invoked the open/closed principle: the factory is closed (defined process) but open for expansion (multiple intake agents). The group aligned on this architecture and started divvying up slide deck responsibilities for Tuesday.

---

## Key Notes

- **Problem Statement Debate:** Redwan proposed framing around teams working in silos building bespoke AI factories. Chuck disagreed — the core problem is accelerating engineering delivery by automating low-priority work (bugs, VOCCs) that would otherwise never reach an engineer's plate. The factory isn't primarily about organizational coordination; it's about throughput. Joe agreed this is the right framing for the factory scope specifically, while acknowledging the silo problem exists as a separate leadership concern.

- **"Linear to Done" as Core Scope:** The team converged on the factory's core responsibility being the path from a Linear ticket to a merged PR. The first milestone is even narrower: take a bug in Linear (with root cause analysis and repo info provided) and produce a PR for human review. This may be scoped to a single repository initially. The system expands by gradually removing the information it needs to be told (e.g., teaching it to determine root cause and target repo on its own).

- **Open/Closed Architecture for Intake:** Chuck proposed an open/closed principle approach — the factory itself has a defined, closed process, but it's open for expansion through intake agents. Examples discussed: a Salesforce triage agent (Arfin's team), OPCop feeding detected issues into the factory, Datadog log monitoring agents. Each intake agent is responsible for getting tickets into Linear in the expected format.

- **Standards Scope Clarification:** The group initially debated whether defining standards for individual developer machines was in scope. Chuck said no — he doesn't care what tools people use locally. Joe initially disagreed but then aligned: standards for the factory itself (repo setup, tooling harness) are in scope; standards for individual dev machines are a separate conversation. Chuck noted that the harness they build for AI will likely benefit local dev as a byproduct.

- **Communication Strategy:** Chuck proposed posting an engineering-wide announcement (engine news or similar) stating that a dedicated team is working on the AI software factory plan, to reduce duplicated effort across the org. The message would invite collaboration once a plan is ready in ~2 weeks, while signaling that people shouldn't go off building their own solutions.

- **OPCop Integration Mention:** Joe noted that OPCop's current flaw is it tries to do everything end-to-end and isn't good at all of it. The vision is for OPCop to focus on detection and feed into the factory for resolution. Side note: Andrew confirmed that OPCop solved the April 6 outage issue he'd been working on since Monday — a concrete early win.

- **Chuck's Multimodal Teaser:** Chuck mentioned he has "fundamental concerns about what we're defining as our agents" and doesn't think multimodal is out of scope for the initial implementation. He didn't have time to elaborate (headed to an interview) but said he'd share more in his vision artifact.

---

## Action Items

- **The group:** Post an engineering news update announcing that a dedicated team is working on the AI software factory plan, to prevent duplicated effort across the org
- **Chuck Haines:** Draft a visual showing the overall AI software factory model/vision — aims to send later today (April 10) or over the weekend
- **Joe Hoppesch:** Create 1-2 slides covering the problem statement and factory purpose, using Cory's existing documentation
- **Redwan Shahzada:** Draft slides focused on objectives and scope, starting Monday (April 13)
- **Joe Hoppesch:** Share the slide deck link in the #ai-software-factory channel
- **Joe Hoppesch (implied):** Schedule a Monday morning meeting to finalize the objectives slide before the Tuesday planning meeting with Cory
