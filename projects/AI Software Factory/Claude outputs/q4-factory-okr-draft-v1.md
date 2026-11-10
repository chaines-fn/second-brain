# Q4 2026 OKR and Plan: Build the Factory (Fieldworks) - Draft v1

*Draft for Chuck, then Cory. 2026-10-07. v1 folds in Cory's Team OS structure proposal and the Team OS article. Targets marked "proposed" are placeholders to calibrate, not commitments. "Fieldworks" is still the provisional platform name from the 2026-10-05 brainstorm.*

## Why this matters to the company

**Theme: improve overall velocity through shared knowledge and agentic assistance.**

What H2 taught us, in our own numbers:

- Tooling alone has not moved throughput. Emerald is at roughly 1.4 PRs per engineer per week against a 3.5 target (from the 2027 roadmap draft).
- The Emerald sessions pointed at friction before the ticket exists (finding the right data source, placing a model in the right layer, chasing undocumented business logic). That is a knowledge and context problem, not a coding-speed problem.
- Once a PR is ready, someone still has to babysit it through to the merge. That attention cost is the real friction, more than any wait-time number.

The factory goes after all three. A shared knowledge layer means people and agents stop rediscovering context. Agents that take tickets from Linear to a reviewed PR take on toil and cut the babysitting. One place that shows cost and results means we can tell whether any of it is paying off. The cost of a change is a first-class number, not an afterthought.

One framing note: throughput is jagged and slow to move (you and Cory both said so on 9/30), so Q4 KRs below use leading indicators we control (factory runs, merged PRs, cost per accepted change, adoption) rather than promising a throughput jump this quarter.

## Objective

**Stand up Fieldworks, a factory that builds itself, so engineering velocity improves through shared knowledge and agentic assistance.**

## Key results

**KR1. Foundation is decided and running.**
Platform and harness decisions are recorded by Oct 21. By Nov 4, every factory run executes in a sandbox with its own agent identity, a trace, and a captured cost, and loads skills from ai-toolbox and the fn Claude marketplace.

**KR2. The factory builds itself.** *(headline proof point)*
By Dec 18, factory agents take tickets on the factory's own repos from Linear to a merged PR, passing Gitar and Sonar, with a human merging. Proposed targets: at least 10 factory-authored merged PRs, at least half of tickets labeled factory-eligible handled this way, and review acceptance comparable to human PRs (baseline to be measured in Nov). Factory work only counts as done when the relevant OS entries are updated (spec, ADR, artifact map entry), borrowing the launch gate from the Team OS article.

**KR3. Anyone on the team can create an agent in the factory, and companyOS is its first customer.**
By Dec 18, at least 3 agents are created through the factory (not hand-deployed) and running on triggers, and they keep the first companyOS instances current. Proposed scope is two instances from Cory's structure: FNOS (the company OS) and the Engineering Senior Leadership OS, each with the common core (README, AGENTS.md, goals, decisions, meeting records) and at least 2 weeks of real use. Candidate agents: meeting synthesis (writes records that link back to the source transcript and marks unknown owners and dates as unknown), project sync (keeps goals, initiatives and dependencies current), and a staleness check (flags artifacts past their re-review date or whose linked systems changed). Stretch: Engineering Artifact Map v0.

**KR4. Cost and results are visible.**
By Dec 16, cost per agent, per run and per accepted change is reported for 100% of factory runs, alongside ticket-to-merged-PR lead time for factory work, in a weekly report. We also track tokens per run, to test the hypothesis that well-indexed OS repos (root AGENTS.md plus nested indexes) keep context small and lower cost per run.

## What Cory's proposal and the Team OS article change

Cory's proposal describes an OS as the maintained context and practices that help a company, function, leadership forum or team operate. It is explicitly not necessarily a software platform or a single repository. That settles a question in v0: companyOS content lives in OS repos, and the factory's job is to run the agents that create and maintain it, not to be the OS.

