# Full Software Factory Vision: Brainstorm / Discovery Notes
Working name for the platform: Fieldworks (provisional, chosen 2026-10-05)
Date: 2026-10-05 · Goal: Start fresh (no bias from prior decisions) and define what a full, ambitious software factory for Field Nation could look like.

## Starting premises (from Chuck, kickoff)
- Core end goal: factory picks up tickets from Linear and executes end to end to a pull request.
- The factory should be MORE ambitious than that.
- Humans can prompt inside the factory and build (interactive, not only autonomous).
- Visual representation of where the various agents are (live visibility).
- People can deploy their own running agents.
- Knowledge sharing across people/agents.
- Deliberately NOT anchored on previous decisions (embed-first pivot, AWS/Bedrock leaning, archived PEV pipeline, etc.). Fresh start.

## Summary / key decisions
**Vision.** Fieldworks (provisional name) is an internal-facing platform that runs the full software lifecycle (signal -> shape -> spec -> plan -> execute -> review -> verify -> deploy -> monitor -> learn). "Done" = verified in production. Engineers first, stretching company-wide (PEX, support, ops). Fieldworks is the platform; AI-DLC is the operating model that runs on it; a standing platform team will own it long term (Q1, Q2, Q23).

**Experience.** A command center is the source of truth (lifecycle-first live map, drill-down to system and run views, steer and chat), and the same agents are reachable from Slack, Linear, GitHub, and CLI/IDE with shared session state. Default is low interruption, user-configurable up to high-touch as trust builds. One-step handoff, takeover without losing context, self-explaining PRs (Q3, Q14, Q26).

