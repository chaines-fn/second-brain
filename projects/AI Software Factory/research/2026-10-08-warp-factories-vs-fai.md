---
title: Warp Factories vs. the Fai design
date: 2026-10-08
status: research, not a decision record
inputs: Warp factories docs (overview, how factories work, factory agents, definitions as code, scorers, benchmarks, self-improvement, infrastructure and security, plans check); Warp blog "A Guide to Cloud Software Factories for Engineering Leaders" (published 2026-07-07); program/OKRs/q4-2026-okrs.md (adopted 2026-10-07); artifacts/2026-10-07-fai-aws-architecture-draft.md; knowledge/industry-landscape.md (existing Warp entry)
---

# Warp Factories vs. the Fai design

Chuck asked whether Warp's factory docs change what the Fai factory should look like.

**Reading caveat.** Warp's pages were read through a summarizing fetch, not as raw text. Field names and exact wording should be checked against the source before anyone relies on them in a decision. Several pages leave important things undocumented; those are listed at the end.

## Bottom line

Warp mostly confirms the shape of the Fai draft: agents defined in git, PR-only output, humans merge, and outcome-based measurement. It does not change the core architecture.

It does expose one real gap in the draft: the feedback loop after a run. The draft has no way to grade production runs over time, compare configurations on fixed tasks, or turn repeated failures into reviewed changes. Warp has all three.

Warp also raises two structural questions: where a factory boundary should sit, and whether a managed control plane belongs in the Q4 platform decision as a comparator. I would not switch to Warp on the strength of these docs. Several parts of Warp's design are weaker than what the draft already specifies, notably control flow, enforced gates, and portability.

## What Warp Factories is

Warp describes a factory as a team of cloud agents attached to a set of repositories. Work arrives from Slack, Teams, GitHub, GitLab, Azure DevOps, Linear, or Jira, or from schedules, webhooks, and a Factory MCP that local coding agents can call. A foreman agent routes each work item through Intake, Triage, Planning, Building, Reviewing, and Human handoff, and returns a pull request. The default agents are triage, spec, implement, and review. Each agent can run on Warp Agent, Claude Code, or Codex, with its own model.

Definitions live in a versioned tree rooted at `factory.yaml`. A factory is either Warp-managed or stored in a GitHub repository, where changes go through PRs and a `warp/factory-config` validation check. Warp applies a definition atomically and keeps the last valid one if validation fails. The product carries an Early Access label.

## Where Warp changes my view

### 1. Add a feedback layer: scorers, benchmarks, and self-improvement

This is the largest change. The Fai draft has two evaluation points. Evals gate changes to an agent definition in CI, and the run ledger records cost, trace, and outcome for the KR numbers. Nothing grades production runs over time, and nothing compares configurations on the same tasks.

