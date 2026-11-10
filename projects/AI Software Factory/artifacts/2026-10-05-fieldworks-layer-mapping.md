# Fieldworks Layer Mapping: Where Today's Tools and Initiatives Land

*Follow-up to [brainstorm/2026-10-05-full-software-factory-vision.md](../brainstorm/2026-10-05-full-software-factory-vision.md) (Q18, Q25). Fieldworks is a provisional name. Draft for Chuck's review, not a decision record.*

**How to read this.** Layers are the sorting rule from Q18: **commodity** (buy or compose behind stable contracts), **control plane** (gateways, identity, audit, governance), and **differentiating** (build in-house). Each item gets a disposition: **Stays** (firm call or already in production), **Component** (becomes a Fieldworks building block), **Seed** (not the thing itself, but its output or pattern feeds a layer), **Park** (not part of Fieldworks now, keep the option), or **Decide** (a real open choice). Rows marked "Chuck's call" were stated in the brainstorm, and the Park calls on fn-review-agent and Tessl were confirmed on 2026-10-05. Everything else is my proposed placement, based on workspace state as of 2026-10-05.

## The short read

Most of what exists today lands in two places: the **knowledge layer** (differentiating) and the **review and measurement parts of the commodity layer**. That is not an accident. The H2 enablement work (ai-toolbox, ADRs, OpenSpec, repo readiness) is effectively the phase 1 knowledge layer, built before the platform around it existed.

The gaps are just as clear. **The control plane is almost empty, and that is expected: it will be designed as part of Fieldworks rather than retrofitted from existing tools** (no AI gateway, no MCP gateway, no agent identity model), and three of the four differentiating layers (traceability chain, command center, cost accounting) have no build yet, only precursors. Execution hosting is the one commodity layer with real in-flight work (AgentCore), and it is also the one with an open platform question. Since the phase 1 platform bar is "every run through the gateways with identity, trace, and cost," the control plane is where phase 1 has the longest road.

## Commodity layer (buy or compose)

