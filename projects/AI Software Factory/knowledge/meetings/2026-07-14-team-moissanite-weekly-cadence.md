# Team Moissanite: Weekly Cadence — 2026-07-14

**Date & Time:** Tuesday, July 14, 2026 (duration not specified in transcript)

**Attendees:**
- Joe Hoppesch — Principal Engineering Consultant, Moissanite co-lead
- Redwan Shahzada — Director of Engineering, Moissanite co-lead
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead
- Cory Muir — Sr. Director, Software Engineering, initiative owner (Chuck's manager)
- Asad Ali — Contractor, agent-hosting POC
- Vitalii Belinskyi — Contractor, ai-toolbox infrastructure
- Mykhailo Sakhnik — Contractor

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/10W-DmdvTr8tN_OJ_9scCUM9kJJqQQ4Dp2c1qEkG7-C4/edit)

---

## Overview

The meeting formalized the embed-first pivot decided at the July 13 H2 Plan Review into two concrete Q3/Q4 objectives. Redwan opened by noting the team needs to rethink how it drives product velocity and measurable Q3/Q4 impact — moving away from infrastructure-first — and Chuck confirmed objectives/milestones were already being revamped for finalization with the broader group Thursday. (The transcript attributes this opening to "the aftermath of the recent FEX presentation" — this is a mistranscription; it refers to the July 13 H2 Plan Review.)

Objective 1 is the "embedded team model": closing repository readiness gaps (skills, context, tooling hygiene) via a repeatable playbook, piloted on one repo by mid-August and scaled to the 10 most-committed repos by end of Q3, measured with a repo scorecard (PR cycle times, AI-vs-human code ratio, first-time-right rate). Quality/testing is explicitly excluded from this objective to avoid diluting focus and to keep tech-debt-timeline conversations with leadership separate. Objective 2 is long-running engineering agents, starting with Joe's existing OPCOP tool (production incident diagnosis via Datadog/logs) as the first case, moving it from local execution to a managed environment, with success measured by MTTR reduction and fewer manual investigations rather than just having an agent deployed.

Sonar integration strategy was set (net-new findings only, default-off in CI, tier-1 services first). Several open questions were surfaced without resolution: event-driven vs. continuous-loop architecture for agents, how to prioritize future agents beyond OPCOP, and centralized governance of MCP server credentials (Joe's concern). Administratively, Mykhailo will cover day-to-day execution during Chuck's two-week vacation starting Monday (July 20), with Joe backing up on blockers.

---

## Key Notes

- **Objective 1 — embedded team / repository readiness:** Replaces the earlier "build the AI software factory first" framing. Rationale: the factory won't succeed until individual repos are ready for AI-assisted development. Playbook proven on one repo by mid-August, then scaled to the 10 most-committed repos by end of Q3. Directly extends the 7/13 H2 Plan Review's embed-first pivot.

- **Two distinct measurement tools clarified:** Redwan and Chuck distinguished the "repo scorecard" (productivity metrics — PR cycle times, PR-cycles-vs-first-time-right, AI-vs-human code ratio, likely via DX and possibly custom tooling) from the "AI repo readiness score" (whether a repo has agent-relevant skills, docs, and tests in place). Redwan suggested adding a tech-debt visualization to help communicate progress to leadership.

- **Quality/testing formally excluded from Objective 1:** Redwan asked if repo readiness covers missing tests. Chuck and Joe said no — testing is a separate initiative — to avoid "muddying the waters" and to keep the scope of leadership conversations about long-standing tech debt separate from repo readiness.

- **Sonar rollout approach:** Focus on net-new findings in net-new PRs to avoid legacy-code noise; integrate into CI pipelines default-off, enabling per-repo starting with tier-1 services. Also exploring "Sonar Vortex" for custom rules and automated verification.

- **Objective 2 — long-running agents, OPCOP as first case:** Joe's existing OPCOP tool (diagnoses production incidents from data/log analysis, currently run locally) is the initial target — Q3 goal is deploying it in a managed environment for continuous monitoring and alerting. The team wants a generic, reusable reference architecture (not a one-off build) so the same infrastructure supports future agents (e.g., flaky-test solvers, triage tools). Success is defined by MTTR reduction, faster diagnosis, and fewer manual investigations — not simply having an agent running.

- **Open architecture question — event-driven vs. continuous loop:** Mykhailo asked whether long-running agents should be event-driven or run continuous loops; Chuck said this is unresolved and expects the architecture to be refined during the build.

- **MCP credential governance flagged:** Joe raised the need to centrally govern MCP server usage/credentials rather than having individual users generate disparate tokens — not yet addressed, just flagged.

- **Agent framework gap:** Joe noted OPCOP is built on proper agent frameworks, but most other internal agents exist as simple markdown files — future long-running agents will need to be built in real frameworks to run as deployed services.

- **Future agent prioritization undecided:** Asad asked how agents beyond OPCOP will get prioritized. Chuck and Joe said it'll come down to feasibility, engineering-hours-saved, and incident reduction; ideally platform engineering would own this but current bandwidth doesn't allow it.

- **Prior POC work still feeds Objective 2:** Asad asked how earlier research/POC work carries forward given the narrower repo-readiness-first scope. Chuck confirmed it still feeds Objective 2 (long-running agents).

- **Culture shift on trusting AI output:** Joe emphasized training engineers on existing tools (CLI, agent skills) and breaking habits like manual code review, sharing his own experience learning to trust AI output as key to velocity gains.

- **Administrative handoff:** Mykhailo Sakhnik takes over day-to-day execution and technical oversight during Chuck's two-week vacation starting Monday (July 20), with Joe available for blockers.

---

## Action Items

- **Chuck Haines:** Create an implementation plan — detailed Q3 execution roadmap in Linear.
- **Chuck Haines:** Develop the repo playbook — validate on a single repository first.
- **Joe Hoppesch:** Create a candidate list of long-running engineering agents for internal discussion/prioritization.
- **Joe Hoppesch:** Train the engineering team on current AI coding tools and agent skills.
- **Chuck Haines:** Onboard Sonar into repo CI pipelines; work with the Sonar team on rule sets.
- **Chuck Haines:** Update the project plan with today's feedback and share via #moissanite-eng.
- **Chuck Haines:** Update milestones to include the verification agent in the Objective 1 roadmap.
- **Chuck Haines, Joe Hoppesch:** Update Sonar documentation — concise, measurable key results for Travis (Emslander) to review.
- **Chuck Haines, Joe Hoppesch:** Convert project documentation into a pre-read Google Doc for team review before Thursday's meeting.