**Knowledge.** One knowledge layer. Phase order: skills/playbooks and repo context first, run learnings (human-curated) fast follow, agent marketplace later. Cross-cutting context is indexed in place at first (Confluence TDRs/runbooks, standards repo -> skills, PagerDuty incidents, Joe's entity topology for architecture and ownership, Google Docs/customer interviews/Slack/Linear for business context), with possible later ingestion for a semantic layer. Long term, product shaping and TDRs are authored inside the factory as one traceable chain (signal -> spec -> ticket -> PR -> outcome). Staleness handled by link-based detection plus re-review dates, visible status (current / under review / superseded), non-current = hint only (Q5, Q7-Q10).

**Control plane and safety.** Minimal guardrails by default, explicit ones where set (Gitar-style risk matrix). Humans review all merges today; any future auto-merge sits behind Gitar risk gating. Sonar and Gitar stay. Central AI gateway (models) and MCP gateway (tools). Agents are first-class principals with own identity, scopes, and a named owner, with hybrid delegated access for interactive runs. Tiered MCP tool catalog; higher tiers approved by a dedicated team including security. Failure default: alert a human, preserve run state, pause/kill at agent and platform level from phase 1 (Q6, Q11, Q12, Q24).

**Execution and posture.** Design for single- and multi-agent, durable, resumable, sandboxed runs with a stable run contract. Technology undecided (AWS default, Workato under evaluation, Joe leans GCP/Azure). Buy/compose commodity layers behind stable contracts and build the differentiating layers (traceability chain, knowledge layer, command center, cost accounting) (Q13, Q18).

**Phasing.** P1: Linear ticket to PR + command center + agents through AI/MCP gateways, capturing run events from day one. P2: native shaping and TDRs. P3: run learnings. P4: user-deployed agents (levels 1 and 2), then marketplace and level 3 (kept open, not a one-way door). Start Q4 2026, may slip to Q1 2027. Phase 1 success = platform bar + trust bar (hard), usage bar (real signal). Design partner: NetDev (Ryan UX, Lynsay PM, Safin and Jemu eng); Cory is considering VOC work as a phase 2 anchor (Q4, Q16, Q17, Q20).

**Measures and worries.** Headline metric is cost per accepted change, growing into cost per developed feature. Top worries: system performance, platform delivery speed, building the right thing, broad adoption incl. product. Trust erosion is acceptable as learning (Q15, Q19).

**Existing work.** ai-toolbox, Sonar, and Gitar stay. Everything else gets mapped onto Fieldworks layers (commodity / control plane / differentiating) in a separate follow-up session (Q25).

## Q&A log
### Q1 - Audience
- Asked: Who is the factory for (engineers only vs. wider)?
- Captured: Confirmed. Engineers first, designed to extend to everyone at Field Nation (PEX, support, ops). Platform framing: anyone can deploy agents, share knowledge, prompt inside the factory.
- Flags: none

### Q2 - Scope / definition of done
- Asked: Should the factory own stages before and after the PR, or stop at the PR/merge?
- Captured: Confirmed, exactly the ambition. Factory owns the full lifecycle: raw idea or customer signal -> shaping -> spec/design -> plan -> execute -> review -> test/verify -> merge -> deploy -> monitor -> learn, feeding back into the next ticket. Autonomy tiers: humans approve each stage at first, low-risk work flows further on its own over time. "Done" = verified in production, not merely merged.
- Flags: none

### Q3 - Human experience / surfaces
- Asked: Command center only, meet-people-where-they-work only, or both?
- Captured: Confirmed option 3 (both). Command center web app is the source of truth: live map of agents and work, click into an agent/ticket, chat and steer. The same agents are also reachable from Slack, Linear, GitHub, and CLI/IDE, all sharing one session and state. Agents are a shared, observable collaboration surface: anyone can watch an agent mid-task, join its session, take over or redirect.
- Flags: none

### Q4 - User-deployed agents
- Asked: What does "deploy your own running agent" mean? Level 1 configured (model + instructions + skills/tools), level 2 triggered (schedule, Linear label, Datadog alert, Slack message), level 3 bring-your-own-code hosted by the platform.
- Captured: Levels 1 and 2 are the initial build-out. Level 3 is NOT in the initial build but the design must not be a one-way door that prevents supporting it later. Every agent (any level) gets the same platform contract: identity, scoped permissions, place on the map, cost and trace visibility, access to shared knowledge.
- Flags: Follow-up not yet answered: can a non-engineer deploy a level 1/2 agent fully on their own, or does it need engineering review? -> Chuck (revisit under governance/trust question)

### Q5 - Knowledge sharing
- Asked: What does "share knowledge" mean: (1) shared skills/playbooks, (2) shared codebase/domain context, (3) learnings from runs written back, (4) shared agents/workflows marketplace?
- Captured: Phasing confirmed. 1 and 2 matter most initially (table stakes, a necessity). 3 (run learnings, agents writing back for human curation) is a fast follow. 4 (share/find/fork agents) is future. Treat as one knowledge layer so later phases extend it rather than replace it.
- Flags: none

### Q5b - Cross-cutting context (raised by Chuck, interrupts Q6)
- Asked: (Chuck-initiated) Knowledge layer also needs shared context that is NOT repo-specific: wider cross-cutting domain context, e.g. business context, engineering context such as TDRs, etc.
- Captured: Add a third context category beyond skills/playbooks and repo context: org-wide, cross-cutting domain knowledge (business, engineering decisions/TDRs, etc.). Details TBD, being explored now.
- Flags: Q6 (autonomy tiers and guardrails) is paused; return to it after this thread.

### Q7 - Cross-cutting context: categories and sources
- Asked: Which cross-cutting context categories matter and where do they live today? Index in place vs dedicated store?
- Captured:
  - Business and product context: "super important" (priority). Source location not stated yet.
  - Engineering decisions: mostly live in the repos; TDRs live in Confluence.
  - Standards and conventions: mostly in a single standards repo, then converted into skills.
  - Architecture and system maps: important. Joe's entity topology project holds some of this.
  - Incidents: PagerDuty holds most of that knowledge. Runbooks live in Confluence.
  - People and ownership: in Joe's entity topology project.
  - Approach: index, don't relocate, to start. Reference external systems. Possible later: ingest into the factory to build a better semantic knowledge layer.
- Flags: Where does business/product context live today (Confluence, Google Docs, Slack, Linear, Productboard?) -> Chuck. Trust/authority marking (current vs stale) not yet discussed.

### Q8 - Business/product context: sources and long-term direction
- Asked: Where does business/product context live today, and what job should it do (1: understand intent for in-flight work; 2: shape work upstream from signals into tickets/specs)? Which first?
- Captured: Sources today: Google Docs, customer interviews, Slack, and possibly some in Linear. Long-term direction: the factory should both interface with external systems AND be able to bring them in, so that product/business context is SHAPED INSIDE the factory rather than externally. (Implies job 2, upstream shaping, is a long-term goal; factory becomes the place where discovery/spec work happens, not just a consumer of it.)
- Not answered: explicit ordering of job 1 vs job 2 for the initial build.
- Flags: Job 1 vs job 2 sequencing for initial build -> Chuck. Customer interview data likely sensitive (customer PII/confidentiality); handling rules TBD -> Chuck.

### Q9 - Shaping inside the factory
- Asked: Does the shaping flow (signals in -> agents+humans synthesize -> artifacts produced in factory -> same chain continues into execution) match? Should initial build include upstream shaping or just read from Google Docs/Slack?
- Captured: Confirmed exactly. Shaping artifacts and execution artifacts form one connected chain with traceability: customer signal -> spec -> ticket -> PR -> production outcome (enables "did what we shipped solve the problem we started with?"). Linear stays system of record for work items. Chuck extension: engineering TDRs should ALSO be shaped in the factory (not Confluence), which helps when knowledge becomes obsolete (living, linked artifacts make staleness detectable).
- Not answered: whether the initial build includes upstream shaping or only reads external sources.
- Flags: Initial-build scope for shaping (read-only external vs native) -> Chuck.

### Q10 - Keeping knowledge from going stale
- Asked: How should the factory detect/handle obsolete knowledge? (1 link-based detection, 2 age/review cadence, 3 contradiction detection, 4 usage feedback)
- Captured: Confirmed. Start with 1 (artifacts link to services/repos/events they govern; substantial change flags them for review) + 2 (re-review-by date). 3 and 4 arrive with the run-learnings fast follow. Every artifact carries a visible status (current / under review / superseded). Agents treat anything not marked current as a hint, not source of truth. Superseding decisions link back to the old one so history survives.
- Flags: none. Cross-cutting context thread CLOSED; returning to Q6.

### Q6 - Autonomy and guardrails
- Asked: How much autonomy, who sets guardrails; earned per workflow/repo by track record with hard stops?
- Captured: Chuck's position: with Sonar and Gitar in place there is a good gate to lean on for human vs. not. Default stance = factory runs with MINIMAL guardrails unless explicitly set (similar to Gitar's risk matrix). Learnings feed back into the guardrails over time. Today: nothing merges without human review. Even in a future auto-merge world, it only happens with Gitar risk gating in place.
- Claude's read: control shifts from "what agents may attempt" to "what the merge gate lets through." Execution is permissive; the merge gate is load-bearing, so gate transparency and pluggability matter. Non-merge actions (deploy, data access, external comms, spend, agent permissions) are not covered by the merge gate and need their own thinking.
- Flags: Which non-merge actions need gates? Non-engineer self-serve deploy of level 1/2 agents still unanswered -> Chuck.