Warp has all three pieces. Scorers are LLM judges. Each one asks a single question about a sample of completed runs (the docs' example: did the agent run tests before opening the PR), assigns a labeled classification with a score, and checks it against a pass threshold. The docs do not describe deterministic checks, so for Fai the CI tests, PR checks, and Gitar and Sonar output should remain the deterministic layer, with a judge as a second layer.

Benchmarks run one agent across fixed tasks and several configurations (model, harness, runner), with correctness criteria per task. The output is a correctness-versus-cost comparison. Warp advises changing one configuration at a time.

Self-improvement groups repeated scorer failures per agent and starts a follow-up run that proposes a change to code, prompts, skills, or config. The change arrives as a PR for review. Warp states that nothing is adopted automatically.

**Why it matters for Fai.** The Q4 OKR's decision gate asks the platform spike which harness and host to use (Strands, OpenCode, pi, and the AgentCore harness are all named). Those are being answered with one-off spikes against one shared test. A benchmark suite would give a repeatable comparison that keeps working as models change. The Q4 risk section also says early output "may need significant human rework." A sampled scorer plus a self-improvement loop turns that rework into a measured trend instead of an impression.

**Two cautions.** First, Warp's default self-improvement trigger is 25 distinct unreviewed failures, or the oldest failure reaching seven days. At Q4's target of 20 completed issues across the whole quarter, that threshold may never fire. Fai should set its threshold explicitly (the docs allow 1 to 50 in GitHub-backed factories) or rely on manual runs. Second, a loop that proposes changes to prompts and skills is the self-modifying risk the Fai draft already names. Its proposals must go through the same registry PR path as the builder agent: CODEOWNERS review, the eval gate, and no merge authority for the agent that proposed the change.

### 2. Make the factory boundary explicit

Warp's sizing advice is to group repositories that ship together into one factory, and to keep separate products in separate factories. It advises against splitting one set of repos by team.

The Fai draft currently implies one platform, one agent registry, one GitHub App for all of Fai, and a pilot that spans Emerald (`engineering-dbt`), Core UI, and FN Core. Those three are a data platform, a UI codebase, and a PHP monolith with different sandbox needs. The draft already flags the fn-core image size as unknown.

Proposed change: keep one shared platform (gateway, ledger, registry tooling, run contract) and define a factory per product group, each with its own agent set, skills, secrets, scorers, and benchmark suites. The GitHub App question (one App or one per factory) follows from this choice. This is a question for Chuck and Cory before it is a design change.

### 3. Give the lifecycle explicit states, and define "complete" as handoff

Warp's stages are Intake, Triage, Planning, Building, Reviewing, Human handoff, then Complete or Cancelled. Warp is explicit that "complete" means handed to a person, not merged or deployed.

The Fai draft has the ledger fields but no state model. Adopt the stage vocabulary in the run ledger, and record merged and deployed as separate events. That lets the KR1 measure (first substantive work to accepted, merged PR) and the KR3 measures (deployment-level change failure rate, escaped defects) be computed from events rather than inferred.

### 4. Put the spec in the repo as a draft PR

Warp's spec agent writes product and technical specs, plus validation criteria, into a draft PR. The implement agent later continues that same branch, and a human approves the spec before implementation starts.

Fai has PRE-VERIFY, the SDD gate (MOI-49), and O1 KR2 requires a spec for every ticket, but the draft does not say where the spec lives. Put it in the target repo as a draft PR. It is then versioned, reviewable under the same gates as code, and the implementation continues the same branch, which gives the ledger a clean chain from spec to code.

### 5. Adopt the reviewer's verdict vocabulary and named checks

Warp's review agent is advisory. It checks unmet requirements, convention breaks, test gaps, security issues, and weak evidence, and it recommends accept, revise, or escalate. That is closer to the "publish how it reviewed" bar Chuck has set for Gitar than Gitar's terse verdicts, although Warp's docs do not say whether the review publishes its coverage.

For Fai, an in-factory reviewer with named checks and a three-way verdict would complement the external gates (Gitar and Sonar). It pairs with the REVIEW.md pattern in the code-abundance research. It also needs its own scorer, so its verdicts can be checked against what humans later find.

### 6. Route models by role and complexity at the gateway

Warp allows a different model per agent, recommends a different model for review than for implementation, and offers routers that choose a model by complexity or prompt. The superseded July design already split models by role (Opus for planning, a different model for plan review), but hard-coded them in each definition.

Put routing rules in the AI gateway. That makes the choice observable and changeable in one place, and the ledger can record which model served each run.

### 7. Add cost per merged PR and autonomy to the measurement set

Warp's dashboard shows autonomy, PR cycle time, cost per PR, and opened versus merged PRs. Fai's measures are better aligned with outcomes (issue cycle time, completed issues, change failure rate, escaped defects), and Q4 deliberately avoids counting agent runs. Still, Fai has no cost-per-accepted-change figure at all, which the code-abundance research flagged as missing. Add cost per merged PR as a secondary metric, and use "opened versus merged" as a sanity check on any throughput number.

### 8. Make attribution an explicit decision

Warp lets a factory choose whether runs act as the creating user or as the agent (a `credentialStrategy` setting). The Fai draft mints one GitHub App token per run, so PRs are authored by the App. The AI-First Definition of Done requires agent-generated code to be attributed, and the ledger already records the run and its requester.

Write the rule down before the walking skeleton: the App is the PR author, the requester is named in the PR body and the ledger, and each agent definition names a human owner.

### 9. Add a local-to-cloud handoff

Warp's Factory MCP lets a local coding agent submit work to the factory, so a task started on a laptop can end as a factory PR. That matches the principle Chuck set in April: one investment (skills, context files, standards) that serves both the factory and engineers' local tools.

The Fai draft lists humans via Slack and CLI as triggers but does not describe a local handoff. Add one, implemented as an ai-toolbox MCP tool, so local and cloud runs share the same skills and agent definitions.

### 10. Make deploys atomic, with keep-last-valid

Warp validates definitions as a required PR check, applies them atomically, and keeps the last valid definition if validation fails. The Fai CI validates `agent.yaml` on PR, but the deploy step should follow the same rule, so a failed deploy never leaves the agent index pointing at a partly applied definition.

## Where Warp does not change my view

**Deterministic control flow.** Warp's foreman is an LLM. It decides whether to skip Triage or Planning, sends work back, and continues conversations across agents. Its docs say skip criteria are judgment-based and do not state a retry limit. The Fai design puts Temporal in charge of control flow, with a hard retry cap and human waits as durable signals. That is the better fit for cost predictability and for the KR3 quality guardrail. A foreman-style conversational front end is a reasonable UX idea, but control flow should stay deterministic.

**Enforced gates.** Warp describes spec approval and other checkpoints as defaults in the foreman's instructions, which can be edited. It says merge control belongs to the repository. Fai enforces these with Cedar policy at the AgentCore Gateway, CODEOWNERS on the registry, and branch protection. Keep Fai's.

**Tool scoping.** Warp agents reach what their configuration includes and what the connected provider permits. Fai's tiered tool model, enforced by Gateway policy and IAM rather than by the definition, is stronger. Keep it.

**Portability.** Warp's definition format (`factory.yaml`, `agent.md` frontmatter with Warp-specific keys, runners, routers, scorers, webhooks) is proprietary. Only the SKILL.md format is an open standard, and Fai's skills already use it. The Fai run contract exists so hosts can be swapped. Warp's format would make Fai's definitions a lock-in risk for an Early Access product.

**Isolation documentation.** AgentCore Runtime documents microVM isolation with explicit resource limits. Warp's Warp-hosted isolation model is not described, and its Direct self-hosted mode shares the host's kernel. Keep AWS-native isolation as the default.

**Outcome measures.** Fai measures what the business cares about. Warp's headline dashboard metrics are mostly activity and cost. Keep Fai's frame, and borrow Warp's extra metrics as secondary only.

## Should Warp be a comparator or a candidate?

**The case for.** Warp covers nearly every part of the Fai design: intake, triage, spec, implement, review, measurement, self-improvement, and self-hosted execution. Adopting it could shorten the walking skeleton from weeks to days. The Q4 decision gate already tests vendor options (Workato is listed), so adding a managed control plane as a comparator costs little. Warp's Oz platform was considered earlier and dropped from the finalists in April (program/references.md), so this would be a second look.

**The case against, from the docs themselves.**

- The product is Early Access, and the pages give no terms.
- Retention for run records, transcripts, and artifacts is not documented. The audit log is not described. Egress controls for Warp-hosted sandboxes are not described. Data residency, compliance certifications, and internal staff access to transcripts are not described.
- Prompts, transcripts, and telemetry pass through Warp even with self-hosted workers. Factory inference is Warp-managed by default. Bringing FN's own Bedrock account (BYOLLM) is listed as an Enterprise option. That conflicts with the Fai design's in-boundary Bedrock path unless FN is on that tier.
- Third-party harnesses (Claude Code, Codex) require a paid plan, and the free plan runs only on Warp Agent. The pages I could reach give no pricing.
- The article behind the product is written by Warp's CEO. It makes build-versus-buy and ROI claims without independent evidence. Its "20-30% of issues are automatable" figure is unsourced, and Warp's own "20-30% of PRs" figure is self-reported.

**Recommendation.** Do not change the platform decision on these docs. If Chuck wants Warp evaluated, the honest version is a time-boxed comparator run against the same shared test the Q4 spike uses, preceded by a security and data-handling review. No FN source code should go into a Warp-hosted sandbox before that review. Whether to authorize that is Chuck's and security's decision.

## Unknowns in Warp's docs

- Foreman retry limits, and the criteria for skipping stages.
- What scorer judges see (transcript, diff, PR outcome, CI results), and whether any deterministic grading exists.
- How the benchmark Correctness Scorer decides success, and how repeatable its results are.
- Whether Warp-hosted sandboxes can restrict egress, and what isolation technology they use.
- Retention, audit log contents, and deletion.
- Pricing, and plan requirements beyond the Claude Code, Codex, and GitHub-backed items.
- How specific repositories are assigned to specific agents.
- Early Access terms and dates.

## Proposed next steps (proposals, not decisions)

1. Add a feedback-layer section to the Fai architecture draft covering scorers, benchmark suites, and the self-improvement loop, with the self-improvement threshold set for Q4 volume.
2. Take the factory-boundary question to Cory alongside the ownership questions already listed in the Q4 open items.
3. Add stage states, and the handoff-versus-merge definition, to the run ledger spec.
4. Decide the attribution rule and the spec location before the walking skeleton starts.
5. Decide whether Warp is a spike comparator. If yes, get the data-handling questions answered first.

## Sources

- [Warp factories overview](https://docs.warp.dev/factories/)
- [How Warp factories work](https://docs.warp.dev/factories/how-factories-work/)
- [Factory agents](https://docs.warp.dev/factories/factory-agents/)
- [Definitions as code](https://docs.warp.dev/factories/factory-as-code/)
- [Scorers](https://docs.warp.dev/factories/measure-and-improve/scorers/)
- [Benchmarks](https://docs.warp.dev/factories/benchmarks/)
- [Self-improvement](https://docs.warp.dev/factories/measure-and-improve/self-improvement/)
- [Infrastructure and security](https://docs.warp.dev/factories/infrastructure-and-security/)
- [A Guide to Cloud Software Factories for Engineering Leaders (Warp blog)](https://www.warp.dev/blog/a-guide-to-cloud-software-factories-for-engineering-leaders)
- Workspace: program/OKRs/q4-2026-okrs.md, artifacts/2026-10-07-fai-aws-architecture-draft.md, knowledge/industry-landscape.md, research/2026-08-25-code-abundance-thesis.md
