# Fieldworks UI Requirements (Prototype)

*Draft for Chuck's review, 2026-10-05. Input for an OpenSpec session. Fieldworks is a provisional name. Built from the [full software factory vision brainstorm](../brainstorm/2026-10-05-full-software-factory-vision.md) and the [layer mapping](./2026-10-05-fieldworks-layer-mapping.md). Q-numbers below refer to the brainstorm Q&A log. Where the brainstorm did not decide something, this doc says so and proposes a default, marked **Assumption**.*

## 1. How to use this document

This is a narrative PRD for a **clickable prototype with fake data**, covering the **full vision across all four phases**. The goal is to let engineers, NetDev (our proposed design partner), and leadership react to what Fieldworks could feel like before anything real is built. Nothing in the prototype needs a backend. Every screen runs on seeded sample data (section 10).

Requirements carry IDs (for example `RUN-3`) so OpenSpec can turn each area into a capability spec and each requirement into a testable statement. Each requirement is tagged with the phase in which the real product would deliver it: **P1** (Linear ticket to PR plus command center, Q4 2026 to Q1 2027), **P2** (native shaping and TDRs), **P3** (run learnings), **P4** (user-deployed agents, then marketplace). The prototype shows all of them, but P2 to P4 areas may be visibly marked as "coming later" so nobody mistakes the mock for the phase 1 scope.

## 2. Product summary

Fieldworks is an internal-facing platform that runs the full software lifecycle: signal, shape, spec, plan, execute, review, verify, deploy, monitor, learn. "Done" means verified in production, not merely merged (Q2). Engineers come first, with a path to PEX, support, and ops (Q1). Fieldworks is the platform and AI-DLC is the operating model that runs on it (Q23).

The UI is a **command center web app** that is the source of truth for what agents are doing, plus **touchpoints in Slack, Linear, GitHub, and CLI/IDE** that share the same session state (Q3). It is internal only. There are no customer-facing screens, though sensitive data (customer interviews, prod data) still needs careful handling.

## 3. Prototype goals and non-goals

**Goals**
1. Make the "factory floor" idea tangible: anyone can see where work and agents are at a glance.
2. Show the human experience: handoff in one step, low interruption by default, takeover without losing context, PRs that explain themselves (Q26).
3. Show the control plane as something a person can see and use (gates, identity, tool tiers, kill switch) rather than plumbing.
4. Show the traceability chain from customer signal to production outcome, and cost per accepted change, which are the two differentiators most unlike anything we have today (Q9, Q15, Q18).
5. Give NetDev (Ryan, Lynsay, Safin, Jemu) something concrete to react to (Q20).

**Non-goals for the prototype**
1. No real agent execution, sandboxes, gateways, or integrations. Slack, Linear, GitHub, and CLI/IDE surfaces are mocked screens or panels, not live connections.
2. No real authentication. A role switcher stands in for Okta so reviewers can see the product as different people.
3. No final visual design. The prototype uses Field Nation brand tokens (see `NFR-6`) but is not a design deliverable.
4. No decision on execution technology (AWS, Workato, GCP, Azure). The UI must not leak vendor names into its core vocabulary (Q13, Q18).
5. No customer-facing features.

## 4. Personas

These are the roles the role switcher in the prototype should cover.