### Q11 - Non-merge guardrails, platform scope, gateways
- Asked: Beyond merging, which actions need guardrails (prod deploy, data access, external comms, spend, agent permissions)? Gate by reversibility?
- Captured: Platform is INTERNAL-FACING: no customer features, used for internal tooling and workflows (so customer-facing messaging risk is out of scope; sensitive data such as customer interviews/prod data still matters). Chuck: tooling agents can access and who can deploy agents will need a gating/safety mechanism. Likely control plane: a central AI gateway (model access) plus an MCP gateway (tool access).
- Not yet answered: day-one list of gated actions; non-engineer self-serve deploy rule; whether reversibility is the right gating principle.
- Flags: Agent identity/permission model and tool-tier gating -> next question.

### Q12 - Agent identity and tool gating
- Asked: Whose permissions does an agent run with (deployer's / own identity with scopes / hybrid)? How are tools tiered in the MCP gateway? Who approves higher tiers?
- Captured: Chose option 3 (hybrid). Each agent is a first-class principal with its own identity and explicit scopes, owned by a named human or team. Autonomous runs use only the agent's own scopes; interactive runs may use the human's delegated access scoped to that session. Tool catalog tiered in the MCP gateway: open read-only tools for any agent; write/sensitive-data tools granted per agent with an approver; new tools/MCP servers registered and reviewed before entering the catalog. Approval of higher tiers: a dedicated team formed for this (likely including security), for now. Implication: non-engineers can deploy level 1/2 agents freely while using open-tier tools only.
- Flags: Confirm non-engineer self-serve for open-tier tools (implied, not explicitly stated) -> Chuck. Exact composition of the approval team TBD -> Chuck.

### Q13 - Execution layer (design principles)
- Asked: Where do agents run (sandboxes), how is work shaped (single vs multi-agent), how long/parallel (durability)?
- Captured: Confirmed. Design for BOTH single-agent and multi-agent. Design for durability from the start (durable, resumable, sandboxed runs; inspectable and steerable state; human can pause and jump in). Run contract: identity, sandbox, inspectable state, output; internals swappable. Execution-layer TECHNOLOGY is undecided: Field Nation is an AWS shop (natural default), Workato and its offerings are being looked at, and Joe leans toward GCP and Azure. Technology choice to be covered later.
- Not answered: biggest worry (fn-core sandboxes vs long-run reliability vs cost).
- Flags: Execution platform choice (AWS vs Workato vs GCP/Azure) -> Chuck + Joe, later. Biggest execution-layer concern -> Chuck.

### Q14 - Visual map
- Asked: What does "where agents are" mean (lifecycle / system / run / people)? Primary view? First question the map should answer?
- Captured: Confirmed. Primary view = lifecycle ("factory floor": work moving through stages), with drill-down to a system view (repos/services/tools, informed by Joe's entity topology) and a run view (what an agent is doing now: thinking, running tests, waiting on human, blocked, failed; trace and cost). People involvement (who is watching/steering) is overlaid. All views read from one shared event stream.
- Not answered: the first question the map should answer when opened (what's stuck / what's running / what shipped today).
- Flags: Primary "first glance" question for the map -> Chuck.

### Q15 - Measuring success
- Asked: How to measure whether the factory works? Proposed headline = cost per accepted change (spend / changes that merged and stayed merged), with throughput, quality, human leverage, outcome, and platform health as supporting measures.
- Captured: Chuck likes cost per accepted change. Long-term it could grow into COST PER DEVELOPED FEATURE, covering the whole chain from the start (shaping the feature, etc.) and breakable down by change. So: change is the unit of delivery, feature is the unit of value; costs roll up change -> feature, including upstream shaping.
- Claude notes: feature-level rollup needs a feature identity in the traceability chain (e.g., Linear initiative/project), a decision on whether human time is costed, and a rule for shared platform overhead.
- Flags: Define "feature" identity and whether human time counts in cost -> later.

### Q16 - Phasing / first thin slice
- Asked: Is the proposed first slice right (Linear ticket -> sandboxed run -> PR gated by existing review; basic command center with lifecycle view and steer; agents via AI + MCP gateways with own identity; level 1/2 agents; single event stream)? What timeframe?
- Captured: Chuck's sequencing: PHASE 1 = Linear ticket to PR + command center + agents running through the AI gateway and MCP gateway. Level 1/2 user-deployed agents are REMOVED from phase 1. PHASE 2 = native shaping and TDRs in the factory. PHASE 3 = run learnings. PHASE 4 = deploying agents (level 1/2), then marketplace and level 3 beyond.
- Claude notes: (a) keep capturing run events and traces in phase 1 so learnings have data later even though curation comes in phase 3; (b) phase 2 shaping agents are likely built with the same primitives users will later get, so building them as platform-owned level 1/2-style agents dogfoods the deploy path; (c) native shaping brings customer interview data, so data-access gating must exist before phase 2.
- Not answered: timeframe.
- Flags: Phase 1 timeframe and success criteria -> Chuck.

### Q17 - Phase 1 success and timing
- Asked: What would make phase 1 a success and by when? (completion bar, trust bar, usage bar, platform bar)
- Captured: Chuck is comfortable with the proposed shape ("this looks good to show for phase 1"): hard criteria = platform bar (every run through the gateways with identity, trace, cost) + trust bar (reviewers accept PRs at roughly human rates); usage bar (engineers choosing to hand over harder work) is the real signal. Timing: aim to START in Q4 2026; may slip to Q1 2027.
- Flags: Exact numeric thresholds not set (completion/trust %) -> later. Pilot repo(s) for phase 1 not chosen -> later.

### Q18 - Build vs buy vs compose
- Asked: Posture across commodity layers, control plane, and differentiating layers?
- Captured: Confirmed exactly. Buy/compose the commodity layers (models, sandboxed compute, durable workflow, review gate, observability) behind stable interfaces, and glue them together with Field Nation-specific domain. Build the differentiating layers in-house: traceability chain, knowledge layer with staleness handling, command center/map, cost-per-feature accounting. Control plane (AI/MCP gateways, identity, audit) can be bought in part but must fit FN systems. Stable run and gateway contracts keep vendors swappable (AWS, Workato, GCP/Azure can compete for the execution layer).
- Flags: none

### Q19 - Biggest worries
- Asked: Top risks (trust erosion, review bottleneck, brittle verification, scope sprawl, security/data exposure, cost surprise, adoption)?
- Captured: Chuck's top worries: (1) PERFORMANCE of the system; (2) SPEED TO DELIVERY of the platform itself; (3) building the RIGHT thing; (4) getting ADOPTION from everyone, including product. Trust erosion is explicitly acceptable: early bad outcomes are learnings that improve the factory.
- Claude read: performance should be a first-class measured property (run latency, sandbox start time, concurrency, map/event-stream responsiveness); platform delivery speed supports the buy-and-glue posture and a thin phase 1; right-thing and adoption argue for involving design-partner users, including product, early even though native shaping is phase 2.
- Flags: Who are the phase 1 design partners and how do product people get involved before phase 2? -> next.

### Q20 - Phase 1 design partners and early product involvement
- Asked: Which teams/people are phase 1 design partners, and how is product involved early?
- Captured: Chuck proposes the NetDev team as the design partner: highly engaged and AI fluent. People: Ryan (UX) who could help shape things; Lynsay (PM) as the product partner; Safin and Jemu on the engineering side. Cory is thinking of some kind of VOC (voice of customer) work but is not sure who would be involved if it goes ahead.
- Claude notes: VOC is a natural anchor for phase 2 native shaping (customer signals in, synthesis, problem briefs out); it brings customer interview data, so data-access gating must be in place first. NetDev's repos need a readiness check (can agents run tests and verify work there?).
- Flags: Confirm NetDev repos are verifiable in a sandbox -> Chuck. VOC owner and scope -> Cory.

### Q21/Q22 - Naming (Chuck-initiated detour before covering remaining topics)
- Asked: Anything not yet touched? Chuck's reply: wants the platform to have a really cool, FN-themed name; help come up with one, then cover the remaining questions.
- Captured: Name should be FN themed. Existing internal naming context: teams are named after minerals/gems (Emerald, Granite, Platinum, Marble, Sapphire, Moissanite). Candidate directions offered: field/agriculture (Fieldworks, Combine, Harvest, Granary), gem/mineral craft that fits the team naming (Lapidary, Facet, Crucible, Quarry), and factory/forge (FieldForge, Foundry). Selection: FIELDWORKS (working name, "for now").
- Remaining untouched topics to cover afterward: naming/framing vs current AI-DLC work, governance and long-term ownership, how existing initiatives fold in, failure and recovery, human experience details (notifications, takeover).
- Flags: none. Name is provisional.

### Q23 - Relationship to AI-DLC and long-term ownership
- Asked: Fieldworks as platform with AI-DLC as operating model running on it (option 1), as successor absorbing the technical workstream (2), or separate (3)? Who owns it long term?
- Captured: Option 1 is correct. Fieldworks is the platform; AI-DLC (process transformation) is the operating model that runs on it. Long term a dedicated platform team will likely be formed to own and run it.
- Flags: Interim ownership before the platform team exists (who builds and runs phase 1) -> Chuck, later.

### Q24 - Failure and recovery
- Asked: How to handle run failure/stall, sandbox or dependency death, wrong gate verdicts, a bad agent, and platform degradation? Human as default fallback?
- Captured: Confirmed to start. Principle: when in doubt, fall back to alerting a human, who decides what to do both immediately and as a long-term change. Unresolvable failures become visible, assigned items on the command center with run state preserved so a human can resume where the agent stopped. Pause/kill control at agent level and platform level from phase 1. Gate misses feed the learnings loop.
- Not answered: any failure severe enough to warrant stopping everything automatically.
- Flags: Define "stop everything" conditions -> later.

### Q25 - Fit of existing initiatives and tools
- Asked: How do today's tools and initiatives fold in (component / run alongside / retire)? Decide now or in a follow-up using layers as the sorting rule?
- Captured: Approach confirmed: do NOT decide piece by piece now; use the layers (commodity / control plane / differentiating) as the sorting rule in a separate follow-up session that takes the current workspace state as input, keeping this brainstorm free of prior decisions. Two firm calls from Chuck: ai-toolbox (the marketplace of skills) will stick around; Sonar and Gitar will definitely not go away.
- Flags: Follow-up session to map existing tools and initiatives onto Fieldworks layers -> Chuck, later.

### Q26 - Day-to-day human experience
- Asked: How should handoff, notifications, mid-run takeover, PR explanation, and team/personal views work? Default "interrupt me only when it matters"?
- Captured: Confirmed, with an addition from Chuck: default is LESS interruption, but users can opt into MORE notifications/"babysitting" as they build trust (notification level is user-configurable, trust-driven). Remaining design intents from the proposal (not contradicted): handoff is one step from where people already work; notifications rare and actionable by default; join or take over a run without losing context; every PR carries its own explanation (plan, trace, what was checked); team and personal views.
- Not answered: single make-or-break interaction for adoption.
- Flags: none blocking.

## Open flags (pending input)
- Map existing tools/initiatives onto Fieldworks layers (separate follow-up session) -> Chuck
- Execution platform choice (AWS / Workato / GCP / Azure) -> Chuck + Joe, later
- NetDev repo verifiability in a sandbox; VOC owner and scope -> Chuck / Cory
- Phase 1 numeric success thresholds and pilot repo choice -> later
- Day-one list of gated actions beyond merge (deploy, data access); non-engineer self-serve rule for open-tier agents (confirm, matters by phase 4) -> Chuck
- Initial-build scope for upstream shaping (read external sources first vs native from day one; phase 2 plan implies native in P2) -> Chuck
- Conditions that trigger a platform-wide stop -> later
- Feature identity for cost rollup; whether human time counts in cost -> later
- Map "first glance" question (stuck / running / shipped today) -> Chuck
- Interim ownership of phase 1 before the platform team exists -> Chuck
- Governance details: approval team composition (security etc.) -> Chuck