| Today | Fieldworks role | Disposition | Notes |
|---|---|---|---|
| **Gitar** (O1 code review) | Review gate | **Stays** (Chuck's call) | Selected 2026-09-14. Fixed price per PR fits cost accounting. Open: role-based unblock, commit permissions, apply-fix defaults. Its risk matrix is the model for future merge gating. |
| **Sonar** (scanning, Vortex) | Verification gate and a context source | **Stays** (Chuck's call) | MOI-78 (Sonar findings to agents via MCP) and the Confluence connector make it a context feeder too. MOI-79 (PR pipeline wiring) still open. |
| **Claude Code and Copilot** (engineer harnesses) | Interactive surface and one candidate agent harness | **Component** | Today's human-in-the-loop front door. Copilot code review is off; Copilot as an IDE assistant continues. Fieldworks needs these to share session state with the command center (Q3), which is untested. |
| **Harness** (CI/CD) | Delivery and deploy gate | **Component** | Already the CI/CD standard. O2 M3 built around it. Harness Evals vs DeepEval (MOI-105, MOI-145) is the eval-engine choice that matters for skill and agent gating. |
| **Linear** | System of record for work items | **Stays** (Q9) | The ticket intake for phase 1. |
| **Datadog** | Observability backend | **Component** | `ai-harness-otel` index is the nearest thing to a run-event stream today. Cost risk at org-wide scale was unresolved as of 8/18 (Cory and Ben sign-off not yet sought at that point). |
| **getDX** | Engineering outcome metrics | **Component** | True Throughput, cycle time, revert rate. Feeds measurement but does not produce cost per accepted change. |
| **AgentCore on AWS** (O2) | Execution layer candidate | **Decide** | Only execution-layer investment so far. O2 M1 was marked Yellow in August with GCP and Azure raised. I could not find workspace evidence for M2 to M4 landing after that, so treat status as unconfirmed. |
| **Workato** | Execution or workflow candidate | **Decide** | Under evaluation per the brainstorm and the 2027 draft. Not yet in workspace detail. |
| **Op Cop, Deployer Agent, RFN triage pilot** (Joe, RFN) | First real agents on the platform | **Seed** | Best candidates to validate the run contract. Op Cop is already the O2 test bed. Each would be a level 1 or 2 agent when P4 arrives. |
| **fn-review-agent** (bespoke review prototype) | Superseded as a gate | **Park** (confirmed) | Gitar is the forward path. Harvest its ideas (ticket-intent verification, review trace) into the self-explaining PR requirement (Q26) rather than maintaining a second reviewer. |
| **Tessl** | Skills registry and hosted evals | **Park** (confirmed) | Lost the review decision. Mykhailo's investigation found hosted evals unreliable. Keep only as a reference for the later marketplace phase. |
| **Copilot code review** | Review gate | **Retire** (already done) | Replaced by Gitar. MOI-97 gave a durable lesson, see Constraints below. |

## Control plane (gateways, identity, audit, governance)

| Today | Fieldworks role | Disposition | Notes |
|---|---|---|---|
| **AI gateway (models)** | Central model access, routing, cost controls | **Gap (to be designed in Fieldworks)** | Nothing in place. The 2027 draft lists it for evaluation. Needed for the phase 1 platform bar. |
| **MCP gateway (tools)** | Tiered tool catalog and approval | **Gap (to be designed in Fieldworks)** | Nothing in place. "MCP credential governance" has been an open O2 item with no owner since July. |
| **Agent identity** | Own principal, scopes, named owner | **Gap (to be designed in Fieldworks)** | Only precursor is the Gitar service account in Harness (stored in 1Password), a one-off, not a model. Okta is the natural identity base. |
| **ai-toolbox marketplace distribution** (Okta force-push plus MDM) | Distribution control point for skills | **Component** | Governs what engineers get, not what runs agents. Claude and Copilot MDM distribution are both done. |
| **OTel telemetry** (MOI-80/81) | Audit and trace feed | **Seed** | Content firewall (no prompt or code exported) is a pattern worth keeping. Covers interactive dev tools, not factory runs. |
| **Skill and agent change gating** (MOI-90, 91, 105, 145) | Evals as a governance gate | **Component** | Today skill changes reach engineers without eval gating. The 2027 draft already calls this out. |
| **Gitar risk matrix** | Merge gate policy | **Component** | The only concrete autonomy control. Non-merge actions (deploy, data access, spend) have nothing. |
| **Third-party license intake question** (Asad, 2026-09-21) | Governance hygiene | **Seed** | The open question of whether FN has a standard intake process is exactly the kind of thing a tool approval process needs. |

## Differentiating layer (build in-house)

| Today | Fieldworks role | Disposition | Notes |
|---|---|---|---|
| **ai-toolbox** (skills, `fn-platform`, `fn-software-engineering`, `fn-event-integration`, hackathon skills) | Knowledge layer, tier 2 skills and playbooks | **Stays** (Chuck's call) | The phase 1 knowledge spine. 8+ hackathon skills still need triage. |
| **AGENTS.md and repo-committed skills** | Knowledge layer, repo context | **Component** | Per-repo context. MOI-97 proved service-side agents only see repo content, so this tier is not optional. |
| **ai-repo-score, repo bootstrap, scorecard** | Repo readiness and onboarding | **Component** | Feeds the repo context tier. Packaging (MOI-113, 118, 119, 124) makes it distributable. Question for the map: readiness measures whether context exists, not whether the agent can verify its own work (the Spotify consistency vs verifiability point). |
| **OpenSpec and ADR suite** (MOI-62, `adr-tools`) | Spec and decision artifacts with staleness handling | **Component** | The seed of the traceability chain and of Q10 (status, re-review-by dates). Still local to repos, not linked to tickets or PRs. ADR rollout has four blockers. |
| **schema-registry** | Contract authority for events | **Component** | Source of truth for event contracts, already out of ai-toolbox by design. |
| **Embed model and Emerald findings** | On-ramp and discovery method | **Stays** (runs underneath) | Emerald showed the bottleneck is pre-ticket context, which strengthens phases 2 and 3 of Fieldworks. |
| **Repo scorecard dashboards** (DX and Datadog) | Precursor to the command center | **Seed** | Shows outcomes per repo. Does not show live agents or runs. |
| **Traceability chain** (signal to spec to ticket to PR to outcome) | Differentiator | **Gap** | Pieces exist (Linear, OpenSpec, ADRs, Gitar PR comments). Nothing links them. |
| **Command center and agent map** | Differentiator | **Gap** | Nothing built. AgentsView (MOI-96) and roborev (MOI-94) are ticketed but unevaluated and may inform the run view. |
| **Cost per accepted change** | Differentiator | **Gap** | Inputs exist in pieces (Gitar per PR, getDX, OTel). Copilot costs hit individual budgets and cannot be rolled up. |
| **Cross-cutting knowledge index** (Guru, Glean, Unblocked, Teamwork Graph, Joe's entity topology) | Knowledge layer, tier 3 | **Decide** | Four overlapping candidates. No Guru or Glean work has been done, and the choice will be made as part of Fieldworks. Unblocked is paused and Teamwork Graph is unevaluated. Fieldworks wants "index in place," so this is a connector choice, not a build. |

## Not a layer

**AI-DLC (process transformation, Joe's workstream, PM Playbook, PEV Loop)** is the operating model that runs on Fieldworks (Q23), so it does not map to a layer. **The AI Guild** is a practitioner forum and a feedback channel, not a component. **The archived AWS PEV pipeline** stays archived, though it is useful as reference material when the execution layer gets decided.

## Constraints these findings put on the design

1. **Service-side tools only read repo content (MOI-97).** Whatever the central knowledge layer looks like, skills and review policy need a repo-committed projection, or any vendor-hosted agent will not see them.
2. **The event stream does not exist yet.** OTel to Datadog covers interactive tooling. The Q14 map and Q16 "capture run events from day one" need a Fieldworks event schema, and the OTel content firewall is a good starting rule.
3. **Merge is the only gated action.** Q11 and Q12 gating for deploy, data access, and agent permissions has no existing foundation to build on.
4. **Copilot costs bill to individuals.** Cost per accepted change cannot cover any tool whose spend is invisible to the platform.

## Decisions this surfaces

1. **Execution hosting.** AgentCore, Workato, or GCP and Azure. Needs a go or no-go on whether O2 M2 to M4 actually landed, since that determines how much is sunk.
2. **Knowledge connector for tier 3.** No Guru or Glean work exists yet, so this is a fresh evaluation inside Fieldworks (Guru, Glean, Teamwork Graph; Unblocked is effectively out).
3. **Control plane design.** Buy or compose the AI and MCP gateways, and who designs it. This is the earliest dependency on the phase 1 critical path.
4. **Tidy-up after parking fn-review-agent and Tessl.** Decide whether the unevaluated ticket stubs (MOI-94, 95, 96) stay open, so they stop competing for attention.
5. **Interim ownership of phase 1.** Still open from the brainstorm, and it determines who closes items 1 to 3.

## Gaps in what I could verify

- O2 M2 to M4 status after August (IaC, Harness CD, long-running agent demo) is not in the workspace.
- Workato scope and status is only in the brainstorm and the 2027 draft.