- **It is a family of OSs, not one.** FNOS, PEX, Technology, other function OSs, leadership forums, and team OSs. Trying all of them in Q4 would swamp a team of about two and a half, so KR3 covers two: FNOS and the Engineering Senior Leadership OS, which is the instance his proposal says its own repository is. Technology OS is the natural next one.
- **One canonical home, link do not copy.** This matches the Fieldworks knowledge layer ("index in place"). His Engineering Artifact Map (goal, initiative, product spec, technical design review, ADR, delivery) is an early version of the traceability chain the factory wants (signal, spec, ticket, PR, outcome). Seeding it is a good bridge between the two efforts.
- **AGENTS.md at each OS root (decided).** Cory's structure uses it and the article uses CLAUDE.md. AGENTS.md is the standard here because it is the tool-neutral choice, so every harness candidate and engineer tool can read the same files. One quick check is worth doing: confirm Claude Code picks it up directly, and if not, a one-line CLAUDE.md that imports it covers the gap.
- **Mechanics worth borrowing from the article.** A short root index plus nested folder indexes (loads only what a query needs), shared skills so summaries have one format, owners per folder, plan files saved in the repo, and a launch gate where work is not done until the repo is updated. The article names context rot (not updating the repo) as the main failure mode, which is why a staleness agent is in KR3 and re-review dates are already in the Fieldworks design.
- **Privacy matches your constraints.** Cory's principle is to link to access-controlled sources instead of copying sensitive material, and raw transcripts stay in their source system. That fits a limited-admin org and least-privilege agent scopes.

## Milestones

| Date | Milestone | KR | Owner (placeholder, teams TBD) |
|---|---|---|---|
| Oct 21 | **Decision gate.** Platform (AWS AgentCore vs the Workato / GCP options) and harness (pi vs opencode vs AgentCore harness) chosen and written up as an ADR. Run contract defined (identity, sandbox, trace, cost, output) so the choice stays swappable. Own AWS account confirmed. | KR1 | Chuck, Cory |
| Oct 21 | FNOS and Engineering Senior Leadership OS skeletons created by hand in Cory's tightly-admined org, using his proposed structure. AGENTS.md at every OS root (decided). Can start before the factory exists. | KR3 | Cory, Chuck |
| Nov 4 | **Walking skeleton.** One agent run end to end in a sandbox with identity, trace, per-run cost, and skills from the marketplace. | KR1, KR4 | Chuck, Mykhailo |
| Nov 18 | **First self-built PR.** Linear ticket to factory agent to PR through Gitar and Sonar, human merge. Baseline for review acceptance measured. | KR2 | Chuck, Mykhailo |
| Dec 2 | **Agent creation path.** A new agent defined by config, deployed and triggered through the factory. Meeting synthesis agent writing records into the Engineering Senior Leadership OS, linked to source transcripts. | KR3 | Chuck, Tashfiq |
| Dec 16 | **Cost view and self-build report.** Per agent, run and accepted-change cost; lead-time report; Linear status flowing back from runs. | KR4, KR2 | Chuck, Mykhailo |
| Dec 16 | Project sync and staleness-check agents running against FNOS and the Engineering Senior Leadership OS. Artifact Map v0 if capacity allows. | KR3 | Chuck, Tashfiq |
| Dec 18 | Q4 readout and Q1 plan. Buffer left for the holidays. | All | Chuck |

Cadence is two weeks per milestone, which fits a team of roughly two and a half people. Late December is a real risk, so nothing hard-lands after the 18th.

## High-level plan

**Phase 0, Oct 7 to Oct 21: decide and shape.** Two or three week-long spikes run in parallel: AgentCore harness, pi, and opencode, all judged against the same short test (run an agent with marketplace skills, in a sandbox, with a trace and a cost per run). The Workato conversation and Cory's GCP lean get resolved here, not later. Cory starts the companyOS org and structure by hand.

**Phase 1, Oct 21 to Nov 18: skeleton, then the first self-built PR.** Build the thinnest path that works: sandbox, identity, trace and cost, skills in, Linear in, PR out, existing gates. Resist building the command center yet.

**Phase 2, Nov 18 to Dec 18: widen it.** Agent creation by config, companyOS agents, cost views, Linear status flowback. Run the factory on its own backlog so every gap we hit becomes a ticket the factory works on.

**Q1 2027 (not committed here):** command center UI, upstream shaping and TDRs, second repo target (NetDev as the design partner per the brainstorm), run learnings, and a decision on who owns the platform long term.

