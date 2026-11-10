# AI-DLC 2027 Roadmap (Draft v0)

*Draft for Cory and Travis. Early thinking, not a commitment. Any feedback is welcome.*

## The short version

Our 2027 goal is an end-to-end AI-native product development lifecycle: an idea comes in, and the path through discovery, prototype, spec, build, verify, ship and operate is mostly agent-driven with humans making the judgment calls at defined gates. The software factory is a core part of that, and it's the part we can start on now. It isn't the whole thing, though. If we only speed up engineering, we hit a ceiling, because the delay people feel most is upstream of the first line of code and in the review and merge steps after it.

So the plan is to keep the enablement work running, stand up the factory on a narrow, meaningful use case, and bring Product and UX into the same model so a prototype and production code come from the same governed path.

## Where we're starting from

Here's what the last six months or so gave us:

- **Enablement is real.** The repo scorecard, repo-readiness score, bootstrap tooling, and the ai-toolbox marketplace are built. Claude Code skills are distributed to engineers, and Gitar is our code reviewer.
- **We can measure it.** True Throughput, PR cycle time and revert rate are live for the first embed repo (engineering-dbt, "Emerald"). Early read: throughput is still flat (around 1.4 PRs per engineer per week against a 3.5 target), so tooling alone hasn't moved it yet.
- **We know why.** The Emerald sessions pointed at pre-ticket friction (finding the right data source, placing a model in the right layer, chasing undocumented business logic), not the development step. That is a knowledge and context problem as much as an agent problem.
- **The factory is easier than it was.** We've learned a lot about harnesses, review, evals and skills. There are also more building blocks on the market now (Workato, agent frameworks, AI gateways, Atlassian's Teamwork Graph), so we should be buying or adopting before we build.

## Where we should focus

Cory's framing was: (a) keep enabling, (b) a factory for toil like bugs and maintenance, (c) a factory for VOC-type items, (d) an end-to-end PDLC, or (e) a hybrid. My recommendation is **(d) as the goal, with (a) continuing underneath and (b) as the first factory slice, then (c)**. That sequence lines up with Travis's interest in starting the factory now, because bugs and small VOCs are the right proving ground for the end-to-end model.

## The tracks

**1. Enablement foundation (continue).** Finish the embed rollout across the repos we've prioritized (Emerald first, then Core UI and FN Core), keep the toolbox and review pipeline healthy, and make repo-committed skills and review policy the standard. This is the base everything else stands on, and it keeps producing the metrics we need.

**2. Software factory (start now, narrow).** Pick one specific, meaningful use case, ideally a bug class or small VOC with a clear owner, and run it end to end through plan, execute and verify with a human at review. The use case should be something the business wants but could tolerate being slower or imperfect while we learn. Build versus buy gets decided here, based on what Workato and the frameworks already give us.

**3. End-to-end PDLC (extend the model upstream).** Bring Product, UX and Design into the same governed path. The idea is that someone like a UX designer or PM can build a prototype inside the factory model so the UI is close to production-ready from the start, and engineering validates rather than rebuilds. This depends on PM coverage. One option to explore is having PMs own domains, paired with the BD teams, with cross-cutting initiatives handled separately.

**4. Knowledge and context layer.** The Emerald findings say this is the real bottleneck for a lot of work. That includes platform architecture briefs, per-repo context, decision records (ADRs), data definitions, and a knowledge graph that connects code, tickets and docs. We should evaluate Teamwork Graph, which looks like it comes with our Atlassian subscription, before building our own.

**5. Platform and governance.** An AI gateway for model routing and cost controls, evals that gate changes to skills and agent instructions, security scanning, and a decision on where long-running agents are hosted (the AWS versus GCP/Azure question is still open). Cory's point that this needs to be highly governed is the design constraint for the whole plan.

**6. Measurement.** True Throughput and revert rate stay as the headline engineering metrics. We add CI duration, cost per accepted change, and a factory-specific set (share of in-scope bugs resolved with human review only, review turn rate, escape rate). Targets for the factory metrics are TBD until we've run the first slice.

## Phasing (proposed)

| When | Focus | What done looks like |
|---|---|---|
| Q4 2026 | Decide and prepare | Direction agreed with Cory and Travis, factory use case chosen, Workato and other build-versus-buy input in, first embed repos wrapped, 2027 resourcing settled |
| Q1 2027 | First factory slice | Chosen use case running through the factory with human review, governance and gateway basics in place, knowledge layer evaluated |
| Q2 2027 | Prove and widen | Factory metrics reported, second use case (the next toil or VOC category), PM and UX pilot building prototypes in the model |
| H2 2027 | End-to-end | PDLC model running for at least one domain from problem discovery through production, enablement extended to all engineering teams |

Dates and quarters are my best guess and will move once we pick the use case.

## Open questions

1. **Which use case first?** I think it needs to be specific and meaningful, and it needs a PM or business owner behind it. A VOC that needs UI work is a good candidate because it exercises the upstream pieces too.
2. **Team shape and resourcing.** Travis's draft 2027 priorities include a contract team for the factory build. We still need to decide where that team sits, who leads it, and how realistic hiring is, since people with this skillset are scarce.
3. **PM coverage.** Can PMs own domains in a way that gives the factory a real counterpart on the product side?
4. **Build versus buy.** What do Workato and the agent frameworks cover, and what is left for us to build?
5. **Hosting.** Do we stay on the AgentCore path or revisit it?
6. **Governance.** What are the approval gates and auto-merge criteria for factory output?

## What I'd like from you

Gut check on the (d) recommendation, a read on the first use case, and a steer on how we handle resourcing. Then I'll turn this into a fuller plan.
