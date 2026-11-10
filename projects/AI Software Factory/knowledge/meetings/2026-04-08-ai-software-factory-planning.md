# AI Software Factory // Planning — 2026-04-08

**Date & Time:** Tuesday, April 8, 2026, 7:00 AM CDT (~57 minutes)

**Attendees:**
- Cory Muir — Sr. Director, Software Engineering (meeting organizer)
- Chuck Haines — Sr. Principal Software Engineer
- Joe Hoppesch — Principal Engineering Consultant
- Redwan Shahzada (Redwan) — Director of Engineering

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/17pdYVUjMz969mhgPKYDBjyyzOrGsrP9njp3VFZ-aspc/edit?tab=t.v0g07qa0zr8k)

---

## Overview

The team held their first focused planning session for the AI Software Factory initiative. Cory framed the goal: establish 1-2 directional objectives (not overly prescriptive) that give the team a north star to move toward. The group converged on **reducing developer toil** as the primary target — starting with automated bug fixes in FN Core — and discussed the architectural scaffolding needed to make that work reliably across engineering.

The conversation covered three major themes: choosing the right initial target and scoping it tightly, designing a multi-agent pipeline architecture from day one, and solving the centralized knowledge/standards problem that underpins all of it. A recurring thread was frustration that engineers aren't broadly sharing their AI work or shifting their development mindset, and the group agreed that operationalization and enablement — not just building cool agents — is what will make this initiative succeed.

---

## Key Notes

- **Initial target: Bug fixes in FN Core.** Chuck proposed starting with what engineers hate doing — bug fixes, VOCCs, OPCOP-type work. The group agreed to scope narrowly to FN Core first (hardest repo, forces proper setup) and expand to other repos once patterns are proven. Joe noted the triage-to-void problem is a real business pain point this addresses.

- **Multi-agent pipeline from the start.** Chuck strongly advocated for a multi-agent architecture even in phase one (simplified version is fine). Proposed structure: Opus for planning → human review gate (via Linear) → orchestration layer → parallel coding agents → multiple review agents (standards, security, etc.) → PR submission. Cory backed this, saying a single-agent approach would be "a huge mess." Joe initially pushed back but came around when Chuck reframed it as simplified multi-agent, not full complexity.

- **Copilot code review inconsistency is a known blocker.** Chuck and Mike have observed that Copilot code reviews inconsistently pull in additional repo files despite instructions. This needs investigation as part of the initiative — reliable automated code review is critical for when humans can't review all generated code.

- **Centralized knowledge management is unsolved.** Multiple teams are independently creating `teams.md` files, facts files, and context docs with no coordination. Joe's "facts file" pattern works locally but doesn't scale. Cory pushed for a single source of truth — could be a knowledge graph tool, a repo, or a memory mechanism. Chuck distinguished between business knowledge and engineering knowledge as potentially needing different solutions. Starting point could be as simple as a shared markdown repo bootstrapped onto agent machines.

- **Skills need to be treated like microservices.** Joe emphasized that skills should do one thing well, be reusable, and composable (parent skills referencing child skills). He's seeing people hardcode logic (e.g., 1Password credential fetching) instead of referencing shared skills, leading to inconsistent and unreliable behavior. The group wants a "skills factory" that enforces standards and a skills catalog with daily update notifications.

- **RFN triage pilot: 80% accuracy, 6 hours → 6 minutes.** Redwan shared that RFN's agentic AI pilot for bug triage compared 20+ cases against human engineers — 80% accuracy, with one case cutting a 6-hour debug session to 6 minutes. Chuck was frustrated this wasn't shared broadly and pushed for a TDM presentation and broader visibility.

- **Mindset shift is the real challenge.** Joe observed that most engineers still have VS Code open writing code manually instead of using orchestration tools. During a recent outage, no one thought to use Claude or OPCOP for log analysis — they reverted to old methods. The group agreed that enablement, sharing (e.g., an "AI did" Slack channel), and possibly incentives (monthly prizes) are needed to drive adoption.

- **Operationalization over one-off demos.** Cory repeatedly stressed that the objective isn't "build an agent that does a thing" — OPCOP already does things. The objective is making agent workflows reliable, repeatable, and usable across all of engineering. Packaging, delivery, naming conventions, and contribution models all need to be defined.

- **Teams must own and contribute, not just consume.** Cory warned that if teams treat the AI factory like a platform team ("it's broken, fix it"), the solution will degrade. Teams need to contribute context, update tooling, and tweak agent behavior in their domain. The standards process needs to be lighter and more community-driven than the current guild approach.

---

## Action Items

- **Cory:** Meet with leadership group (Dennis, Shane, Travis) to communicate the plan and get budget approval.
- **Redwan + Joe:** Draft 1-2 focused objectives targeting developer toil, framed as a thin slice through a reference architecture. Avoid overly specific KRs like "fix X VOCCs" — focus on operationalizing a workflow.
- **Redwan:** Coordinate with Rafin to schedule a Friday TDM presentation on the RFN agentic triage pilot.
- **Chuck (implied):** Document the multi-agent pipeline architecture proposal (from the spitball in the meeting) as a starting point for discussion with the team and incoming contractors.