## What I read about your platform and harness options

- **AgentCore harness (AWS, now GA).** Two API calls (create and invoke a harness). Model choice across Bedrock, OpenAI, Gemini and LiteLLM, tools through MCP or Gateway, skills loadable from Git or S3, microVM sandboxes, traces to CloudWatch, versioned configs, and export to code. No harness fee, consumption pricing. It already covers most of "infra, agents, skills, MCP" in your first bullet, which supports "AWS until it causes issues." Still to verify: how finely cost can be attributed per agent and per run, and fit for an fn-core sandbox image.
- **pi.** Minimal harness, TypeScript extensions, Node SDK, a JSON RPC mode, 15+ providers including Bedrock, MIT license. Fits "build our own." I did not see MCP support in what I read, so that needs checking.
- **opencode.** I only saw search listings, not a proper read. Treat it as unevaluated.
- **Worth testing in the spike:** the harness and the platform may be separable. If AgentCore can run a container with pi or opencode inside it, you could take AWS for hosting and trace and pick the harness on its merits. That is a hypothesis, not something I confirmed.

## Risks

- **The team is small and changed.** Vitalii and Asad have rolled off, so ai-toolbox ownership and the agent-hosting work need a named owner. O2's M2 to M4 from Q3 (IaC, Harness CD, demo) have no confirmed status in the workspace, so I can't tell how much AWS groundwork exists.
- **Platform decision drag.** AWS vs GCP is still live with Cory, and the Workato outcome is unknown. The Oct 21 gate protects against this, but only if it is a real deadline.
- **Metric pressure.** You flagged that a metric focus can pull time away from building. KRs are built on leading indicators for that reason.
- **Bootstrapping.** A factory that builds itself can stall if its own early output is poor. Humans merge everything, and failures default to alerting a human.
- **companyOS scope.** Cory's structure covers many kinds of OS. Q4 deliberately covers two, and each needs a named human owner for updates, or the content goes stale no matter how good the agents are.
- **Scope creep from the vision.** The brainstorm describes a much bigger Fieldworks (command center, shaping, marketplace). Q4 deliberately covers only the thin slice.

## Open items

1. **Cory's proposal and the article are now incorporated** (the GitHub link was blocked for me earlier, so the article text came from your upload).
2. **Targets to calibrate:** the 10 PRs, 50% of eligible tickets, and 2 weeks of companyOS use are my placeholders.
3. **Is a UI in Q4?** It isn't in your list. I assumed Linear status flowback plus a simple run and cost view, with the full command center in Q1.
4. **Teams and ownership are deferred.** This covers who owns ai-toolbox after Vitalii, who owns updates for each OS, and Cory's exact role. Owners in the milestone table are placeholders until that is settled.
5. **What happens to the Emerald / Core UI / FN Core embeds** in Q4 (continue as is, or move onto the factory as it matures).
6. **Which two OSs first.** I proposed FNOS and the Engineering Senior Leadership OS. Confirm with Cory when the teams are settled.
7. **Engineering Artifact Map home.** Cory lists this as his own open question. It affects where the stretch goal in KR3 lives.

## Sources

- Slack DMs, Chuck and Cory, 2026-09-30, 2026-10-01, [2026-10-05](https://fieldnation.slack.com/archives/D067TF410Q1/p1791231457415679)
- Workspace: brainstorm/2026-10-05-full-software-factory-vision.md, artifacts/2026-10-05-ai-dlc-2027-roadmap-draft.md, artifacts/2026-10-05-fieldworks-layer-mapping.md, program/OKRs/h2-2026-okrs.md
- [Amazon Bedrock AgentCore harness is now generally available (AWS)](https://aws.amazon.com/blogs/machine-learning/amazon-bedrock-agentcore-harness-is-now-generally-available-go-from-idea-to-production-grade-agent-in-minutes/)
- [pi.dev](https://pi.dev/)
- Cory Muir, Proposed Team OS Structure (draft, uploaded 2026-10-07)
- [Build a Team OS with Claude Code, Aakash Gupta and Hannah Stulberg (CC BY-NC 4.0)](https://www.news.aakashg.com/p/claude-code-team-os)
