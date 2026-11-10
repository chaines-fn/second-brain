# AI-DLC Q3 OKR Review — 2026-07-15

*Calendar invite was titled "AI Software Factory — Demo" (organized by Redwan Shahzada), but Chuck repurposed the meeting: instead of a demo, this was a re-review of the reproposed Q3 AIDLC OKRs coming out of the July 13 H2 planning session.*

**Date & Time:** Wednesday, July 15, 2026, CDT (~44 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer; Architecture Lead, sole lead of Workstream 1 (AI Software Factory / Embed Model). Repurposed the meeting from a planned demo into an OKR review.
- Cory Muir — Sr. Director, Software Engineering; initiative owner
- Joe Hoppesch — Principal Engineering Consultant; leads Workstream 2 (AI-DLC Process Transformation)
- Redwan Shahzada — Director of Engineering; organized the meeting invite. Note: per workspace records he rolled off the AI-DLC project as of 2026-07-14, but he attended and spoke briefly here.
- Travis Emslander — CTO
- Shane Kinder — EVP Product
- Ben Moormeier — Director of Information Security
- Stephen Wald — Product Manager (invited; no captured speaking lines)
- Kelley Manuel — role not on file, please confirm title
- Lisa Sacquitne — role not on file, invited with no captured speaking lines, please confirm title
- Shadab Ahmed — role not on file, invited with no captured speaking lines, please confirm title

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1smD8DtCtyG9zLrtR4yfH-X79IvUUFYDBc9JDEDMviEM/edit)

---

## Overview

What was scheduled as a demo of prior work became a walkthrough of reproposed Q3 OKRs for AI-DLC, following the July 13 H2 planning session. Chuck framed two objectives: first, increase engineering velocity through the AIDLC process by building a repo scorecard (DX metrics such as PR throughput, cycle time, and first-time-write rate) and a repeatable "repo readiness" playbook applied through an embed model, where the team parachutes into specific engineering teams to make their repositories more AI-native. The goal is the top 10 most-committed-to repositories fully AI-enabled by end of Q3, with embeds providing both quantitative metrics and qualitative engineer feedback on whether the changes actually help.

Several threads pushed back productively on the plan. Ben Moormeier confirmed the scope excludes fully enabling Claude Code app rollout per repo (that stays agent-agnostic, supporting both Copilot and Claude Code) but flagged code review as the real scaling bottleneck: without a way to trust a change is low-risk enough to merge without human review, none of this scales. Chuck agreed and framed quality as two-sided: feed-forward mechanisms (Sonar, skills, automated tests) to catch issues before code review, plus the review itself, citing Field Nation's own API design skill and DDL skill as proof this already works. Travis separately worried the scorecard exercise could become the actual work rather than a means to find and fix real friction, which Chuck addressed by clarifying the embed model (working directly with teams) is the mechanism, and the scorecard is only the before/after measurement layer.

The most concrete outcome was on code review tooling: the group agreed to move away from open-ended tool choice there specifically, evaluating Claude Code review against Sonar's code review offering ("Gitar," transcribed inconsistently) against current Copilot review, targeting a decision in weeks rather than a quarter. Cory's data point that PRs wait a 36-hour average from "ready to merge" to actual merge was cited as the core motivating number. On broader tooling, the group settled on staying standards-based and tool-agnostic to preserve flexibility, even though Chuck personally leans toward avoiding same-vendor authorship-and-review (arguing cross-vendor review catches more) and floated Copilot as the safer long-term bet against single-vendor lock-in. Team selection for the embed model was left open; Shane proposed a separate working session this week to align choices with H2 strategic priorities. The meeting ran a few minutes over and the group agreed to reconvene the next day to cover the second OKR (Objective 2).

---

## Key Notes

- **H2 OKR reproposal:** Chuck, Cory, and Joe reworked the AIDLC Q3 OKRs after the July 13 planning session, shifting the primary framing to engineering velocity gains as the prerequisite for a working software factory model, rather than building the factory pipeline first.

- **Repo scorecard and DX metrics:** The team will use DX platform metrics (PR throughput, cycle time, first-time-write rate) to establish a before/after baseline per repository. Cory stressed this is necessary to avoid the effort looking anecdotal to leadership; it was one of the direct asks coming out of Monday's H2 review.

- **Embed model is the actual mechanism, scorecard is just the measurement:** In response to Travis's concern that scorecards could become the work itself, Chuck clarified: the embed model (working inside a team to find and fix real friction) is how the work gets done, the scorecard is only how progress gets proven.

- **Code review named as the critical bottleneck:** Ben Moormeier pushed hard that a code review framework or methodology is missing and needs to land this quarter or next, given the team currently has no way to be confident a change is safe to merge without a human. Chuck agreed but noted feed-forward mechanisms (Sonar, skills, unit tests) are just as important as the review step itself, and are already proven out via FN's own API design and DDL skills.

- **Code review tool evaluation:** The team will compare current Copilot code review, Claude Code review, and Sonar's review tool ("Gitar" per the transcript, name uncertain) rather than build anything custom. Chuck was explicit this should be a few weeks of work, not a quarter-long project. Copilot review was called out as currently required on all PRs but not delivering the desired quality.

- **Vendor neutrality debate on broader tooling:** Travis and Joe pushed for picking a standard tool to avoid inconsistent onboarding ("wild west"). Chuck cautioned against locking into a single AI vendor given how fast the space moves, and noted using a different vendor for review than for authorship has shown better results than a model reviewing its own code. The group's landing point: stay standards-based so switching tools stays cheap, treat code review specifically as the one area worth being prescriptive about.

- **36-hour PR wait time:** Cory cited internal data showing a roughly 36-hour average (possibly P75) wait from "PR ready to merge" to actual merge, used as the concrete number motivating the whole initiative.

- **Team selection for the embed model still undecided:** Shane suggested aligning the choice with H2 strategic priorities and proposed a separate working session this week. Cory flagged API work and data platform work as repetitive, high-leverage candidates. Kelley Manuel raised the open question of whether to pick a team for immediate velocity impact versus one that yields the most generalizable learnings; Chuck's view was to lean toward immediate impact even if that slows the learnings. Travis pushed back that any learnings need to generalize across repos, not be specific to one team's setup.

- **Decisions reached (per meeting record):** Adoption of the team embed model as the primary velocity strategy; continued commitment to tool-agnostic, standards-based development over prescribing a single AI coding assistant vendor.

---

## Action Items

- **Ben Moormeier:** Connect Chuck with a Dell contact who is using spec-driven development methodologies.
- **The group:** Carve out time this week to select embed-model target teams and repositories, aligned with H2 priorities.
- **Chuck Haines, Cory Muir:** Evaluate code review tools (Claude Code review, Sonar's review tool, current Copilot review) and propose the most effective option for the workflow.
- **The group:** Reconvene the next day (2026-07-16) to continue the OKR review, covering Objective 2.
- **The group:** Define clear success metrics and estimated magnitude of impact before further build-out (raised by Shane Kinder).
- **The group (implied):** Update project standards documentation to formally require standards-based development to preserve tool flexibility.