| Persona | What they want from Fieldworks | Primary surfaces |
|---|---|---|
| **Engineer (handoff user)** | Hand a ticket over, watch or ignore it, step in when needed, review a PR that explains itself. | Command center, Linear, GitHub, CLI/IDE |
| **Reviewer** | Trust or challenge a PR quickly using the plan, trace, and what was checked. | GitHub PR, change record |
| **Architecture lead** (Chuck's archetype) | Keep platform-wide consistency, see which decisions and skills are current, approve higher-risk tools. | System view, knowledge, governance |
| **Product manager** (Lynsay-style) | Bring customer signal in, shape it into specs, see whether what shipped solved the problem. | Shaping, traceability chain |
| **UX designer** (Ryan-style) | Contribute to shaping and review how work looks. | Shaping, run view |
| **Platform admin / approver** | Approve tools, set guardrails, pause or kill agents, watch platform health and cost. | Governance, cost, platform health |
| **Non-engineer builder** (PEX, support, ops) | Deploy a simple agent without engineering help. | Agent builder (P4) |
| **Leader (Cory-style)** | See whether the factory is working in cost and outcome terms. | Measures, cost |

## 5. Design principles

1. **One event stream, many views.** Every view (lifecycle, system, run, people overlay, cost) reads from one shared stream of events (Q14). In the prototype, one seeded dataset drives all views, so a change in one place shows up everywhere.
2. **Quiet by default, loud on request.** Default is less interruption. Users can opt into more babysitting as trust builds (Q26).
3. **A human is the fallback.** Failures become visible, assigned items with state preserved (Q24).
4. **Everything is explainable.** Every PR, run, gate decision, and cost number can be traced to its inputs.
5. **Status is always visible.** Any artifact (skill, decision, TDR) shows current, under review, or superseded. Anything not current is a hint, not truth (Q10).
6. **Performance is a feature.** The map and event stream must feel instant. Performance is the top worry (Q19), so the prototype should demonstrate fast navigation and live-feeling updates, even though the data is fake.
7. **Not a one-way door.** UI concepts must not assume agent internals. An agent is an identity, scopes, a place on the map, cost and trace, and shared knowledge, regardless of how it runs (Q4, Q13).

## 6. Information architecture

Top-level navigation of the command center:

1. **Floor** (default home): the lifecycle map.
2. **Systems**: repos, services, tools, owners.
3. **Runs**: all runs, with search and filters.
4. **Inbox** (labelled "Needs you"): items waiting on a human.
5. **Knowledge**: skills and playbooks, repo context, cross-cutting context, status and staleness.
6. **Shaping** (P2): signals, specs, TDRs, traceability.
7. **Agents**: roster of agents, builder and marketplace (P4).
8. **Governance**: gates, identity, tool catalog, approvals, kill switches.
9. **Measures**: cost per accepted change, throughput, quality, platform health.

A persistent global bar holds search, the notification dial, a platform-wide status indicator with a pause control, and the role switcher.

## 7. Functional requirements

### 7.1 Floor: the lifecycle map (P1)

The primary view is the "factory floor": work moving through lifecycle stages (Q14).

- `FLR-1` The Floor shall show lifecycle stages left to right: signal, shape, spec, plan, execute, review, verify, deploy, monitor, learn. In the prototype, stages beyond the P1 slice (signal, shape, spec, monitor, learn) render in a visibly muted "later" style.
- `FLR-2` Each work item (ticket, change, or feature) shall appear as a card in its current stage, showing title, Linear ID, repo, assigned agent or human, state, and elapsed time.
- `FLR-3` Card state shall be one of: running, waiting on human, blocked, failed, passed gate, done. State shall be conveyed by icon and label as well as color.
- `FLR-4` Agents shall appear on the map where they are working, with a presence marker and a short activity summary (for example "running tests").
- `FLR-5` A people overlay shall be toggleable, showing who is watching or steering each item (Q14).
- `FLR-6` The user shall be able to filter by team, repo, owner, state, and "mine".
- `FLR-7` The Floor shall show a summary strip: running, waiting on you, blocked, shipped today. **Assumption**: the brainstorm never decided the single "first glance" question (Q14 open flag). The prototype defaults to "what needs me" and lets the user switch the headline between "what's stuck", "what's running", and "what shipped today", so we can learn which one people reach for.
- `FLR-8` Opening a card shall go to the run view for that item, with a breadcrumb back to the Floor.
- `FLR-9` The Floor shall update live in the prototype (simulated events moving cards and changing state on a timer) so the experience of a living map can be judged. A control shall pause or speed up the simulation.

### 7.2 Systems view (P1)

- `SYS-1` The Systems view shall show repos, services, and tools as a graph or grouped list, informed by the entity topology work (Q14).
- `SYS-2` Each system shall show owner or team, repo readiness, active runs, recent changes, linked decisions and skills, and linked incidents.
- `SYS-3` The user shall be able to see which agents are currently touching a system.
- `SYS-4` Each system shall show whether agents can verify their own work there (tests runnable in a sandbox), since consistency of context and verifiability of results are different prerequisites (see layer mapping, ai-repo-score note). The prototype shows this as a simple readiness indicator with two parts.
- `SYS-5` The user shall be able to move between Floor, Systems, and Runs while keeping filters, where it makes sense.

### 7.3 Run view (P1)

A run is what one agent (or a small group of agents) does on a piece of work (Q13, Q14).

- `RUN-1` The run view shall show the run's current status in plain language: thinking, running tests, waiting on human, blocked, failed, finished (Q14).
- `RUN-2` The run view shall show a timeline of steps and decisions, expandable to detail (tool calls, files touched, test results). **Assumption**: in line with the OTel content firewall pattern, the prototype shows step metadata and summaries by default, not raw prompts or source content.
- `RUN-3` For multi-agent runs, the view shall show each agent, its role, and how they relate (parent and child). Single-agent runs are the simple case of the same view (Q13).
- `RUN-4` The run view shall show identity: which agent principal ran, its named owner, and the scopes it used (Q12).
- `RUN-5` The run view shall show cost so far and a running trace of where cost was spent (Q15).
- `RUN-6` The run view shall show gate results: Sonar findings, Gitar verdict and risk rating, CI status (Q6, Q18).
- `RUN-7` The run view shall show a persistent actions bar: pause, resume, take over, redirect, stop, and "hand to human" (see 7.4).
- `RUN-8` A failed or stalled run shall show why in plain language, the preserved state, and a one-click path to resume from where the agent stopped (Q24).
- `RUN-9` Runs shall be reachable by direct link so they can be shared in Slack, Linear, and GitHub.

### 7.4 Handoff, steering, chat, and takeover (P1)

- `STR-1` Handing a ticket to Fieldworks shall take one step from where the user already is (a button or label in Linear, a command in CLI or Slack, or a button in the command center) (Q26).
- `STR-2` The user shall be able to chat with a running agent in the run view and steer it with a new instruction or constraint (Q3).
- `STR-3` The user shall be able to join an agent's session as a watcher or participant, and see who else is in it (Q3).
- `STR-4` The user shall be able to take over a run (to continue by hand in CLI/IDE) or hand it back, without losing context. The prototype shows the same session and transcript appearing in the command center and in a mocked CLI or IDE panel (Q3, Q26).
- `STR-5` When a run needs a human, it shall produce a Needs-you item with a clear ask (a decision, an approval, missing context), the run state, and a one-click response (Q24).
- `STR-6` The prototype shall demonstrate that the same session is reachable from Slack, Linear, GitHub, and CLI/IDE with consistent state (see 7.13).

### 7.5 Needs you: Inbox (P1)

- `INB-1` The Inbox shall list items waiting on the current user or their team, ordered by urgency, with the reason, age, and a one-click action.
- `INB-2` Unresolvable failures shall appear as visible, assigned items with preserved state (Q24).
- `INB-3` The user shall be able to reassign, snooze, or dismiss with a reason.
- `INB-4` An empty Inbox shall be a clear, calm state ("nothing needs you").

### 7.6 Notifications and the trust dial (P1)

- `NTF-1` Default notification level shall be low: only items that need a human or have failed (Q26).
- `NTF-2` Each user shall have a trust dial with at least three levels: quiet, balanced, and watchful (Q26). A watchful setting shall also surface progress, step completions, and gate results.
- `NTF-3` The dial shall be settable globally and overridable per repo, per agent, or per work item.
- `NTF-4` The prototype shall show notifications as they would appear in Slack and in-app, and shall let the user see how the same run looks at each dial level.
- `NTF-5` Notifications shall be actionable: each carries the one action the user most likely needs.

### 7.7 Self-explaining change record (P1)

- `CHG-1` Every change (PR) shall have a change record showing: the plan, the trace, what was checked, what was judged fine, and what the agent was unsure about (Q26). This directly answers the "how did it review" concern that shaped the Gitar decision.
- `CHG-2` The record shall show ticket intent versus what changed, and whether the change meets that intent. **Assumption**: this carries forward the ticket-intent verification idea from fn-review-agent, per the layer mapping (Q26 and the fn-review-agent Park note).
- `CHG-3` The record shall show review and gate outcomes (Gitar risk matrix, Sonar, CI) and whether a human approved. Today nothing merges without human review, and the UI shall say so clearly (Q6).
- `CHG-4` The record shall show cost for that change (Q15).
- `CHG-5` The prototype shall show the same record in two forms: the command center page and a GitHub PR description or comment (mocked).
- `CHG-6` After merge, the record shall continue to track "stayed merged" and production outcome, since "accepted change" means merged and stayed merged (Q15).

### 7.8 Governance and control plane (P1 for visibility, later for depth)

Q6, Q11, Q12, Q24.

- `GOV-1` A Governance area shall show: platform status, gates, agent identities, tool catalog, approvals, and kill switches.
- `GOV-2` **Gates.** The user shall be able to see which gates apply to which action, starting with the merge gate (Gitar risk matrix, Sonar, human review). Non-merge actions (deploy, data access, spend, agent permissions) shall appear as defined-but-unconfigured gates, since the brainstorm did not set a day-one list (Q11 open flag). Default stance is minimal guardrails unless explicitly set (Q6).
- `GOV-3` **Agent identity.** Each agent shall have a profile showing its principal, scopes, named owner (a person or team), tools it can use, and recent activity (Q12). Autonomous runs show the agent's own scopes only. Interactive runs show the human's delegated access scoped to that session.
- `GOV-4` **Tool catalog (MCP gateway).** The catalog shall show tiers: open read-only tools for any agent; write and sensitive-data tools granted per agent with an approver; new tools registered and reviewed before entering the catalog (Q12).
- `GOV-5` **Approvals.** A request flow shall let an agent owner request a higher-tier tool, and an approver (the dedicated approval team, composition TBD) accept or reject with a reason. **Assumption**: the prototype labels the approver role "Tool approval team" without naming members.
- `GOV-6` **Model access (AI gateway).** The Governance area shall show which models are available, to whom, with spend limits per agent or team (Q11).
- `GOV-7` **Pause and kill.** The user shall be able to pause or stop any single agent, and an admin shall be able to pause the whole platform, from phase 1 (Q24). Platform-wide stop shall require confirmation and show its effect before applying. The conditions that would trigger an automatic platform-wide stop are undecided (Q24 open flag), so the prototype shows manual control only.
- `GOV-8` **Audit.** Every governance action (approval, pause, scope change) shall appear in an audit log with who, what, when, and why.
- `GOV-9` Role-based visibility: in the prototype, switching roles shall change which governance actions are available.

### 7.9 Measures and cost (P1 for cost capture, grows later)

Q15, Q19.

- `MSR-1` The Measures area shall headline **cost per accepted change**: spend divided by changes that merged and stayed merged (Q15).
- `MSR-2` Supporting measures shall include throughput, quality (for example revert rate), human leverage, outcome, and platform health (Q15). Existing metrics such as True Throughput and PR cycle time can serve as realistic sample values.
- `MSR-3` Cost shall be drillable from platform to team to repo to change, and to individual runs.
- `MSR-4` The prototype shall show the long-term roll-up to **cost per developed feature**, where change costs roll up into a feature including upstream shaping costs (Q15). **Assumption**: a feature is a Linear project or initiative for prototype purposes. Feature identity and whether human time is costed are open (Q15 flag), so the prototype includes a visible toggle for "include human time" to provoke the conversation.
- `MSR-5` Platform health shall show performance measures: run start latency, sandbox start time, concurrency, and event stream lag (Q19, performance is the top worry).
- `MSR-6` Tools whose spend cannot be seen by the platform shall be marked as outside the cost number, so the figure is not presented as complete when it is not (layer mapping, Copilot constraint). **Assumption**: shown as a small "not included" note.

### 7.10 Knowledge layer (P1 for skills and repo context, P2 to P3 for the rest)

Q5, Q7, Q10. One knowledge layer in three tiers, so later phases extend it rather than replace it.

- `KNW-1` Knowledge shall be browsable by tier: (1) skills and playbooks, (2) repo context (AGENTS.md and repo-committed skills), (3) cross-cutting context (Q5, Q5b).
- `KNW-2` Every item shall show a status: **current**, **under review**, or **superseded**, plus an owner and a re-review-by date (Q10).
- `KNW-3` Items not marked current shall be visually distinct and labelled "hint only, not source of truth" (Q10).
- `KNW-4` Items shall show what they govern (services, repos, events) and flag themselves for review when something they link to changes substantially (Q10).
- `KNW-5` Superseded items shall link forward to what replaced them, so history survives (Q10).
- `KNW-6` Tier 3 shall be shown as **indexed in place**, with source badges (Confluence TDRs and runbooks, standards repo converted to skills, PagerDuty incidents, entity topology for architecture and ownership, Google Docs, customer interviews, Slack, Linear) (Q7). The prototype does not pick a connector vendor, since that choice is open (layer mapping).
- `KNW-7` Items that carry sensitive material (for example customer interviews) shall show an access marker, and the prototype shall show how access gating appears to a user who lacks it (Q8 flag, Q16 note).
- `KNW-8` For any run, the user shall be able to see which knowledge items it used, so they can judge whether it had the right context.
- `KNW-9` **Repo-committed projection.** Where a skill or policy applies to a repo, the UI shall show whether it is committed in that repo, since service-side tools only see repo content (layer mapping, MOI-97 constraint).

### 7.11 Shaping and TDRs (P2)

Q8, Q9. The factory becomes the place where discovery and specs happen.

- `SHP-1` The Shaping area shall show customer and business signals coming in (customer interviews, Slack, Google Docs, Linear), with source and sensitivity markers.
- `SHP-2` The user shall be able to work with agents to synthesize signals into problem briefs, specs, and TDRs, inside the factory, with humans and agents co-authoring.
- `SHP-3` Shaping artifacts shall be living, linked documents with status (current, under review, superseded), so staleness is detectable (Q9, Q10).
- `SHP-4` TDRs shall be authored in the factory, not Confluence, with links to the services, repos, and events they govern (Q9).
- `SHP-5` The **traceability chain** shall be a first-class view: customer signal to spec to ticket to PR to production outcome, navigable in both directions (Q9).
- `SHP-6` The chain shall answer "did what we shipped solve the problem we started with?" by showing the original signal next to the measured outcome.
- `SHP-7` The prototype shall include one end-to-end chain (see section 10) so the whole idea can be walked through.
- `SHP-8` A VOC (voice of customer) example shall be included as a realistic anchor use case, since Cory is considering it. **Assumption**: seeded with fictional interview themes only. No real customer data goes in the prototype.

### 7.12 Run learnings (P3)

- `LRN-1` Agents shall be able to propose what they learned from a run (a missed convention, a useful pattern) as write-backs to the knowledge layer. Humans curate: accept, edit, or reject (Q5).
- `LRN-2` A curation queue shall list proposed learnings with the run that produced them, and the effect of accepting them.
- `LRN-3` Gate misses (a bad verdict, a missed defect) shall feed the learnings loop and be visible as candidate guardrail changes (Q6, Q24).
- `LRN-4` Contradiction detection and usage feedback arrive here (Q10), shown as flags on knowledge items.

### 7.13 Other surfaces (mocked, P1)

Q3. People should be able to meet agents where they already work. The prototype mocks these as panels that look like each tool and share the seeded state.

- `SRF-1` **Slack**: a thread where an agent posts a run summary, accepts a steer, and asks for a decision, with a link into the run view.
- `SRF-2` **Linear**: a ticket showing a Fieldworks status block (run state, cost, link) and a one-step handoff control.
- `SRF-3` **GitHub**: a PR with the self-explaining description and gate results (see `CHG-5`).
- `SRF-4` **CLI/IDE**: a terminal-style panel showing the same session, supporting take over and hand back.
- `SRF-5` A "same session" indicator shall make clear that the user is looking at one shared session across surfaces.

### 7.14 Agents: roster, builder, and marketplace (P4)

Q4. Levels 1 and 2 first, level 3 and marketplace later.

- `AGT-1` The Agents area shall list all agents with owner, level, status, last run, and cost. Every agent, at any level, shall show the same platform contract: identity, scoped permissions, place on the map, cost and trace, and shared knowledge (Q4).
- `AGT-2` **Level 1 builder (configured agent).** A guided form shall let a user pick a model, write instructions, and attach skills and tools (Q4).
- `AGT-3` **Level 2 (triggered agent).** The builder shall let a user add a trigger: schedule, Linear label, Datadog alert, or Slack message (Q4).
- `AGT-4` The builder shall show which tier of tool the agent will use. Open-tier tools deploy freely, including for non-engineers. Higher tiers route to the approval flow (`GOV-5`) (Q12). **Assumption**: non-engineer self-serve for open-tier tools is implied by the brainstorm but not explicitly confirmed (Q12 flag).
- `AGT-5` The builder shall make the agent's named owner required.
- `AGT-6` **Level 3 (bring your own code)** shall appear only as a "not yet" entry with an explanation. The data model and screens shall not make it impossible later (Q4).
- `AGT-7` The **marketplace** (share, find, fork agents and skills) shall be shown as a future concept, built on the same knowledge layer (Q5).
- `AGT-8` Deploying an agent shall add it to the Floor with its own presence and cost visibility.

## 8. Non-functional requirements

- `NFR-1` **Performance feel.** Navigation between views shall feel instant, and simulated live updates shall not cause layout jumps or flicker. Performance is the top worry (Q19). If the prototype is also used to test large views, it shall be seeded with enough agents and items (for example 50 or more cards) to show how the map copes.
- `NFR-2` **Accessibility.** The prototype shall meet WCAG 2.1 AA as a target, with state shown by more than color, keyboard navigation for all primary actions, and visible focus.
- `NFR-3` **Responsive.** The command center shall work on desktop first and remain usable on a tablet. A phone view is needed at least for Inbox and run status, since people may check in away from their desk.
- `NFR-4` **Light and dark themes** shall both be supported.
- `NFR-5` **Honesty about data.** Every screen shall make clear when it is showing sample data, with a persistent "sample data" marker, so a screenshot cannot be mistaken for real results. The prototype shall contain no real customer data, credentials, or real employee performance data.
- `NFR-6` **Brand.** The UI shall use Field Nation brand tokens (colors, typography, logo) from the `fn-brand` skill. The UI skill work on MOI-93 uses the same source. This is an internal product, but consistent branding helps adoption.
- `NFR-7` **Vocabulary.** Core terms shall match the brainstorm: Floor, run, agent, gate, change record, knowledge tier, trust dial. Vendor names appear only as source or tool labels, never as core nouns.
- `NFR-8` **Extensibility.** Screens shall be built from reusable components driven by one data model, so adding a new view or agent type does not require redoing existing ones.
- `NFR-9` **Single-file or simple deploy.** The prototype shall be easy to run locally and to share by link, with no backend required.

## 9. Key journeys the prototype must support

These are the demo paths. Each should be completable by clicking through.

1. **Hand off and ignore.** An engineer assigns a Linear ticket, goes away, and gets one notification when the PR is ready with a self-explaining change record. (`STR-1`, `NTF-1`, `CHG-1`)
2. **Watch and steer.** The engineer opens the Floor, finds their run, joins the session, redirects the agent with a chat message, and watches it adjust. (`FLR-4`, `RUN-1`, `STR-2`, `STR-3`)
3. **Takeover.** The agent stalls on an ambiguous requirement. The engineer takes over in a mocked CLI with full context, finishes the change, and hands it back. (`RUN-8`, `STR-4`, `STR-5`)
4. **Review with trust.** A reviewer opens a PR, reads what was checked and what the agent was unsure about, and approves. Cost for the change is visible. (`CHG-1` to `CHG-4`)
5. **Raise the dial.** A new user starts on quiet notifications, then moves to watchful for one repo and sees how the same run now looks. (`NTF-2`, `NTF-3`, `NTF-4`)
6. **Approve a tool.** An agent owner requests a write-tier tool. An approver reviews and approves with a reason. The audit log records it. (`GOV-4`, `GOV-5`, `GOV-8`)
7. **Pause.** An admin pauses one agent, then pauses the whole platform, and sees what each action affects. (`GOV-7`)
8. **Spot stale knowledge.** An architecture lead sees a TDR flagged under review after a linked service changed, and finds the decision that superseded an older one. (`KNW-2`, `KNW-4`, `KNW-5`)
9. **Signal to outcome.** A PM follows one customer signal through spec, ticket, PR, and production outcome, and sees whether it solved the problem. (`SHP-5`, `SHP-6`)
10. **Cost.** A leader reads cost per accepted change, drills to a repo, then to a single run, then toggles human time on and off. (`MSR-1`, `MSR-3`, `MSR-4`)
11. **Deploy an agent.** A non-engineer builds a level 2 agent using open-tier tools, sets a trigger, and sees it appear on the Floor. (`AGT-2`, `AGT-3`, `AGT-4`, `AGT-8`)
12. **Curate a learning.** A tech lead accepts one proposed learning from a run and sees it become part of repo context. (`LRN-1`, `LRN-2`)

## 10. Mock data requirements

The prototype shall be driven by one seeded dataset, realistic but fictional. Use Field Nation themed names where helpful (team names from the mineral and gem convention, such as Emerald, Granite, Platinum, Marble, Sapphire, Moissanite), but do not use real tickets, real PR contents, real customer details, or real people's performance data.

- 4 to 6 teams, 12 to 15 repos and services (a mix of well-prepared and poorly prepared repos for `SYS-4`).
- 8 to 12 agents, mixed: platform-owned, team-owned, and user-built (levels 1 and 2), each with owners, scopes, and a tool tier.
- 40 to 60 work items spread across lifecycle stages, with realistic failure and wait states, including at least one failed run, one blocked run, and one waiting on a human.
- 3 fully fleshed-out runs: a clean success, a stall-and-takeover, and a multi-agent run.
- 1 complete traceability chain with a fictional customer signal (VOC themes), spec, TDR, tickets, PRs, and a measured outcome.
- Knowledge items across all three tiers with a mix of current, under review, and superseded statuses, and at least one sensitive item.
- 2 weeks of cost data that supports cost per accepted change, drillable to run level, including one tool marked "not included".
- A simulation script that emits events (state changes, new items, a failure, a human reply) so the Floor moves on its own.

## 11. Open questions and assumptions to carry into OpenSpec

These come from the brainstorm's open flags. The prototype proposes a default for each so work can proceed, but each is worth settling.

| # | Question | Prototype default | Owner |
|---|---|---|---|
| 1 | What is the first-glance question for the Floor (stuck, running, shipped today)? Q14 | Default to "needs me", switchable | Chuck |
| 2 | Which non-merge actions are gated on day one (deploy, data access, spend, agent permissions)? Q11 | Show all as defined-but-unconfigured | Chuck |
| 3 | Can a non-engineer deploy a level 1 or 2 agent fully alone? Q4, Q12 | Yes for open-tier tools only | Chuck |
| 4 | Composition of the tool approval team? Q12 | Generic "Tool approval team" role | Chuck |
| 5 | What defines "feature" for cost roll-up, and does human time count? Q15 | Linear project, with a toggle | Later |
| 6 | What conditions trigger a platform-wide stop? Q24 | Manual only | Later |
| 7 | Handling rules for sensitive customer data in shaping? Q8 | Access marker and gated example | Chuck |
| 8 | Is the first build read-only for external sources or native shaping? Q9 | P2 native, shown in prototype | Chuck |
| 9 | Interim owner of phase 1 before the platform team exists? Q23 | Not shown in UI | Chuck |
| 10 | Can Claude Code and Copilot genuinely share session state with the command center? Q3 (layer mapping says untested) | Mocked as working | Engineering |
| 11 | Are NetDev's repos verifiable in a sandbox? Q20 | Shown as a readiness example, not a claim | Chuck |

## 12. Suggested OpenSpec change breakdown

A reasonable way to slice this into OpenSpec changes, each producing a clickable increment:

1. **Shell and seeded data model**: navigation, role switcher, sample-data marker, theme, event simulation (`NFR-*`, `FLR-9`).
2. **Floor and run view**: lifecycle map, cards, run detail, actions bar (`FLR-*`, `RUN-*`).
3. **Handoff, steering, and takeover**: chat, join, takeover, Inbox (`STR-*`, `INB-*`).
4. **Change record and surfaces**: self-explaining PR and mocked Slack, Linear, GitHub, CLI panels (`CHG-*`, `SRF-*`, `NTF-*`).
5. **Governance**: gates, identity, tool catalog, approvals, pause and kill, audit (`GOV-*`).
6. **Systems and knowledge**: system view, three knowledge tiers, status and staleness (`SYS-*`, `KNW-*`).
7. **Measures and cost**: cost per accepted change, drill-down, feature roll-up (`MSR-*`).
8. **Shaping and traceability** (P2): signals, specs, TDRs, the chain (`SHP-*`).
9. **Learnings and agent builder** (P3 and P4): curation queue, builder, marketplace concept (`LRN-*`, `AGT-*`).

Changes 1 to 7 map to the phase 1 slice and are the recommended starting point. 8 and 9 can follow once 1 to 7 are clickable.
