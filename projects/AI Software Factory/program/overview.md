# AI-DLC — Field Nation Overview

*Rebranded 2026-07-09 from "AI Software Factory" (see [Rebrand to AI-DLC](#rebrand-to-ai-dlc-2026-07-09) below). AI-DLC is the umbrella initiative with two workstreams: **AI Software Factory** (technical build) and **AI-DLC (Process Transformation)**. Most content below predates the rebrand and refers to the initiative by its former names ("AI Software Factory," "AI SDLC") — left as-is since it's a historical record.*

**Document source:** "AI Operating Models - Eng" (Google Doc, owned by Cory Muir)
**Last synced:** 2026-11-09 (automated daily project-sync; 11/09: work-order-management added to the factory (MOI-229 Done, MOI-231 In Review), factory simplification MOI-243/244 Done, Warp order form received 11/04 and Chuck's redlines sent 11/06 (outcome not recorded), no Warp session outcome in Slack, no Team Moissanite standup transcript since 10/30, see [Factory Queue Updates 11/06 to 11/09](#factory-queue-updates-2026-11-06-to-11-09); 11/05: no Team Moissanite standup transcript or calendar entry (series still cancelled), so no meeting note; new factory work MOI-213 (route ONYX team's Linear agent sessions to the Engineering factory) Done, MOI-224/226/227/228 Done or in progress, MOI-225 PR-description skill In Progress, see [Factory Queue Updates 11/04 to 11/05](#factory-queue-updates-2026-11-04-to-11-05); Chuck called a Warp meeting for 11/05 (outcome not visible yet); 11/04: no Team Moissanite standup transcript (series cancelled 11/02, nothing in Drive or on today's calendar); MOI-208 Canceled, MOI-209 Done, MOI-211/212/214 Done, MOI-213 In Review, MOI-215 In Progress; unverified Warp security report, see [Factory Queue Updates](#factory-queue-updates-and-warp-security-report-2026-11-03-to-11-04); 11/03: factory work now runs through Warp in #warp-engineering-factory, MOI-207 moved to Warp, MOI-206 Done, MOI-208 to MOI-210 added, see [Factory Work Moves into Warp](#factory-work-moves-into-warp-moi-206-to-moi-210-2026-11-02-to-11-03); no 11/03 standup transcript exists since the series was cancelled 11/02; 11/02: Team Moissanite standup series cancelled by Chuck after the DST shift, Mykhailo moving to new product work; Q4 factory backlog MOI-194 to MOI-202 and Warp POC direction, see [Warp POC Direction and Q4 Factory Backlog](#warp-poc-direction-q4-factory-backlog-and-standup-series-cancelled-2026-10-30-to-11-02); 10/30: Warp Factories POC kicked off 10/29 and its first ticket ran through the Warp factory (per Warp's recap in #ext-fieldnation-warp; follow-up working session Wed 11/04 3pm CT); MOI-192 (sync Engineering Factory skills from ai-toolbox default-branch changes) Done and MOI-193 (resolve skill conflicts) Canceled 10/30; new Backlog tickets MOI-194 (Wiz MCP in the factory) and MOI-195 (agents acting on Gitar, Sonar and Wiz PR findings) under Chuck in "Fai: Factory Q4 2026"; Cory routing a feature request from Mari through the Factory; MOI-185 OpenTofu design decided 10/30 (GCP only, Harness IaCM); the 10/30 standup has no Gemini summary and its transcript ends at 00:04:49, so no meeting content beyond attendance, see [tech-context](../knowledge/tech-context.md#warp-factories-poc-kicked-off-2026-10-29); 10/29: Harness Evals cut-over closed in Linear (MOI-145 and MOI-151 Done 10:42 UTC, MOI-150 Canceled, MOI-165 Done, MOI-182/183 Done); MOI-191 pilot to move fn-ddl-design into a fn-engineering-standards plugin In Progress (Chuck, since 10/28), see [tech-context](../knowledge/tech-context.md#standards-ship-with-their-skills-moi-191-pilot-2026-10-28); no Team Moissanite standup transcript in Drive at 09:06 CDT, though the 08:15 to 08:30 CDT standup is on the calendar, so no meeting note yet and the run should be repeated once Gemini notes land; Mykhailo's Slack status download is due Thursday 10/29 before he logs off Friday 10/30; 10/28: 10/28: no Team Moissanite standup on the calendar or in Drive today, so no meeting note written; MOI-106 (publish FN skill-creator skill in ai-toolbox, Repo-Readiness Playbook KR2) completed 13:12 UTC; MOI-181 (repo-scanner datetime fix, Mykhailo) updated 10/27; calendar shows a "Claude Skill Workshopping" call at 1pm CDT with Radhika Pandya, Erik Rutt and Craig Kasel about broader team concerns over the Claude skill (title only, no detail, lower signal); 10/27: standup (08:15 CDT, Gemini notes captured) covered the Docker harness build, core UI as the starting repo with the Factory kept on a branch, a new workspace-wide "AI software factory" label, and Mykhailo's transition to the parl team on 11/02 (see knowledge/meetings/2026-10-27-team-moissanite-daily-standup.md); 10/26: today's Team Moissanite standup transcript (Drive, 08:17 CDT) has no usable content: Gemini produced no summary and the transcript ends at 00:01:12, so no meeting note was written; MOI-187 opened 10/26 for a shared Factory Docker execution environment (linux/arm64, Python 3.14, Node) as the AgentCore Harness default, see [tech-context](../knowledge/tech-context.md#fai-mvp-signals-2026-10-24-to-10-26); Warp PoC kickoff planned today, outcome unknown; no outcome found for the Fri 10/23 demo to Cory; 10/23: no Team Moissanite standup transcript in Drive and no standup on the calendar today, so no new meeting note; calendar shows a Workato MCP POC alignment call (1pm CDT) and an ITMethods "Reign Factory + AI Gateway" next-steps call (2pm CDT) today, outcomes unknown; MOI-183 created 10/23 to point the eval pipeline at Chuck's new Harness AWS connector, see [tech-context](../knowledge/tech-context.md#fai-mvp-calendar-signals-and-harness-connector-2026-10-23); 10/22: standup synthesized, [2026-10-22 note](../knowledge/meetings/2026-10-22-team-moissanite-daily-standup.md): planner UI demo (approve gate, step-function trace, token metrics panel with a suspected output-token bug), core UI monorepo chosen as Factory test ground, Sonar MCP integration proposed, LiteLLM supply-chain concern raised (advisory link pending), pre-Friday goals set (MCP integration, one core UI ticket, cost-per-run baseline) for a demo to Cory on 10/23, Warp PoC starts Mon 10/26, Mykhailo's customer-team move now around 11/2 for 1-2 months (earlier note said week of 10/19); MOI-182 (AgentCore tracing, X-Ray IAM gap) opened 10/22, lower signal; 10/21: MOI-167 and MOI-168 (AgentCore Harness and event-driven HITL spikes) marked Done 10/21 at about 5:53am CDT, outcome write-up not captured in the Linear ticket bodies, see [tech-context](../knowledge/tech-context.md#agentcore-harness-spikes-moi-167--moi-168-2026-10-13); new Mykhailo tickets MOI-180 (mypy in repo-readiness, Backlog) and MOI-181 (repo-scanner datetime fix, In Progress) created 10/21, lower signal; no 10/21 standup transcript found in Drive as of this run, so no new meeting note; 10/20: M2 (MOI-173) and M2.1 (MOI-176) Done, M3 quality gates milestone (MOI-174) Canceled with no recorded reason, see [tech-context](../knowledge/tech-context.md#fai-mvp-scope-update-m2-and-m21-done-m3-canceled-2026-10-20); no 10/20 standup transcript found, so no new meeting note; 10/19: standup synthesized, 10/19: standup synthesized, [2026-10-19 note](../knowledge/meetings/2026-10-19-team-moissanite-daily-standup.md): GitHub App permissions update in progress, gateway evaluation (Workato) and open-router model-selection idea raised, no scope change; 10/16 standup now synthesized, [2026-10-16 note](../knowledge/meetings/2026-10-16-team-moissanite-daily-standup.md): live end-to-end Fai MVP demo on a real ticket produced a draft PR, GitHub App replaces personal tokens, and Mykhailo is reassigned full-time to a customer-facing agentic project for 1-2 months starting week of 10/19 (impact on MOI-169 M2 to M3 unassessed); 10/16: no new standup transcript (10/13 is still the latest and is already synthesized; no standup was held 10/14 and none is on the calendar for 10/15; today's standup is rescheduled to 11:30am CDT); MOI-169 milestone tickets M0-M3 created 10/14, with M0 webhook ingress, M1 HITL workflow and M1.1 React approval UI Done and M2 AgentCore execution, M2.1 repo agentification and M3 quality gates Todo, see [tech-context](../knowledge/tech-context.md#fai-mvp-milestones-and-infra-benchmark-2026-10-16); Chuck is using Warp as the infra benchmark for the AgentCore path, Warp call 10/16 outcome unknown; 10/14: MOI-169 opened as the Fai event-driven MVP build ticket (AWS Step Functions orchestration, AgentCore Harness execution, human approval gate, automated PR with CI/Gitar/Sonar gates), with Mykhailo's first PR open at [fieldnation/ai-software-factory#1](https://github.com/fieldnation/ai-software-factory/pull/1); no standup today (next 10/15); see [tech-context](../knowledge/tech-context.md#fai-mvp-build-moi-169-2026-10-14); 10/13 standup has real content: Mykhailo's AgentCore Harness spikes MOI-167 (autonomous coding in the harness sandbox, first real repo fix) and MOI-168 (event-driven HITL on AgentCore), see [2026-10-13 note](../knowledge/meetings/2026-10-13-team-moissanite-daily-standup.md) and [tech-context](../knowledge/tech-context.md#agentcore-harness-spikes-moi-167--moi-168-2026-10-13); Chuck scheduled Warp Factories (infra alternative to compare with AgentCore, call 10/16) and an AWS expert call (10/14); prior: Harness eval pipeline progress 10/09 to 10/12: Harness eval pipeline progress 10/09 to 10/12: MOI-149 and MOI-164 Done, DeepEval-to-Harness-Evals cut-over in review (MOI-151); no new standup transcript; Chuck scaffolded the `fieldnation/ai-software-factory` monorepo for Fai on 10/08; 10/09 standup transcript has no usable speech, see [2026-10-09 note](../knowledge/meetings/2026-10-09-team-moissanite-daily-standup.md); MOI-149 still awaiting Bedrock IAM access) standup transcript (series cancelled 11/02, nothing in Drive or on today's calendar); MOI-208 Canceled, MOI-209 Done, MOI-211/212/214 Done, MOI-213 In Review, MOI-215 In Progress; unverified Warp security report, see [Factory Queue Updates](#factory-queue-updates-and-warp-security-report-2026-11-03-to-11-04); 11/03: factory work now runs through Warp in #warp-engineering-factory, MOI-207 moved to Warp, MOI-206 Done, MOI-208 to MOI-210 added, see [Factory Work Moves into Warp](#factory-work-moves-into-warp-moi-206-to-moi-210-2026-11-02-to-11-03); no 11/03 standup transcript exists since the series was cancelled 11/02; 11/02: Team Moissanite standup series cancelled by Chuck after the DST shift, Mykhailo moving to new product work; Q4 factory backlog MOI-194 to MOI-202 and Warp POC direction, see [Warp POC Direction and Q4 Factory Backlog](#warp-poc-direction-q4-factory-backlog-and-standup-series-cancelled-2026-10-30-to-11-02); 10/30: Warp Factories POC kicked off 10/29 and its first ticket ran through the Warp factory (per Warp's recap in #ext-fieldnation-warp; follow-up working session Wed 11/04 3pm CT); MOI-192 (sync Engineering Factory skills from ai-toolbox default-branch changes) Done and MOI-193 (resolve skill conflicts) Canceled 10/30; new Backlog tickets MOI-194 (Wiz MCP in the factory) and MOI-195 (agents acting on Gitar, Sonar and Wiz PR findings) under Chuck in "Fai: Factory Q4 2026"; Cory routing a feature request from Mari through the Factory; MOI-185 OpenTofu design decided 10/30 (GCP only, Harness IaCM); the 10/30 standup has no Gemini summary and its transcript ends at 00:04:49, so no meeting content beyond attendance, see [tech-context](../knowledge/tech-context.md#warp-factories-poc-kicked-off-2026-10-29); 10/29: Harness Evals cut-over closed in Linear (MOI-145 and MOI-151 Done 10:42 UTC, MOI-150 Canceled, MOI-165 Done, MOI-182/183 Done); MOI-191 pilot to move fn-ddl-design into a fn-engineering-standards plugin In Progress (Chuck, since 10/28), see [tech-context](../knowledge/tech-context.md#standards-ship-with-their-skills-moi-191-pilot-2026-10-28); no Team Moissanite standup transcript in Drive at 09:06 CDT, though the 08:15 to 08:30 CDT standup is on the calendar, so no meeting note yet and the run should be repeated once Gemini notes land; Mykhailo's Slack status download is due Thursday 10/29 before he logs off Friday 10/30; 10/28: 10/28: no Team Moissanite standup on the calendar or in Drive today, so no meeting note written; MOI-106 (publish FN skill-creator skill in ai-toolbox, Repo-Readiness Playbook KR2) completed 13:12 UTC; MOI-181 (repo-scanner datetime fix, Mykhailo) updated 10/27; calendar shows a "Claude Skill Workshopping" call at 1pm CDT with Radhika Pandya, Erik Rutt and Craig Kasel about broader team concerns over the Claude skill (title only, no detail, lower signal); 10/27: standup (08:15 CDT, Gemini notes captured) covered the Docker harness build, core UI as the starting repo with the Factory kept on a branch, a new workspace-wide "AI software factory" label, and Mykhailo's transition to the parl team on 11/02 (see knowledge/meetings/2026-10-27-team-moissanite-daily-standup.md); 10/26: today's Team Moissanite standup transcript (Drive, 08:17 CDT) has no usable content: Gemini produced no summary and the transcript ends at 00:01:12, so no meeting note was written; MOI-187 opened 10/26 for a shared Factory Docker execution environment (linux/arm64, Python 3.14, Node) as the AgentCore Harness default, see [tech-context](../knowledge/tech-context.md#fai-mvp-signals-2026-10-24-to-10-26); Warp PoC kickoff planned today, outcome unknown; no outcome found for the Fri 10/23 demo to Cory; 10/23: no Team Moissanite standup transcript in Drive and no standup on the calendar today, so no new meeting note; calendar shows a Workato MCP POC alignment call (1pm CDT) and an ITMethods "Reign Factory + AI Gateway" next-steps call (2pm CDT) today, outcomes unknown; MOI-183 created 10/23 to point the eval pipeline at Chuck's new Harness AWS connector, see [tech-context](../knowledge/tech-context.md#fai-mvp-calendar-signals-and-harness-connector-2026-10-23); 10/22: standup synthesized, [2026-10-22 note](../knowledge/meetings/2026-10-22-team-moissanite-daily-standup.md): planner UI demo (approve gate, step-function trace, token metrics panel with a suspected output-token bug), core UI monorepo chosen as Factory test ground, Sonar MCP integration proposed, LiteLLM supply-chain concern raised (advisory link pending), pre-Friday goals set (MCP integration, one core UI ticket, cost-per-run baseline) for a demo to Cory on 10/23, Warp PoC starts Mon 10/26, Mykhailo's customer-team move now around 11/2 for 1-2 months (earlier note said week of 10/19); MOI-182 (AgentCore tracing, X-Ray IAM gap) opened 10/22, lower signal; 10/21: MOI-167 and MOI-168 (AgentCore Harness and event-driven HITL spikes) marked Done 10/21 at about 5:53am CDT, outcome write-up not captured in the Linear ticket bodies, see [tech-context](../knowledge/tech-context.md#agentcore-harness-spikes-moi-167--moi-168-2026-10-13); new Mykhailo tickets MOI-180 (mypy in repo-readiness, Backlog) and MOI-181 (repo-scanner datetime fix, In Progress) created 10/21, lower signal; no 10/21 standup transcript found in Drive as of this run, so no new meeting note; 10/20: M2 (MOI-173) and M2.1 (MOI-176) Done, M3 quality gates milestone (MOI-174) Canceled with no recorded reason, see [tech-context](../knowledge/tech-context.md#fai-mvp-scope-update-m2-and-m21-done-m3-canceled-2026-10-20); no 10/20 standup transcript found, so no new meeting note; 10/19: standup synthesized, 10/19: standup synthesized, [2026-10-19 note](../knowledge/meetings/2026-10-19-team-moissanite-daily-standup.md): GitHub App permissions update in progress, gateway evaluation (Workato) and open-router model-selection idea raised, no scope change; 10/16 standup now synthesized, [2026-10-16 note](../knowledge/meetings/2026-10-16-team-moissanite-daily-standup.md): live end-to-end Fai MVP demo on a real ticket produced a draft PR, GitHub App replaces personal tokens, and Mykhailo is reassigned full-time to a customer-facing agentic project for 1-2 months starting week of 10/19 (impact on MOI-169 M2 to M3 unassessed); 10/16: no new standup transcript (10/13 is still the latest and is already synthesized; no standup was held 10/14 and none is on the calendar for 10/15; today's standup is rescheduled to 11:30am CDT); MOI-169 milestone tickets M0-M3 created 10/14, with M0 webhook ingress, M1 HITL workflow and M1.1 React approval UI Done and M2 AgentCore execution, M2.1 repo agentification and M3 quality gates Todo, see [tech-context](../knowledge/tech-context.md#fai-mvp-milestones-and-infra-benchmark-2026-10-16); Chuck is using Warp as the infra benchmark for the AgentCore path, Warp call 10/16 outcome unknown; 10/14: MOI-169 opened as the Fai event-driven MVP build ticket (AWS Step Functions orchestration, AgentCore Harness execution, human approval gate, automated PR with CI/Gitar/Sonar gates), with Mykhailo's first PR open at [fieldnation/ai-software-factory#1](https://github.com/fieldnation/ai-software-factory/pull/1); no standup today (next 10/15); see [tech-context](../knowledge/tech-context.md#fai-mvp-build-moi-169-2026-10-14); 10/13 standup has real content: Mykhailo's AgentCore Harness spikes MOI-167 (autonomous coding in the harness sandbox, first real repo fix) and MOI-168 (event-driven HITL on AgentCore), see [2026-10-13 note](../knowledge/meetings/2026-10-13-team-moissanite-daily-standup.md) and [tech-context](../knowledge/tech-context.md#agentcore-harness-spikes-moi-167--moi-168-2026-10-13); Chuck scheduled Warp Factories (infra alternative to compare with AgentCore, call 10/16) and an AWS expert call (10/14); prior: Harness eval pipeline progress 10/09 to 10/12: Harness eval pipeline progress 10/09 to 10/12: MOI-149 and MOI-164 Done, DeepEval-to-Harness-Evals cut-over in review (MOI-151); no new standup transcript; Chuck scaffolded the `fieldnation/ai-software-factory` monorepo for Fai on 10/08; 10/09 standup transcript has no usable speech, see [2026-10-09 note](../knowledge/meetings/2026-10-09-team-moissanite-daily-standup.md); MOI-149 still awaiting Bedrock IAM access)

### Warp POC Direction, Q4 Factory Backlog, and Standup Series Cancelled (2026-10-30 to 11-02)

- **Chuck's stated direction: the 4-week Warp POC "likely becomes the factory for FN."** In a 10/30 DM to Tashfiqul Islam he described the long-term goal as the full SDLC flowing through the factory (product uses it to ideate and prototype, engineering uses it to build), with a Workato-POC MCP that knows bounded contexts and entity topology. This is Chuck's working read, not a recorded leadership decision. The 10/13 to 10/16 note that no Warp-vs-AgentCore decision is recorded still stands until one is written down. The Warp working session is Wed 11/04, 3:00pm CT.
- **Warp Slack app approved 10/30, restricted.** Travis Emslander approved it on Ben Moormeier's condition: one test channel only, no use in other channels, and any app or bot creation needs separate review. Ben flagged that Warp's SOC 2 is not yet a full year.
- **Q4 factory backlog filled out 10/30 to 11/02 under "Fai: Factory Q4 2026."** Done: MOI-194 (Wiz MCP, 10/30) and MOI-195 (agents that act on Gitar, Sonar and Wiz PR findings, 11/02). In Backlog: MOI-196 (move MCP servers off Chuck's OAuth to a service account), MOI-198 (dedicated AWS account for factory), MOI-199 (custom runners for PHP, Node and Go), MOI-200 (resolve skill discrepancies between factory and ai-toolbox plus fn-engineering-standards, unassigned), MOI-201 (add fn-engineering-standards skills with auto-sync). In Progress: MOI-197 (product team and factory for discovery, PRD and prototyping, using Matt Pocock skills). In Review: MOI-202 (CODEOWNERS requiring @chaines-fn review on the factory repo, 11/02).
- **Factory repo naming is unresolved.** MOI-202 and Chuck's 10/30 DM name `fieldnation/warp-engineering-factory`. The sources do not say how it relates to `fieldnation/ai-software-factory` (the Fai monorepo).
- **Team Moissanite Daily Standup series cancelled 11/02.** Chuck cancelled the recurring series after the DST shift moved it an hour earlier, expecting Mykhailo to move to another team. Mykhailo replied that he has already started the new product work (Spencer shared project details) and would skip. The 10/30 standup is the last instance. Its Gemini transcript has no usable content, so there is no meeting note for 10/30 and none for 11/02.

---

### Factory Queue Updates (2026-11-06 to 11-09)

- **work-order-management joins the factory.** MOI-229 (add the repo to factory repositories) is Done as of 11/05. MOI-231 (AI-enable `work-order-management`, Chuck) is In Review. A warp-factories bot PR, `work-order-management` #140 (agent guidance and Sonar integration), was posted ready for review in #sapphire-bots on 11/09.
- **Factory simplification landed.** MOI-243 (simplify blockers so unverifiable bookkeeping does not stop PR and retry work, High, Chuck) and MOI-244 (audit and simplify factory agents, rules, and automations) are Done. MOI-235 (remove fieldnation-php routing guidance, audit stop and retry rules) is Done. In #ext-fieldnation-warp on 11/09, the factory was described as "humming along" after a round of removing overly restrictive rules. MOI-225 (PR-description skill) is still In Progress and unassigned.
- **Warp commercial terms in progress.** Warp sent its order form and legal terms to Cory on 11/04. Field Nation sent proposed redlines to Warp on 11/06, mainly on performance commitments and material changes that could affect usage. No outcome recorded. Ben's 10/30 condition still applies (Warp's SOC 2 is not yet a full year).
- **Warp direction still unresolved in Slack.** No decision or summary from the 11/04 working session appears in Slack or Drive. A Warp onboarding notes doc (11/05) exists in Drive but was not parsed this run. The Warp-as-factory direction remains Chuck's working read.
- **Security incident 8098 (lower signal for this workstream, involvement unverified).** #secincident_8098 discussion on 11/07 and 11/08 concerns access through Warp tunnel egress. MOI-245 (SOPS re-encrypt and secret rotation for team-owned files, Triage) sits under an "Incident 8098 Remediation" project. The ticket notes the team's feature-ownership page lists zero features, so whether Moissanite owns any affected files is unknown.
- **Team Moissanite standup.** The last Drive transcript is 10/30 (no usable content). The series was cancelled 11/02 and none was found for 11/03 through 11/09, so no meeting note was written.

### Factory Queue Updates (2026-11-04 to 11-05)

- **Factory now serves the ONYX team.** MOI-213 (route the ONYX team's Linear agent sessions to the Engineering factory, Chuck) is Done as of 11/05. This extends the factory beyond Moissanite and Engineering.
- **Linear team filter removed, access gated by Warp accounts.** MOI-222 (Done 11/04) removed the Linear team filter from Engineering factory agent sessions. Chuck's reasoning in #warp-engineering-factory: any user needs a Warp account, which is sufficient gating.
- **Clean-approval gate rework.** MOI-220 and MOI-223 (Done 11/04) exempt verified merge commits and accept a verified clean Gitar approval in the PR gates. This is the replacement path after MOI-208 was Canceled 11/03.
- **Notifications and PR quality.** MOI-224 (Done 11/04) opens a Slack DM thread for web-started Factory tasks. MOI-225 (In Progress) adds a PR-description skill for agent-authored PRs, built on fn-write-clearly and the repo PR template.
- **PR bot receipt reader fixes.** MOI-226 (Done), MOI-227 (Done) and MOI-228 (Done 11/05) fixed a blocker where the duplicate-receipt check could not read `get_conversation` pages and rejected unknown record types. Operational, lower signal.
- **Warp meetings.** The 11/04 3pm CT Warp working session took place; no outcome has surfaced in Slack. Chuck asked #proj-ui-modernization members to join a Warp meeting on 11/05 at 9:04am CST, so Warp is still active this week. The Warp security report remains unverified (see below).
- **Team Moissanite standup.** No transcript in Drive after 11/02, and no standup on the calendar 11/04 through 11/06. No meeting note written.

---

### Factory Queue Updates and Warp Security Report (2026-11-03 to 11-04)

- **MOI-208 Canceled 11/03, no reason recorded.** It was the bounded clean-results gate on the Implement handoff (CI, Sonar, Gitar, Wiz). Review-routing work continues under MOI-214 (Done 11/04, check-run timestamps GitHub does not expose) and MOI-215 (In Progress 11/04, consolidate PR review automations into one bot/human handler set).
- **MOI-209 Done 11/03.** Foreman starts from app mentions in #warp-engineering-factory. This supersedes the In Progress status in the section below.
- **Other Chuck-owned factory tickets.** MOI-211 (add user-sync-external to factory) and MOI-212 (require a ticket ID in factory PR titles, commits, and branches) Done 11/03. MOI-213 (route the ONYX team's Linear agent sessions to the Engineering factory) In Review. MOI-199 (custom PHP, Node and Go runners) In Progress. MOI-203 (MCP server for bounded contexts and entity topology) and MOI-204 (Company Second Brain) Todo.
- **Chuck's direction, 11/04 DM to Cory (not a decision).** He wants the factory to interrogate old code, produce a spec and tickets to migrate it to core-ui, then do the migration work.
- **Warp security report, unverified.** At 02:02 CST on 11/04, Gautam Korlam (Sonar, Gitar team) DM'd Chuck a public post suggesting Warp may have had a security breach. Chuck replied that it has not affected FN so far and that he is watching it. No vendor confirmation was found in Slack. Relevant to today's Warp working session and to the Warp-as-factory direction.
- **Business Systems backlog grew 11/04 (Vitalii).** MOI-216 (service account that can create app projects and link billing), MOI-217 (Okta or Google for app access groups), MOI-218 (limit deploy identity sign-in to the app's own Harness project) and MOI-219 (custom domains) were created in Backlog. MOI-185 (OpenTofu through Harness IaCM) is In Progress and MOI-142 (require Okta sign-in) is Done. These look like Business Systems Phase II work, not an AI-DLC OKR item.
- **Calendar, 11/04.** "Field Nation / Warp Factories working session" at 3pm CT (Killian Jackson organizing; Cory and Chuck accepted, Ben Moormeier and Ian McConchie invited). Outcome not yet known. A 10:00am CT "Chuck Haines / Marko - Let's talk" call with SuperPlane is also on the calendar; the vendor is not otherwise in this workspace, so no context was found.
- **Q4 PEX OKR Tracker exists.** The "PEX OKR Tracker | Q4 2026" sheet is linked from the Tuesday owner-update reminder (11/03), which lists Chuck as an owner. No Q4 update from Chuck was found.
- **Not searched this run.** Gmail (the connector is not connected) and Atlassian (needs authorization). Slack covered 11/02 onward; Linear covered MOI updates from the last three days.

---

### Factory Work Moves into Warp; MOI-206 to MOI-210 (2026-11-02 to 11-03)

- **MOI-207 moved to Warp.** "Make fn_ete_playwright more agentic friendly" (audit of the `fn_ete_playwright` repo, qz-main branch) was reassigned from Chuck to Warp on 11/02. The Factory posted its start to #warp-engineering-factory at 17:56 CST, and Warp commented on the ticket repeatedly through the evening.
- **MOI-206 Done (11/02).** Adds an agent to the Factory that audits a repo and makes it agentic-ready. It is the agent MOI-207 runs.
- **MOI-208 In Progress (11/02).** Gates the Implement agent's handoff on a bounded clean-results check across CI, Sonar, Gitar and Wiz: a 30-minute budget, at most three fix pushes, and human-judgment items treated as blockers, never as clean.
- **MOI-209 In Progress (Chuck, 11/02 to 11/03).** Slack attention notifications for Factory tasks. Retitled 11/03 to "Start Foreman from app mentions in the factory Slack channel." Chuck's steer on 11/03: keep the default Slack behavior (no overrides) and add a trigger so that mentioning the Foreman in #warp-engineering-factory starts it. The first attention alert posted 11/03 08:36 CST, asking Chuck to review warp-engineering-factory PR #20 on GitHub.
- **MOI-210 Todo (11/03).** Web discovery agent that logs into test environments as the test-account users, clicks through the UI, and documents how it works, for scoping agent work and QA.
- **Team visibility.** Chuck set #warp-engineering-factory as the work channel on 11/02 (adjusted posting permissions), and Cory Muir joined it the same day.
- **Not verified this run.** The Linear/Jira search for MOI tickets timed out, so these states come from Slack bot notifications. MOI-203 to MOI-205 were not seen. The 11/03 Team Moissanite standup has no transcript in Drive because the series was cancelled 11/02.

---

## Q2 North Star

**One complete workflow running end-to-end: Linear ticket → reviewed PR.**

That's the concrete Q2 success definition. As of 2026-04-08, the team has converged on a more specific initial target:

**Initial target: Automated bug fixes in FN Core.** The team chose developer toil (bug fixes, VOCCs) as the first domain because (a) engineers don't like doing it, making buy-in easier, and (b) it's a concrete business problem — triaged bugs currently "go into a void." FN Core was chosen as the starting repo because it's the hardest (PHP monolith, custom framework) — solving it there forces proper agentic setup that can be applied to easier repos afterward.

**Approach: Multi-agent pipeline from day one.** The team agreed that a single-agent approach would fail. Even in phase one, the architecture will be multi-agent (simplified): planning agent (Opus) → human review gate → orchestration layer → parallel coding agents → multiple review agents (standards, security, etc.) → PR submission. This will be refined iteratively — the team expects early versions to work inconsistently and plans to continuously fix "the harness."

**Key concepts emerging:**
- **Skills Factory** — A tool/process that builds skills according to defined standards, replacing Confluence pages that go stale. Skills should be microservice-like: do one thing well, be composable and reusable.
- **Skills Catalog** — A centralized registry of available skills with notifications when new skills are published or updated. Daily visibility into skill creation is a success criterion.
- **Operationalization over demos** — The objective is not "build an agent that does a thing" but making agent workflows reliable, repeatable, and usable across all of engineering.

**Primary risk Chuck is managing:** Quality and humans-in-the-loop. Making sure the right review gates exist so agents don't "go off the rails" producing bad output that slips through. This is the architectural problem, not just a process concern.

### Q2 OKR Scoping (as of 2026-04-13)

Blueprint v0.3 (drafted by Chuck) includes comprehensive OKRs. Team aligned on right-sizing:

- **Objectives 0 + 1: Committed** — Foundations. These are the real Q2 deliverables.
- **Objective 2 (Implement + Review pipeline): Stretch** — Only if Phase 0+1 go smoothly.
- **Objectives 3+: Deferred to Q3** — Intake integrations, full review pipeline, engineering-wide rollout.

Cory's guidance: prove the concept first, then scale. One engineer approving one AI-generated plan on one real ticket is a meaningful Q2 win.

### Architecture Direction (as of 2026-04-10)

Chuck's position: use existing tools (Claude, etc.) and build a harness/orchestration layer on top — not a bespoke end-to-end solution. Analogy: "building your own datacenter instead of using AWS is silly unless you're Microsoft." The team needs a **code name for the harness** to avoid confusion with Harness (the CI/CD product Field Nation already uses). Joe also flagged the need for a **product name for ROE**.

### Dual-Value Framing & Deliverables Direction (as of 2026-04-15)

Key thread in #ai-software-factory: Cory reviewed the presentation with Redwan and asked for prioritization examples beyond just agent work. Chuck emphasized that the initiative's deliverables serve both the factory harness AND engineers using AI tools locally — skills, context files, specs, and standards that work in both contexts. One investment, two delivery channels.

**Tooling decisions under consideration:**
- **APM (Microsoft Agent Package Manager)** as the standard for packaging and distributing skills, context files, specs, and review configs. Ian is already working with APM. Adopting it unifies distribution across factory and local use.
- **SDDF (Spencer Moulton's Spec-Driven Development Framework)** as the spec-driven development standard. Maps directly onto the PEV Loop — the "Plan" phase is the spec. Industry-validated pattern (Thoughtworks, GitHub Spec Kit, AWS Kiro).

**Full deliverables breakdown:** see Tier 2 (Shared Library) framing in the architecture working doc — [artifacts/2026-05-01-moissanite-architecture-working-doc.md](../artifacts/2026-05-01-moissanite-architecture-working-doc.md)

### Broader Stakeholder Alignment (Q2 NNL Plan Review, 2026-04-16)

Chuck, Joe, and Redwan walked the broader stakeholder group — Cory (eng leadership), Ben Moormeier (Director of InfoSec), Sandro Perez (InfoSec Manager), Shane Kinder (EVP Product), Stephen Wald (PM), Ann-Bettina Colace / ABC (Product Design Lead), plus others — through the Q2 plan. Key outcomes:

- **Scope reaffirmed as internal tool.** Explicitly distinct from the Pearl team's customer-facing agentic work. Ben flagged an inconsistency in the deck between "not a platform" and references to the Field Nation platform — Chuck and Joe clarified the line: this is engineering enablement for toil reduction, not customer feature acceleration.
- **Phase 1 concrete target: single-repo bug, Linear → PR via PEV loop.** Two human-in-the-loop gates: plan approval and PR review. Chuck: "agents executing, humans authorizing." Rejections at each gate are the primary learning signal for harness improvement.
- **Target repo pivot from FN Core to a Go/Node.js microservice.** Reasoning: removes UI testing complexity and keeps scope provable. FN Core remains the long-term target but is not the Phase 1 starting point. Notifications floated as a candidate; integrations mentioned but has cross-repo sprawl.
- **Devil's advocate confirmed as non-negotiable.** Stephen pushed back on whether it was premature — Chuck defended with industry research (90% of outcome quality comes from plan quality; multimodal adversarial review improves first-time-right results). Martin Fowler's feed-forward vs. feedback frame cited as the design mental model.
- **Observability is a hard platform requirement.** Full lifecycle telemetry (OpenTelemetry-style) plus a strong eval framework. Will gate platform selection.
- **Platform decision deferred to end of April.** Options on the table: AWS Bedrock / Agent Core, Azure, Claude managed agents, Langmith deep agents, third-party managed options. Strong preference for off-the-shelf over bespoke. Ian to be involved in the spikes.
- **Human-in-the-loop removal is a future problem to solve deliberately.** Ben planted the seed; Chuck has ideas from industry direction and will discuss offline. The goal is a safe path to removing gates over time, not keeping them forever.
- **Immediate next steps:** Redwan to schedule a RACI meeting to lock in R/A/C/I (not a 20-person committee); Chuck to select a specific backlog bug ticket now so the group has a concrete "this is what done looks like" reference.

**Full synthesis:** [knowledge/meetings/2026-04-16-q2-26-nnl-plan-review.md](../knowledge/meetings/2026-04-16-q2-26-nnl-plan-review.md)

### Team Naming and Channel Rename (2026-04-30)

The team officially became **Moissanite**. The Slack channel `#ai-software-factory` was renamed to `#moissanite-eng` (same channel ID `C0AQW1KRQ20`). Linear team prefix is **MOI-**. The "harness code name" question Joe raised on 4/10 is resolved by the team name itself.

### Current Architecture State (v0.5, 2026-05-03)

The architecture has converged into a clean model after several iterations. **Full working doc:** [artifacts/2026-05-01-moissanite-architecture-working-doc.md](../artifacts/2026-05-01-moissanite-architecture-working-doc.md). **Diagrams:** [artifacts/Architecture - High Level.drawio](../artifacts/Architecture%20-%20High%20Level.drawio) and [artifacts/Architecture - Detailed.drawio](../artifacts/Architecture%20-%20Detailed.drawio).

The shape: three things sit on top of a shared Platform — a knowledge plane (what agents know), a PEV pipeline (Plan → Execute → Verify, with human gates), and a verification plane (how we trust what gets produced). Birgitta Böckeler's vocabulary from her harness-engineering article is adopted: *guides* (feedforward) and *sensors* (feedback), each *computational* or *inferential*.

**Platform layer (shared infrastructure for the entire PEV loop):**
- **Brain** — model + harness (LLM-serving + general agent infrastructure: auth, retries, observability, tool dispatch). Three spikes locked in at the 2026-05-05 weekly cadence: (1) **Cursor SDK on AWS** (Chuck) — likely EKS or equivalent lightweight container hosting; (2) **Azure AI Foundry out-of-the-box** (Joe) — full Microsoft agent framework; (3) **Cursor SDK on Azure AI Foundry** (Chuck + Joe) — hybrid, Foundry as runtime/infra with Cursor's harness ergonomics. **AWS Bedrock AgentCore was dropped** from the active spike set — no compelling AWS-native offering and weak signal even from AWS-side engineers.
- **Hands** — sandbox + tools (Daytona / Modal / self-hosted, sandbox per agent run with token isolation and no production access).
- **Session** — durable event log so a new sandbox can pick up from a failure.
- **Observability** — lifecycle telemetry; cost-per-ticket as the primary metric.

This is a refinement of Anthropic's brain/hands/session model. Rather than treating the brain as one thing combining model+harness+orchestrator, we split it: the Platform Brain is shared infrastructure; each PEV stage has its own **Orchestrator** (a stage-specific workflow controller riding on the Platform Brain). Plan, Execute, and Verify all use the same Brain; what differs is the workflow each Orchestrator runs.

**Knowledge plane (3 tiers):**
- **Tier 1 · Repo-Local** — AGENTS.md plus repo-specific skills and agents. Specific to one repo only, not shared. agentrc generates first drafts.
- **Tier 2 · Shared Library** — skills, agents, and MCP servers shared across repos via APM / ai-toolbox. Includes non-negotiables (security standards, compliance constraints, forbidden patterns) as a transitive dependency every repo consumes. Org-wide guardrails are just non-negotiable Tier 2 skills. Default new skills repo-local; promote to Tier 2 when proven across 2+ repos.
- **Tier 3 · Cross-cutting Context** — architectural decisions, EDA schemas, technical designs that span repos. Unblocked is the candidate.

**Verification plane (Q2 deliverable):**
- Multi-agent inferential review with computational sensors feeding it. Computational first (Build, Lint, Tests, Sonar, Static Analysis) — fail returns to coding agent immediately, no AI review burned. Inferential second: Standards Review, Security Review, Database Review, Architecture Fitness, Code Review (generic catch-all, ensures code accuracy), AC Verification. Human PR gate at the end.
- The Code Review sensor is intentionally generic — not specifically adversarial. The earlier "adversarial review" framing was reframed as just "code review" in v0.5.
- **Sonar specifically:** SonarSweep is for foundation-model companies cleaning training data and is the wrong product. SonarQube Advanced Security plus the Sonar MCP Server is the right Sonar fit for our verifier.

**Operational flow (PEV pipeline):**
- **Plan** — multi-agent draft (Plan Agent + Plan Review Agent + Devil's Advocate, in debate). Human Plan Gate approves. Approved plan becomes the **Spec** (durable, versioned, sandbox-restartable execution contract).
- **Execute** — Sub Agents (coding agents) implement against the Spec, in sandboxes. Spec is durable so a new sandbox can re-fetch on failure.
- **Verify** — sensors review the PR diff (computational then inferential). Human PR Gate approves the merge. Failures route back to the Execute Orchestrator (failed / P0-P2 findings / rejected) which decides whether to retry, escalate to a fresh fix agent, or hand back to a human.

**Steering loop (Q3+ concern, called out for awareness):** rejected reviews → root cause analysis → fix the harness. The fix can be anything: a skill update, a new sensor, a prompt change, an agent tweak, a knowledge update. Intentionally generic — not just an APM publish.

**Eval framework:** kept as a Q2 deliverable but intentionally not visualized in the architecture diagram — it's more an implementation concern than an architectural component. Synthetic PRs with known-good and known-bad findings, scored over time on TP/FP rates. Tooling, corpus ownership, scoring TBD.

### Weekly Cadence Outcomes (2026-05-05)

First Moissanite weekly cadence locked several Q2 decisions:

- **Buy over build is now a stated Q2 strategic principle.** Chuck: "buy whatever we can off the shelf — that way six months from now when it's no longer the right thing, we just buy the next thing and migrate it." Goes on Redwan's deck. Maps directly to the existing "harness on top of existing tools, not bespoke end-to-end" architectural direction.
- **Sonar locked in as the deterministic verification standard.** Sonar provides the deterministic "does this meet our standards" check that AI-reviewing-AI cannot. Sonar's existing knowledge base also feeds forward as basic good-engineering-practices context. Sonar does **not** host agents — they only provide an MCP server, so it fits cleanly as a computational sensor without affecting the harness/infra question.
- **Harness vs. infrastructure is the operating mental model.** Chuck named the split explicitly: the *harness* is how agents connect, orchestrate, and execute (Cursor SDK, Copilot, Claude Code); the *infrastructure* is what they run on (AWS, Azure, Cursor managed cloud). Azure AI Foundry tries to bundle both; Cursor SDK is more clearly just a harness. The deciding factor for picking one suite vs. mixing: how much per-agent sandbox / tool-scope / token-cost control each managed cloud actually exposes.
- **Unblocked deprioritized for Q2.** Not on the critical path. Eventually relevant when the system becomes multi-repo and architecture-aware. Chuck will start a conversation with Unblock with the explicit "not Q2" caveat.
- **Contractor focus = AI toolbox in a real repo, regardless of harness/infra outcome.** Independent of the spike result. Joe owns target-repo selection (Provider Match or Integrations are the candidates; FN Core is **not** back on the table — the 4/16 NNL pivot to a Go/Node.js microservice for Phase 1 still stands).

**Full synthesis:** [knowledge/meetings/2026-05-05-team-moissanite-weekly-cadence-1.md](../knowledge/meetings/2026-05-05-team-moissanite-weekly-cadence-1.md)

### Weekly Cadence Outcomes (2026-06-23)

- **M3 milestone (harness selection) pushed to July 6, 2026.** June 23 was the previous extended deadline. New date is caveated on the WWT launch completing successfully so Chuck can refocus. WWT bandwidth crunch (Andrew and Maria both out) is the primary cause of the slip. Chuck acknowledged being the bottleneck.
- **Two contractors now active on the team:** Vitalii Belinskyi (ai-toolbox bundle system — completed; FN Core onboarding next) and Asad Ali (agent-hosting POC — AWS Agent Core POC deployed with guardrails, orchestration, and observability benchmarking; Gemini/ADK POC on Vertex AI Agent Engine in progress). Joe provisioning Linear API key for Asad so he can build the harness-to-Linear pipeline.
- **Sonar and Unblocked both showing early positive signal.** Unblocked is already answering FN-specific architecture and business logic questions from code + Confluence. Sonar is actively reviewing PRs. **Cory raised Glean and Guru as Unblocked alternatives** — both are similar tools and Cory wants due diligence before committing. Chuck leading evaluation calls (Sonar check-in + Guru call both on June 24).
- **Deep Agents confirmed private beta — dropped.** Joe confirmed waitlist-only; team consensus to move on. Active platform candidates: AWS Agent Core (Anthropic Managed Agents), Gemini/ADK (Vertex AI Agent Engine), Azure AI Foundry.
- **Process change:** Engineering blockers should surface in #moissanite-eng channel rather than via 1:1 messages, so the team can unblock in parallel.
- **Asad published AWS vs. GCP comparative analysis** to Confluence: https://fieldnation.atlassian.net/wiki/x/EACvZgE

**Full synthesis:** [knowledge/meetings/2026-06-23-team-moissanite-weekly-cadence.md](../knowledge/meetings/2026-06-23-team-moissanite-weekly-cadence.md) *(not yet created)*

---

### Sonar POC Concluded; Unblocked Dropped in Favor of Guru (2026-06-30 – 2026-07-01)

**Sonar 2-week POC (MOI-44) is done — validated successful trial.** Chuck to Redwan (2026-07-01): "the Sonar POC is done, things went well and next step is working with Cory on budget and implementation plan." Sonar (Jack Kolis) confirmed the same on 2026-06-30 in #fieldnation-sonar and flagged that Sonar is releasing **"Sonar Vortex"** — Context Augmentation & Agentic Analysis as a new, lower-cost module (separate from full SonarQube licensing) — with a commitment to hold overall pricing steady. Sonar also offered a premium **Launchpad onboarding program** (~60 hours: maturity assessment, developer-friction workshops, quality-gate setup) at no extra cost if finalized before late July. **Next step:** The Sonar sync is now confirmed for **Tuesday, July 7, 2026, 11:00–11:30am CDT**. Chuck still owns pushing the OKR/budget update to Cory. _(updated 2026-07-03)_

**Unblocked is being dropped — reverses the 2026-06-02 re-prioritization.** Chuck to Mike Mueller (2026-06-30): "we are likely not going to go with unblocked because Cory had been looking at Guru for enterprise wide and after talking with them it does everything unblocked does and more. We are in the process of getting a POC set up." **Chuck and Redwan are now taking over the Guru pilot from Cory** — the mutual NDA was signed as of 2026-06-30 (confirmed with Eamon Walsh), clearing the way for the Guru POC to start. This directly supersedes the Cory-directed "Glean/Guru pressure-testing" framing from the 2026-06-23 update — Guru is now the active alternative being piloted, not just evaluated.

---

### Weekly Cadence Outcomes (2026-07-07)

- **Sonar: trial lapsed, POC successful, moving to contract negotiation.** The trial period itself has ended, but the POC was validated as successful (consistent with the 6/30–7/1 conclusion above). Next step is contract negotiation; actual implementation/rollout is targeted to start **August 1, 2026**. Today's confirmed Sonar sync (11:00–11:30am CDT) is presumably where this was discussed, though the outcome wasn't independently confirmed beyond the weekly cadence notes.
- **Mykhailo Sakhnik onboarded.** Welcomed by Redwan. Has Linear board access; GitHub repo access still pending. No workstream assigned yet.
- **"Harness POC" = Asad's existing agent-hosting POC**, not a new/separate candidate. It remains the lead demo candidate for the upcoming leadership demo, pending the AWS-vs-GCP platform decision (i.e., AWS Agent Core vs. GCP Vertex Agent Engine, per Asad's existing comparative analysis).
- **Possible "AI DLC" rebrand — open, unresolved question.** Notes mention "AI Software Factory rebranding to AI DLC to align with enterprise strategic planning." It's unclear whether AI Software Factory becomes part of a broader AI-DLC workstream or is being renamed outright. Not a settled decision — flag for Chuck to confirm with Cory. **Resolved 2026-07-08** — see below: it's "AI SDLC," and the Factory is a deliverable within it, not a rename. *(Superseded again 2026-07-09 — see [Rebrand to AI-DLC](#rebrand-to-ai-dlc-2026-07-09) below: the name lands back on "AI-DLC" after all, per Chuck's direction.)*
- **Q3 punch list still to be defined.** Group wants to align on Q3 objectives (agent-first SDLC transformation) before looping in leadership; Chuck suggested a Cory/Joe alignment session first.

**Full synthesis:** [knowledge/meetings/2026-07-07-team-moissanite-weekly-cadence.md](../knowledge/meetings/2026-07-07-team-moissanite-weekly-cadence.md)

**Alignment session held (2026-07-08):** Redwan's "AI SF → AI SDLC: H2 vision and Q3 plans" session happened as scheduled (Cory, Chuck, Joe, Redwan). Confirmed naming: **"AI SDLC,"** not "AI DLC." Full outcomes below and in the [meeting synthesis](../knowledge/meetings/2026-07-08-ai-sdlc-h2-vision-q3-plans.md).

---

### Weekly Cadence / Alignment Outcomes (2026-07-08): AI SF → AI SDLC H2 Vision

The Cory/Joe/Redwan alignment session Chuck called for on 7/7. Key outcomes (process/org side — see the section below for the technical architecture output of the same day):

- **"AI SDLC" confirmed, relationship to AI Software Factory clarified.** The Factory is a deliverable/tool *within* the broader AI SDLC process-transformation effort, not a rename of it. Two workstreams: (1) the Factory (technical build) and (2) AI SDLC (the process transformation it serves).
- **Northstar reframed by Cory:** compress delivery cadence from months/quarters to days/weeks. Explicitly not "AI writes all the code" (Travis's framing) — leadership only cares about velocity and features, not who/what produces the code.
- **Operational bottleneck reframed:** Cory estimates (not measured data) ~70% of engineering time goes to deciding/debating/documenting what to build, 20-30% to building, ~10% to polish. The bottleneck is planning/requirements churn, not engineering throughput or a lack of AI tooling.
- **"Vibe coding" guardrail:** reserved for high-risk/experimental work only, not standard production. The Factory's job is correctly-integrated, production-ready output the first time.
- **Pilot team direction shifted away from Breakfix** (too many stakeholders/opinions) toward **Sapphire or Basalt** — lower visibility, full product+engineering control. Diamond also mentioned as an option.
- **Resourcing gap named directly:** no one has been dedicated full-time this quarter; Chuck's Breakfix load has consumed his bandwidth. Cory wants at least one FTE 100% dedicated — likely Chuck pulled off Breakfix, backfilled short-term by a contractor until a new principal engineer is hired for that area.
- **Contractor norm set:** any question raised should come paired with a proposed recommendation, not just the question. Group generally satisfied with Asad, Vitalii, and Mykhailo.
- **New dedicated AWS account planned** (OpEx, not COGS) for AI SDLC production work, separate from the existing dev/sandbox account. Chuck and Joe to raise with Ben and Ian.
- **Milestones:** M1 — propose an updated SDLC process for Field Nation, circulate to leadership. M2 — identify and execute with a pilot team, iterate, aim to onboard more teams by end of quarter.
- **July 15 PEX leadership demo confirmed** — Linear ticket run through the harness, AWS vs. GCP evaluation as the metric (code generation only, no PR yet).

**Full synthesis:** [knowledge/meetings/2026-07-08-ai-sdlc-h2-vision-q3-plans.md](../knowledge/meetings/2026-07-08-ai-sdlc-h2-vision-q3-plans.md)

---

### Paper-Cuts Team Proposal & Pilot-Scope Counter-Proposal (2026-07-08, afternoon)

Shane raised standing up a dedicated **"Paper-Cuts" team** — a VOC-style tiger team that would jump into different parts of the platform fixing operational inefficiencies for the business.

**Chuck is against running this as a dedicated tiger team.** Concerns: (1) it bogs the tiger team down and pulls capacity away from teams' own product work, and (2) papercut-scoped problems often span multiple teams anyway, so a single standing team isn't the right shape for the work — it should go to whichever team(s) actually own the affected part of the platform.

**Cory's counter-proposal:** use the AI SDLC pilot as the vehicle instead of a standalone tiger team. Problems get brought in and solved *by the owning team(s)* through the AI SDLC pipeline, with **Tashfiq** as a dedicated intake PM running that front door (bringing in problems, scoping them, routing them to the right team). Framed benefits: lets the group take on "meaty but smaller-scoped" problems without materially impacting Prod Eng teams' capacity, while proving out AI SDLC on real, valuable work.

**Status:** idea stage — not yet discussed with Shane, Redwan, or Joe. Directly relevant to the open AI SDLC pilot-scope decision (see M2 / team selection above) — this could become the pilot mechanism instead of (or alongside) Sapphire/Basalt.

---

### Go-Forward Architecture Candidate (2026-07-08): LangGraph DeepAgents PEV on AWS — ARCHIVED 2026-07-14

> **Superseded for H2.** The detailed doc and implementation plan below were moved to `program/archive/` and delinked on 2026-07-14 — the 2026-07-13 H2 Plan Review deprioritized standing up this pipeline for H2 (see [Embed-First Pivot](#embed-first-pivot--pilot-scope-narrowing-2026-07-13)). Left here as a historical record of what was drafted 2026-07-08/09.

Chuck drafted a detailed architecture and delivery plan for the PEV loop built on **LangChain DeepAgents / LangGraph**, hosted on **AWS**, integrated with **Linear** (tickets and approvals) and **GitHub** (PRs and code review), with all models on **Amazon Bedrock** (in-boundary). At the time, this was the potential go-forward plan for the factory build, still under the open **M3 platform decision** (AWS Agent Core vs. GCP Vertex Agent Engine) and not yet ratified by the team.

**Diagram:** [AI Software Factory Architecture (Lucid)](https://lucid.app/lucidchart/9b39ee36-446e-4192-b4f5-bb98fd66ef28/edit) (still maintained; docs are archived, not this)

Shape of the plan:

- **Agents do the reasoning; a durable orchestrator owns control flow.** Fixed Plan, Execute, Verify loop with a hard retry cap and human gates the orchestrator enforces (not the LLM).
- **Orchestration matures in two stages:** LangGraph state machine for the POC, then port the orchestration tier to **Temporal** for production (durable multi-day human waits, retries, distributed workers).
- **Models per agent (all on Bedrock):** Plan = Opus; Plan Review + Devil's Advocate = OpenAI GPT-5.5; Execute = Sonnet orchestrating OpenAI **Codex** implementers; Verify = Sonnet with an Opus advisor for hard calls. OpenAI frontier models (GPT-5.5, Codex) are now GA on Bedrock, so this needs no new vendor relationship.
- **Runtime:** Bedrock AgentCore for the POC (managed runtime, memory-as-checkpointer, sandbox, identity, observability), which doubles as M3 evidence; move to ECS/EKS + Temporal for production.
- **Human gates are async:** the plan is posted to Linear and the run parks on a durable signal, resumed later by an inbound webhook (API Gateway + Lambda). Bounded plan-revision and verify loops; escalate via Linear comment.
- **Biggest lift:** an FN-Core-parity sandbox image so tests run in-sandbox.

This supersedes an earlier Managed-Agents alternative, now archived at [artifacts/archive/pev-loop-on-aws-agentcore-guide.md](../artifacts/archive/pev-loop-on-aws-agentcore-guide.md).

### Pre-Planning Validation Gate Proposed (2026-07-09)

Mykhailo Sakhnik reviewed the LangGraph PEV POC diagram and proposed adding a **pre-planning validation stage** between INTAKE and the Planner, task-type-specific: bug → reproducer-first (check must fail before the fix, pass after); feature → acceptance-test-first (checks may fail before implementation, pass after); chore → validation/safety-first (prove existing behavior stays green). Goal: the Planner receives a clear task contract — verification commands, assumptions, risks, and the expected validation artifact — before planning starts.

Chuck agreed with the direction. He drew the distinction from Asad's SDD framework evaluation (MOI-49): SDD is for *after* a ticket is accepted into the factory (ensuring the plan and documentation are in place for implementation); this new gate is *before* acceptance — screening whether a ticket even meets the bar to enter the factory at all (including rejecting tickets by type, e.g. non-bug tickets in the initial scope). Incorporated 2026-07-09 as the one-shot **PRE-VERIFY** node (Sonnet) between INTAKE and PLAN — builds a task contract (reproducer/acceptance/safety-first by ticket type) for the Planner, escalating on failure with no retry. Added to the Lucid diagram and the renamed "AI Software Factory - Architecture & Overview" doc (now archived, see note above). The ticket-type rejection gate itself is still deferred (not load-bearing while tickets are curated by hand) — tracked as an open item.

This is also Mykhailo's first substantive technical contribution to the architecture since joining 2026-07-06.

**Team review requested (2026-07-09):** Chuck posted the updated doc/diagram/implementation-plan revisions to #moissanite-eng and asked Joe and Redwan for feedback by **EOD 2026-07-10**, since it will likely inform how the team moves forward. Also 2026-07-09: Redwan asked Chuck to get Mykhailo a real ticket to work on and set up 1:1 time to discuss his architecture ideas further — Mykhailo agreed to write up questions/ideas as a Confluence page first. See [program/people.md](./people.md) for Mykhailo's status.

---

### Rebrand to AI-DLC (2026-07-09)

Chuck directed a further rebrand: the initiative is now **AI-DLC**, superseding the "AI SDLC" naming resolved just one day earlier (2026-07-08, see above). The two-workstream structure carries over unchanged in shape, just relabeled:

1. **AI Software Factory** — the technical build (the production PEV pipeline). Unchanged in scope. **As of 2026-07-14, Chuck is sole lead** (with contractors) — see leadership update below.
2. **AI-DLC (Process Transformation)** — the new workstream turning Field Nation's current SDLC into a more AI-native process. This is the same workstream previously referred to as "AI SDLC (Process Transformation)" in the 2026-07-08 alignment outcomes and the Q3 OKR draft — scope, team, and OKRs from that work still apply, just under the new name. **As of 2026-07-14, Joe leads this workstream** — see leadership update below.

**Not yet confirmed:** whether this rebrand has been discussed with or ratified by Cory, Joe, or Redwan — this note reflects Chuck's direction to update the workspace, not a documented team decision. Flag for confirmation with the team if that matters going forward.

---

### H2 Roadmap Drafting Underway; Harness POC PEV-Completeness Unconfirmed (2026-07-13)

**Redwan is drafting the H2/Q3 deliverables from the 7/8 alignment session (M1: propose an updated SDLC process, circulate to leadership).** New draft artifacts surfaced this week: **"AI-SDLC_H2_2026_Vision_and_Q3_Roadmap_DRAFT-v1.2"** (Google Slides), a companion **"Draft-AI-DLC - H2 2026 / PEX Roadmap"** spreadsheet, and a **"Draft-AI-DLC - PEX OKR Tracker - Q3 2026"** spreadsheet. A **"Catchup on H2 Roadmap for AI-DLC"** meeting (Redwan organizer; Joe, Chuck, Cory invited) ran today 8:30-9:00am CDT to review the draft. Per Joe's 7/9 comment on the deck, it wasn't obvious the term "AI-DLC" would land with a broader audience — Redwan added an explainer slide (#3) in response.

**Open risk, unresolved as of today: does the harness POC actually implement the full PEV loop, or just Plan?** Joe flagged in deck comments (7/9): "the current harness POC has P, but not EV unless it's changed since I looked at it." Chuck pointed to Asad's POC ([agent-hosting-poc, MOI-46, poc15_software_factory_e2e](https://github.com/fieldnation/agent-hosting-poc/tree/MOI-46/pocs/poc15_software_factory_e2e)) as having "everything," but Joe pushed back that the flow he'd seen only generates a plan and opens a PR — no visible execute/verify step. Redwan relayed that per his standup with Asad, the orchestration layer can fire any agent type (planner/executor/verifier), but this wasn't independently confirmed. Chuck said (7/9) he'd get it running himself "tomorrow" to check — but as of this morning (7/13) Redwan is still pinging him to confirm, so it's genuinely unresolved. **This matters because the July 15 PEX leadership demo (2 days out) depends on this POC** — the 7/8 alignment notes scoped the demo as "code generation only, no PR yet," but Joe's question suggests even that narrower scope may not be fully wired up. Worth Chuck confirming before 7/15.

**fn-event-design skill (MOI-48) architecture revised, pending Chuck's review.** Vitalii posted an update to [ai-toolbox PR #52](https://github.com/fieldnation/ai-toolbox/pull/52) proposing the skill split by **authority** rather than producer/consumer **role**: Home 1 (Contract Authority — the single source of truth for event contract shape, versioning, and reliability rules; language-agnostic; this is fn-event-design, already built) vs. Home 2 (Integration Skill — per-language wiring recipes: PHP producer outbox plumbing, Go/TS consumer retry/DLQ handling; not yet built, deferred as its own ticket). Reasoning: splitting by role would force the same contract to be copied into both a producer and consumer skill — the exact duplication Tier 2 skills are meant to eliminate. Chuck is reviewing as of this morning (2026-07-13). See [program/people.md](./people.md) and [knowledge/tech-context.md](../knowledge/tech-context.md).

---

### Leadership Change: Redwan Rolls Off, Workstream Leads Split (2026-07-14)

**Redwan Shahzada rolled off the AI-DLC project.** Chuck is now sole lead of **Workstream 1 (AI Software Factory / Embed Model)**, working with contractors. **Joe Hoppesch now leads Workstream 2 (AI-DLC Process Transformation)** — previously a shared/TBD area, now his primary focus. See [program/people.md](./people.md) for updated role notes and [artifacts/2026-07-14-q3-implementation-plan.md](../artifacts/2026-07-14-q3-implementation-plan.md) for the Q3 ticket-level breakdown reflecting this split. Historical mentions of Redwan co-leading or driving decisions elsewhere in this doc are left as-is — accurate records of what happened at the time, not current state.

---

### Resourcing Gap Resolved: New Principal Engineer Hire Confirmed (2026-07-15)

> **Superseded 2026-07-30 on two counts.** The start date is **August 3, 2026**, not August 17, and the hire (full name **Ehtasham Tamvir**) is **not** the Breakfix backfill: Redwan's 7/30 proposal sends him to Granite and routes **Atiqul Haque** onto Break/Fix instead, which Chuck has pushed back on. See [the 7/17 to 8/3 sync section](#vacation-window-sync-telemetry-workstream-lands-gitar-enters-as-code-review-candidate-tamvir-reassigned-2026-07-17--2026-08-03) and [program/people.md](./people.md#ehtasham-tamvir--principal-engineer-bangladesh-started-2026-08-03). The section below is left as a record of what was believed on 2026-07-15.

**Tamvir** (surname not yet in this workspace) confirmed as a new Principal Engineer hire, starting **August 17, 2026** — Redwan relayed the confirmed date to Chuck, Cory, and Joe via group DM. His current employer requires a one-month transition period before he can start. This directly addresses the 2026-07-08 resourcing gap noted above (Cory wanting an FTE 100% dedicated to AI-DLC, likely meaning Chuck gets pulled off Breakfix once backfilled). Chuck was a strong advocate for Tamvir during the hiring process (2026-07-08 panel thread with Andrew Kandels and Atiqul Haque) — panel consensus was a strong hire. **Not yet confirmed:** whether Tamvir's mandate on arrival is specifically the Breakfix backfill role implied by the 7/8 resourcing note, or a general Principal Engineer hire — worth confirming closer to his start date. See [program/people.md](./people.md#ehtasham-tamvir--principal-engineer-bangladesh-started-2026-08-03).

### Q3 OKR Tracker Reworked; Workstream Framing Refined (2026-07-15)

Chuck and Joe spent the morning of 2026-07-15 (DM thread, ahead of Chuck's TDR and a townhall presentation he's co-giving today) refining the Q3 OKR spreadsheet structure. Key outcomes:

- **New Workstream 1 Objective 2 (Q3, priority 2) drafted by Joe:** *"Create an end-to-end process for deploying and operating long-running agents to AgentCore."* KRs: (1) a standard workflow for engineers to deploy/operate long-running agents on the platform, (2) the platform enforces security/compliance/governance policies, (3) at least one long-running agent running and usable in production. Milestones: **M1 (July 31)** AgentCore architecture plan approved; **M2 (Aug 28)** IaC modules created and deployable; **M3 (Aug 28)** CD pipeline created in Harness (the CI/CD product, not the agent harness) for deploying an agent to the platform; **M4 (Sept 18)** demonstrate a repeatable end-to-end DLC of a long-running agent.
- **Workstream framing sharpened:** Joe's phrasing (Chuck agreed): **Workstream 1 = "the platform and long-running agents"**; **Workstream 2 = "AI-DLC, the process, how we measure and AI readiness."** This refines (doesn't replace) the existing Software Factory / Process Transformation split.
- **Joe flagged a structural mismatch:** Workstream 1's two objectives (the existing embed-first/skill-building Objective 1 vs. the new AgentCore/long-running-agents Objective 2) don't clearly support each other, whereas Workstream 1 Objective 1 *does* support Workstream 2 Objective 1. He proposed restructuring the tracker around that alignment and took the pen on updating the spreadsheet, since Chuck was jammed with TDR/townhall prep. Also floated merging the two workstreams' "priority 1" objectives so the AI-DLC process is what's used to execute a real ticket to done — not yet resolved, Chuck didn't fully confirm he understood the proposal.
- **Note/possible discrepancy — not yet reconciled:** In this same thread Chuck said "the second workstream (the AI-DLC) I think will mostly be me doing it," which reads as inconsistent with the 2026-07-14 leadership-split record (Joe leads Workstream 2, Chuck leads Workstream 1 — see [Leadership Change](#leadership-change-redwan-rolls-off-workstream-leads-split-2026-07-14) below). Could just mean Chuck offering to draft that workstream's spreadsheet content rather than a leadership change — flagging rather than overwriting the existing leadership record until confirmed.
- **Rationale for the Shane-facing framing (Joe):** Shane and the broader leadership audience don't care about platform mechanics — they care about repos being AI-ready, a way to measure that, and velocity improvement through today's process/tools. This is shaping how Workstream 2 gets pitched upward.

This is in-progress OKR-tracker drafting, not a finalized decision — check the actual "Draft-AI-DLC - PEX OKR Tracker - Q3 2026" spreadsheet (see [program/references.md](./references.md)) for the latest state next sync. **Superseded 2026-07-16:** the canonical, current H2 OKR doc is now [program/OKRs/h2-2026-okrs.md](./OKRs/h2-2026-okrs.md) — ground any discussion of H2 objectives/KRs/milestones there rather than this section or the spreadsheet.

---

### MOI-97 Resolved, Metrics Locked In With Leadership, Gitar Order Form Arrives (2026-08-21 → 08-24)

**MOI-97 (Copilot code review + marketplace skills) closed with a clean, evidence-backed answer (2026-08-24).** Asad's controlled A/B test in `core-ui` proved marketplace-distributed skills (shipped via MOI-47) do not reach Copilot's automated PR review — only repo-committed `.github/skills/` skills do. Root cause: Copilot code review runs service-side with no per-user client context, so it can only read repo-local customization, not user-scoped marketplace plugins. This doesn't pick a winner in the broader O1 code-review decision (Gitar/Tessl/`fn-review-agent`/native Copilot review/roborev all still open), but it does settle a structural question: **whichever tool wins, review-relevant skills need to be committed per repo.** It also has a knock-on implication for the MOI-47 marketplace-vs-vendored debate — Copilot's automated review is a second blind spot for marketplace distribution, beyond the already-known cloud-surfaces gap from 8/5. Full findings in [knowledge/tech-context.md](../knowledge/tech-context.md).

**New leadership-level meeting: "AI-DLC Metrics Overview" (2026-08-24, Chuck organizer, with Cory Muir, Shane Kinder, and Travis Emslander)** — a step up from Moissanite-internal syncs, this is Chuck reviewing repo-scorecard metrics directly with product/exec leadership. Outcomes: **Repo 1 named "Emerald"** (confirmed as `fieldnation/engineering-dbt`, MOI-64) with **Core UI and FN Core confirmed as the next two targets** — notably bringing FN Core back into scope for the embed model, a reversal of the 2026-04-16 pivot away from it (that was for the original single-repo pipeline build, so this is a scope expansion, not a direct contradiction, but worth watching). **True Throughput** (complexity-weighted PRs/engineer/week) is now the primary KPI with a stated target of 3.5 against a P90 industry benchmark of 2.6; PR revert rate is the quality guardrail; measurement moves to a repository basis (not team) to handle multi-repo teams. Full detail in [program/OKRs/h2-2026-okrs.md](./OKRs/h2-2026-okrs.md).

**Gitar order form received (2026-08-24).** Jack Kolis (Sonar) sent Cory and Chuck the official Gitar order form, following up on the earlier pricing conversation — a concrete procurement step, though the Gitar trial (running through 9/2) hasn't formally concluded and Chuck's "how it reviewed" objection to Gitar remains unresolved. Worth watching whether procurement outpaces the actual trial verdict.

**Process note (2026-08-24, Chuck to #moissanite-eng):** Chuck asked the team to make agent output (Linear tickets, PR descriptions) more human-readable — flagging that heavy AI-generated text is becoming hard for him to review, and pointing at "good skills out there to make AI less AI." Not yet a formal ticket or standard, but worth tracking if it turns into one (a quality-of-agent-output concern distinct from the harness/architecture work).

**Sonar SSO trouble deepens: a second, more substantive ticket (#73160) opened 2026-08-21.** Distinct from the 8/19 GitHub-login ticket (#73101), this one is a real Okta SCIM/SSO bug — users authenticate via Okta but aren't attached to the SonarQube Cloud org and disappear from the member list. Troubleshooting with Sonar support surfaced a genuine root-cause candidate (the original license was EU-region while FN signs in via the US endpoint; Chuck supplied a new US license key 8/21) but the underlying Okta OIDC integration is still broken as of 8/24, with Sonar now suggesting a switch to SAML. Full detail in [knowledge/tech-context.md](../knowledge/tech-context.md).

### H2 OKRs Operationalized into Linear Tickets; Repo 1 Assigned (2026-07-16/17)

**O1 broken into a real Linear project.** A new Linear project, **"Engineering Velocity via Repo Enablement,"** was created under Team Moissanite with four milestones matching the canonical [H2 OKR doc](./OKRs/h2-2026-okrs.md)'s O1 structure — Repo Scorecard (KR1), Repo-Readiness Playbook (KR2), Embed Model Execution: Top 10 Repos (KR3), and a new **KR4: Proven Enablement for Embedded Teams** (go/no-go statement + proof readout + qualitative feedback collection, tickets MOI-74–77). Roughly 35 tickets were created, almost all by Chuck on 2026-07-16 — consistent with his 7/16 note about pushing OKRs into concrete Jira/Linear tickets ahead of vacation. Old placeholder tickets **MOI-4 (Agent standards v1) and MOI-5 (Skill standards v1) were canceled 2026-07-16**, superseded by this structure.

**Repo 1 concretely assigned: `fieldnation/engineering-dbt`** (MOI-64, created 2026-07-16 08:31am CDT — notably *before* that day's "AI-DLC H2 Regroup" team-selection meeting, see below). This is the data-platform candidate flagged in the OKR doc's "still being figured out" section, not yet reflected as resolved there. Repos 2–10 (MOI-65 through MOI-73) remain stub tickets pending readiness-score sequencing.

**Event-skill split (MOI-48, open since 2026-07-13) resolved.** Vitalii's skill was recast from `fn-event-design` to **`fn-event-integration`** — owns layers B (integration reliability: outbox, DLQ, schema rollout) + C (per-language recipes: PHP producer, Go consumer), lives in `ai-toolbox`. Layer A (contract authority — proto authoring, topic naming/infra, versioning) moved entirely to **`fieldnation/schema-registry`**, tracked as a separate Phase 2 ticket (MOI-52, in progress) — not an ai-toolbox skill at all. MOI-51 (an earlier separate Home-2 ticket) was closed as a duplicate, subsumed into MOI-48. Eval results on the recast skill: Opus-graded, with-skill 100% (63/63 assertions) vs. baseline 70% (44/63), a +30pp delta. PR #52 (renamed) plus new PR #55 are up for `@fieldnation/principals` review — the one remaining acceptance-criterion checkbox on MOI-48.

**Sonar now has concrete integration tickets**, both assigned to Joe: MOI-79 (wire Sonar into the PR pipeline as a standard review-gate step) and MOI-78 (expose Sonar findings to coding agents via MCP as a feed-forward context source, alongside the readiness score and spec/ADR tooling).

**Dedicated AWS prod account (from the 7/8 alignment session) reconfirmed, ticket forthcoming.** In a 2026-07-17 morning DM, Joe asked Chuck to confirm they're still aligned on getting a dedicated PROD AWS account for the agentic platform; Chuck confirmed yes — "we are going to need it anyway once we pick back up the software factory work." Joe said he'd enter a ticket.

---

### Gitar Trial Kickoff Today; BD Hackathon Repurposed Toward AI-DLC Repo Work; Copilot MDM Push Hits a Snag (2026-08-18/19)

**Gitar trial kickoff happening today (2026-08-19), on schedule per the plan agreed 8/10.** "Sonar & FieldNation: Trial Kickoff Call - Gitar," 1:00-2:00pm CT — Jack Kolis (organizer), Jace Key, Gautam Korlam (Sonar/Gitar), Joe (accepted), Chuck (accepted), Cory (tentative), and **Redwan Shahzada (optional, accepted)** — Redwan continuing to show up on Gitar meetings more than a month after rolling off AI-DLC (see [program/people.md](./people.md)). The Gitar pricing call with Cory (previously slipped from 8/17) also landed today, moved to 3:00-3:30pm CT, with Chuck now accepted rather than just optional.

**BD Field Week AI Hackathon (Sept 13, Le Meridien) being steered toward AI-DLC repo work.** Redwan relayed to Chuck (8/18 DM) that Cory wants to avoid repeating May's hackathon format and asked whether the BD engineering hackathon could instead work on repo tasks connected to AI-DLC "so we get some value out of this." Chuck's response: let Redwan and Cory align on specifics first, then loop back — nothing concrete decided yet, but this is a new potential channel for embed-model/repo-readiness work beyond the current traveling-crew model.

**Copilot MDM marketplace push tested live for the first time — didn't work on the first attempt.** Chuck had IT (Paul Wochnick, via Ben Moormeier) push the researched `com.github.copilot` / `extraKnownMarketplaces` MDM profile to his own machine as a test. The profile installed correctly (confirmed the plist value is typed as a string, as required) but the marketplace still didn't surface in Copilot. Chuck's working theory is that the MDM channel may not reach Copilot CLI specifically; he had the test profile removed to try an alternate approach. This is the blocker behind Chuck's 8/17 note that the Claude Code side of the ai-toolbox marketplace is already live ("available in claude code and desktop") but the wider engineering announcement is on hold until Copilot's side works too. Full detail in [knowledge/tech-context.md](../knowledge/tech-context.md).

**Harness CI/CD wiring for ai-toolbox now has an owner: Joe.** Chuck asked Joe (8/18 DM) who could wire up the Harness pipeline for ai-toolbox CI, since the scripts exist but Chuck doesn't have Harness admin access — Joe agreed to take it on. Same thread: Joe flagged that turning on Claude/Copilot OTel span export org-wide (beyond the pilot) could meaningfully affect the Datadog bill given a recent volume-based renegotiation; proposed a separate tracking index and floated 3-day retention (Chuck wants a week, could live with 3 days) — both agree this needs Cory + Ben sign-off before wider rollout.

**MOI-97 opened: GitHub's native Copilot code review can now use agent skills and MCP servers.** Chuck flagged GitHub's docs on this in #moissanite-eng (8/18) as "a big thing they lacked before." The open question the new ticket (assigned Asad Ali, already In Progress) is testing: does the marketplace-distributed `fn-platform` skill bundle (MOI-47) actually reach Copilot's automated PR review, or only the interactive agent? Test plan: stand up a `.github/skills/code-review` skill in `core-ui` and confirm via Copilot's review-comment attribution footer. This adds a sixth angle to the O1 code-review decision alongside Gitar, Tessl, the bespoke `fn-review-agent`, and two new unevaluated tools ticketed the same day — **MOI-94 (roborev.io)** and **MOI-95 (Clanker constitution)** — plus **MOI-96 (AgentsView)** for token-usage optimization. All three are Backlog/unassigned under a new "Additional Tooling" milestone. Full detail in [knowledge/tech-context.md](../knowledge/tech-context.md#gitar-ai-code-review-candidate-sonar-live-as-of-2026-07-27).

---

### Gitar Goes Live on Real PRs — First Direct Evidence for the "How It Reviewed" Debate; Weekly Cadence Flags Sapphire (2026-08-18/20)

**Gitar is now actually reviewing production PRs in `core-ui`, and the results are the first concrete (not vendor-claimed) evidence for Chuck's long-standing objection.** Confirmed today (8/20) on `fieldnation/core-ui#489` and `#490`: `gitar-bot` posts a collapsed "Code Review ✅ Approved" block with a one-line description of the diff and "No issues found" — no detail on what it checked or why it's confident, exactly the gap Chuck has held Gitar to since 7/17. The same two PRs also got full reviews from **Claude Code** (itemized correctness reasoning, explicit "nothing else stood out" with justification) and **GitHub Copilot** (which caught two real issues via line comments — an incorrect Split-flag doc substitution and a render-purity/stale-timestamp bug — that Gitar's terse verdict missed). SonarQube's quality gate also ran clean on both. This is a natural side-by-side comparison worth citing directly the next time the Gitar "how it reviewed" conversation comes up with Sonar. **New Gitar capability, not previously documented:** it also auto-diagnoses CI failures with root-cause categorization and offers a `gitar auto-apply:on` mode to auto-commit fixes — a capability beyond code review itself. Full detail in [knowledge/tech-context.md](../knowledge/tech-context.md#gitar-ai-code-review-candidate-sonar-live-as-of-2026-07-27).

**Team Moissanite Weekly Cadence held 2026-08-18 (auto-generated Gemini notes, so treat specifics as directional, not verbatim).** Notable items: a decision to use **bespoke database evaluations over external frameworks** for skill testing; confirmation that repo-scorecard and event-driven-skills work is progressing; and a new next step for Chuck to **organize a meeting with the Sapphire team to present repository improvement plans** — the first sign since the 7/8 alignment session (where Sapphire was floated as a possible pilot team before Basalt/`engineering-dbt` were named instead) that Sapphire may be back in play as an embed-model target, worth confirming before treating it as settled. Other next steps: Chuck to review Asad's Copilot PR (MOI-97), Mykhailo to send Mykhailo's `engineering-dbt` scorecard findings to Chuck, and the group to finalize the FN brand-guidelines skill (MOI-93) and distribute remaining `engineering-dbt` tickets.

**Sonar SSO/login friction continues.** A SonarSource support ticket (#73101, opened 8/19 evening) reports a team member unable to log into the US Sonar instance via GitHub; Chuck put a "Sonar SSO Setup" meeting on calendar for today (8/20) with **Paul Wochnick** (FN's MDM owner) — plausibly pulling him in to help troubleshoot, though the connection isn't confirmed. This follows the 8/18 Launchpad SSO session with ITMethods, whose explicit goal was getting Chuck logged in — suggesting it isn't fully resolved two days later.

---

### Field Week Hackathon: Chuck Prototypes a Bespoke Code-Review Agent; AI-DLC Charter Requested (2026-08-13/14)

**Chuck used the 8/13 Field Week Technology Team Hackathon (1:00-4:00pm CT, Redwan + Mobarok judging) to prototype a bespoke agentic code-review tool, and is continuing the work post-event.** New repo: `fieldnation/fn-review-agent` (has `docs/architecture.md` and `.planning/PROJECT.md`), shared by Chuck with Joe and Mike Mueller on 8/13 as a starting point they could build on too ("I would love if we can get multiple agents running using the same underlying stuff — will help really prove it out"). On 8/14 in #eng-news, Chuck flagged an Augment Code blog post, ["Solving Code Review with Cosmos"](https://www.augmentcode.com/blog/solving-code-review-with-cosmos), as "the closest thing I've seen that aligns with my vision of agentic code review," noting the post's interactive pair-review concept parallels what Andrew Kandels's team already built. **This is the first concrete action behind the "bespoke coding agent" option** the [H2 OKR doc](./OKRs/h2-2026-okrs.md) flagged as a real O1 code-review candidate alongside Gitar and Tessl — Chuck is now actively building toward it, not just holding it in reserve pending the vendor evaluations.

**Mike Mueller's hackathon idea: an incident-process-management agent**, which he floated in a DM with Chuck and Joe on 8/13 as potentially pairing with Joe's existing Op Cop (the O2 test-bed agent). Chuck confirmed nothing like it exists yet ("that would be amazing... nothing like that is in progress that I'm aware of"); Joe said it had been on his to-do list for a while. Worth keeping in mind as a candidate "Agent 2" for O2 Q4 KR1 if it gets built out further.

**Uncoordinated AI code review already happening on at least one team, outside AI-DLC's visibility.** Mike Mueller flagged (8/13 DM) that Claude is already auto-posting PR review comments in the private `#emerald-eng` channel and asked whether that was set up as part of AI-DLC or done independently by that team. Chuck: "I'm not 100% sure how to turn it off to be honest need to look at it" — meaning he doesn't yet know the answer or how it was configured. Worth investigating given the open O1 code-review-tool decision; if teams are already running ad hoc AI review, that's exactly the kind of duplicated effort the embed model is meant to consolidate.

**Mike Mueller asked whether AI-DLC has a shareable charter, to prevent duplicated effort org-wide.** He referenced an idea from someone named Garrett (team/context not specified) that "looks very much like" what he expects AI-DLC to deliver. Chuck: no polished visual charter exists yet, but he could put one together quickly, and agreed it's "probably good to get something out to the org in the next week or so that says what the initiative is about to not duplicate." Chuck noted he's been keeping the AI Guild informed for the same reason; Joe added he suspects other teams are independently working on similar things that haven't surfaced yet. **Open action item, not yet done:** a shareable AI-DLC charter/one-pager, targeted for the next week or so as of 2026-08-14.

**New "Q3 OKRs Calibration Meeting" on today's (2026-07-17) calendar**, organized by Shane Kinder, 9:00-10:00am CDT, tied to the "PEX OKR Tracker | Q3 2026" spreadsheet — a much broader invite list than prior OKR sessions (Cory, Joe, Travis Emslander, Travis Lang, Stephen Wald, Arefin Hossain, Lisa Sacquitne, Ann-Bettina Colace, Spencer Moulton (declined), and others). Likely the continuation of yesterday's "AI-DLC H2 Regroup" team-selection/Objective-2 discussion at a wider leadership altitude. Outcome not yet known as of this sync.

---

### Vacation-Window Sync: Telemetry Workstream Lands, Gitar Enters as Code-Review Candidate, Tamvir Reassigned (2026-07-17 – 2026-08-03)

Chuck was **OOO 2026-07-20 through 2026-07-31** (calendar block; partial engagement: he kept up with Slack/email and attended AI Guild 7/23 and 7/30 and the 7/22 OKR Review, but declined Moissanite standups). Back **2026-08-03**. The contractors ran O1 execution through the window and produced most of the substance below.

**New KR1 workstream that didn't exist at last sync: AI-coding-assistant execution telemetry (OTel).** MOI-80 (Claude Code, Vitalii) and MOI-81 (Copilot, Asad) are the concrete instrumentation behind the repo scorecard. Both proved out end-to-end into Datadog. Architecture landed as a two-layer split:
- **Layer 1 (baseline adoption/usage metrics) → getDX.** Already flowing; DX's GitHub Copilot adoption report covers the Copilot side with no custom build.
- **Layer 2 (per-invocation diagnostic events: `tool_result`, `tool_decision`, `api_request`, `skill_activated`) → Datadog.** Native OTel events, essentially no custom instrumentation needed.
- **Content firewall verified:** with the `OTEL_LOG_*` flags unset (the default), prompts/code/tool inputs are redacted or absent while completion state, error type, duration and tokens still flow. Spot-checked independently twice on the Copilot side.
- **Joe created a single Datadog log index, `ai-harness-otel`, on 2026-08-03**, covering both `service:claude-code` and `service:copilot-chat`; Vitalii confirmed the same morning that events are now queryable. Joe reversed his 7/31 decision to create two separate indexes. Rationale: 7-day retention makes it cheap to change later. Filter by service to separate them.
- Known gaps flagged, not solved: no distinct "accepted-then-edited" signal (native records accept/reject only); aborted/timed-out are edge states that couldn't be captured cleanly; a *trigger miss* (a skill that should have fired but didn't) leaves no trace at all. Per-skill attribution must come from `api_request.skill.name`; `skill_activated` labels every custom skill as `custom_skill`.
- One real shipping dependency: the exporter is centrally pinned to getDX via a managed `remote-settings.json`, so any routing change goes through that config's owner.
- Cost is a non-issue: layer 2 at full org rollout adds <0.15% of events / <0.1% of bytes against FN's existing ~142M Datadog log events/day.

**MOI-47 skill-delivery mechanism decided: vendored-with-automation.** This closes the long-standing Tier 2 sync open question. Skills stay committed in each consumer repo; Renovate detects each ai-toolbox release, bumps the pinned tag and **auto-merges**; an `apm-refresh` workflow regenerates committed skills + lockfile post-merge; bulk onboarding runs `apm install` from a runner across N repos; a `fleet-status` dashboard shows who's current. Fetch-on-use was spiked and rejected: a `SessionStart` hook can't provision skills for the *current* session (Claude Code loads from disk at startup), and the git-hook alternative has a silent-failure chain (per-clone `core.hooksPath`, `apm` CLI, private-repo auth, network). A notify-only SessionStart hook was kept. **Open governance call: auto-merge means no per-repo human review of standards changes.** PR [ai-toolbox#51](https://github.com/fieldnation/ai-toolbox/pull/51) awaits Chuck's review/merge.

**Repo-readiness tooling actually built** (Mykhailo, MOI-82–87, all Done 7/21–7/29). The readiness score is now code, not a checklist: `repo-scanner` (rank org repos by recent activity), `repo-passport` (static technical passport: languages, build/test/lint/type-check commands, CI config, Docker, docs), `repo-health` (remote GitHub/CI health: branch protection, required checks, workflow runs), and `repo-readiness` (combined score, category scores, blockers, warnings, remediation, evidence). All write to `.ai-factory/` on a shared YAML contract. `repo-readiness` also gained a `--sandbox` flag with `host` and `docker` execution modes so a repo's tests actually run isolated (Go works; Node/TS next). MOI-86 added SQL-repo support, directly relevant to `engineering-dbt` as Repo 1. MOI-57 (readiness score v1) moved to In Review 7/30.

**Gitar is now the live code-review candidate, and Chuck has a stated decision criterion.** Gitar is Sonar's AI code-review product (Ali Adl-Tabatabai, EVP and Gitar founder, was looped into #fieldnation-sonar 7/28). Chuck's position, posted 7/17: most code-review tools only comment on what's *wrong*; without a summary of **how the agent reviewed (its process, what it checked, what it judged fine)**, the human reviewer is left assuming, which *raises* cognitive load rather than reducing it, so review time isn't actually saved. Jack Kolis replied 7/27 that Gitar's team deliberately decided against that to keep reviews low-noise, pointing to [their blog post](https://gitar.ai/blog/ai-code-review-without-the-comment-spam). Chuck (7/28): "I am of a firm belief that without the summary of findings / how the agent code reviewed you are left to assume." **A Gitar technical demo and a call with Ali are both pending Chuck's return**: this is the sharpest movement to date on the open O1 code-review-tool decision, and it now looks like a philosophy disagreement with the leading vendor rather than a feature comparison.

**Sonar moved from signed to onboarding.** Sonar confirmed 7/16 they received the signed copy (executed 7/15). Invoice 2601692 issued 7/29. Support account activation 7/28. Jack introduced the delivery side 7/27: **Loren Williams** (Sonar CSM) plus **Shane Nazareth** and **Vinod K. Avinane** (ITMethods, delivery). Joe set **`sonarqube-admins@fieldnation.com`** as the main owner account. **Launchpad kickoff being scheduled for Aug 4** (Joe deliberately gave Chuck a day to catch up). Chuck also formally removed Redwan from the vendor thread: "he has transitioned off the project and it's just Joe and I now."

**Tamvir hire: name, start date, and role assignment all differ from the 7/15 record.** Redwan posted in #eng-corys-directs on 7/30: the new BD Principal is **Ehtasham Tamvir**, starting **August 3, 2026**, *not* August 17 as relayed on 7/15. And he is **not** the Breakfix backfill: Redwan's proposal is Tamvir → **Granite** (fintech background), with **Atiqul Haque** moving off Granite to take over Break/Fix instead, explicitly "since Chuck's now dedicated full-time to AI-DLC and we're short on principal bandwidth there." **Chuck pushed back the same day** (group DM with Cory and Redwan): "Atiq isn't strong enough for break/fix so I don't believe that a good idea. I can give some more details when I'm back next week." Mike Mueller partially agreed on the Granite fit but hedged on Atiq for Break/Fix. **Unresolved as of this sync**: the allocation was floated for a gut check, not locked in.

**Chuck's full-time dedication to AI-DLC is now stated as fact**, not a plan; Redwan's 7/30 note treats it as the given that forces the Break/Fix reshuffle. This closes the 2026-07-08 resourcing-gap thread, though the specific backfill mechanism is what's now contested.

**Q3 OKR tracking consolidated into the official PEX tracker.** Shane Kinder ran a **"Q3 OKRs Calibration Meeting"** 7/17 (31 attendees across product, eng, design leadership) against a new **[PEX OKR Tracker | Q3 2026](https://docs.google.com/spreadsheets/d/1g2TP4RppTtteFlXkJGaQp3GQOUvMeQZu_0zw3FLFVKE/edit)** spreadsheet, a different doc from the "Draft-AI-DLC - PEX OKR Tracker - Q3 2026" the team had been drafting in, so the draft has been folded into the org-level tracker. Lisa Sacquitne now runs a recurring Tuesday **"Update OKR Tracker [No Meeting]"** reminder with an explicit expectation that each owner updates their cell weekly with progress, blockers, wins and learnings. **Chuck has not responded to any of the three occurrences (7/21, 7/28, 8/4)**, worth clearing on return. Cory's biweekly PEX **OKR Review Meeting** continues; next occurrence **8/5**. Outcome of the 7/17 calibration itself isn't captured in any reachable source.

**O2 M1 (AgentCore architecture plan approved by stakeholders, target July 31): no evidence it landed.** Chuck was out through 7/31 and no approval, meeting, or artifact surfaced in Slack, Linear, Gmail, or calendar. Treat as slipped/unconfirmed pending Chuck's check. The dedicated PROD AWS account ticket Joe said he'd enter on 7/17 also has no visible follow-up.

**Possible duplicate agent-platform effort: Ian Loverink.** Chuck flagged on 7/28 (group DM with Cory and Joe) that Ian appears to be running an **"Internal Agent Architecture Deep Dive"** (rescheduled to Aug 5, 1:30–2:30): "that seems to have a lot of cross over with the AI-DLC work for running agents... maybe something worth asking WTF he's working on unless anyone knows." Joe shrugged. Unresolved, and it overlaps O2's scope directly.

**Guru still not started; Unblocked pilot formally paused.** Chuck turned off the Unblocked Slack integration on 7/17 ("we paused the pilot"; trial had 8 days left) and told Mike Mueller "We are going to look at Guru when I get back." No Guru activity in the window.

**AI Guild (adjacent, tracked separately):** OpenSpec **confirmed as the spec-driven-development standard** (7/22 agenda). This ratifies the MOI-49 recommendation. New Guild items touching AI-DLC: **CodeRabbit** exploration (flagged as paid, needs a decision before going further), **[PR Agent](https://github.com/The-PR-Agent/pr-agent)** as a Copilot-review alternative, Harness's **"Agent-DLC + AI Evals"** launch overlapping the Guild's CI/CD and DeepEvals threads, and Azizul Hakim's **`fn-kafka` skill** (Chuck routed him to Vitalii on 7/23 to avoid duplicating the `fn-event-integration` work). Four Chuck-owned Guild items sat on hold for his vacation: the SDD presentation to Standards Guild, the ai-toolbox OpenSpec skill, the AI-DLC + ADR framework proposal, and the FN Personal Workspace cowork plugin / marketplace codeowners.

**ADR/CODEOWNERS mechanics confirmed as the AI-DLC starting point.** Nick Khaustov asked on 7/17 whether the concrete mechanics he'd worked out (team-level CODEOWNERS, PR isolation via label, major/minor split, worked example) carry into the AI-DLC build or are just directional. Chuck: "Unless the guild finds strong reasons not to use your concrete mechanisms we'd likely use them as is", with the caveat that he asked the Guild to first check whether an existing framework covers it (OpenSpec is building something for ADRs).

---

### Repo Scorecard Goes Live; Tessl's Code Review Enters the O1 Vendor Decision; New FN UI Skill Ticket Tied to Brand Refresh (2026-08-12 – 08-13)

**O1 KR1's repo scorecard is now real, not just definitions.** MOI-55 ("Stand up the repo scorecard") moved to Done 2026-08-12 — it pulls three live DX metrics (TrueThroughput, PR cycle time, PR revert rate) for the embed-pilot repo on a refresh cadence, using the definitions/targets MOI-53/54 reconciled on 8/11. MOI-56 ("Publish and document the scorecard") is now In Progress, closing the loop by documenting where it lives, how to read it, and confirming it's ready to serve as KR3's before-state baseline tool. This is the concrete deliverable behind the open action item Chuck flagged after the 8/10 DX/getDX sync ("this is on my to get set up"). See [knowledge/tech-context.md](../knowledge/tech-context.md#repo-scorecard-kr1-built-and-going-into-documentation-2026-08-12).

**Tessl's code review capability now enters the same O1 decision as Gitar — this was previously scoped only to skills-registry/governance.** Adrian Flückiger (Tessl) sent a written recap of the 8/10 intro call on 2026-08-13, and it surfaces something new: Tessl demoed its agentic code review (Beta), and Chuck gave the same feedback he's given Gitar — reviewers need to show *how* they reviewed and what they judged correct, not just flag problems. Tessl's product team received that feedback directly. A dedicated **"Tessl Code Review overview" call is now on the calendar for Monday, 2026-08-17, 8:30-9:15am CT** — the same day as the already-scheduled 10:00am Gitar pricing discussion with Cory, so 8/17 is now a full vendor-comparison day for the open code-review decision. Enterprise pricing was also shared. This changes the shape of the O1 code-review evaluation: it's no longer just "Gitar vs. build bespoke," Tessl is a live third option. See [knowledge/tech-context.md](../knowledge/tech-context.md#tessl--new-evaluation-candidate-surfaced-2026-08-0607).

**New ticket: MOI-93 "FN UI skill" (Backlog, Chuck, created 2026-08-12), under KR3.** Updates the Tier 2 UI skill for the latest MUI + FN Design tokens and the brand refresh launched during this week's Field Week, building on [ai-toolbox PR #14](https://github.com/fieldnation/ai-toolbox/pull/14). Explicitly scoped to use the `fn-brand` skill for branding guidance and will need UX collaboration for FN-specific UX patterns. Ties directly to today's (8/13) "Brand Refresh Update" Field Week session on Chuck's calendar.

**Context: Field Week continues through 8/13, including a Technology Team Hackathon (1:00-4:00pm CT today).** Teams build a reusable agent or skill/plugin (not a one-off task or demo); Redwan and Mobarok are judging. Chuck co-owns fielding questions for it (with Cory) but it's an engineering-wide event, not a Moissanite-specific deliverable — worth watching for any output that lands in `ai-toolbox` or overlaps AI-DLC scope.

**No new #moissanite-eng Slack activity found beyond the already-logged 8/12 async-standup thread** — consistent with the team being heads-down on Field Week activities rather than AI-DLC-specific work today.

---

### Plugin Roll-Out Meeting Held; Marketplace Model + Distribution Mechanism Agreed; ai-toolbox Gap Tickets Filed; O1 KR1 Closes Two More Items (2026-08-11 – 08-12)

**"AI-DLC Plugin Roll-Out Strategy" meeting (2026-08-11, 2:00-2:30pm CT) reached a concrete decision, per Chuck (2026-08-12).** Chuck, Cory, Ben Moormeier, and Mike Mueller agreed (Joe optional):
- Move forward with marketplaces for **both Claude and Copilot**, built on the existing `ai-toolbox` — it becomes "the engineering marketplace."
- **Claude Code:** an Okta group force-pushes the common engineering plugin; MDM pushes the marketplace configuration.
- **Copilot:** MDM pushes the Copilot marketplace settings.
- **MOI-90 and MOI-91** (the two ai-toolbox gap tickets, below) are explicitly **not blockers** for this rollout — additional hardening work, not gating work.

This resolves the "outcome unconfirmed" flag from the 8/12 sync and finalizes the 8/4-8/5 APM-removal pivot into a concrete distribution mechanism. PR [ai-toolbox#51](https://github.com/fieldnation/ai-toolbox/pull/51) still needs merging; next steps are standing up the Okta group and both MDM push configs. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**Two ai-toolbox capability-gap tickets flagged in the 8/11 standup note are now filed:** **MOI-90** ("Evals for ai-toolbox for ensuring skill is invoked properly") and **MOI-91** ("CI for ai-toolbox"), both created 8/11, Backlog, unassigned. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**Repo-doctor promoted from in-progress work to a tracked ticket: MOI-92** (created 2026-08-12, Todo, assigned Mykhailo) — formalizes the remediation-companion tool to `repo-readiness` that Mykhailo has been building since 8/7.

---

### Team Moissanite Standup: ADR Skills Demoed Live, Immutability Bug Surfaces; 100-Repo Scorecard Rollout Begins (2026-09-15)

**Live demo of the ADR skill suite (delivered 2026-09-14, see below) surfaced a real bug and two open design questions.** Asad walked the group through create/enforce/review end-to-end on a dummy repo. Two things came out of it that the 09-14 "delivered" write-up didn't capture: (1) a revert-the-decision attempt appeared to succeed when the ticket's stated immutability requirement says it shouldn't have — Asad and Chuck flagged this needs verification/fixing before rollout; (2) after Chuck dropped off the call, Mykhailo and Asad debated two unresolved design questions at length — whether create/enforce/review should really be three skills (Mykhailo leans toward merging enforce into create or review; Asad thinks create+enforce could merge but review, which triggers later on expiration/re-review and after human approval, should stay separate), and whether a newer ADR should automatically outrank an older one in a conflict (Asad's answer: no — the system is an audit log, not a priority engine; a human decides which decision wins). Both were punted to Slack, not resolved. **Chuck's decision reaffirmed:** ADR generation stays out of the general repo scorecard for now — not every repo needs one yet — pending his own review of the skills via a draft PR Asad will open (repo-scorecard first, ai-toolbox later).

**Copilot code review confirmed fully off; Gitar is the only reviewer running.** Chuck confirmed in the same standup (consistent with the 09-14 #moissanite-eng decision below) that Copilot code reviews are now off for all new PRs — this directly resolved Asad's complaint that Copilot kept re-surfacing issues one at a time across pushes instead of all at once.

**Repo scorecard template effectively done; 100-repo rollout begins.** Mykhailo finished stripping Copilot-specific checks out of the scorecard template (now that Copilot isn't part of the evaluation) and is about to run it across multiple repos, then move to a harness-vs-baseline comparison. Chuck's own next step is grinding through the 100 highest-priority repos he identified, tracked in a new spreadsheet he set up the same day.

---

### Team Moissanite Async Standup (2026-09-28)

**Format:** async in Slack thread due to Chuck's scheduling conflict.

**Mykhailo's status:** Two PRs ready for review — [ai-repo-score#23](https://github.com/fieldnation/ai-repo-score/pull/23) and [ai-repo-score#20](https://github.com/fieldnation/ai-repo-score/pull/20) — with focus continuing on **MOI-110** (bring ai-repo-score to ~100 repo-readiness). No blockers flagged.

**Asad's status:** Moved `adr-tools` to ai-toolbox as [ai-toolbox PR #79](https://github.com/fieldnation/ai-toolbox/pull/79); addressed Gitar-bot review comments. PR awaiting review.

**Chuck's inquiry:** Asked Asad on where ADR work (bringing everything into the skills) currently stands — directly addressed by Asad's PR #79 status above.

**Chuck's reviews:** Approved ai-repo-score PR #20; holding on #23 pending potential prompt changes (working on combining writing for agents + skill-creator in ai-toolbox for easier maintenance).

**Key ongoing:** Mykhailo continuing repo scorecard completion; Asad/Vitalii finishing ADR skills consolidation in ai-toolbox. No new blockers or personnel updates from this standup beyond what's documented in [program/people.md](./people.md) (Vitalii's transitioning-off status, Asad's recent health issues).

---

### ADR Skills Consolidation Decision; Tool Packaging Becomes Next Priority; Vitalii Transitions Off (2026-09-16)

**ADR skills (create/enforce/review) consolidation decision made: keep 3 separate.** Asad and Mykhailo spent 45 minutes on the 09-16 standup discussing whether the three skills should consolidate into one or two, given the perceived duplication. Decision: maintain three separate skills because (1) they fire at distinct moments (create when decision drafted, enforce when change described but not written, review when record review threshold passes), and (2) the current ai-toolbox architecture prevents skill-to-skill calling, making consolidation technically awkward and less elegant than three focused implementations. This validates the design rather than suggesting a flaw. **Next step (09-16 standup):** Mykhailo's ADR architecture diagram to be reviewed by Chuck, then pushed to ai-toolbox for code review and rollout. See [knowledge/tech-context.md](../knowledge/tech-context.md#adr-architecture-decision-record-skill-architecture--2026-09-16).

**Tool packaging emerges as the next H2 priority.** After Vitalii's repo bootstrap PR is reviewed and merged, the focus shifts to packaging both repo bootstrap and repo scorecard as downloadable CLI tools (GitHub releases or similar), eliminating the need to clone the repo to use them. Chuck proposed GitHub Actions releases as the distribution mechanism, or a private Homebrew tap. Vitalii is asked to scope what this looks like and create tickets. Also discussed: **setup documentation initiative** — a Confluence page listing all required developer tools (OpenSpec, Sonar MCP, AI tools, etc.) and their roles, to be created as a ticket by Chuck. This becomes the reference sheet for rolling the agentic experience out to teams as the tooling ecosystem expands.

**Repo scorecard progress: 97 → 100, now accounting for main branch history.** Mykhailo reported scorecard now factors in commit history on main, bringing the score from 97 to 100. This is the central metric for O1 repository readiness KR and is now production-ready.

**Vitalii Belinskyi transitioning off Moissanite project effective 2026-09-16.** Rolling off after completing repo bootstrap and Harness 911 ticket work (expected end of week ~2026-09-20). Currently blocked on Harness admin access to configure toolbox create-trigger for pipelines. Mykhailo requested a handover call to review contributions. See [program/people.md](./people.md) and [knowledge/tech-context.md](../knowledge/tech-context.md#vitalii-transition--2026-09-16).

**AI Guild meeting tomorrow (2026-09-17) has several meaningful items pending:** (1) Gitar vs. Copilot final decision (Chuck); (2) OpenSpec rollout status check — PRs #65, #77, #78 were closed unmerged this week, needs investigation; (3) Matt Pocock skill standardization via ADR; (4) Harness CI/CD & AI Evals update (Joe); (5) AI Hackathon output triage — 8+ new skill PRs landed from Field Week (fn-rabbitmq-audit, fn-feature-flags, fn-repo-readiness, fn-standards-compliance, fn-qa-testing, fn-repo-score-uplift, fn-nest-migration/agent-readiness), pending intake and prioritization. The OpenSpec PR closures and Hackathon skill volume are unexpected signals worth monitoring — potential scope/resource constraint or priority shift.

**Minor CI/CD items:** Vitalii merged the harness PR Joe had requested and finished the repo-SDK bootstrap (OpenSpec init + ticket-spec generation); confirmed the "red validate run" check correctly flags a broken pipeline (earlier flakiness there is fixed). The Kafka PR is approved and just needs outdated comments resolved before it merges. See [knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md](../knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md).

---

### Gitar Formally Selected for O1 Code Review; ADR Skill Suite Completed; OpenSpec Standardized (2026-09-08 – 09-15)

**Gitar selected as the O1 code-review solution (2026-09-14, Chuck in #moissanite-eng).** "We are moving forward with gitar, so we should actually look for gitar and that copilot doesn't exist." This closes the four-way evaluation (Gitar vs. Tessl vs. native Copilot review vs. bespoke fn-review-agent). Resolves a three-month open decision; next work is rubric/policy refinement and CI integration. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**ADR (Architecture Decision Record) workstream delivered complete (2026-09-14, Asad async standup).** A full skill-based ADR system with three new skills — **`fn-adr-create`** (scaffolds a repo's ADR folder and config), **`fn-adr-enforce`** (immutability checks, flags changes to accepted decisions), and **`fn-adr-review`** (due-date flagging for reviews) — shipped in ai-toolbox under `plugins/fn-software-engineering/skills/`. Companion tooling in ai-repo-score: new `adr-tools/` Python workspace, `.github/workflows/adr-check.yaml` reusable workflow, and ADR index/validation. Practice split: judgment lives in skills; determinism lives in CLI. **Two format options still open** (numbered `0001-title.md` vs. topic-versioned `ADR_Topic_v1.md`), decision deferred to team feedback. **Four blockers before org-wide rollout:** (1) no tag cut on ai-repo-score, (2) uvx auth against private repos unresolved, (3) cross-repo reusable workflows need org enablement, (4) filename convention TBD. PRs pending team feedback post-demo. This is a new, substantial deliverable not previously logged in this workspace. See [program/references.md](./references.md) for ADR source references and [knowledge/tech-context.md](../knowledge/tech-context.md).

**OpenSpec standardized for SDD (Spec-Driven Development, 2026-09-10–09-14, #moissanite-eng thread).** OpenSpec (openspec.dev) confirmed as the standard. PR #65 (`ai-toolbox`, in-progress) needs rework to focus on OpenSpec setup/documentation rather than reimplementing it — **MOI-104 should close** (OpenSpec ships its own commands per repo); **MOI-60 playbook needs rework** to guide setup and use. Human spec review (before implementation) carries over as the one gate OpenSpec itself doesn't have. Vitalii flagged a detail: FN-specific rules (EARS format, acceptance-criteria quoting, requirement citation) should stay in `openspec/config.yaml` per repo, vanilla OpenSpec + local customization. Chuck confirmed that's fine. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**Repo-readiness rubric v2 improvements underway (MOI-109, 2026-09-11–09-14).** Mykhailo improving rubric accuracy for Gitar/Copilot code review scoring, incorporating audit findings from Harness reports. Also investigating Harness Evals vs. DeepEval in ai-toolbox (MOI-105) and planning to update ai-repo-score against the rubric v2. No blockers, work in progress.

**BD hackathon weekend prep (2026-09-10).** Chuck working on updated repo scorecard for weekend Field Week use (no dates/context surfaced yet, but flagged as weekend work 9/10).

**No staffing changes, timeline slips, or strategic pivots found.** Jira/Confluence search returned only historical/archived references; no new substantive activity detected.

**Two more O1 KR1 tickets closed 8/11:** MOI-53 (reconcile DX metric definitions against FN's usage) and MOI-54 (define scorecard success metrics and target impact magnitude), both Chuck-owned, both Done.

**Gitar's Aug 17 pre-trial sync is now a specific calendar invite:** "Sonar & Field Nation: Gitar Pricing Discussion," Mon Aug 17, 10:00-10:30am CDT (Jack Kolis to Cory, cc Chuck) — the pricing conversation flagged as the next step on 8/10 is now scheduled.

**Team Moissanite Weekly Cadence (8/11) was low-attendance; team proceeded async.** Per auto-generated meeting notes: Vitalii continued closing out marketplace-deployment code review comments and trace-logging updates; Asad's session-tracing work now aggregates data by ID, with research continuing into flowchart visualization for AgentCore CloudWatch (O2-adjacent, not previously logged — treat as early-stage, not a milestone). No other new substantive content.

**Context: today (8/12) is a Field Nation "Field Week" day** — Chuck's calendar is full of non-project activities (team building, volunteer event), and he moved standup to async in #moissanite-eng ("I'm going to be tied up during standup time today"), which explains the otherwise-quiet Slack signal.

---

### Gitar Trial Agreed; Tessl Outcome Lands ("Proceed as Planned"); Plugin Roll-Out Strategy Meeting Today (2026-08-11)

**Tessl intro call (8/10) outcome: interesting, but not changing course for now.** Chuck's 8/11 standup note (posted from the road — he's traveling and missing standup today) confirms the 8/10 meeting happened: "Mykhailo, Joe and I met with Tessl yesterday, will continue to discuss with them as they have a lot of interesting offerings that would be useful for us. However in the meantime we will continue to move forward with our original plan." This resolves the outcome that was unknown as of the 8/10 sync — Tessl stays an open, ongoing conversation rather than a pivot. Consistent with Chuck's stated 8/7 position (proceed on current path, pivot quickly if adopted).

**Gitar: demo happened Friday 8/7, and a concrete trial plan is now agreed — the O1 code-review decision has real momentum for the first time.** Jack Kolis (Sonar) emailed Cory 2026-08-10: "Chuck and I had a great conversation with Ali (the founder of Gitar, now VP at Sonar) on Friday. We discussed Gitar in detail and Ali demonstrated the solution." Chuck flagged it's a priority to have a code review solution running before **end of September**, and the two aligned on a project plan: **Aug 17** pre-trial sync (convene stakeholders, review technical setup), **Aug 19** trial kickoff call (review Gitar setup + success criteria), **Aug 19 – Sep 2** run the Gitar trial, **after Sep 2** execute remaining steps to finalize the Gitar partnership. Next step is a pricing conversation with Cory. **Note: this doesn't resolve the underlying philosophy disagreement** (Chuck's requirement that the reviewer publish *how* it reviewed, which Gitar's team has said they deliberately omit) — Chuck appears to be moving to a live trial anyway, likely driven by the September deadline pressure. Worth watching whether the "how it reviewed" gap becomes a blocker mid-trial. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**Sonar Launchpad kickoff (held 8/6) had a real outcome, previously unconfirmed: presentation delivered, follow-up scheduled.** Shane Nazareth (ITMethods) emailed Joe and Chuck 8/10 with the Launchpad presentation deck attached and confirmed a follow-up discussion is scheduled "to continue the momentum and help you deploy Sonar successfully internally." No specific date surfaced for that follow-up in this sync.

**Two new ai-toolbox capability gaps identified (Chuck, 8/11 standup), tickets to be filed today (not yet created as of this sync):** (1) evals for ensuring a skill is actually invoked (using its description) — a gap distinct from the existing skill-content evals; (2) Harness CI/CD for running evals automatically when skills change. Worth checking Linear next sync for the new ticket numbers.

**Chuck met with DX (getDX) 8/10** — likely the "Field Nation | DX - AI DLC Sync" meeting flagged on yesterday's calendar. Per his standup note: "they have the dashboard / metrics we need to track to prove the team we embed with is getting better, this is on my to get set up" — an open action item, not yet done.

**"AI-DLC Plugin Roll-Out Strategy" meeting on today's (8/11) calendar, 2:00–2:30pm CT** — Chuck organizer, Ben Moormeier and Mike Mueller (InfoSec/Sr. Principal) accepted, Cory accepted, Joe optional/accepted. Description: "Getting this group together to discuss the plan for rolling out the ai-toolbox to engineers and get the buy-in from this group." This is the review meeting flagged in the 8/5 sync as being set up for 8/11 — now confirmed with a real title. Chuck's standup note frames it as specifically about "using the claude / copilot marketplace as the rollout mechanism for our skills." Outcome not yet known as of this sync (meeting is same-day, this afternoon).

**MOI-89 (incorporate Matt Pocock's "writing great skills" into FN's skill builder) was canceled 2026-08-10** — no reason surfaced in reachable sources; was In Progress/Todo as of the 8/5 sync. **MOI-54 (define scorecard success metrics and target impact magnitude) moved to In Review 2026-08-10.**

---

### Tessl Capability Comparison Lands Ahead of Today's Intro Call; New DX/Atlassian Scorecard Sync on Calendar (2026-08-10)

**Mykhailo produced a detailed capability-comparison doc ahead of today's Tessl intro call** ([Confluence](https://fieldnation.atlassian.net/wiki/spaces/~712020b505f5c3576344a78bc4d1f7dc47b0da/pages/6149701634), dated 2026-08-07, not previously surfaced in this workspace). Scoring ~30 capabilities across Tessl, the current `repo-score` toolchain, and the planned AI Software Factory, the doc's headline finding is that Tessl's strengths cluster almost entirely in the **skills-registry/governance layer** (versioning, quality review, behavioral evals, security scanning, org-wide skill enforcement, activation analytics) — an area the factory plan hasn't built yet — while **repo inventory, CI health, and readiness scoring are already strong internally** and should stay proprietary. Mykhailo's explicit recommendation: **"pilot Tessl before building"** the skills-registry piece. Full detail in [knowledge/tech-context.md](../knowledge/tech-context.md#tessl--new-evaluation-candidate-surfaced-2026-08-0607).

**Tessl intro call is on today's calendar, 11:30am CT** (moved from 11:00am at Tessl's request on 8/7). Attendees: Chuck, Joe (accepted), Mykhailo (accepted), Cory (tentative), Tessl's Adrian and Shawn. Outcome not yet known as of this sync — Chuck's stated position from 8/7 still holds going in: proceed on the current path unless/until a decision is made, then pivot quickly if adopted.

**New meeting on today's calendar, not previously logged: "Field Nation | DX - AI DLC Sync"**, organized by Atlassian's Taylor Brown (tbrown3@atlassian.com, also taylor.brown@getdx.com) with Cheng Tang (cheng.tang@getdx.com) — timing lines up with Shane Kinder's unanswered 8/6 question about whether the KR1 repo scorecard would be ready to review this week (see [program/OKRs/h2-2026-okrs.md](./OKRs/h2-2026-okrs.md)). Likely the venue for that answer; outcome not yet known as of this sync. Added Taylor Brown and Cheng Tang to [program/people.md](./people.md) as new DX/getDX contacts.

**No movement found on:** the Gitar demo outcome (call was 8/7, no readout in Slack or Gmail), the AllCloud follow-up outcome (call was 8/6), the Sonar Launchpad kickoff outcome (was 8/6), or the Tamvir/Atiqul Break/Fix allocation. Linear (MOI team) shows no ticket updates in the last 3 days. Slack was quiet aside from the Tessl thread above — Chuck was on-site in Minneapolis for Field Nation's "bi-yearly on-site" 2026-08-07/08, which likely explains the lull.

---

### Tessl Surfaces as a New Evaluation Candidate; Gitar and Tamvir Meetings Land Today (2026-08-07)

**Tessl (tessl.io) surfaced at the 2026-08-06 AI Guild meeting and is now an open evaluation question for Moissanite.** Chuck flagged it in #moissanite-eng the same morning: "looks incredibly interesting and solves a number of problems that we are trying to solve," and said he'd request a demo. **Vitalii asked the load-bearing follow-up question this morning (2026-08-07):** is Tessl an evaluation or a commitment, and if adopted, does it replace the SDD/ai-toolbox-skills approach (MOI-60, MOI-47) or feed into it — he doesn't want to keep investing in overlapping tickets if the direction is about to change. **Chuck's answer (2026-08-07, 6:16am CDT):** he has a meeting with Tessl on **Monday, 2026-08-10**; until then, the team should "proceed forward as if we are not going to introduce it" and work a parallel path — if they do decide to move on Tessl, the decision will be made quickly and they'll pivot. No independent detail on what Tessl actually does has surfaced in this workspace yet — worth capturing after the 8/10 meeting. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**Gitar technical conversation is on today's calendar** — "Sonar & Field Nation: Gitar Conversation," 12:00-12:45pm CDT, with Ali Adl-Tabatabai and Jack Kolis (Sonar), Chuck (accepted), Joe (tentative), Redwan (declined). This resolves the "no demo slot picked yet" flag carried since 8/3. Outcome not yet known as of this sync — the underlying philosophy disagreement (does Gitar publish *how* it reviewed a PR) is still unresolved going in.

**Chuck has a 1:1 with Ehteshamul Tamvir on today's calendar** ("Connect with Ehteshamul Tamvir (new BD Principal)," 9:30-10:15am Asia/Dhaka, Chuck organizer, Atiqul Haque optional, Redwan declined), linked to an onboarding doc and the eng org chart. Possible venue for resolving the still-unlocked Break/Fix/Granite/Platinum allocation question. **Naming note:** the calendar invite and linked Confluence onboarding doc both spell the name **"Ehteshamul Tamvir"** (email `ehteshamul.tamvir@fieldnation.com`), not "Ehtasham Tamvir" as recorded elsewhere in this workspace — flagging as a likely spelling correction rather than overwriting outright; worth confirming. See [program/people.md](./people.md#ehtasham-tamvir--principal-engineer-bangladesh-started-2026-08-03).

**MOI-81 (Copilot OTel) closed out its open questions (Asad, 2026-08-05, surfaced this sync — missed in the 8/6 run):** DX connector confirmed already covering layer 1 (no custom build needed). **Architectural finding:** OTel does not work outside VS Code — the Copilot Chat extension (VS Code only) has an OTLP exporter, but `copilot-language-server` (everything else runs on this) has none and ships to Azure Application Insights instead; no config fixes this, workaround is Copilot CLI in the IDE terminal. Per-skill attribution is worse on Copilot than Claude Code: `skill_name` is confirmed absent from both logs and metrics (Claude Code at least exposes it via `api_request.skill.name`). See [knowledge/tech-context.md](../knowledge/tech-context.md).

**MOI-80 (Claude Code OTel) effectively functionally complete; Vitalii posted the production punch list (2026-08-06):** shared OTel collector (fan-out to getDX + Datadog) needs a platform/SRE owner; `remote-settings.json` needs updating to route OTLP to it org-wide; per-repo `repo.name`/`team.name` tagging is the remaining small piece; the Datadog index is done. Hooks are optional — ~90% is native telemetry already, a small hook (shipped as a plugin) could later add the "edited" override signal and cleaner per-skill attribution, but isn't required to ship.

**Mykhailo is building `repo-doctor`**, a new tool on top of the repo-readiness toolchain that gives detailed remediation instructions for improving a repo's score, and has redshift credentials to continue the `engineering-dbt` (Repo 1) investigation.

---

### O2 M1 Marked Yellow — AgentCore Path Questioned; AllCloud Follow-Up and Sonar Launchpad Both Land Today (2026-08-06)

**O2 M1 status resolved from "no evidence of landing" to a concrete Yellow, with the platform choice itself back in question.** Shane Kinder commented directly on the org-level [PEX OKR Tracker | Q3 2026](https://docs.google.com/spreadsheets/d/1g2TP4RppTtteFlXkJGaQp3GQOUvMeQZu_0zw3FLFVKE/edit) on 2026-08-04 (surfaced to Chuck via email 8/5): *"Yellow due to ongoing conversations around M1 and if AgentCore is the correct path forward or we should be looking at Google or Azure since they are more 'out of the box ready.'"* Shane notes M2 and M3 are on track, credits the repo-scorecard tool as delivered with actionable outputs, and flags the next step as running it against `engineering-dbt` (Repo 1). He also asked Chuck directly: *"Are we on track to have Scorecard ready to review next week?"* — unanswered as of this sync. This reopens a platform question (AWS Bedrock AgentCore vs. GCP/Azure) that the team had treated as settled in AgentCore's favor since the 7/8 alignment session — worth Chuck weighing in given he owns O2 jointly with Joe.

**AllCloud follow-up call on today's calendar.** "AllCloud & Field Nation - AI DLC discussion," organized by Monica Zamora, 11:00-11:25am PT, with Danny Beachnau (AllCloud), Cory, Chuck, and Lauren Hood (AWS) — the Thursday follow-up flagged in the 8/3-8/5 entries above. Outcome not yet known as of this sync.

**Sonar Launchpad kickoff confirmed for today, not 8/4 as previously logged.** "LaunchPad Kick Off <> iTmethods - FieldNation," 2:30-3:00pm CDT, organized by Shane Nazareth (ITMethods), with Loren Williams, Joe, and the broader ITMethods delivery team (Ivan Lima, Vinicius Melo, Vinod Avinane). See [knowledge/tech-context.md](../knowledge/tech-context.md).

**Glean sales motion still alive — new point of contact reached out, cc'ing Cory, Ben Moormeier, and Chuck.** Sandra Rabadi (Sr. Technical Account Executive, Glean) emailed 8/5 introducing herself as the replacement for Dan (who left Glean, per Dylan Kuntz's 7/22 note) and asked how "the Anthropic rollout is going" and whether Glean can complement it, proposing times to connect. Glean was last raised in this workspace by Cory on 2026-06-23 as a Guru/Unblocked alternative worth due-diligence, but hasn't had a dedicated contact or thread since. Not urgent, but worth Chuck/Cory deciding whether to engage given Guru's own pilot is five weeks stalled. See [program/people.md](./people.md) and [knowledge/tech-context.md](../knowledge/tech-context.md).

**No movement found on:** the Tamvir/Atiqul Break/Fix allocation, Ian Loverink's Agent Architecture Deep Dive follow-up, the Gitar demo scheduling, or MOI-47 PR #51's merge status.

---

### MOI-47: APM Removal Becomes the Working Direction; Skill-Builder Tooling Started; AllCloud Read as Validation, Not New Direction (2026-08-05)

**MOI-47 (ai-toolbox distribution): the 8/4 "emerging pivot" away from APM has solidified into the working direction, backed by real usage data — though not yet merged.** Vitalii posted a long Linear comment (cross-posted to #moissanite-eng) reversing his own 7/27 "vendored-with-automation" call: he checked actual APM usage and found **zero real consumers** (searches for APM-related config/PRs came back empty, while a control query for `AGENTS.md` returned hits — confirming search itself works, so the absence is real). Meanwhile skills adoption is healthy but entirely bypasses Tier 2/APM: **~186 hand-authored skills across 19 active repos**, all committed directly (fn_react_native 50, meridian 22, agent-sdk 18, netdev-recruitments 14, fieldnation 13, etc.). His recommendation: drop APM entirely in favor of native **Claude Code + Copilot plugin marketplaces** — one `marketplace.json` + `.claude-plugin/plugin.json` in `ai-toolbox`, delivered via Anthropic Admin Console managed settings (Claude) and GitHub/MDM managed settings (Copilot), with nothing committed into any of the 739 product repos. Two things vendoring still wins at, flagged explicitly: reproducibility (an old commit no longer pins old skills under latest-always) and CI/offline use (committed skills need no auth). A real gap: Copilot's *cloud* surfaces (PR review, coding agent, github.com chat) run in GitHub's environment and read `.github/skills/` from the repo — managed settings can't reach them, so that path needs org custom instructions or a thin `.github/skills/` later (phase two). Also surfaced: `consumers.json` has been pointing at a repo that 404s (`fieldnation/fn-core` instead of `fieldnation/fieldnation`) — the old delivery pipeline was aimed at nothing. Chuck confirmed the direction the same day (independently, per the 8/4 record above). **PR [ai-toolbox#51](https://github.com/fieldnation/ai-toolbox/pull/51) was renamed 8/5 to `feat(MOI-47)!: replace APM with a plugin marketplace for Claude Code + Copilot`** (breaking-change marker) and now mostly *deletes* the APM delivery machinery (Renovate preset, `apm-refresh`, `consumers.json`, `apm-onboard`, `fleet-status`) rather than merging it — keeping only Harness validate/release pipelines, CHANGELOG discipline, the eval convention, and `apm audit`'s hidden-character scan folded into CI. Two things still need proving before anything is deleted for good: whether Claude Code's managed-settings auto-install actually fires reliably (an open, inactive upstream GitHub issue reported it not installing at all), and private-repo auth via SSH for background auto-update. Chuck is actively reviewing the renamed PR as of 8/5 (comments on the Harness pipeline YAML, PR template checklist, and README wording).

**Concrete next step for org-wide rollout: a "birthright" engineering group for force-installed plugins.** Chuck/Cory DM (8/4 afternoon): plan to stand up an engineering-wide group (mechanics likely via Okta/C1, owner probably "Paul") so every engineer gets the Claude/Copilot plugin marketplace force-installed automatically, with changes to the marketplace itself following normal PR/review SDLC. Chuck is setting up a meeting with Cory, Ben Moormeier (InfoSec), and Mike Mueller for Tuesday afternoon (2026-08-11) to walk through it, and hopes to have "how do we deploy skills to everyone" solved by the engineering hackathon during Field Week.

**AllCloud's "AI-DLC Offering Phase 0" reviewed by Chuck — reads as validation, not new direction.** Closes the "contents not accessible" flag from 8/3-8/4: Chuck told Cory (8/4) the deck is "just a couple slides" and "pretty much exactly what we had proposed for the AI-DLC," so he reads the engagement as Field Nation wanting AllCloud to **poke holes in / validate the existing plan**, not supply a new one. Separately, Cory gave AllCloud direct feedback that their initial proposal (heavy discovery/interviews) isn't the right shape for Engineering — he wants **less discovery, more third-party review of what's already planned**, likely as a joint Product + Engineering workshop rather than something Chuck/Cory run themselves. Follow-up call still set for Thursday, 2026-08-06.

**New dedicated repo for the repo-readiness toolchain: `fieldnation/ai-repo-score`.** Created 2026-08-03 by Chuck (Mykhailo has admin, rest of team has write) to give Mykhailo's `repo-scanner`/`repo-passport`/`repo-health`/`repo-readiness` tools (MOI-82–87) their own home instead of living inline elsewhere. First PR (CI/linter fixes + doc updates) merged 8/4.

**Chuck opened two new tickets on skill-authoring tooling, both self-assigned:** **MOI-88** (In Progress) — build a utility to optimize/tighten skill descriptions in ai-toolbox, since that capability was lost when the team forked the Claude skill builder, then re-run it against all existing skills. **MOI-89** (Todo) — incorporate [Matt Pocock's "writing great skills" skill](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-great-skills/SKILL.md) into FN's own skill builder. Both sit under the O1 "Engineering Velocity via Repo Enablement" Linear project.

**Open flags carried forward, no new movement:** Ian Loverink's "Internal Agent Architecture Deep Dive" — Chuck told Cory 8/4 he "still hasn't dug into that message from Ian," so the possible O2-overlap concern flagged 7/28 remains unaddressed. The Gitar demo — Ali proposed concrete call windows 8/3; no evidence Chuck has picked one yet. The Tamvir/Atiqul Break/Fix-vs-Granite-vs-Platinum allocation — no update since the 8/4 third-proposal state; still not locked in.

---

### Resourcing Re-Proposed; APM Distribution Reconsidered; AWS Partner AllCloud Surfaces (2026-08-04)

**Break/Fix reshuffle reopened with a different allocation than 7/30.** In a Chuck/Cory/Redwan group DM this morning, Redwan proposed a new split: **Tamvir starts on Break/Fix** (not Granite, reversing the 7/30 plan) so he can ramp up with Chuck's mentorship, with a possible later move to Granite given his fintech background; **Atiqul Haque moves from Granite to Platinum** (not to Break/Fix as floated 7/30, which Chuck had opposed) since Platinum works the PHP monolith and Atiq's PHP strength fits there, also giving Mike Mueller additional monolith support. Chuck's read: happy either way — Tamvir doing both Break/Fix and Granite, or Chuck managing Break/Fix directly if needed — and confirmed he's fine focusing most of his time on AI-DLC: *"I certainly won't complain about being able to focus most of my time on the AI-DLC."* New team-structure detail surfaced in the thread: Break/Fix is being reduced to two teams as **Marble moves to the Data Platform**. Not yet locked in — this is the third distinct version of this allocation in three weeks (7/15 → 7/30 → 8/4); treat as evolving, not settled. See [program/people.md](./people.md#ehtasham-tamvir--principal-engineer-bangladesh-started-2026-08-03).

**MOI-47 (ai-toolbox distribution): emerging pivot away from APM, toward native Claude Code / Copilot marketplaces.** On an Aug 4 call, Vitalii and Chuck independently converged on the same idea — Chuck: *"we don't even need APM anymore at all... if we just purely convert it to the two marketplace approaches that's going to be the easiest way to distribute it."* Vitalii's research: VS Code supports agent plugins + marketplaces (`chat.plugins.marketplaces`) and auto-detects the Claude plugin format (`.claude-plugin/plugin.json`), so **one manifest serves Claude Code, VS Code Copilot, and Copilot CLI**. Copilot also has a July-2026-GA enterprise push mechanism (`extraKnownMarketplaces`/`enabledPlugins`, three delivery channels: native MDM, server-managed, or file-based `managed-settings.json`) and reads `.claude/skills/` directly. Caveats: VS Code agent plugins are still Preview, channel precedence is winner-take-all, and Copilot's update cadence trails Claude Code's by up to a day. Sequencing: Claude Code pilot first (force-install via managed settings + SSH auth on 2-3 machines), Copilot second once proven. [ai-toolbox PR #51](https://github.com/fieldnation/ai-toolbox/pull/51) already has both tools working locally (skills resolve for Claude Code and Copilot alike); the one untested piece is force-install/auto-update via managed settings, which needs Chuck's Admin Console access. Chuck wants a working POC with a small force-installed group before taking this to the AI Guild — and has come around to pushing all skills to every repo by default ("seems to be the way in which everyone is doing it"), reversing his earlier hesitation. **This is a live reconsideration of the 7/27 MOI-47 "vendored-with-automation" resolution, not yet finalized** — see [knowledge/tech-context.md](../knowledge/tech-context.md).

**New AWS-adjacent partner: AllCloud.** Lauren Hood (AWS) forwarded an "AllCloud - AIDLC Offering Phase 0" PDF to Cory and Chuck on 2026-08-03, following a call the same day, ahead of a Thursday (2026-08-06) follow-up. New contacts: **Monica Zamora** and **Daniel Beachnau** (AllCloud). Contents of the offering weren't accessible this sync (secure, passcode-protected AWS attachment); flagging the existence and the Thursday meeting as a signal worth Chuck's attention, not a summarized decision. See [program/people.md](./people.md).

---

### Sonar Contract Signed; AI-DLC H2 Regroup Convenes on Objective 2 & Team Selection (2026-07-16)

**Sonar contract signed.** A SonarQube order confirmation (Order #00011413) landed 2026-07-16 00:03 CDT, addressed to Field Nation accounts payable, Redwan, and Chuck — closing out the procurement question left unconfirmed on 2026-07-15. License delivery + Launchpad onboarding still targeted for **August 1, 2026**. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**"AI-DLC H2 Regroup" meeting today (11:00-11:30am CDT), Chuck organizer** — Cory, Joe, Kelley Manuel, Shane Kinder, Travis Emslander, Lisa Sacquitne invited. Per Chuck's reply to a Lisa question about the agenda: today's session covers **Objective 2 (long-running agents)**, which the group didn't get to at yesterday's "AI-DLC Objectives" meeting (2026-07-15, 8:15-9:00am CDT, Cory organizer), plus **Shane has asked the group to work on identifying pilot teams** — directly actioning the "O1 team selection: pending a group working session (Shane to organize)" open item in [program/OKRs/h2-2026-okrs.md](./OKRs/h2-2026-okrs.md). Outcome not yet known as of this sync.

**Chuck pushing OKRs into Jira/Linear tickets ahead of his 2-week vacation (starts 2026-07-20).** In #moissanite-eng this morning, Chuck shared a video he called "almost exactly my vision for the AI-DLC" (also sent to Joe and Mykhailo the night before) and said he's "working on the actual tickets and things in JIRA and will be extending tomorrow's [7/17] meeting longer so we can go over the full plan for while I'm on vacation." Consistent with the 2026-07-14 note that Mykhailo covers day-to-day during Chuck's vacation.

**Evaluation-phase Linear tickets closed out today:** MOI-42 (Harness POC — planning agent connected to Linear, assignee Joe), MOI-46 (framework & platform evaluation, assignee Asad), and MOI-49 (SDD framework evaluation, assignee Chuck) were all marked Done 2026-07-16 — closing the loop on the spring/early-summer platform-and-framework evaluation work as the team shifts to Q3-OKR-driven epics/tickets.

**Mykhailo shared a 90-day repo-activity Google Sheet** in #moissanite-eng — directly relevant groundwork for Objective 1's repo scorecard/DX-metrics work. See [program/people.md](./people.md#mykhailo-sakhnik).

### July 15 PEX Leadership Demo — Confirmed on Calendar, Outcome Not Yet Known (2026-07-15)

The demo flagged as at-risk on 2026-07-13 (see below) is on today's calendar: **"AI Software Factory — Demo,"** organized by Redwan, with Chuck, Joe, Cory, Shane Kinder, Travis Emslander, Ben Moormeier, Kelley Manuel, Lisa Sacquitne, and Shadab Ahmed invited (Stephen Wald declined). Description: *"Demo of the AI Software Factory's POC showing the agent hosting/planning pipeline running end-to-end on our two candidate platforms — AWS Bedrock AgentCore and GCP Vertex Agent Engine. Will cover platform comparison findings and where we are at right now."* This description is consistent with the 7/8 scoping ("code generation only, no PR yet") but doesn't by itself resolve whether Execute/Verify are wired up — that open question (below) is still not independently confirmed as of this sync. Outcome/notes not yet available.

### Embed-First Pivot & Pilot-Scope Narrowing (2026-07-13)

**"H2 Plan Review | AI SDLC"** — broader leadership review (Cory, Redwan, Chuck, Joe, plus Shane Kinder/EVP Product, Travis Emslander, Kelley Manuel, Lisa Sacquitne) of the draft H2/Q3 roadmap surfaced earlier today. Full synthesis: [program/meetings/2026-07-13-h2-plan-review-ai-sdlc.md](./meetings/2026-07-13-h2-plan-review-ai-sdlc.md).

**Major strategic pivot: embed-with-teams-first, not pipeline-first.** The team had been planning to build the AWS PEV pipeline first, then apply it across repos. Chuck, Travis, and Cory converged on reversing that order: pause heavy pipeline build-out, embed with target teams to find why they can't move faster (missing repo-specific skills, no reliable code-review agent, etc.), fix those specific gaps directly, and let the "software factory" emerge from doing that repeatedly. Chuck's reasoning: 85% of the codebase lacks the curated skills that make tools like the LTE/Labor Flex workflow fast — building those skills is the actual velocity unlock, not a production pipeline. Work already done (Sonar, architecture decisions) is still considered valuable and reusable either way. **This changes the near-term build priority described in the [Go-Forward Architecture Candidate](#go-forward-architecture-candidate-2026-07-08-langgraph-deepagents-pev-on-aws) section above** — that architecture is not abandoned, but the AWS production account build-out described there is now deprioritized in favor of manual, repo-by-repo skill-building on the top 10 most-committed repos (excluding innovation repos).

**Pilot scope narrowed to 1-2 teams, not all 18.** Cory argued that rolling AIDLC out across all 18 teams simultaneously in H2 is too risky and would cause gridlock. Agreed approach: prove measurable velocity improvement (e.g., cycle time from 8 days to 3-4) on one or two core teams first, then expand org-wide starting H1 next year. This narrows/supersedes the Sapphire/Basalt-over-Breakfix pilot-team framing from 2026-07-08 — specific team(s) still TBD.

**Mid-August PR-merge milestone formalized as a Q3 OKR.** Kelley Manuel proposed pinning "a PR merged via the factory process by mid-August" as an explicit Q3 objective (with a Q4 follow-on metric, e.g. number of repos with a merged PR); Cory directed Chuck and Redwan to lock this into the OKR tracker.

**Resourcing / single-threaded-leadership risk raised directly.** Redwan flagged that neither he nor Chuck is fully dedicated to this initiative and asked for single-threaded ownership in Q3 — consistent with the 2026-07-08 resourcing gap note above (Cory wanting an FTE 100% dedicated). Redwan also flagged that current contractor capacity assumptions (6 people-sprints scaled to 18 for Q3) are unproven given how recently contractors started.

**Automated testing reaffirmed as a separate-but-dependent workstream.** Shane Kinder pushed that none of this scales across the core product surface without automated testing coverage in place; Cory confirmed testing stays a separate initiative for now, with an open question on whether/how it converges with AIDLC later.

**July 15 PEX leadership demo confirmed to proceed** regardless of the pivot — Cory saw no reason to hold it. The [open PEV-completeness risk](#h2-roadmap-drafting-underway-harness-poc-pev-completeness-unconfirmed-2026-07-13) flagged earlier today is still unresolved as of this meeting.

---

### Weekly Cadence Outcomes (2026-06-02)

- **Factory scope sharpened — narrow charter holds.** Joe pushed a holistic "AI enablement for all of FN" framing (no centralized governance, everyone doing their own thing → chaos). Chuck held the line: the Factory's charter is building production-ready *platform features from specs*, not solving the org-wide problem of letting product teams deploy prototypes or internal agents. Both agree the broader gap is real but it's *nobody's* job today — hence the recurring push to Cory for a dedicated "COO-type" AI owner.
- **Chuck's four-problem-spaces framing.** (1) customer-facing agentic → Pearl; (2) production platform features end-to-end → Moissanite/us; (3) product-team prototype deployment → buy a tool (e.g., revisit Replit); (4) internal-tooling agents → enterprise tool (Glean/Guru). One tool won't span all four.
- **Unblocked re-prioritized (reverses the 2026-05-05 deprioritization).** Consensus that buy-over-build via Unblocked solves the cross-cutting context (Tier 3) problem without internal maintenance burden. Chuck now owns the Unblocked evaluation/implementation. Sonar continues on a parallel track (timeline from Jack). Both targeted for go/no-go the week of June 15–19.
- **POC reframed for apples-to-apples comparison.** Same use case — a planning agent connected to Linear that writes a plan back — built on each option. Explicitly separates two layers: **agent stack** (Deep Agents / MS Agent Framework / Google ADK) vs. **operational platform** (Azure AI Foundry / Gemini Enterprise / managed Deep Agents). Joe owns the POC (Chuck took Sonar off his plate); target reviewable by ~June 15. **Deep Agents added** to the candidate set — recent coding/long-running-agent improvements are compelling and it parallels Pearl's Pydantic/LangChain work.
- **Hosting mandate aligned.** Agents default to one of three approved operational platforms — Gemini, Azure, or managed Deep Agents — unless there's a specific reason to roll your own. Less concern about standardizing the *framework* than the *hosting* platform. Both flagged MS and Google as uphill adoption battles (Travis/BD was "basically hell no on Microsoft").
- **Harness selection (M3 goal) deadline pushed June 2 → June 9.**
- **OKRs trending red.** Chuck owned being the bottleneck — BD travel, PTO (out June 8–12), and exploding break-fix work.

**Full synthesis:** [knowledge/meetings/2026-06-02-team-moissanite-weekly-cadence-1.md](../knowledge/meetings/2026-06-02-team-moissanite-weekly-cadence-1.md)

---

## What Is AI-DLC?

Field Nation is building an **AI-First Product, Engineering, & Design (PEX) Operating Model** under the umbrella initiative now called **AI-DLC** (rebranded 2026-07-09 from "AI Software Factory," which had briefly been called "AI SDLC" as of 2026-07-08). The goal is not to replace people but to restructure the relationship between human judgment and artifact production. Humans move from *producers* of artifacts to *directors and reviewers* of artifacts.

AI-DLC has two workstreams: the **AI Software Factory** (technical build — the production PEV pipeline) and **AI-DLC (Process Transformation)** (turning Field Nation's current SDLC into a more AI-native process). The rest of this section, and most of the doc below, describes the model as it applies to the Factory workstream specifically, since that's where the work has concretely happened so far.

The initiative is owned by Cory Muir (Sr. Director, Software Engineering — Chuck's manager). Chuck's role is cross-cutting: architecture, technical design, POCs, mentoring, and code review across teams implementing this model.

---

## The Two Operating Models

The doc defines two complementary models:

### 1. PEX Operating Model (Product, Engineering, Design)
Covers how work flows from strategic planning through engineering handoff — the upstream pipeline.

### 2. Engineering Operating Model
Covers how engineering teams take the handoff package and turn it into production software using agentic development practices.

---

## Core Principles (PEX)

1. **Humans Decide, AI Produces** — Humans are directors/reviewers; AI produces artifacts
2. **Review Gates Over Handoff Walls** — Continuous AI passes with human checkpoints replace hard handoffs
3. **Parallel Evaluation Replaces Sequential Opinion** — Multiple AI evaluations run simultaneously (HCI, technical feasibility, business viability)
4. **Prototype Before You Specify** — Build to learn, then specify with concrete evidence
5. **Quality Is Automated, Not Ceremonial** — AI-assisted code review, automated test generation, architecture compliance checking
6. **Context Is a First-Class Asset** — Structured knowledge files (CLAUDE.md, platform briefs, decision logs) maintained as living documents

## Core Principles (Engineering)

1. **Plan, Execute, Verify (PEV Loop)** — The foundational agentic dev workflow
2. **Code Health Is Critical for AI Readiness** — Healthy codebases are a prerequisite for agentic development
3. **Humans Orchestrate, Agents Execute** — Engineers shift from code authors to code directors
4. **Agent-Scale Development Requires Agent-Scale Safety** — Security must be embedded in the dev harness
5. **Encode Everything, Assume Nothing** — CLAUDE.md / AGENTS.md files are the OS for agentic development
6. **Speed Amplifies Both Good Design and Bad Decisions** — AI amplifies whatever patterns exist in the codebase

---

## The Foundation Layer: Knowledge Infrastructure

The single most important prerequisite for AI-first workflows. Four required knowledge assets:

| Asset | Owner | Update Cadence |
|---|---|---|
| **Business Context** | Group Product Leads | Quarterly minimum |
| **Platform Architecture Brief** | Architecture Leads | When significant architectural changes ship |
| **Brand & Design Guidelines** | Group Design Leads | When design system evolves |
| **Decision Log** | Each PEX team | Per significant decision |

Stale knowledge assets degrade every AI-assisted workflow. These are not optional — they are the fuel.

---

## The End-to-End Process (PEX)

### Step 1: Problem Discovery (AI-Accelerated)
- **Before:** 1-2 weeks, PM writes docs manually
- **After:** 1-3 days using PM Playbook Step 1
- Multi-pass: Domain Extraction → Persona Development → Critical Challenge → **Review Gate** → Document Synthesis → Fresh-Eyes Review
- PM curates inputs and makes judgment calls at the gate; AI produces the prose

### Step 2: Solution Exploration (AI-Parallelized)
- **Before:** 2-4 weeks
- **After:** 3-5 days using PM Playbook Step 2
- Parallel evaluations: HCI, Technical Feasibility, Business Viability + per-persona walkthroughs
- All feed into a scoring matrix with weighted totals and sensitivity analysis

### Step 3-4: Prototype Design & Build (AI-Generated)
- **Before:** 4-8 weeks (linear design-to-build pipeline)
- **After:** 1-2 weeks rapid prototyping cycle
- Generates a *working interactive prototype*, not a static mockup
- Critical output: **Prototype Learnings document** — what was harder than expected, what was discovered, what feeds production build

### Step 5: Engineering Handoff (AI-Drafted, Human-Refined)
- **Before:** 1-2 weeks
- **After:** 2-4 days using PM Playbook Step 5
- Complete package: User Stories + Acceptance Criteria, Technical Specification, Risk Register, Scope & Phasing Recommendations, Completeness Review
- Engineering team's first task: *validate* the spec against actual codebase, not author one from scratch

### Step 6: Delivery Execution (AI-Augmented)
- Sprint cadence unchanged (2-week increments)
- AI-Assisted Development (Claude Code, Copilot)
- AI-First Code Review (mandatory AI pass before human review)
- AI-Assisted Testing (acceptance criteria → test scaffolding)
- AI-Powered Incident Response (root cause analysis, auto-draft regression tests)

---

## The Engineering Execution Lifecycle

### Handoff Intake & Technical Validation
1. **Handoff Validation** — Verify stories have machine-testable acceptance criteria, spec is implementable, risks are documented
2. **Architecture Lead Review** — 1-2 days to validate AI-generated spec vs. actual codebase (not weeks to author from scratch)
3. **Team Lead Decomposition** — Stories → implementation tasks; classify each as Agent-Primary / Human-Primary / Hybrid
4. **Codebase Context Preparation** — Update CLAUDE.md with initiative-specific context, verify test infrastructure health

### The PEV Loop (Plan → Execute → Verify)
- **Plan (Human-Led):** Define files to modify, expected behavior, testing requirements, constraints
- **Execute (Agent-Led):** AI implements autonomously within plan boundaries
- **Verify (Human-Led):** Engineer reviews against plan, acceptance criteria, tech spec — this is not cursory

### Multi-Agent Task Pipeline (for complex features)
1. Feature Author Agent — core implementation
2. Test Generator Agent — unit, integration, E2E tests
3. Code Reviewer Agent — patterns, standards, security
4. Documentation Agent — API docs, README, code comments

### The 3-Layer Code Review
- **Layer 1:** Real-time guardrails during development (linting, type checking, security scanning)
- **Layer 2:** AI Code Review (pre-human) — checks coverage tiers, security, architectural compliance, API contracts, standards
- **Layer 3:** Human Code Review (post-AI) — architectural fitness, business logic correctness, design decisions, security-sensitive paths

Human reviewers focus on judgment calls; AI handles mechanical checks.

---

## Agentic Development Tiers

| Tier | Work Type | Agent % | Human Role |
|---|---|---|---|
| **Tier 1: Agent-Primary** | New API endpoint, CRUD, test writing, docs, routine migrations | 80-90% | Plan, review, merge |
| **Tier 2: Hybrid** | Complex business logic, new integrations, refactors | 50-70% | Handle complex sections, review everything |
| **Tier 3: Human-Primary** | New service architecture, auth/authz flows, performance-critical, complex data migrations | 10-30% | Agent assists with boilerplate/tests/docs only |

**Never agent-primary (always human review):** authentication, payment processing, PII handling, rate limiting, database access for sensitive data.

---

## Agent-Ready Definition of Ready

Everything in the current DoR, plus:
- CLAUDE.md / AGENTS.md updated with relevant patterns/constraints
- Test infrastructure verified (CI green, coverage baseline documented)
- Agentic classification assigned (Agent-Primary / Human-Primary / Hybrid)
- Acceptance criteria are machine-testable
- Security sensitivity flagged

---

## AI-First Definition of Done (additions)

**Code Quality additions:**
- AI review report (Layer 2) shows no unresolved blocking findings

**Documentation / Knowledge Layer additions:**
- CLAUDE.md / AGENTS.md updated if new patterns were established
- Platform Architecture Brief updated if service boundaries, APIs, data models, dependencies, or infra changed
- Service Dependency Map updated if dependencies changed
- Decision Log updated for significant architectural decisions
- API Catalog (OpenAPI specs) updated

**Security additions:**
- AI security scan completed, no unresolved critical findings
- Security-sensitive code paths confirmed as human-reviewed

**Agentic Quality additions:**
- Agent-generated code clearly attributed in commit messages or PR metadata
- Test coverage meets tiered thresholds (enforced by gates)
- No hallucinated dependencies

---

## Test Coverage Tiers (unchanged thresholds, automated enforcement)

| Service Type | Coverage Requirement |
|---|---|
| FN-core and existing microservices | +5% per PR on modified files |
| Business-critical services (payments, work order creation, matching) | +20% per PR on modified files |
| New microservices | 80% minimum from launch |
| Services at 80%+ | No reduction allowed |

---

## Knowledge Layer (CLAUDE.md) per Service — Minimum Viable

Every service needs a CLAUDE.md with at minimum:
1. **Service Overview** (50-100 words) — what it does, dependencies
2. **Development Patterns** (200-300 words) — how to write endpoints/errors/tests, with real examples
3. **Common Gotchas** (100-200 words) — what developers learned the hard way
4. **Code Examples** — actual code snippets: endpoint, test, error handling
5. **Forbidden Patterns** — anti-patterns, performance pitfalls, security considerations

**Update triggers:** After code review reveals a gap; sprint end; monthly Architecture Lead validation; quarterly full review.

---

## How Chuck's Role Fits In

Chuck is a Sr. Principal Software Engineer — the Architecture Lead archetype described in the model. Per the doc, the Architecture Lead in the AI-first world:

- Becomes the **primary validator of AI-generated technical specifications**
- Guards **platform-wide architectural consistency**
- Maintains the **Platform Architecture Brief** (the living doc feeding all AI passes)
- Defines/updates **AGENTS.md patterns** for cross-service architectural constraints
- Reviews **AI-generated impact analysis** for major changes
- Uses AI to **continuously scan for architectural drift**, dependency issues, compliance violations
- Shifts from *authoring* tech designs → *reviewing and refining* AI-generated specifications

This is where Chuck's bias-to-action and technical-to-non-technical translation skills are most valuable: connecting the AI-generated artifacts to reality, catching what AI gets wrong, and keeping the knowledge layer honest.

---

## The PM Playbook

The PM Rapid Prototyping Playbook (Steps 1-5) is the core workflow tool for the PEX model. Steps are implemented as AI skills in this workspace:

- `pm-step-1` — Problem Discovery
- `pm-step-2` — Solution Exploration
- `pm-step-3` — Prototype Design
- `pm-step-4` — Prototype Build
- `pm-step-5` — Engineering Handoff

Each step runs multiple AI passes with review gates. The PM validates at checkpoints before the next phase begins.

---

## Key Metrics

**Velocity:** Issue/PR Cycle Time, PRs completed, ratio of AI PRs vs. Engineer PRs

**Quality:** Change Failure Rate (must remain stable), defect density (AI vs. human code), test coverage trajectory, security vulnerability density

**Agent Effectiveness:**
- Agent PR first-pass success rate (target 70%+; below 50% = audit CLAUDE.md)
- CLAUDE.md quality score (measured by agent success rate on the service)

**Knowledge Layer Health:**
- Platform Architecture Brief currency (sections updated within 90 days relative to active deliveries)
- CLAUDE.md completeness (5 required sections for all actively maintained services)
- Knowledge layer update compliance (% of deliveries completing required updates as part of DoD)

---

## Common Agent Failure Modes (watch for these in Verify phase)

1. **Hallucinated Constraints** — Agent invents constraints that don't exist; validate all constraints are sourced in CLAUDE.md or spec
2. **Overly Generic Solutions** — Correct but doesn't follow team patterns; verify against CLAUDE.md examples
3. **Incomplete Edge Case Handling** — Happy path is solid, edge cases thin; check for edge-case test assertions
4. **Security Oversights** — Syntactically correct but with security implications missed; never delegate auth/payment/PII to agent-primary
5. **Inconsistent Abstractions** — New code duplicates existing logic agent didn't recognize; watch for duplication
6. **Assumption-Based Failures** — Agent makes one wrong critical assumption (e.g., horizontal vs. vertical scaling)

**Strategy:** Each failure mode usually indicates a CLAUDE.md gap. Update the context file so the next agent benefits.

---

## What Stays the Same

- Two-week delivery increments
- Team check-ins (2-3/week, 15 min, board right-to-left)
- WIP limits (Team Size or Team Size + 1)
- KTLO allocation model
- Escalation paths
- Bi-weekly Quality Review Meeting
- Technical Design Reviews for novel decisions
- Deployment pipeline (feature flags, integration env, product acceptance, CVV, fit-and-finish)
- Team type distinction (Product Delivery Teams vs. Platform & Enablement Teams)

---

## Recent Activity: ADR Skills in PR Review, Harness CI Blocker, Licensing Question (2026-09-18 — 2026-09-21)

**ADR Skills (MOI-62) moving to ai-toolbox PR review:** Chuck reviewed `ai-toolbox PR #79` (2026-09-18) which carries three ADR skills (`fn-adr-create`, `fn-adr-enforce`, `fn-adr-review`) in their final form from earlier iterations. Skills handle judgment calls (is this decision worth recording? does it break a prior decision? does an old one still hold?); deterministic checks live separately in `ai-repo-score` as `adr-tools` Python CLI. All three skills ship in `fn-software-engineering` v1.1.0.

**Third-party licensing compliance gap surfaced:** Asad Ali flagged (2026-09-21) that `fn-adr-create/references/` contains MIT-licensed code adapted from `mattpocock/skills` with only inline attribution, no SHA, no THIRD-PARTY file, and the plugin declares itself proprietary. **Open question:** Does FN have a standard intake process (FOSSA/Black Duck/SBOM/legal form) for pulling third-party code into repos? If so, Asad proposes treating notice files as the *output* of that process, not a substitute. No resolution; Chuck has not yet responded. See references.md for vendor contact: Matt Pocock.

**Harness CI setup blocked on admin access:** Vitalii flagged (2026-09-18) that ai-toolbox still needs before going live: (1) PR trigger UI config in Harness (YAML ready, needs admin click), (2) GitHub connector secrets access for the AI Guild without it, (3) commit-status write on the GitHub App. No resolution yet; likely Joe Hoppesch owns this pending his return from labor-day period.

**New ticket MOI-125: Global Developer Readiness Checks:** Chuck created (2026-09-21) a new ticket scoped to add "global" checks to the repo bootstrap — follow-on from MOI-112 (bootstrap work) wrapping up. Appears to be checks that apply across all repos, not per-repo. Status: Just created, assigned context unclear.

**Async standups continuing:** 2026-09-17 async due to AI Guild meeting; 2026-09-14 async due to engineering all-hands conflict; pattern suggests either recurring conflicts or team preference for async flexibility. No formal meeting transcript available for 2026-09-21 (Saturday evening — likely no standup scheduled).

---

## Gitar Validation Lands; Harness Service Account Unblocks CI; Packaging Timeline Clear (2026-09-18 – 2026-09-22)

**Gitar code-review validation from a second team member (Mykhailo Sakhnik, 2026-09-18 Google Meet).** Chuck scheduled a live demo call with Mykhailo to walk through Gitar's actual output on real PRs. Mykhailo's reaction: *"gitar really provides review like human!"* and *"it gives the prompt for an agent!"* — genuine independent enthusiasm for review quality and agent-readiness of the output format. This is **the first substantive external validation** of Gitar's capability from someone other than Chuck, and a material signal favoring O1's code-review decision over the alternatives (Tessl, native Copilot, bespoke `fn-review-agent`). Addresses the long-standing tension between Chuck's requirement ("show how you reviewed") and Gitar's design philosophy ("keep it terse") — the demo showed Gitar's verdicts are indeed agent-compatible despite the terseness gap. See [knowledge/meetings/2026-09-18-team-moissanite-daily-standup.md](../knowledge/meetings/2026-09-18-team-moissanite-daily-standup.md).

**Harness service account for Gitar integration created; Joe Hoppesch unblocked CI pathway (2026-09-18).** Joe set up a dedicated Gitar service account in Harness and stored credentials in 1Password (`start.1password.com/open/i?a=7SQ3GK6VWFBTBBUGJWMWCEDG54&v=jahzrdpx2jmvfwljc3icyyxdsa&i=fx6dabicvnfq43kz3k5wi4fjei&h=fieldnation-team.1password.com`), unblocking integration into FN's CI pipeline for broader testing beyond the initial trial scope. No timeline slips introduced.

**Sonar enhancements in motion (2026-09-18).** Chuck requested two capabilities from Gautam Korlam (Sonar): (1) a **risk-assessment dashboard per PR**, so teams can review and adjust risk thresholds dynamically (Gautam committed to adding and surfacing it in multiple places); (2) **Atlassian Confluence connector integration** feeding design-context guidance into code reviews (currently in Sonar's internal dogfood, targeted availability: next week). First concrete linkage between code review quality and upstream design-context richness.

**Packaging roadmap solidified; Apple Developer ID signing identified as critical path (2026-09-18 → 2026-09-22).** Mykhailo and Chuck scoped out the macOS packaging work:
- **MOI-118 (Package ai-repo-readiness CLI):** Implementation complete locally; PR #14 (UI runner) awaiting review/merge.
- **MOI-119 (Add macOS CI packaging):** In Review; local signing validation complete per PR comments.
- **MOI-124 (Add macOS Developer ID signing + notarization):** New, critical path for release automation. Chuck sourced FN's existing Apple Developer account (mobile app team) and offered to set up GitHub Actions secrets. Mykhailo will send exact secret names/format needed, then set up Workflow Dispatch once secrets are in place. No timeline slips expected once secrets are configured; PR #14 merge is prerequisite.

**Personnel & capacity (2026-09-18 → 2026-09-22).** Vitalii Belinskyi continues compressed hours (evenings/nights CDT) for family care (grandmother's Parkinson's symptoms flare-up). Joe may pull Vitalii for a separate project the week of 2026-09-22, but split-time arrangement is confirmed available — no timeline impact on AI-DLC work. Handover planning is underway between Vitalii and Mykhailo for repo bootstrap + Harness 911 close-out (target end of week ~2026-09-20).

**Open blockers (end of day 2026-09-22).** Apple credentials setup (sourcing/GitHub Actions secret creation), PR #14 merge, Harness admin config (PR trigger UI, GitHub connector secrets, commit-status write on GitHub App). None are timeline-critical as of this sync; all have clear ownership and paths to resolution.

**Signals summary.** Gitar code-review validation is a material step forward on the 3-month open O1 decision. Harness integration unblocked. Packaging/CLI distribution pathway clear. Sonar enhancements incoming. Two concurrent workstreams (ADR skills in review, packaging/macOS release) both progressing smoothly with no new blockers emerging.


---

## Repo Bootstrap Packaging Reaching Final Testing; Basalt Integration Repos Added to Embed Scope (2026-09-24)

**Daily standup moved to async due to AI Guild conflict (2026-09-24).** Chuck and Mykhailo conducted standup via Slack thread due to the recurring AI Guild weekly meeting. Three concurrent packaging tickets nearing completion:
- **MOI-131 (Add prompt command to repo-bootstrap):** PR ready for Chuck review.
- **MOI-132 (Standardize ai-repo-readiness workflow names):** Small quality improvement, PR ready.
- **MOI-113 (Package repo bootstrap tool for easy install):** In final testing; PR drafted; release workflow will roll out automatically on GitHub after merge. **Signal:** CLI distribution reaching general-availability status.

**Basalt integration repos identified for Sonar + agentic improvements (2026-09-24).** Chuck started work on bringing Basalt team's integration repos into Sonar (for quality scanning) and improving their agentic shape using established prompts. Signal: Expanding the embed-first model beyond the initial Q3 pilot scope to adjacent teams/repos as proof-of-concept results consolidate.

**Documentation quality focus during review (2026-09-21).** Chuck flagged PR #4 (ai-repo-score) for containing obviously AI-written language in README and playbook sections (references to unfinished work, meta-commentary). Requested rewrites using human-readable voice rather than AI-native verbose style. Vitalii implemented fixes; PR marked "Done." Signal: Team establishing consistency expectations on documentation tone as part of the quality bar (reusable skills and playbooks must be consumable by engineers, not just parseable by machines).

**Capacity & personnel (2026-09-24).** Vitalii continues compressed hours for family care (grandmother's Parkinson's); Asad noted a prior payroll conflict (2026-09-23, now resolved). No new timeline risks.

**No new blockers as of standup.** Chuck committed to reviewing Mykhailo's pending PRs this morning.

---

## ADR Rollout Path Clear; MOI-105 Evaluation Complete; Async Standup (Chuck Unwell) (2026-09-25)

**Async standup (Chuck unwell, 2026-09-25 07:18 CDT).** Today's standup moved to Slack thread; Chuck woke up not feeling well and requested async updates from the team.

**ADR Skills (MOI-62) rollout path confirmed.** Asad's three skills (`fn-adr-create`, `fn-adr-enforce`, `fn-adr-review`) shipped in ai-toolbox PR #79 (under Chuck review as of 2026-09-18). Deterministic Python CLI tooling (`adr-tools` in ai-repo-score) complete. Ships as `fn-software-engineering` v1.1.0. **Two format options still TBD:** (A) numbered files (matches buyer-reporting convention); (B) versioned by topic (team's Draft v4 preference). Asad tested Option A against a dummy repo; awaiting team feedback on format choice before raising official PRs. **Four rollout blockers identified:** (1) ai-repo-score v1 tag not cut, (2) uvx auth against private repos unresolved, (3) cross-repo reusable workflows need enabling, (4) filename convention still open. See [knowledge/tech-context.md](../knowledge/tech-context.md).

**MOI-105 (Harness Evals vs Deep Eval) evaluation complete.** Asad completed evaluation comparing Harness Evals against the team's custom Deep Eval setup (2026-09-25 06:52 CDT). Evaluation scripts ready for team review; findings will be posted to Linear. **Signal:** platform decision (move toward Harness evals vs. keep custom Deep Eval) can now proceed with concrete data. Scripts written for evaluation only, no merge required.

**Gitar remains O1 code review choice (confirmed 2026-09-14).** MOI-109 (Gitar integration into repo-readiness score) in progress (Mykhailo owns). Copilot confirmed fully off org-wide as of 2026-09-15; Gitar is sole reviewer running. Repo-readiness rubric v2 improvements underway.

**Harness CI integration remains blocked on three items.** From 2026-09-18: (1) PR trigger UI config in Harness (YAML ready, needs admin), (2) GitHub connector secrets for AI Guild, (3) commit-status write on GitHub App. No resolution as of 2026-09-25; blocking toolbox create-trigger workflow setup. Joe Hoppesch owns.

**Team quality expectation formalized (from 2026-09-24).** Chuck flagged in last standup: PR descriptions and Linear tickets too AI-heavy to review easily. Each owner responsible for using skills like `fn-write-clearly` to humanize agent output. Not yet a formal ticket but a live expectation — watch for patterns.

**Capacity & personnel.** Vitalii continues compressed hours for family care (grandmother's Parkinson's). Asad had payroll conflict (resolved). No new timeline risks.

**Summary.** ADR rollout has clear path; awaiting format decision + four blockers. MOI-105 complete, unblocks Harness-vs-Deep-Eval decision. Gitar on track. Harness CI needs admin action. Team calibrating on output quality. No scope changes, timeline slips, or staffing issues.

---

## AI Enablement Session with engineering-dbt (Emerald): Pre-Ticket Friction Named as the Real Bottleneck (2026-09-17)

Chuck held the first direct feedback session with engineering-dbt (Emerald) team engineers (Rakibul Hoque, Amran Hossain, Imtiaz Chowdhury, Tanin Rahman, Ruzlan Karim, Rezaul Karim), plus Mike Mueller, to understand why true throughput has stayed flat at ~1.4 PRs/engineer/week despite the repo scorecard work run roughly two weeks prior. **Headline finding: the bottleneck is pre-ticket, not development itself.** Issue cycle time once a ticket is picked up is healthy; the real time sink is identifying the correct data source across warehouse layers, deciding which DBT layer a model belongs in, chasing down undocumented business logic with product/business teams, and manually cross-checking for duplicate or conflicting column definitions across models. This reframes what "fixing engineering velocity" means for this repo, and Chuck flagged it as an important data point to bring back to Cory and Travis regardless of which way it points.

**Mixed signal on whether agents can help with layer-placement decisions.** Imtiaz argued this requires human judgment about future reuse; Mike Mueller pushed back, arguing agents could at least prompt engineers with the right guiding questions. A step-by-step layer-placement skill and ADR guidance were already built from the team's own documentation, but no one in the room confirmed having actually tried it, so its real-world effectiveness is still unverified. Mike separately suggested DBT lineage tracing may need to be a cross-repo capability rather than something scoped to engineering-dbt alone.

**Business logic validation and data governance are the parts hardest to solve with agents alone.** SQL doesn't encode business rules, so engineers have to manually consult product/business teams to validate conditions; data-governance definitions (dimensions, measures, columns) live in a manually maintained Google Sheet, updated roughly weekly, with no automated validation against it today. Rakibul proposed a skill that auto-approves low-risk changes against that spreadsheet and routes only ambiguous ones for manual review.

**One clear positive data point:** Tanin Rahman (working with the Marble team) said the existing repo-standards and layer-placement skills worked well across a full sprint — the only remaining friction for that team is the same business-logic/data-source-discovery gap everyone else raised.

**Next steps:** Rezaul Karim will send Chuck the link to a rough ML-team hackathon project (a knowledge graph connecting schemas, product definitions, and Jira/migration data) for async review as a possible foundation for the data-source-discovery problem; Chuck will also scope a data-governance cross-reference skill. Chuck was explicit throughout that engineering-dbt was picked as the embed test bed for its healthy backlog and multi-team contribution pattern, not as a performance critique, and that he was running the same conversation with other embed teams the same day (mentioned "Basalt"). See [knowledge/meetings/2026-09-17-ai-enablement-engineering-dbt.md](../knowledge/meetings/2026-09-17-ai-enablement-engineering-dbt.md) and [program/people.md](./people.md#engineering-dbt-emerald-team).
