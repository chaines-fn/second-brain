# Field Nation Tech Context

*Relevant background for AI Software Factory architecture decisions. AI Software Factory is the technical-build workstream of the broader **AI-DLC** initiative (rebranded 2026-07-09 — see [program/overview.md](../program/overview.md#rebrand-to-ai-dlc-2026-07-09)).*

## Platform Overview

Field Nation runs on a mix of a PHP monolith and microservices, built up over time — the repo/tooling landscape is inconsistent (some services have AI tooling, some don't, everything in between).

## Tech Stack

| Layer | Technology |
|---|---|
| Core backend | PHP monolith (custom framework) |
| Microservices | Go, TypeScript (NestJS) |
| Messaging | Kafka (event-driven) |
| Mobile | React Native with Expo |
| Web UI | React with MUI (Material UI) |

## Repository Structure

All over the place — no monorepo standard. Mix of organization. Some repos have some AI tooling configured, most don't. AGENTS.md adoption is effectively zero at the start of the AI Software Factory initiative.

**Note on AGENTS.md vs CLAUDE.md.** The Moissanite team has standardized on **AGENTS.md** as the per-repo context file (Knowledge Tier 1) rather than CLAUDE.md. Reasoning: AGENTS.md is the cross-vendor convention (works with Cursor, Copilot, Claude Code, Codex via APM); we want the knowledge layer portable across whatever Brain candidate wins the Platform spike. Some earlier docs reference CLAUDE.md — that's the same concept, just the older naming.

## AI Tooling (Current)

- **GitHub Copilot** — In use across engineering teams
- **Claude Code** — In use across engineering teams
- **OPCOP Agent** (Joe Hoppesch) — DataDog log analysis and operational diagnostics agent. Uses raw API wrapper against DataDog (not the MCP server). Has scripted skills for DataDog, GitHub, and other tools to prevent model drift. Also starting to open PRs with Copilot reviewing them. Not yet operationalized for broader engineering use.
- **Deployer Agent** (Joe Hoppesch) — Deployment automation agent, in development. Working with Omron on topology mapping.
- **RFN Triage Pilot** (Rafin, RFN team) — Agentic AI for bug triage/debugging. Pilot compared 20+ cases against human engineers — 80% accuracy. One case cut a 6-hour human debug session to 6 minutes by tracing the issue into FN Core. Not yet shared broadly with engineering (TDM presentation pending).
- Not constrained to these — open to expanding the toolchain

### Known Issues

- **Copilot code review inconsistency:** Chuck and Mike have observed that Copilot code reviews inconsistently pull in additional repository files when given instructions (e.g., "use the repository pattern"). Sometimes it follows instructions correctly, sometimes it ignores them entirely. This is a blocker for reliable automated code review at scale and needs investigation as part of the AI Software Factory initiative.

## Go-Forward PEV Build Stack (candidate, 2026-07-08) — ARCHIVED 2026-07-14, not current H2 direction

Drafted as the go-forward architecture for the factory build. **Superseded for H2** by the 2026-07-13 H2 Plan Review (embed-first pivot — see [program/overview.md](../program/overview.md#embed-first-pivot--pilot-scope-narrowing-2026-07-13)); the full doc was moved to [../program/archive/AI Software Factory - Architecture & Overview.md](<../program/archive/AI Software Factory - Architecture & Overview.md>) 2026-07-14 (diagram still lives in [Lucid](https://lucid.app/lucidchart/9b39ee36-446e-4192-b4f5-bb98fd66ef28/edit)). Left below as a historical stack snapshot, not active guidance.

- **Harness:** LangChain DeepAgents on LangGraph. Agents: Plan, Plan Review + Devil's Advocate, Execute (orchestrator + coding subagents), Verify + advisor. The orchestrator owns the fixed loop, human gates, and a hard retry cap.
- **Orchestration:** LangGraph state machine for the POC; port the orchestration tier to **Temporal** for production (durable multi-day human waits, retries, distributed workers).
- **Models (per agent, all on Amazon Bedrock, in-boundary):** Plan = Claude Opus; Plan Review + Devil's Advocate = OpenAI GPT-5.5; Execute = Claude Sonnet orchestrating OpenAI **Codex**; Verify = Claude Sonnet with a Claude Opus advisor. OpenAI frontier models (GPT-5.5, Codex) are GA on Bedrock via the Responses API, so no new vendor relationship is needed.
- **Runtime:** Bedrock AgentCore for the POC (managed runtime, memory-as-checkpointer, sandbox, identity, observability; doubles as M3 evidence); ECS/EKS + Temporal for production.
- **Integrations:** Linear (tickets + async approval webhooks) and GitHub (PRs + code review) as MCP tools; API Gateway + Lambda webhook ingress with dedupe/correlation.
- **Repo + sandbox:** fresh clone and branch per run; tests run in-sandbox, so the sandbox must reproduce FN-Core's Docker dev environment (the single biggest infra lift).
- Supersedes an earlier Managed-Agents alternative, archived in `artifacts/archive/`.

## Engineering Scale

~100 engineers total across US and Bangladesh teams.

## Key Architectural Implications for AI Software Factory

**The PHP monolith is the hard problem.** Custom framework means:
- Agents will have a harder time inferring patterns (no standard Rails/Laravel conventions to lean on)
- AGENTS.md files for services touching the monolith are *more* critical, not less
- Code health assessment before agentic deployment is essential — agents amplify existing patterns
- The monolith likely has uneven test coverage, which is a risk multiplier for agentic development
- In Birgitta Böckeler's vocabulary, the monolith has low **harnessability** (limited computational sensors, weak structural boundaries, custom framework agents can't lean on). This validates the Phase 1 pivot away from FN Core toward Go/NestJS microservices for the M3 verifier.

**Kafka event-driven patterns need explicit documentation.** Async event flows are easy for agents to get wrong (wrong topic names, wrong message schemas, missing error handling). Service-level AGENTS.md must document event contracts explicitly.

**Go and NestJS microservices are more agent-friendly.** These follow established conventions and are more amenable to AGENTS.md pattern documentation. Good candidates for early agentic development pilots and the most likely M3 target repo.

**React/React Native UI is straightforward territory.** MUI component patterns are well-established and Claude Code handles React well. Lower-risk for agentic development.

---

## Current Architecture Model (v0.5, 2026-05-03)

The Moissanite team has converged on a clean architecture. Full working doc: [`artifacts/2026-05-01-moissanite-architecture-working-doc.md`](../artifacts/2026-05-01-moissanite-architecture-working-doc.md).

**Three things on a shared Platform:**
- **Platform layer** — Brain (model + harness), Hands (sandbox + tools), Session (durable event log), Observability. Shared infrastructure used by every PEV stage. **As of 2026-06-02 the evaluation is framed as two separable layers:** the *agent stack* (Deep Agents / Microsoft Agent Framework / Google ADK) vs. the *operational platform* (Azure AI Foundry / Gemini Enterprise / managed Deep Agents). POC = same planning-agent-→-Linear scenario built across options for an apples-to-apples comparison. Hosting mandate: agents default to Gemini, Azure, or managed Deep Agents unless there's a reason to roll your own. (AWS Bedrock AgentCore and the earlier Cursor-SDK-on-AWS spike were dropped.) **Superseded 2026-07-08:** Bedrock AgentCore is back in play as the leaning-favorite runtime (LangGraph on AWS) per the AI SDLC alignment session — see the Go-Forward PEV Build Stack section above. Not formally finalized, but Gemini/Azure/Deep-Agents-managed as the default hosting mandate no longer reflects current direction.
- **Knowledge plane (3 tiers)** — Tier 1 (Repo-Local: AGENTS.md, repo-specific skills/agents); Tier 2 (Shared Library: skills, agents, MCP via APM/ai-toolbox, includes non-negotiables); Tier 3 (Cross-cutting Context: architectural decisions, EDA schemas). **Candidate changed 2026-06-30: Guru, not Unblocked** — see Tooling Status below.
- **PEV pipeline + Verification plane** — Plan → Execute → Verify with human gates. Multi-agent inferential review for verification (Standards / Security / Database / Architecture Fitness / Code Review / AC Verification).

**Brain/Orchestrator split.** Per Anthropic's Managed Agents post, the Brain is decoupled from Hands and Session. The Moissanite team further refined this: the Platform Brain is shared LLM-serving + general-harness infrastructure; each PEV stage has its own **Orchestrator** (workflow controller). Plan, Execute, and Verify all use the same Platform Brain — what differs is the workflow each Orchestrator runs.

---

## Tooling Status

### APM — Agent Package Manager (Microsoft)
**Status: adopted as Tier 2 distribution mechanism.** All shared skills / agents / MCP servers will be distributed via APM through `fieldnation/ai-toolbox`. Ian's PR #3 was the foundational APM work; Chuck's PR #6 is adding standards-for-reviews skills. Default is repo-local (Tier 1), promote to Tier 2 (APM) when proven across 2+ repos.

**Tier 2 sync mechanism, RESOLVED 2026-07-27 (MOI-47): vendored-with-automation.** The long-open drift question is closed. Skills stay **committed** in each consumer repo, but delivery is event-driven and zero-touch:
- **Renovate** detects each `ai-toolbox` release, bumps the pinned tag, and **auto-merges** the PR.
- An **`apm-refresh` workflow** regenerates the committed skills + lockfile on the default branch immediately after merge.
- Bulk **onboarding** runs `apm install` from a runner across N repos (no local `apm` required); a **`fleet-status`** dashboard shows which repos are current vs. behind.
- No per-consumer cron, no manual merges.

**Fetch-on-use was spiked and rejected.** Findings: (1) a `SessionStart` hook cannot provision skills for the *current* session: Claude Code loads skills from disk at startup, so a hook that creates the skills dir only takes effect next session; (2) a git `post-merge`/`post-checkout` hook can populate before a session, but only if per-clone `core.hooksPath` is activated *and* the `apm` CLI is installed *and* private-repo auth (`gh`/PAT) exists *and* there's network. Any missing link means a session silently runs without the standards. A **notify-only** `SessionStart` hook (checks and nudges "you're behind, pull") was kept as the reliable middle ground. Incidental finding: `apm install` picks up the `gh` CLI token automatically (no extra secret), and `targets: [claude]` is required or skills don't deploy to `.claude/skills/`.

**Why vendored won:** committing guarantees the skills are present in every clone, session, and CI run with zero setup and offline; non-vendored trades that guarantee for a silent-failure chain, the wrong trade for an org-standards mechanism.

**Open items on this decision:** auto-merge means **no per-repo human review of standards changes** (governance call to confirm); infra needs Renovate read-access to `ai-toolbox` plus a GitHub App/PAT for onboarding + refresh; not yet validated end-to-end (needs a cut release + tokens). [ai-toolbox PR #51](https://github.com/fieldnation/ai-toolbox/pull/51) is awaiting Chuck's review and merge to main so the flow can be tested against real repos.

**Update 2026-08-04: emerging pivot away from APM entirely, reopening this decision.** On an Aug 4 call, Vitalii and Chuck independently converged on dropping APM as the packaging layer in favor of native Claude Code / Copilot marketplace-plugin mechanisms — Chuck: *"we don't even need APM anymore at all... if we just purely convert it to the two marketplace approaches that's going to be the easiest way to distribute it."* Findings: a single `marketplace.json` + `.claude-plugin/plugin.json` manifest can serve **Claude Code, VS Code Copilot, and Copilot CLI** simultaneously. VS Code supports agent plugins/marketplaces via `chat.plugins.marketplaces` and auto-detects the Claude plugin format. Copilot's enterprise push (GA July 2026) uses `extraKnownMarketplaces`/`enabledPlugins` (mapped internally from `ChatExtraMarketplaces`/`ChatEnabledPlugins`) across three delivery channels — native MDM (Intune/Jamf/GPO), server-managed (signed-in GitHub account), or file-based `managed-settings.json` — with channel precedence winner-take-all. Copilot also reads `.claude/skills/` directly, same `SKILL.md` format as Claude Code. Caveats: VS Code agent plugins are still Preview; Copilot's update check (every 24h, or manual) trails Claude Code's (near-immediate at session start) by up to a day. [ai-toolbox PR #51](https://github.com/fieldnation/ai-toolbox/pull/51) (now on branch `feat/MOI-47-ai-toolbox-integration-standard`) already has both tools resolving skills locally end-to-end; the one untested piece is force-install/auto-update via managed settings, gated on Chuck's Admin Console access. Sequencing: Claude Code pilot (2-3 machines, force-install + SSH auth) first, Copilot second once proven; APM removal is being built as a separate commit so the pilot can run with APM still standing as fallback. Chuck wants a working POC with a small force-installed group before taking this to the AI Guild, and has come around to pushing all skills to every repo by default ("seems to be the way in which everyone is doing it"), reversing his earlier hesitation. **Not finalized — this is a live reconsideration of the "vendored-with-automation" resolution above, not a superseding decision yet.**

**Update 2026-08-05: reconsideration solidifies into the working direction, backed by usage data — still not merged.** Vitalii checked actual APM adoption and found **zero real consumers**: APM-related config/PR searches came back empty against a working search (a control query for `AGENTS.md` did return hits, so the absence is real, not a search-tooling gap). Meanwhile skills adoption is healthy but happens entirely outside Tier 2/APM — **~186 hand-authored skills across 19 active repos**, all committed directly (fn_react_native 50, meridian 22, agent-sdk 18, netdev-recruitments 14, fieldnation 13). New mechanism detail: nothing gets committed into any of the 739 product repos — delivery is purely via Admin Console managed settings (Claude) + GitHub/MDM managed settings (Copilot). Two things vendoring still wins, stated plainly: reproducibility (old commits no longer pin old skills under latest-always) and CI/offline use (committed skills need no auth; marketplaces need `CLAUDE_CODE_PLUGIN_SEED_DIR` baked into images or `gh auth setup-git` in pipelines). One real gap: Copilot's *cloud* surfaces (PR review, coding agent, github.com chat) run in GitHub's environment and read `.github/skills/` from the repo directly — managed settings can't reach them; needs org custom instructions or a thin `.github/skills/` later (phase two). Bug found along the way: `consumers.json` has been pointing at `fieldnation/fn-core`, which 404s — should be `fieldnation/fieldnation` — so the old delivery pipeline was aimed at a repo that doesn't exist. **PR #51 renamed to `feat(MOI-47)!: replace APM with a plugin marketplace for Claude Code + Copilot`** (breaking-change marker) and now mostly *deletes* the APM delivery machinery (Renovate preset, `apm-refresh`, `consumers.json`, `apm-onboard`, `fleet-status`) rather than merging it — keeping Harness validate/release pipelines, CHANGELOG discipline, the eval convention, and `apm audit`'s hidden-character scan folded into CI validate. Still to prove before deleting anything for good: whether Claude Code's managed-settings auto-install reliably fires (an open, inactive upstream GitHub issue reported it not installing at all in one case) and private-repo auth via SSH for background auto-update. Chuck is actively reviewing the renamed PR. Org-rollout mechanism: an engineering-wide "birthright" group (Okta/C1, likely set up with "Paul") so the plugin marketplace force-installs for every engineer automatically — Chuck is setting up a review meeting with Cory, Ben Moormeier, and Mike Mueller for 2026-08-11. See [program/overview.md](../program/overview.md#moi-47-apm-removal-becomes-the-working-direction-skill-builder-tooling-started-allcloud-read-as-validation-not-new-direction-2026-08-05).

**New ai-toolbox capability gaps flagged 2026-08-11 (Chuck, standup note) — tickets filed same-day, confirmed 2026-08-12:** (1) **MOI-90**, "Evals for ai-toolbox for ensuring skill is invoked properly" — evals for ensuring a skill is actually *invoked* (using its description), distinct from evals of a skill's content/output quality; (2) **MOI-91**, "CI for ai-toolbox" — Harness CI/CD to run evals automatically whenever skills change. Both Backlog, unassigned as of 2026-08-12.

**MOI-47 resolved: marketplace model confirmed at the "AI-DLC Plugin Roll-Out Strategy" meeting (2026-08-11, 2:00-2:30pm CT).** Chuck, Cory, Ben Moormeier, and Mike Mueller (Joe optional) agreed:
- **Moving forward with marketplaces for both Claude and Copilot, built on the existing `ai-toolbox`** — ai-toolbox becomes "the engineering marketplace," the single source for both tools.
- **Claude Code distribution:** an **Okta group** force-pushes the common engineering plugin; **MDM** pushes the marketplace configuration to Claude Code.
- **Copilot distribution:** **MDM** pushes the Copilot marketplace settings.
- **MOI-90 and MOI-91** (the two ai-toolbox capability-gap tickets Chuck added — skill-invocation evals and CI for ai-toolbox) are **not blockers for the rollout** — they're additional hardening work that can proceed in parallel.

This confirms the 8/4-8/5 APM-removal pivot and closes the open "what does the group decide" question from the 8/12 sync. PR [ai-toolbox#51](https://github.com/fieldnation/ai-toolbox/pull/51) still needs to be merged to put this into effect; next mechanical steps are standing up the Okta group and the MDM push configs for both tools.

**MOI-47 marked Done 2026-08-17 — PR #51 merged, but Harness CI execution isn't wired up yet.** Vitalii's final comment on the ticket confirms what shipped: the `fn-platform` bundle (org-standard skills — `fn-api-design`, `fn-ddl-design`, `fn-opentofu-module` — as one versioned, pinnable dependency), a self-contained consumer auto-update workflow (`templates/apm-update.yml`: weekly cron bumps the pinned tag → runs the official `microsoft/apm-action` → opens a review PR, never auto-merges), and ai-toolbox's own CI/CD moved from GitHub Actions to **Harness** (`.harness/` validate + release pipelines), released as **v0.2.0**. Chuck flagged 8/17 that the Harness config exists in the repo but he couldn't see it actually executing anywhere; Vitalii's 8/18 follow-up confirmed three requirements still have to be added by hand in the Harness UI (can't live in git): a webhook on `fieldnation/ai-toolbox` so Harness picks up `.harness/` from main, confirming the `account.GitHub` connector can reach the repo, and the PR-trigger (`ai_toolbox_validate`) + push-trigger (`ai_toolbox_release`) pair. Whoever has Harness admin access needs to either do this or grant Vitalii access — open as of 2026-08-18. Once wired, `validate` becomes a real required check on ai-toolbox PRs, which is the mechanical prerequisite for MOI-91 (CI for ai-toolbox evals). Separately, a new **ai-toolbox PR #60** is up for review (Chuck, posted in #eng-guild-ai 8/18) to finish out the Copilot-integration side of this work. **MOI-56 ("Publish and document the scorecard") also moved to Done 2026-08-17**, closing out O1 KR1 end to end (scorecard built *and* documented).

**Update 2026-08-18: Harness admin gap resolved — Joe owns wiring it up.** Chuck DM'd Joe asking who to talk to about getting the Harness pipeline for ai-toolbox CI set up ("we have the scripts, just need someone to put them in Harness — don't think I have access to do that"). Joe: "I can do #1." Chuck pointed him at [ai-toolbox `.harness/`](https://github.com/fieldnation/ai-toolbox/tree/main/.harness). Closes the "needs a Harness admin" open item from 8/18 above; Joe still needs to actually do the webhook/connector/trigger setup.

**Update 2026-08-17/18: Claude Code side of the marketplace is live for engineers.** Chuck, 8/17 in #moissanite-eng: "I'll make an engineering announcement tomorrow once I get copilot MDM settings pushed but great work all. Our ai-toolbox is now available in claude code and desktop!" The Copilot side is what's blocking the wider announcement — see the MDM push test failure logged above. No org-wide announcement had gone out as of the 8/19 sync.

**Same DM thread, second question: OTel-for-all-of-engineering cost risk, unresolved.** Chuck asked Joe whether turning on the Claude/Copilot OTel span export (MOI-80/81's mechanism) for all of engineering, not just the pilot, would blow up the Datadog bill. Joe: possibly, since Datadog pricing was just renegotiated on log volume; proposed a separate index (to track spend in isolation) and asked whether 3-day retention would be enough. Chuck wants a week but "could live with 3 days." Both agree this needs Cory + Ben Moormeier sign-off before rolling out further — not yet sought as of 8/18.

**New: Vitalii extended Claude OTel telemetry into the `ai-repo-score` toolchain itself (2026-08-18).** [ai-repo-score PR #6](https://github.com/fieldnation/ai-repo-score/pull/6) adds Claude Code OTel telemetry to Asad's repo-scoring tool, with an example trace confirmed landing in Datadog. This is the OTel/MOI-80 work now feeding the repo-readiness toolchain's own development, not just general engineering sessions.

**New evaluation candidates ticketed 2026-08-18, all Backlog/unassigned, under a new "Additional Tooling" milestone (not one of the four canonical O1 KR milestones):**
- **MOI-94 "Investigate roborev"** — [roborev.io](https://www.roborev.io/), advertised as an "internal code review for agents" tool; worth checking whether it belongs in the AI-DLC toolkit. Another candidate in the increasingly crowded O1 code-review space (alongside Gitar, Tessl, `fn-review-agent`, and now native Copilot review — see MOI-97 below).
- **MOI-95 "Clanker constitution"** — investigate [github.com/kenn-io/constitution](https://github.com/kenn-io/constitution) for possible value to AI-DLC. Not yet reconciled with any existing workstream.
- **MOI-96 "AgentsView"** — investigate [agentsview.io](https://www.agentsview.io/) as a potential token-usage-optimization tool.

**MOI-97 "Copilot code review + agent skills" — new, In Progress (Asad Ali), a sixth angle on the O1 code-review decision.** Chuck flagged in #moissanite-eng (8/18) that GitHub Copilot code review can now use repo-local agent skills (`.github/skills/`, ideally a `code-review`-named directory) and MCP servers, with each review comment carrying an attribution footer naming which skill/MCP server produced it — [GitHub docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review). The ticket's core open question: **do marketplace-distributed skills (the `fn-platform` bundle shipped via MOI-47) actually reach Copilot's automated PR review, or only the interactive Copilot agent?** Plan: stand up a `.github/skills/code-review` skill in `core-ui`, open test PRs, and use the attribution footer to confirm both repo-local and marketplace-distributed skills get picked up. Asad picked this up same-morning (8/19); in #eng-guild-ai, Safin Ahmed separately asked whether this overlaps AI-DLC's code-review exploration — Chuck confirmed someone's already testing it, avoiding duplicate work.

**Copilot MDM marketplace push — mechanism researched 2026-08-18 (per VS Code enterprise docs, current as of 8/12/2026).** To register the ai-toolbox marketplace via MDM with no plugins force-enabled by default, use the **Copilot managed settings native MDM channel** (governs both VS Code and Copilot CLI from one definition — the right fit since MOI-47 wants one marketplace definition for both tools):
- **Windows:** registry key `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\GitHubCopilot`, value `extraKnownMarketplaces` (structured setting → JSON string), e.g. `{"ai-toolbox":"fieldnation/ai-toolbox"}`.
- **macOS:** managed preferences domain `com.github.copilot`, same key/value.
- Do **not** set `enabledPlugins` — omitting it means the marketplace becomes searchable (`@agentPlugins` in the Extensions view) but nothing auto-enables; developers opt in per plugin.
- Verify on a test machine with the **Developer: Policy Diagnostics** command (Managed Settings section).
- **Not verified against a working example:** the exact JSON shape for `extraKnownMarketplaces` when adding an auto-update override (docs only confirm the plain `"owner/repo[#ref]"` shorthand string), and whether `com.github.copilot` plist values are typed as strings or dicts. Test on one machine before pushing at scale. Full writeup with Windows/macOS steps handed to Chuck as a standalone doc; see [artifacts/copilot-ai-toolbox-marketplace-mdm.md](../artifacts/copilot-ai-toolbox-marketplace-mdm.md).
- Separate from this: VS Code also has its own ADMX/`.mobileconfig` enterprise-policy path (`Software\Policies\Microsoft\VSCode`, same `ChatExtraMarketplaces`/`ChatEnabledPlugins` policy names) — VS-Code-only, doesn't cover Copilot CLI, and if both paths configure the same key the Copilot managed-settings value wins.

**Update 2026-08-18: first live MDM push test — policy applied, marketplace still didn't surface.** Chuck asked Paul Wochnick (owns FN's MDM, per Ben Moormeier) to push the `com.github.copilot` / `extraKnownMarketplaces` profile to his own machine as a test subject. Paul pushed a "Visual Studio Code v0" MDM profile; it took about an hour to land, and Chuck confirmed via Mac settings that the profile was installed. Despite that, the marketplace did not appear in Copilot. Chuck's hypothesis (checked and confirmed correct, so ruled out): the plist might be typing `extraKnownMarketplaces` as a `<dict>` instead of a `<string>` — Paul confirmed the pushed profile does use `<string>{"ai-toolbox":"fieldnation/ai-toolbox"}</string>` as expected. Chuck's next hypothesis, not yet confirmed either way: **the MDM channel may simply not reach Copilot CLI** (as opposed to VS Code's Copilot extension) — he asked Paul to remove the test profile so he could try an alternate approach. **Open, unresolved as of 8/18 EOD:** why a correctly-typed, confirmed-installed profile isn't surfacing the marketplace in Copilot. This is the first real-world test of the mechanism documented above, and it didn't work on the first attempt.

**Emerging Tier 2 skill-splitting pattern — resolved 2026-07-16/17.** Vitalii's fn-event-design rework ([ai-toolbox PR #52](https://github.com/fieldnation/ai-toolbox/pull/52), MOI-48) proposed splitting cross-cutting skills by *authority* rather than by *role*; the split landed as follows:
- **Skill recast: `fn-event-design` → `fn-event-integration`.** Reviewer feedback on naming ("you're implementing, not designing") plus the "ai-toolbox = skills spanning 2+ repos" principle drove the split. `fn-event-integration` owns layer B (integration reliability — producer transactional outbox, consumer idempotency/DLQ, schema-evolution rollout) and layer C (per-language recipes: `php-producer.md` grounded in `fieldnation`, `go-consumer.md` grounded in `provider-match`). Lives in ai-toolbox (MOI-48, In Review, PRs #52 + #55, pending `@fieldnation/principals` approval).
- **Layer A (contract authority) moved out of ai-toolbox entirely**, to **`fieldnation/schema-registry`** — proto authoring, topic naming/infra, the `EventMetadata` envelope shape, versioning mechanics (MOI-52, In Progress). Rationale: contract authoring is single-repo, so it doesn't belong in a cross-repo Tier 2 skill.
- MOI-51 (an earlier separate Home-2 ticket) was closed as a duplicate, subsumed into MOI-48.
- **Eval results (Opus-graded, `claude-opus-4-8`):** with-skill 100% (63/63 assertions) vs. baseline 70% (44/63) — a +30pp delta across 14 evals covering generate/detect/confirm for every requirement. 10 additional real-repo validation runs (read-only, against `fieldnation`/`provider-match`/`schema-registry`) all passed with no skill defects.
- This is now a candidate general pattern for other Tier 2 skills, not just event design — the "split by authority layer, not by producer/consumer role" principle.
- **Update 2026-08-14: PR #55 got a "changes requested" review, reopening the pending-approval status.** A reviewer (GitHub handle `AHS12FN`) flagged that the branch is missing NestJS producer/consumer code (pointing to a related PR #36) and that the branch itself may be out of date. Still awaiting `@fieldnation/principals` approval, not yet merged.

**Sonar integration now has concrete tickets (2026-07-16), both assigned to Joe:** MOI-79 (wire Sonar into the PR pipeline as a standard review-gate step — findings posted back before merge) and MOI-78 (expose Sonar's findings/rules to coding agents via MCP as a feed-forward context source, alongside the readiness score and spec/ADR tooling).

---

## Warp Factory Scope Expands and Commercial Terms (2026-11-04 to 11-09)

- **Scope.** The factory now covers the ONYX team (MOI-213), the Engineering factory, and `work-order-management` (MOI-229 Done, MOI-231 In Review, PR #140 posted ready for review 11/09). Linear team filter removed (MOI-222), so access is gated by Warp accounts.
- **Gating simplified.** MOI-243 and MOI-244 removed blockers that stopped PR and retry work on unverifiable bookkeeping (both Done). Clean-approval gates accept verified Gitar approvals (MOI-220, MOI-223, MOI-236).
- **Runner issues.** PHP runner validation runs ended without transcripts or artifacts on 11/05, and Warp is investigating an artifacts-screen dependency in the factory on older runs (11/06). Status open.
- **Terms.** Warp order form and legal terms arrived 11/04. Redlines sent 11/06 on performance and material-change language. Outcome not recorded.

_(recorded 2026-11-09 from Slack, Linear and Drive. No Warp decision is recorded.)_

## Factory Q4 Backlog and Warp Factory Repo (2026-10-30 to 11-02)

- **MOI-195 Done 11/02.** Chuck built an automation on the factory itself: a watcher on PRs carrying a factory label that addresses Gitar and Sonar comments (Sonar via the Sonar MCP) and Wiz findings (via the Wiz MCP). Example PRs cited: core-ui #707 and fn-engineering-standards #71. MOI-194 (Wiz MCP) is Done.
- **MOI-202 In Review 11/02.** CODEOWNERS in `fieldnation/warp-engineering-factory` with `* @chaines-fn` as sole owner. Verified the org ruleset already requires code-owner approval on the default branch.
- **Open Q4 items (Backlog unless noted).** MOI-196 move MCP servers from Chuck's OAuth to a service account. MOI-198 dedicated AWS account for factory runners, infra and Bedrock. MOI-199 custom runners with PHP (FN Core), Node and Go tooling. MOI-200 resolve skill discrepancies across factory, ai-toolbox and fn-engineering-standards (unassigned). MOI-201 add fn-engineering-standards skills with auto-sync. MOI-197 product team and factory (In Progress). MOI-192 sync from ai-toolbox is Done; MOI-193 conflict resolution was Canceled 10/30. MOI-191 pilot is still In Progress.
- **Warp Slack app approved 10/30 for one test channel only.** Ben Moormeier's condition; Warp's SOC 2 is not yet a full year. No additional apps or bots without review.
- **Platform direction not yet reconciled.** Chuck's 10/30 DM says the Warp POC "likely becomes the factory for FN." The platform sections above still show AgentCore as the leaning Fai path. Reconcile after the 11/04 Warp working session produces an outcome.

_(recorded 2026-11-02 from Linear, Slack and Drive. The Warp "likely" statement is Chuck's working read, not a decision.)_

## Fai Q4 Walking-Skeleton Spikes (Gate 1), 2026-10-08

The Q4 OKR for Fai ([program/OKRs/q4-2026-okrs.md](../program/OKRs/q4-2026-okrs.md)) starts the walking skeleton in an existing sandbox while keeping the run contract portable. The draft spike plan is [artifacts/2026-10-08-fai-spike-ticket-drafts.md](../artifacts/2026-10-08-fai-spike-ticket-drafts.md), built on the [AWS architecture draft](../artifacts/2026-10-07-fai-aws-architecture-draft.md) and the [agent definition standards research](../artifacts/2026-10-07-agent-definition-standards-and-agentcore-deployment.md). Slack notifications show MOI-153 through MOI-162 created in Linear at 08:59 CDT on 2026-10-08. The draft still reads "nothing created yet," so verify the IDs in Linear before relying on them.

The spikes gate the platform choice:

- **Sandbox:** MOI-157 tests AgentCore Runtime as the per-run sandbox. Its caps are 2 vCPU, 8 GB memory, a 2 GB image, and a 14,400-second maximum session. MOI-158 tests a per-run EC2 sandbox, which has none of those caps and could run the fn-core sandbox image, but FN would operate isolation, startup, and teardown itself.
- **Harness:** MOI-153 (pi, MCP support unconfirmed), MOI-154 (OpenCode, not yet evaluated), and MOI-155 (Strands, the leading candidate because the managed AgentCore harness is built on it). MOI-156 tests the managed AgentCore harness for config agents and per-agent, per-run cost attribution.
- **Plumbing:** MOI-159 sets the S3 layout and read-only snapshot access for runs. MOI-160 decides how ai-toolbox marketplace skills reach a run, since runs do not accept skill overrides and version pinning is deferred. MOI-161 captures cost per run keyed by run_id and reconciles it to Bedrock usage; it is blocked by the Bedrock IAM role under MOI-149. The Q4 OKR (KR4) requires cost per agent and per run for 100% of runs. MOI-162 defines one agent by hand, deploys it to a harness endpoint, and runs it on demand, which settles the definition format and storage location.

Which harness and which sandbox the walking skeleton uses is undecided until these spikes report.

**Fai home repo (2026-10-08):** Chuck scaffolded a basic repo for Fai at `fieldnation/ai-software-factory`, structured as a mono repo for now (Slack, #moissanite-eng, 10/08 13:53 CDT). Repo layout and contents were not reviewed in this sync.

## AgentCore Harness Spikes (MOI-167 / MOI-168), 2026-10-13

Two Fai spikes opened 2026-10-13, both owned by Mykhailo and In Progress. **MOI-167** evaluates the Amazon Bedrock AgentCore Harness as a managed execution environment for autonomous coding tasks, without custom orchestration or Kubernetes coding sandboxes. **MOI-168** evaluates event-driven execution and human-in-the-loop workflows on AgentCore, where runs are triggered externally, wait for approval, and resume reliably after interruption.

**First result (2026-10-13 standup):** Mykhailo ran a real repository bug through the harness's built-in sandbox and it was fixed, matching his local run. Limits found: the default sandbox lacks Node, NumPy, uv, and tar, so images would need pre-building (options raised: an S3 hot cache of the cloned repo, or pre-built ECR images). X-Ray tracing was not enableable for his user. The harness includes an `ask human` tool, which Mykhailo considers usable for HITL. Proposed HITL pattern: ambient agents on event-driven connections, with DynamoDB holding run status for idempotency. Chuck granted full sandbox access (including local Terraform) with teardown after use.

**Direction (Chuck, same day):** start the architecture with simple AWS services and defer Temporal until there is a reason for it. Compare the AgentCore work with **Warp Factories** (a Warp product Chuck thinks may cover much of the infrastructure need; call Fri 10/16) and with open-code and pi as alternative harnesses. Warp is also logged as a source in [references.md](../program/references.md). The AWS expert call (Wed 10/14) is about AgentCore Harness features. Chuck asked that the AgentCore work continue so the options can be compared directly.

Source: [2026-10-13 standup note](./meetings/2026-10-13-team-moissanite-daily-standup.md). Status of the Fai Gate 1 spikes (MOI-153 to MOI-162) is still not verified from Linear.

**Status update 2026-10-21:** MOI-167 (AgentCore Harness spike) and MOI-168 (event-driven HITL spike) were both marked Done on 10/21 at about 5:53am CDT, owned by Mykhailo. The Linear ticket bodies do not record findings or a decision, so the spike outcomes are unknown from this sync; check the closing comments before treating them as settled. _(recorded 2026-10-21 from Linear; status only, not a decision.)_

## Fai MVP Standup Update (2026-10-22)

- **Planner and execution UI demoed.** Run view with a human approval gate (approve starts the coder), a diagnostics panel tracing Step Functions state and agent tool calls, and an agent metrics panel showing model, input tokens, and output tokens. Source: [2026-10-22 standup](./meetings/2026-10-22-team-moissanite-daily-standup.md).
- **Suspected token accounting bug.** Output tokens are reported about 13x below input tokens, which Mykhailo judges implausible and attributes to the coder agent. Treat the metrics and the cost-per-run baseline as unreliable until fixed.
- **Scope:** only Python repos work today because the Docker images cover Python. Node support needs new images. Core UI (Node/React/TypeScript monorepo) is the chosen first non-Python test target. Repo selection remains manual.
- **Sonar MCP integration proposed** by Chuck so repo Sonar rules feed forward into agent runs. Agreed as the next test case. Not yet built.
- **LiteLLM supply-chain concern (security).** Mykhailo reported a compromised LiteLLM package (transcript wording uncertain) that is risky on install, not only on import. Removed from the harness migration PR but still present in a development environment (skill creator). Advisory link requested by Chuck. Affected versions and FN exposure are unknown.
- **Infra comparison:** Warp proof of concept starts Mon 2026-10-26, to compare against the AgentCore-based Factory.
- **MOI-182 (opened 2026-10-22, In Progress, child of MOI-169):** validating AgentCore run tracing against existing AWS observability. An IAM error on `xray:UpdateIndexingRule` blocked the X-Ray indexing setup. Status only, not a decision.

_(recorded 2026-10-22 from the standup transcript and Linear; the token figures and LiteLLM details are unverified.)_

## Fai MVP Calendar Signals and Harness Connector (2026-10-23)

- **Harness AWS connector added.** Chuck added a Harness AWS connector, `ai_toolbox_connector` (OIDC, Harness Platform connectivity). MOI-183 (created 10/23 by Mykhailo, In Progress, child of MOI-145) moves the `ai_toolbox_eval` pipeline's Bedrock auth from the `plugins/aws-oidc` step to this connector. Related: fn-terraform-dev PR #817 (LAZ-717, Bedrock role for ai-toolbox Harness CI in the DEV account) was opened 10/22 by `iloverink-fn`. PR status not verified.
- **Vendor and gateway calls today, no recorded decisions.** A "Workato & Field Nation: MCP POC Alignment" call runs 1:00pm CDT (organized by Workato; Cory, Mike Mueller, Chuck; Ben Moormeier pending). It follows the 10/19 gateway evaluation. A "Next Steps: Reign Factory + AI Gateway - Field Nation <> iTmethods" call runs 2:00pm CDT (organized by ITMethods' Daniel Goldman; Cory and Chuck). It is the follow-up to the ITMethods factory pitch that Chuck wanted a deep dive on, to compare against Warp. A "FieldNation <> iTmethods - Harness assistance" call was set for 8:00am CDT.
- **No standup today, and the Friday demo is unconfirmed.** No Team Moissanite Daily Standup is on the calendar for 10/23 and no new transcript exists in Drive, so no meeting note was written; the 10/22 note above is current. The Friday 10/23 demo to Cory (planner UI, cost-per-run baseline) does not appear on today's calendar, so its time is unverified.
- **Business Systems batch (10/22, lower signal).** Vitalii's fn-content-auditor tickets moved on 10/22: MOI-178, MOI-177, MOI-152, MOI-137 and MOI-136 to Done; MOI-179, MOI-139, MOI-138 and MOI-135 to In Review. MOI-179 (Python 3.13 move) still needs agreement with Team Pearl on the version.

_(recorded 2026-10-23 from Linear, Slack notifications, Google Calendar and Drive; the vendor calls are scheduled, not outcomes.)_

## Fai MVP Signals, 2026-10-24 to 10-26

- **Shared Factory execution environment (MOI-187, opened 2026-10-26, lower signal until scoped).** A reusable Docker image for the Factory coding environment, targeting `linux/arm64` (required by AgentCore Harness), with Python 3.14, uv, and Node/npm, meant to be the default coding environment for AgentCore Harness and the base for multi-repo execution. This addresses the 10/22 finding that only Python repos work because the Docker images cover Python only. Source: Linear notification in #moissanite-eng-notifications.
- **Warp comparison underway.** Chuck said on 10/24 he may try a generous Warp trial tier in parallel with the Warp PoC (which the 10/22 standup set for Mon 10/26). No outcome recorded yet.
- **Friday 10/23 demo to Cory: no trace found.** No Slack or calendar record of the demo or its outcome was found. Treat as unknown.
- **Business Systems Phase II split (2026-10-25, lower signal).** MOI-186 (prod project plus Harness approval gate) and MOI-185 (OpenTofu for GCP resources) were moved out of Phase I. Phase I now deploys to dev only; prod and OpenTofu move to Phase II. Note: Vitalii is still shown as assignee on these Business Systems tickets, although the 2026-09-16 entry records his transition off the project. Ownership not reconciled.

_(recorded 2026-10-26 from Slack notifications and Linear. No 10/26 standup content was captured; see the meeting note for that date.)_

## Fai MVP Build (MOI-169), 2026-10-14

**Scope change: the Fai walking skeleton now has a formal MVP ticket.** MOI-169 ("Build Event-Driven AI Software Factory MVP with Human-in-the-Loop") was created 10/13 and moved to In Progress 10/14, owned by Mykhailo. Scope: event-driven intake from Linear `MOI` issues, AWS Step Functions orchestration with human approval, AgentCore Harness for coding execution, persistent workspace and execution artifacts, automated PR creation gated by CI, Gitar, and Sonar, execution tracing and cost tracking, and Terraform-managed infrastructure. Success criterion: a Linear issue labeled `ai-factory` passes human approval, autonomous implementation, verification, and an accepted, merged PR, with full execution history. Its first PR is [fieldnation/ai-software-factory#1](https://github.com/fieldnation/ai-software-factory/pull/1).

This is the first place the build names Step Functions as the orchestrator, with Temporal still deferred, which matches Chuck's 10/13 direction to start with simple AWS services. Still open: the Linear webhook for the Moissanite team (Issue events) and an `ai-factory` label for the team, both requested by Mykhailo in DM on 10/13 and 10/14 and needing workspace admin action from Chuck. Status of the Fai Gate 1 spikes (MOI-153 to MOI-162) is still unverified.

**Related, lower signal:** MOI-170 (filed 10/14, Mykhailo, In Progress) fixes `instruction_hygiene` in `repo-agent-context`, which flags every documented `npm run <script>` as a conflicting start command. That penalizes repos for documenting more commands in AGENTS.md, and it gates `eligible_for_first_embed` through `agent_guidance`.

_(recorded 2026-10-14 from Linear; verify MOI-169 scope against the ticket before treating it as settled)_

## Fai MVP Milestones and Infra Benchmark (Linear + DMs, 2026-10-16)

MOI-169 now has milestone children, all created 10/14 and owned by Mykhailo. M0 (MOI-171, Linear webhook ingress) and M1 (MOI-172, human-in-the-loop workflow) are Done, and M1.1 (MOI-175, React approval UI with Cognito auth) is Done. M2 (MOI-173, replace the mock workflow with real AgentCore Harness execution) and M3 (MOI-174, quality gates and factory completion) are Todo, and so is M2.1 (MOI-176, use the factory to agentify one real repository: readiness assessment, human-approved improvement plan, AgentCore implementation, re-run readiness, PR). The M2 acceptance criterion is the real test for the MVP: an approved MOI issue must produce a real, test-verified PR, not a demo.

**Infra benchmark.** Chuck is judging the AgentCore path against Warp Factories. In a 10/13 DM with Cory he called Warp "our golden standard right now we are judging everything against." Cory's read was that Warp can centrally control inference profiles and is priced as compute plus an enterprise platform fee. ITMethods pitched their own factory offering, which Chuck said is less customizable than Warp, and he asked them for a deep dive that includes Cory. Chuck also sent Mykhailo the Warp Factories docs on 10/13. No Warp decision is recorded, and the outcome of the 10/16 Warp call is unknown.

_(recorded 2026-10-16 from Linear and Slack DMs; the DM content is Chuck's and Cory's working read, not a decision.)_

## Fai MVP Live Demo, GitHub App Auth, and Gateway Evaluation (standups 2026-10-16 and 2026-10-19)

**Live end-to-end demo (2026-10-16).** Mykhailo demonstrated MOI-169 on a real ticket. A Linear ticket labeled `ai-factory` triggers a webhook (API Gateway and Lambda, added by Chuck), then a Step Functions workflow that selects a planner, validates the workspace in the AgentCore Harness (repo cloned once, persistent storage for up to 14 days), and generates a plan. A React approval UI gates coding. After approval, coding and verification run in the harness, state is held in DynamoDB across the wait, and a draft PR is opened. The demo produced a PR. Architecture is serverless Step Functions plus Lambda, not Temporal, to avoid always-on cost; Chuck accepted the POC as built in a few days. Source: [2026-10-16 standup](../knowledge/meetings/2026-10-16-team-moissanite-daily-standup.md).

**Token cost controls (2026-10-16).** The first iteration used about 80,000 tokens. Planning is now capped at 16,000 tokens and the loop uses Haiku. Development also hit runaway recursion that burned tokens and a harness eval that attempted disallowed package installs.

**Limitations (2026-10-16).** Only one AI repo is supported for now, because the harness needs prebuilt Docker images (uv configured for Python, and one for numpy). Repo selection is manual: a `factory context` field on the ticket tells the factory which repo to use. Chuck called automatic repo determination one of the biggest unsolved problems in software factories, tied to build-versus-buy talks with Warp. Configuration lives in Terraform, to be pushed as a PR.

**GitHub auth decision (2026-10-16, aligned).** Chuck created the FN software factory GitHub App and will issue client credentials that generate short-lived tokens. The team agreed this replaces the personal token used for the demo. On 2026-10-19, the App requested additional permissions including content access; Chuck approved them, and Mykhailo will retrigger the flow to verify (MOI-169 path).

**Gateway and model routing exploration (2026-10-19).** Mykhailo proposed an "open router" that selects models per task, plus a token-price monitor that favors the cheapest suitable model. Chuck said the team is evaluating AI gateways, including Workato's AI gateway and single-plane MCP gateway, which lets agents request MCP tools on demand instead of loading every MCP server into context. No decision recorded. Bedrock test harness work is waiting on Joe (Chuck to nudge 2026-10-19).

_(recorded 2026-10-19 from standup transcripts; the vendor evaluation and router concept are not decisions.)_

## Fai MVP Scope Update: M2 and M2.1 Done, M3 Canceled (2026-10-20)

- **M2 (MOI-173, real AgentCore Harness execution) and M2.1 (MOI-176, agentify one real repo) moved to Done** on 10/20 at about 7:14am CDT, owned by Mykhailo. His 10/20 Slack status says the Factory now runs end-to-end, from a Linear ticket to a draft PR, after the GitHub App integration, with [fieldnation/ai-software-factory#1](https://github.com/fieldnation/ai-software-factory/pull/1) as the example. The M2 acceptance criterion requires a real, test-verified PR. The sources reviewed do not show test-verification evidence, so that part is unconfirmed.
- **M3 (MOI-174, quality gates and factory completion) was Canceled** on 10/20 at about 7:11am CDT. Its scope was GitHub CI, Gitar, and Sonar gates, PR and merge tracking, and end-to-end cycle time. The Linear ticket has no comment, so the reason is unknown. Whether the Gitar and Sonar gating for Fai PRs moved to another ticket is also unknown.
- **Blockers on harness-eval CI, per Mykhailo (10/20):** LAZ-717 (new connector in the dev account for Harness CI Bedrock access for ai-toolbox) and LAZ-718 (a scoped GitHub App for posting ai-toolbox PR comments). These use a LAZ- Linear prefix, which has not appeared before in this workspace.

_(recorded 2026-10-20 from Linear and Slack; the M3 cancellation rationale is not recorded in the sources.)_

## Business Systems App Platform Phase I: fn-content-auditor Moving to FN Standards (Linear, 2026-10-14)

**New Linear project under the Moissanite team, owned by Vitalii Belinskyi.** "Business Systems App Platform - Phase I" (created 9/29) takes the `fn-content-auditor` app through two milestones, "Code Migrated" and "Meets FN Standards." MOI-133 (repo cleanup: pinned dependencies in `pyproject.toml` and `uv.lock`, non-root container, Streamlit UI and unused features removed) was marked Done 10/14. Four tickets were In Review as of 10/14: MOI-138 (Harness CI pipeline for lint and tests), MOI-140 (FN Enterprise GCP project with mandatory cost labels, meant as the pattern future apps reuse), MOI-141 (build, Wiz scan, push to Artifact Registry, deploy to Cloud Run through Harness CD), and MOI-152 (serve via gunicorn instead of the Flask dev server). The existing FN Harness templates only cover AWS ECR and EKS, so this is the first concrete GCP deploy path on the team. Unknown: how this work relates to the AI-DLC H2 objectives, and how it fits Vitalii's status after he rolled off the Factory on 9/16.

_(recorded 2026-10-15 from Linear; verify scope against the tickets before treating it as settled)_

## Standards Ship With Their Skills: MOI-191 Pilot (2026-10-28)

**Decision (agreed, pilot in progress): each engineering standard ships together with the skill that applies it.** MOI-191 (Chuck, In Progress since 10/28) pilots this with one skill, `fn-ddl-design`: add a `plugins/fn-engineering-standards/` plugin to the fn-engineering-standards repo, point the skill at the existing `standards/sql-ddl-standards.md`, run the MOI-90 skill-invocation evals against both the ai-toolbox copy and the new copy, and confirm the skill triggers on the same prompts. The ai-toolbox copy stays in place during the pilot. `fn-api-design` waits until its standard lives in that repo. Gitar skill wiring, Okta/MDM marketplace changes, and removing the ai-toolbox copy get follow-up tickets once the pilot passes. _(decided 2026-10-28, per Linear MOI-191.)_

Related: MOI-106 (publish FN skill-creator in ai-toolbox) completed 2026-10-28. This is the same repo-committed-standards direction as the MOI-97 finding (2026-08-24) that review-relevant skills must live in each repo.

---

## Warp Factories POC (kicked off 2026-10-29)

- **Kickoff ran 10/29** (Warp calendar invite; Chuck, Cory, and Ben Moormeier invited). Warp's recap in #ext-fieldnation-warp: the first ticket run through the Warp factory was reported as "impressive out of the box." Linear, GitHub, Figma, Datadog, and Sonar are connected; Slack needs a Slack admin to approve the factory bot; pre-warmed sandboxes are enabled. Follow-up working session is Wed 11/04, 3:00 PM CT. Warp shows a per-run cost breakdown in its platform; a rollout budgeting call was proposed for the week of 11/02.
- **Chuck's 10/29 direction:** an automation that watches ai-toolbox for skill changes, pulls them into the factory, resolves conflicts where it can, and escalates the rest to a human. This became MOI-192 (sync, Done 10/30) and MOI-193 (conflict resolution, Canceled 10/30, reason not recorded).
- **Open integration issue:** the factory's PR-review trigger did not fire from GitHub while Gitar was set to approve-or-not. On 10/29 Chuck asked Sonar (Gautam Korlam) whether Gitar can run in comment-only mode.
- **Backlog under "Fai: Factory Q4 2026":** MOI-194 (add the Wiz MCP so the factory can see and remediate Wiz findings) and MOI-195 (agents that act on Gitar, Sonar, and Wiz PR findings). Both assigned to Chuck, both Backlog.
- **Status:** POC progress only. No Warp-vs-AgentCore decision is recorded. Chuck had named Warp the benchmark for the AgentCore path (2026-10-13 to 10-16).
- _(recorded 2026-10-30 from Slack, Linear, and Calendar; the "impressive" comment is Warp's own assessment.)_

## MOI-185 design decision (2026-10-30)

Vitalii's 10/29 Linear comment records the design decided 10/30: OpenTofu manages the app's GCP resources through Harness IaCM, GCP only, following FN's IaCM pattern. Draft PR is fn-content-auditor #31, stacked on #30. Builds on the Business Systems Phase I work above.

## Harness Evals Migration (DeepEval retired) - 2026-10-12

**Decision: ai-toolbox skill evals move from DeepEval to Harness Evals (MOI-145), with DeepEval removal tracked in MOI-151 (In Review as of 2026-10-12).** Mykhailo owns the migration. Milestones so far: the grader and provider layer with offline tests (MOI-147) and the run_quality_evals integration with DeepEval parity check (MOI-148) are Done; the non-blocking Harness CI eval pipeline with runtime and cost tuning (MOI-149) was Done 2026-10-09; the `ai_toolbox_eval` pipeline wired into Harness with its first real recorded run (MOI-164) was Done 2026-10-12. Still open: docs, skill content and eval assertions update (MOI-150, Todo) and posting skill eval results as a PR comment (MOI-165, In Progress). Pipeline reads AWS Bedrock credentials via OIDC; the Bedrock IAM role blocker noted on 2026-10-06 and 10-08 is no longer listed as blocking on the Linear tickets above, but its status is not independently confirmed here. _(decided 2026-10-12, per Linear.)_

**Update 2026-10-29: cut-over closed in Linear.** MOI-145 (migration epic) and MOI-151 (cut over to Harness Evals and remove DeepEval) were marked Done 2026-10-29 at 10:42 UTC. MOI-150 (docs, skill content, and eval assertions) was Canceled the same minute, so the docs and the fn-skill-creator eval assertions that name DeepEval may still need a separate pass. MOI-165 (post skill eval results as a PR comment) and MOI-182/MOI-183 (AgentCore tracing validation; ai_toolbox_connector for AWS auth in the eval pipeline) were also marked Done 2026-10-29. Linear closure is not independently verified against the repo: confirm with a grep for `deepeval` before treating it as removed.

## ADR (Architecture Decision Record) Skill Architecture — 2026-09-16

**Decision: maintain three separate ADR skills (create, enforce, review) rather than consolidating.** Asad and Mykhailo debated consolidation (45 min standup discussion 2026-09-16) given concerns about skill duplication. Rationale for keeping 3:

- **Distinct firing moments**: create fires when a decision is being drafted, enforce fires when a change is described but not yet written, review fires when a record review threshold (e.g., 90 days) passes.
- **AI toolbox architecture constraint**: current toolbox does not allow a skill to call another skill, making consolidation technically difficult and less elegant than independent implementations.
- **Single responsibility**: each skill has a clear, focused purpose; trying to combine them would muddy that separation.

**Next step (2026-09-16 standup)**: Mykhailo's ADR architecture diagram to be reviewed by Chuck and then pushed to the ai-toolbox for code review and rollout.

---

## Repo Readiness Scorecard Progress — 2026-09-16

**Status: at 97/100, target 100 incoming.** Mykhailo reported scorecard now accounts for main branch commit history in the readiness calculation, bringing the score from 97 to 100. This is the core metric for the O1 repository readiness KR, and MOI-110 (bringing ai-repo-score to ~100 repo-readiness) is now In Review as of 2026-09-16.

**Tool packaging strategy (new priority, 2026-09-16 standup):** After Vitalii's repo bootstrap PR is reviewed and merged, the focus shifts to **packaging both repo bootstrap and repo scorecard as downloadable CLI tools**. Chuck wants to eliminate the need to clone the repository — instead, users run a simple CLI command to fetch the latest release and execute it. Potential distribution channels: GitHub action releases, or private Homebrew tap. Tickets will be created for: (1) packaging repo bootstrap and scorecard for easy install; (2) incorporating Chuck's most recent prompt into the repo bootstrap tool (as a follow-up after packaging is complete). **This is a significant shift in distribution strategy** — moving from "clone repo, run scripts" to "download CLI, run it."

**Setup documentation (new workstream, 2026-09-16 standup):** Chuck proposed creating a Confluence page documenting all required developer tools for the agentic experience (OpenSpec, Sonar MCP, AI toolbox, Claude Code, Copilot, etc.), why each is needed, and how they work together. This will be the reference distributed to engineers so they know what to install and configure. Ticket will be created; work likely starts after repo bootstrap lands and is documented.

---

## Vitalii Transition — 2026-09-16

**Vitalii Belinskyi transitioning off Moissanite project effective 2026-09-16.** Rolling off after completing repo bootstrap and Harness 911 ticket work (expected end of week ~2026-09-20). Mykhailo requested a handover call to review contributions. Vitalii currently blocked on Harness admin access to configure toolbox create-trigger for pipelines (open blocker: Harness admin needs to wire webhook + connector + PR/push triggers in the Harness UI, not in git).

---

## OpenSpec Rollout Status — 2026-09-16

**Alert: OpenSpec rollout PRs (#65, #77, #78) closed unmerged this week.** Three related PRs (exact scope TBD — exact content not examined) all went unmerged in the 2026-09-15/16 window. Context: the AI Guild confirmed OpenSpec as the spec-driven-development standard (2026-07-22), and Asad's MOI-49 framework evaluation (2026-07-15) recommended Option B (OpenSpec as sole spec system). This apparent stall contradicts the stated direction and suggests either a technical blocker, a scope/priority shift, or resource constraint. **Action: investigate why these PRs closed and whether OpenSpec rollout remains the intended path or has been deprioritized.** The AI Guild agenda for 2026-09-17 mentions "Status Check: OpenSpec rollout into AI Toolbox" — this may surface context.

---

## Field Week AI Hackathon: 8+ New Skills Landed — 2026-09-16

**Eight or more new skills from the 2026-09-13 Field Week Technology Team Hackathon are pending intake and triage.** Listed skills: `fn-rabbitmq-audit`, `fn-feature-flags`, `fn-repo-readiness`, `fn-standards-compliance`, `fn-qa-testing`, `fn-repo-score-uplift`, `fn-nest-migration/agent-readiness`. This is a meaningful volume of new ai-toolbox candidates that likely need prioritization, ownership assignment, and integration-readiness vetting before wider adoption. The AI Guild agenda for 2026-09-17 mentions "AI Hackathon output triage" as a New Business item — this is where the decision on what to carry forward will likely surface.

- **Repo:** https://github.com/microsoft/apm
- **What it is:** An open-source dependency manager for AI agent configuration — think `npm`/`pip` but for skills, instructions, prompts, agents, hooks, plugins, and MCP servers. You declare dependencies in an `apm.yml` manifest, run `apm install`, and every developer gets a fully configured agent setup.
- **Key capabilities:** Transitive dependency resolution, content security scanning (`apm audit`), works with Claude Code / Copilot / Cursor / Codex, supports authoring and distributing plugins, CI/CD ready via GitHub Action.
- **Why it matters for us:** Gives us a standard packaging and distribution layer for everything the AI Software Factory produces — skills, context files, spec templates, review configs. Same packages work in the factory harness AND on every engineer's local machine. Ian has already been working with APM, so adopting it as our standard makes his work foundational rather than a parallel effort.
- **Status:** v0.8.11 as of April 2026, 1.2k GitHub stars, 889 commits. Active development.
- **Also related:** [agentrc](https://github.com/microsoft/agentrc) — analyzes codebases and generates tailored agent instructions from real code. Outputs `.instructions.md` files compatible with APM packaging.

### Tessl — new evaluation candidate, surfaced 2026-08-06/07

**Status: unevaluated, explicitly not being adopted yet.** Tessl (tessl.io) was shared at the 2026-08-06 AI Guild meeting; Chuck flagged it in #moissanite-eng the same morning as "incredibly interesting" and said it "solves a number of problems that we are trying to solve," including parts of what Vitalii is actively building. Vitalii asked directly (2026-08-07) whether it's an evaluation or a commitment, and whether — if adopted — it replaces the SDD/ai-toolbox-skills approach (MOI-60, MOI-47) or feeds into it, since some of his in-flight tickets might overlap. **Chuck's direction (2026-08-07):** he has a Tessl meeting Monday 2026-08-10; until then, proceed as if it won't be introduced and keep working the current path — if the team does decide to adopt it, the call will be made quickly and the team will pivot.

**Update 2026-08-10: first substantive product comparison, ahead of today's Tessl intro call.** Mykhailo authored a capability-comparison table ([Confluence, "The repo-score/ai-software-factory comparison vs tessl agent"](https://fieldnation.atlassian.net/wiki/spaces/~712020b505f5c3576344a78bc4d1f7dc47b0da/pages/6149701634), dated 2026-08-07) scoring ~30 capabilities across Tessl, the current `repo-score` toolchain, and the planned AI Software Factory. Headline finding: **Tessl's core strengths cluster entirely in the skills-registry/governance layer** — skill versioning, skill quality review, behavioral skill evals, skill security scanning, required-org-skills governance, activation analytics, and mining agent sessions/PR feedback for skill creation — none of which the repo-score toolchain or factory plan currently do. Conversely, **repo inventory, CI health, and readiness scoring are already strong internally** (`repo-passport`, `repo-health`, the readiness score) and Mykhailo's recommendation is to keep those proprietary rather than replace them. His explicit recommendation: **"pilot Tessl before building"** the skills-registry piece, and add skill versioning "immediately" regardless of the Tessl decision. Note: the doc references a **`repo-probe`** tool ("provides executable evidence," feeds Coder/Verifier sandboxed execution) that hasn't appeared elsewhere in this workspace — likely Mykhailo's working name for the sandbox-execution piece of the toolchain (see `repo-readiness --sandbox`) rather than a new tool, but not yet reconciled with the MOI-82–87 naming.

**Tessl meeting confirmed for today, 11:30am CT** (originally 11:00am, Adrian from Tessl moved it 2026-08-07 for a scheduling conflict). Attendees: Chuck, Joe (accepted), Mykhailo (accepted), Cory (tentative), plus Tessl's Adrian and Shawn. Outcome not yet known as of this sync (meeting is same-day). Chuck's Gmail thread with Tessl (amyw@tessl.io, adrian@tessl.io) confirms he requested the demo after "research suggested a potential fit," and that he was on-site in Minneapolis 2026-08-07 for Field Nation's "bi-yearly on-site," which is why the meeting landed on 8/10 rather than sooner.

**Update 2026-08-11: meeting outcome — interesting, not changing course.** Chuck's 8/11 standup note: "Mykhailo, Joe and I met with Tessl yesterday, will continue to discuss with them as they have a lot of interesting offerings that would be useful for us. However in the meantime we will continue to move forward with our original plan." Tessl stays an open, ongoing conversation rather than triggering a pivot — consistent with Chuck's 8/7 stated position.

**Update 2026-08-16: follow-up article shared in the same #eng-news thread.** Chuck posted [Stop Burning Tokens on Code Review](https://swizec.com/blog/stop-burning-tokens-on-code-review) as a "thought provoking read relevant to this conversation," continuing the 8/13-14 Mike Mueller/Cosmos thread on agentic code review. No further update yet on the charter Chuck agreed to produce for Mike (asked 8/14, ~1 week timeline) — not found in any reachable source this sync.

**Update 2026-08-14: Chuck's bespoke code-review agent enters the picture alongside Gitar/Tessl.** During the 2026-08-13 Field Week Technology Team Hackathon, Chuck personally prototyped an agentic code-review tool and is continuing the work post-event in a new repo, `fieldnation/fn-review-agent`. On 8/14 in #eng-news he cited an Augment Code blog post, ["Solving Code Review with Cosmos"](https://www.augmentcode.com/blog/solving-code-review-with-cosmos), as closest to his own vision for agentic code review, and noted its interactive pair-review pattern parallels what Andrew Kandels's team has already built. This is the first concrete action behind the "bespoke coding agent" option the [H2 OKR doc](../program/OKRs/h2-2026-okrs.md) already listed as a real O1 code-review candidate — Chuck is now actively building toward it in parallel with the Gitar trial and the Tessl evaluation, not just holding it as a fallback. See [program/overview.md](../program/overview.md#field-week-hackathon-chuck-prototypes-a-bespoke-code-review-agent-ai-dlc-charter-requested-2026-081314).

**Update 2026-08-13: Chuck's first explicit read on Tessl pricing, shared with Cory.** Forwarding Tessl's written recap email to Cory the same day, Chuck added: "Their pricing doesn't seem to bad for what we get from them." First signal that Chuck is leaning favorably on cost specifically, ahead of the dedicated "Tessl Code Review overview" call on 2026-08-17.

**Update 2026-08-13: written recap lands, and code review specifically enters scope.** Adrian Flückiger (Tessl GTM) emailed Chuck, Joe, and Mykhailo a recap of the 8/10 call: covered Tessl Review/Eval Run (structural + functional skill validation), Verifiers (defining/confirming expected agent behavior), the Tessl Registry + workspace models (cross-team distribution) and Inventory (org-wide GitHub scanning for skill quality/security), and blocking governance policies (e.g. Snyk integration to stop vulnerable plugins). **New: Tessl's agentic code review (Beta) was discussed** — Adrian confirmed Chuck's feedback (agents should highlight what's *correct*, not just flag issues — the same "how it reviewed" bar Chuck has held Gitar to) was passed to Tessl's product team. This puts Tessl into the same O1 code-review evaluation space as Gitar (see the Gitar section below), not just the skills-registry space Mykhailo's 8/7 comparison scoped it to. **Next step: a dedicated "Tessl Code Review overview" call, Monday 2026-08-17, 8:30-9:15am CT** — same day as the existing 10:00am Gitar pricing discussion with Cory, making 8/17 a full code-review-vendor day. Enterprise pricing (PDF) and the call recording were also sent for internal review; not yet reviewed as of this sync.

**Update 2026-08-17: today's "Field Nation - Tessl Code Review and Agent discussion" call is confirmed on calendar, 8:30-9:15am CDT**, with a fuller Tessl-side bench than previously logged — Adrian (organizer), plus Sandra, Tom G., and Colin H. from Tessl; Mykhailo, Joe, and Chuck from FN. This is the dedicated code-review-focused follow-up flagged 8/13. Same day as both Gitar meetings and a Glean placeholder (see the Gitar section above and Glean section below) — 8/17 is effectively a four-vendor code-review/knowledge-tooling day. The call happened per calendar (ended 9:15am); no transcript/notes surfaced in Slack or Confluence as of the 8/18 sync.

**Update 2026-08-17/18: Mykhailo ran a rigorous, independent Tessl skills investigation (Confluence), separate from the vendor call.** Report: ["Tessl Skills & Evaluation: Investigation Report on Agent Skills Impact and Governance Challenges"](https://fieldnation.atlassian.net/wiki/spaces/~712020b505f5c3576344a78bc4d1f7dc47b0da/pages/6174801949). Methodology: a controlled manual A/B (same repo, same prompt, same coding agent, one Git branch with a Tessl-delivered skill and one without) plus Tessl's hosted scenario-generation/quality-check/eval pipeline. **Headline finding: the skill measurably changed agent behavior** — without it, Claude picked a reasonable-but-self-chosen HTTP retry policy (4 attempts, retried `500`, no `Retry-After`); with the skill, it followed the encoded org policy exactly (3 attempts, excluded `500`, added `Retry-After` handling). A negative-routing test confirmed the skill didn't leak into an unrelated rename-only task. **Caveats that matter for the O1 code-review/governance decision:** the hosted formal eval never completed — the Free-plan solver (`deepseek-v4-flash`) failed at the execution layer before meaningful model inference and the run sat "In Progress" for ~49 minutes; Tessl's own quality gate rejected 2 of 3 generated scenarios for rubric leakage; scenario generation alone cost 300 of 1,000 free credits for 3 scenarios; model selection for diagnosis is paid-plan-only. Mykhailo's bottom line: **Tessl's differentiated value is the lifecycle/governance layer (package → distribute → review → evaluate → version → govern), not skill authoring itself** — skills are most valuable for encoding *organizational policy* (reliability rules, PR/review policy, migration conventions) rather than general engineering knowledge the model already knows. Recommends a small pilot on 2-3 real org policies with hand-curated evals rather than a large generated-scenario catalog, and flags hosted-eval reliability as the open blocker before treating Tessl evals as a CI/governance gate. This is a substantive, independent data point for both the Tier 2 skill-governance question and the O1 code-review tool decision (Tessl's code review is Beta and shares the same underlying eval infrastructure this report stress-tested).

**Also worth noting: a "Field Nation | DX - AI DLC Sync" meeting is on calendar for today** with Atlassian/getDX contacts **Taylor Brown** (tbrown3@atlassian.com / taylor.brown@getdx.com) and **Cheng Tang** (cheng.tang@getdx.com) — timing suggests this may be the venue for answering Shane Kinder's unanswered 8/6 question about whether the KR1 repo scorecard is ready to review this week (see [program/OKRs/h2-2026-okrs.md](../program/OKRs/h2-2026-okrs.md)). Outcome not yet known as of this sync.

### AI Coding-Assistant Execution Telemetry (OTel): new, 2026-07-21 → 08-03

The instrumentation layer under O1 KR1 (the repo scorecard). Two tickets, two assistants, one architecture: **MOI-80** (Claude Code, Vitalii) and **MOI-81** (Copilot, Asad). Both verified end-to-end against real sessions, not docs.

**Two-layer model, split by destination:**

| Layer | What it is | Destination | Status |
|---|---|---|---|
| **Layer 1** | Baseline adoption/usage metrics: session count, active time, lines of code, token usage, cost | **getDX** (keep) | Already flowing. Copilot side covered free by DX's GitHub Copilot adoption report. |
| **Layer 2** | Per-invocation diagnostic events: `tool_result`, `tool_decision`, `api_request`, `skill_activated` | **Datadog** | Proven end-to-end; queryable as of 2026-08-03. |

Layer 2 wins on Datadog for event-grain + skill/session/repo faceting, least plumbing, and negligible cost. getDX and Prometheus were ruled out on capability (getDX can only store aggregates); SigNoz/OpenObserve on "not worth standing up." **Strong alternative left on the table:** Datadog for *both* layers, if the org prefers one pane and full-fidelity layer 1 over getDX's ready-made scorecard dashboards, a call for Chuck / the scorecard owners.

**Datadog index (2026-08-03).** Joe created a **single** log index, **`ai-harness-otel`**, covering both `service:claude-code` and `service:copilot-chat`; Vitalii confirmed the same morning that events are landing and searchable. Joe reversed his 7/31 plan to create two separate indexes. Reasoning: these logs only retain 7 days, so it's cheap to split later. (Asad had argued for separate indexes per Datadog's own best-practice guidance around independent quotas/retention/billing segmentation. Correct but overridden; worth revisiting only if usage tracking gets messy.) The earlier blocker (events reached Datadog and were visible in Live Tail but weren't indexed, so invisible in Log Explorer, dashboards and monitors) is now cleared.

**Content firewall (verified twice, independently).** With `OTEL_LOG_TOOL_DETAILS` and the other content flags **unset** (the default), prompts come through as literal `<REDACTED>`, `tool_input` is absent, and error *message* is null, while completion state, `error_type`, duration and token counts all still flow. Turning that flag on is the only thing that adds command/path/message text. **Keep all `OTEL_LOG_*` content flags unset.**

**Tagging:** every record carries `repo.name`, `team.name`, `session.id`, `user.id` (hashed), plus `model` and `terminal.type`; `skill.name`/`agent.name` attach where relevant. repo/team are set via `OTEL_RESOURCE_ATTRIBUTES`.

**Gotchas worth remembering:**
- **Per-skill attribution:** `skill_activated` labels *every* custom/project skill as `skill.name=custom_skill`; only `invocation_trigger` varies. Real skill names appear on **`api_request.skill.name`**; `tool_result`/`tool_decision` carry no skill name. To trace which skill fires or breaks, either use `api_request.skill.name` or add a small `PostToolUse` hook that stamps `skill.name` onto tool events. This is the one place custom instrumentation is genuinely justified.
- **"Accepted-then-edited" is invisible**: native telemetry records accept/reject only.
- **Aborted vs. interrupted:** interrupting a running tool (ESC) shows "Interrupted" but emits no distinct completion event; `source=user_abort` only comes from aborting the turn. "Aborted" and "timed out" are edge states that couldn't be captured cleanly.
- **Trigger-miss is undetectable**: a skill that *should* have fired but didn't leaves no invocation to trace. Handle via spot-checks.
- Reject/discard and abort states only emit in *interactive* sessions.

**One real shipping dependency:** the exporter is centrally pinned to getDX via a managed `remote-settings.json`, so any routing change (per-signal endpoints, or a shared OTel Collector fan-out) must go through that config's owner.

**Cost, measured:** FN already ingests ~142M log events/day (~256 GB/day) into Datadog. Layer 2 at full org rollout adds **<0.15% of events and <0.1% of bytes**. Negligible.

**Still open on the Copilot side (MOI-81):** a few layer-2 signals (real error, real hook outcome, the edit-acceptance metric) confirmed synthetically but not yet on live data; the `.vscode/settings.json` rollout mechanism drafted but unvalidated on a clean checkout (the one genuinely open decision, not just thin evidence); JetBrains/standalone-CLI coverage untested; DX's AI Code Insights not yet checked for layer-2 overlap.

**MOI-81 final-report findings (Asad, 2026-08-05):** Q1 — DX connector is already enabled and covers layer 1 with real FN data (86.81% WAU, acceptance-rate split, LOC by feature, per-team breakdown); no custom API pull needed, confirming layer 1 as a pure buy. Caveat: the pilot engineer's own usage is invisible to it (Copilot Free seat; enterprise metrics only cover enterprise seats). Q2 — **architectural limitation, not fixable by config: OTel does not work outside VS Code.** The Copilot Chat extension (VS Code only) has an OTLP exporter; `copilot-language-server`, which every other surface (JetBrains, other IDEs) runs on, has none and ships telemetry to Azure Application Insights instead. Workaround for non-VS Code editors: Copilot CLI in the IDE terminal. Q3 — the ghost-text-acceptance gap is out of scope either way (DX already reports completion acceptance separately), but the ticket's premise may be wrong: `copilot_chat.inline.done` does exist and is untested — a 5-minute check remains open. Q4 — the skill-trigger-miss gap stands (no span, nothing to query, not solvable by instrumentation), **and a second, worse limitation compounds it on Copilot specifically: `skill_name` is confirmed absent from both logs and metrics**, even when a skill does fire (Claude Code at least exposes the name via `api_request.skill.name`).

**MOI-80 status update (Vitalii, 2026-08-06): functionally verified end-to-end, production wiring is what's left.** Everything the ticket needs is confirmed flowing into Datadog (both layers, tagged by repo/team/skill/session, no prompt/code content). Remaining punch list before this is production-grade: (1) a shared OTel **collector** to fan out metrics → getDX + Datadog and logs → Datadog (platform/SRE-owned); (2) update the managed `remote-settings.json` to route OTLP to that collector org-wide, pushed automatically to every dev/repo; (3) a per-repo `repo.name`/`team.name` tag (small, or git-derived) — the Datadog index itself is already done. Hooks remain optional: ~90% of what's needed is already native; a small hook, shipped as a plugin, could later add the "edited" override signal and cleaner per-skill attribution, but isn't required to ship.

### Repo Scorecard (KR1): built and going into documentation, 2026-08-12

**MOI-55 ("Stand up the repo scorecard") moved to Done 2026-08-12 (Chuck).** The scorecard now pulls three live metrics from DX — TrueThroughput, PR cycle time, PR revert rate — for the initial embed-pilot repo, using the metric definitions/targets reconciled in MOI-53/54 (both Done 8/11). Refreshes on a defined cadence (daily/weekly) rather than being a one-off snapshot, and is extensible to additional repos as KR3 adds them. This is the concrete O1 KR1 deliverable behind the "DX dashboards" Chuck flagged as an open action item after the 8/10 DX/getDX sync.

**MOI-56 ("Publish and document the scorecard") moved to In Progress 2026-08-12 (Chuck).** Follow-on to make the scorecard team-usable: document where it lives and how to read it (what each metric means, target values), and confirm it's ready for KR3's baseline-capture use case (snapshotting a repo's before-state ahead of an embed pass).

### Repo-Readiness Toolchain: built 2026-07-21 → 07-29 (Mykhailo, MOI-82–87)

The AI Repo Readiness Score went from checklist to working code during Chuck's vacation. Four CLI tools, all writing to **`.ai-factory/`** on a shared YAML contract:

- **`repo-scanner`**: analyzes repos across the GitHub org and ranks them by recent development activity (primary language, recent commits, default-branch activity, repo age, last update). This is the mechanical version of Mykhailo's earlier 90-day repo-activity spreadsheet, and the input to top-10 repo sequencing.
- **`repo-passport`**, deterministic static scan of a locally available repo producing its technical passport: languages/ecosystem, runtime + package manager, build/test/lint/type-check commands, structure, CI config files, Docker support, documentation, detected tooling and frameworks. First version supports the JavaScript family and Go.
- **`repo-health`**, remote GitHub/CI health: default-branch status, latest commit checks, required and optional status checks, branch protection, recent workflow runs, failing/pending/skipped/missing checks, activity and maintenance signals.
- **`repo-readiness`**: combines passport + health into an overall readiness score, category scores, detected blockers, warnings, missing capabilities, recommended remediation, and supporting evidence for every score.

**Sandbox execution (MOI-87, Done).** `repo-readiness` gained a `--sandbox` flag with `host` and `docker` modes so a repo's own test cases actually run in an isolated environment; Go working as of 7/24, Node.js/TypeScript next. Docker Sandboxes ([docs.docker.com/ai/sandboxes](https://docs.docker.com/ai/sandboxes/)) raised as a candidate 7/22. **MOI-86** added SQL-based repo support, directly relevant to `fieldnation/engineering-dbt` as Repo 1.

**MOI-57 (Define v1 of the AI Repo Readiness Score) moved to In Review 2026-07-30.** Its four scored dimensions remain: test command present and runnable, Dockerfile/containerized local setup, AGENTS.md or CLAUDE.md present, local setup documented; mostly pass/fail, with test and doc coverage scoreable as a percentage.

**Toolchain given its own repo: `fieldnation/ai-repo-score` (created 2026-08-03).** Mykhailo has admin access, rest of team has write access. First PR (CI/linter fixes + doc updates) merged 2026-08-04.

**`repo-doctor` now a tracked ticket: MOI-92 (created 2026-08-12, Todo, assigned Mykhailo).** Formalizes the tool Mykhailo has been building since 2026-08-07 as a companion to `repo-readiness` — instead of just scoring a repo, it gives detailed, actionable remediation instructions for improving that score. He also obtained Redshift credentials to continue the `engineering-dbt` (Repo 1) investigation.

**MOI-53 and MOI-54 both moved to Done 2026-08-11** (Chuck): "Reconcile DX metric definitions against FN's usage" and "Define scorecard success metrics and target impact magnitude" — closes out two open O1 KR1 items from the 7/16 ticket wave.

**New adjacent tickets (2026-08-04), both Chuck-owned, on skill-authoring tooling rather than repo scoring itself:** **MOI-88** (In Progress) — add a utility to optimize/tighten skill descriptions in ai-toolbox (lost during the fork of the Claude skill builder), then re-run it against existing skills. **MOI-89** (Todo) — incorporate [Matt Pocock's "writing great skills" skill](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-great-skills/SKILL.md) into FN's own skill builder.

### Gitar: AI code review candidate (Sonar), live as of 2026-07-27

**Status: leading candidate for the open O1 code-review decision, with a substantive open disagreement.** Gitar is Sonar's AI code-review product (Ali Adl-Tabatabai, EVP and Gitar founder, joined #fieldnation-sonar 2026-07-28). Previously referenced in this doc as "Guitar" under Linear MOI-45.

**Correction (2026-08-06, per Chuck): Gitar is NOT included in the executed Sonar contract — it is a separate contract.** Earlier notes in this workspace implied Gitar came bundled with the Sonar/Agent Essentials package; that is wrong. Sonar Vortex and SAST *are* included (see the Sonar section below); Gitar is not.

**Chuck's evaluation criterion, stated 2026-07-17 (this is the thing to hold vendors to):** most code-review tools only comment on what's *wrong* with a PR. They don't expose **how the agent reviewed it**: its overall process, what it examined, what it judged fine. Without that, the human reviewer can't tell what the AI actually covered, so they still have to review everything themselves; the tool adds cognitive load instead of removing it, and human review time isn't reduced. Chuck considers this the missing piece in every out-of-the-box reviewer he's seen.

**Gitar's position (Jack Kolis, 2026-07-27):** the Gitar team considered exactly this and **deliberately decided against it** to keep reviews low-noise. Reference: [AI code review without the comment spam](https://gitar.ai/blog/ai-code-review-without-the-comment-spam).

**Update 2026-08-20: Gitar is now live on real PRs in `core-ui`, and the "how it reviewed" gap is now directly observable, not just theoretical.** Following the 8/19 trial kickoff (GitHub connection was the concrete next step out of the pre-trial sync), `gitar-bot` is actively commenting on production PRs — confirmed on `fieldnation/core-ui#489` and `#490` today. Its review format matches Chuck's stated concern exactly: a collapsed "Code Review ✅ Approved" block with one or two sentences describing *what the diff does*, and "No issues found" — no listing of what was examined, what was judged fine, or why. Both PRs also had **Claude Code review** and **GitHub Copilot review** run on the same diffs, giving a natural side-by-side comparison: Claude's review on #489 was a fully itemized LGTM (correctness reasoning per file, explicit "nothing else stood out — no security concerns, no missed edge cases" with justification), and Copilot caught two genuine issues via line comments (an incorrect Split-flag substitution in docs on #489; a `useMemo`/render-purity violation and a stale-timestamp bug on #490) — both are real, that Gitar's terse "No issues found" didn't surface. **New Gitar capability observed, not previously documented:** it also auto-diagnoses CI failures (e.g. categorized a Prettier formatting failure and a "Storage Blob Not Found" infra error with root cause + suggested fix) and offers a `gitar auto-apply:on` mode to auto-commit fixes to the branch — a capability beyond code review itself, worth factoring into the O1 tool comparison. SonarQube quality gates also ran clean on both PRs, confirming the Sonar MCP/CI integration (MOI-79) is active in at least `core-ui`. This is the first concrete, in-production evidence for the ongoing Gitar/Chuck disagreement, not just vendor claims — worth referencing directly next time the "how it reviewed" conversation comes up with Sonar/Gitar.

**Update 2026-08-19/20: MOI-80 (Claude Code OTel) formally marked Done.** Linear shows MOI-80 moved to Done status 2026-08-19, closing out the Claude Code side of the telemetry instrumentation work described above (Vitalii). MOI-97 (Copilot code review + agent skills) remains In Progress with Asad Ali.

**Update 2026-08-21: MOI-97 moves from ticket to concrete test evidence in `core-ui`, and a mirrored ticket opens for Gitar.** Asad Ali stood up `.github/skills/code-review` in `core-ui` ([PR #492](https://github.com/fieldnation/core-ui/pull/492)) and opened four throwaway probe PRs (#493-#497, explicitly marked "DO NOT REVIEW / DO NOT MERGE") isolating the open question from the ticket: does a skill delivered via the MOI-47 marketplace bundle actually surface during Copilot's *automated* PR review, or only when a developer invokes the interactive agent directly. Probes cover a vendored-skill positive control, a marketplace-only negative control, and two repo-local attribution variants. **Notable side effect: Chuck's `fn-review-agent` bespoke prototype is now auto-commenting on PRs as a bot** ("Tracer run complete," with a review ID) — the first evidence it's running as an operational bot beyond the 8/13 hackathon prototype stage, even though these are just test probes. **New ticket MOI-98 ("Gitar code review + repository rules"), created and started by Chuck 2026-08-20**, mirrors MOI-97's structure for Gitar: Gitar supports "Repository Rules" (`.gitar/rules/*.md`, YAML frontmatter, natural-language `when` triggers, up to 5 custom rules on Pro tier) that fire on PR events and can post comments, apply labels, or suggest changes. The ticket's premise is directly the 8/20 finding that Gitar's default "Approved, no issues found" missed real issues Copilot caught — testing whether narrowly-scoped custom rules (e.g. targeting security-sensitive paths) close that gap. Plan: confirm Gitar's plan tier/rule ceiling, add 1-2 rules to `.gitar/rules/` in `core-ui`, verify via `gitar display:verbose`. As of 2026-08-25, MOI-98 is still In Progress with no findings posted yet. Both tickets explicitly feed the same O1 code-review decision alongside Tessl and `fn-review-agent`.

**Update 2026-08-24: MOI-97 answered definitively — marketplace-distributed skills do NOT reach Copilot's automated PR review; repo-committed skills do.** Asad's controlled A/B (one probe diff, byte-identical across branches, varying only where the review-time skill lives) in `core-ui`: PR #494 (no skill in repo) caught 0 of 11 planted `fn-api-design` violations with generic OpenAPI/RTK comments; PR #497 (repo-committed `.github/skills/code-review`) caught 6 of 6 planted violations with correct rule IDs and supporting doc citations, ruling out the model reading rule IDs off the diff itself. PR #496 tested review-time skill installation via `copilot-setup-steps.yml` — the workflow ran green and the skill materialized on disk, but the review still returned only the generic findings; root cause not established (candidates: workflow must be on default branch, or skills resolve from repo content before setup steps run at all, making review-time install structurally impossible regardless of branch). **Primary evidence for why marketplace distribution can't reach code review:** the enterprise managed-settings support matrix (`enabledPlugins`/`extraKnownMarketplaces`/`strictKnownMarketplaces`) lists five client surfaces — Copilot CLI, VS Code, Copilot app, cloud agent, JetBrains — and code review isn't one of them; review runs service-side with no user client, so it reads customization from the repository instead. **Separately, Asad found `core-ui` already has 2 of the 3 relevant Copilot customization files live on `main`** (`AGENTS.md` and a path-scoped `.github/instructions/MUI.instructions.md`); only the repo-wide `.github/copilot-instructions.md` is missing. **Recommendation (Asad, adopted): commit review skills per repo under `.github/skills/`** — proven, zero new infrastructure, and arguably correct anyway since review conventions are substantially repo-specific. MOI-97 moved Todo → In Progress (8/19) → In Review (8/24); findings written up in `MOI-97-findings.md` (Linear attachment) plus an `evidence.zip`. This is the first fully-resolved, evidence-backed answer among the six-plus O1 code-review angles (Gitar, Tessl, `fn-review-agent`, native Copilot review, roborev, Clanker) — it doesn't pick the winning tool, but it does settle that **whichever tool wins, org-standard skills need to be committed per-repo, not distributed solely via the MOI-47 marketplace, if code review is the use case.** This has a direct implication for the MOI-47 marketplace-vs-vendored debate more broadly: marketplace distribution's blind spot isn't just Copilot's cloud surfaces (already known, 8/5) but Copilot's *review* product specifically.

**Update 2026-08-20: Sonar login/SSO friction continues.** A new SonarSource support ticket (#73101, opened 2026-08-19 evening) reports a team member unable to log into the US Sonar instance via GitHub — SonarSource's support confirmed sonarqube.us access is involved. Same morning, Chuck put a "Sonar SSO Setup" meeting on calendar with **Paul Wochnick** (FN's MDM owner, not previously involved in Sonar) — plausibly pulling Paul in to help troubleshoot the login issue, though the connection isn't confirmed. This follows Daniel Goldman's (ITMethods) 8/18 Launchpad SSO session, whose stated goal was "getting Chuck logged in so he can enable his team and set up SSO" — suggesting SSO still isn't fully sorted out two days later.

**Update 2026-08-21/24: a second, more substantive SSO ticket (#73160) opened — Okta SCIM group-mapping bug plus a licensing region mismatch.** Distinct from #73101 above (GitHub login), this ticket describes users being authenticated via Okta SSO but not attached to the SonarQube Cloud organization, then disappearing from the member list. Troubleshooting with SonarSource's Joe (Tingsanchali) surfaced two findings: (1) Chuck confirmed all Okta groups are SCIM-mapped to the org, so mapping isn't the root cause; (2) **the original license key was provisioned for the EU region (sonarcloud.io)** while FN's users sign in via the US SSO endpoint (`sonarqube.us/sso-initiate?enterprise_key=field-nation`) — Chuck supplied a new US-region license key (`16832a94-...`) 8/21. As of 8/24, Sonar's Joe is still investigating the Okta **OIDC** integration specifically and has proposed switching to **Okta SAML** instead as a faster path; a troubleshooting call (originally 10am CDT Mon 8/24, moved 30 minutes later same day by Chuck) is the next step. Unresolved as of this sync — worth checking whether this blocks the broader Sonar rollout (Launchpad, MOI-78/79) beyond just login.

**Chuck's response (2026-07-28):** unmoved. *"I am of a firm belief that without the summary of findings / how the agent code reviewed you are left to assume."* Will read the article and continue the conversation. **A Gitar technical demo and a call with Ali are both pending Chuck's return from vacation.** Treat this as a philosophy disagreement with the leading vendor, not a feature gap; it may end up favoring the bespoke-agent option already flagged as a real candidate in the [H2 OKRs](../program/OKRs/h2-2026-okrs.md).

**Update 2026-08-03:** Ali proposed concrete call windows for the demo/discussion (Wed 2:00-2:30pm & 3:30-5:00pm, Thu 11:00am-2:00pm & 4:00-4:30pm, Fri 8:30-9:30am & 12:00-1:00pm, all CST). Awaiting Chuck's pick as of this sync.

**Update 2026-08-07: call scheduled for today.** "Sonar & Field Nation: Gitar Conversation," 12:00-12:45pm CDT, with Ali Adl-Tabatabai and Jack Kolis, Chuck (accepted), Joe (tentative), Redwan (declined). Outcome not yet known as of this sync — the underlying disagreement over whether Gitar publishes *how* it reviewed a PR is still unresolved going in.

**Update 2026-08-10: the 8/7 call happened, and a concrete trial plan is agreed — real movement despite the unresolved philosophy disagreement.** Jack Kolis emailed Cory: Chuck and Ali discussed Gitar in detail and Ali demoed the solution. Chuck named a priority to have a code-review solution running before **end of September**. Agreed plan: **Aug 17** pre-trial sync, **Aug 19** trial kickoff call (setup + success criteria), **Aug 19 – Sep 2** run the Gitar trial, **after Sep 2** finalize the Gitar partnership. Next step: a pricing conversation with Cory. **The "how it reviewed" disagreement was not resolved** — Chuck is proceeding to trial anyway, likely under September-deadline pressure. Worth watching whether that gap resurfaces as a blocker once the trial is running.

**Update 2026-08-11: the Aug 17 pre-trial sync is now a concrete calendar invite — "Sonar & Field Nation: Gitar Pricing Discussion," Mon Aug 17, 10:00-10:30am CDT**, sent by Jack Kolis to Cory (cc Chuck). Confirms the "pricing conversation with Cory" flagged as the next step on 8/10 is scheduled, not just planned.

**Update 2026-08-11: new Gitar-side contact, Gautam Korlam**, added to #fieldnation-sonar by Jack Kolis — part of the Gitar founding team, brought in to assist with the trial. See [program/people.md](../program/people.md).

**Update 2026-08-17: today is actually a two-meeting Gitar day, not one — a second, previously-unlogged sync surfaced on calendar.** Besides the known 10:00-10:30am "Gitar Pricing Discussion" (Chuck listed as optional, still needsAction as of this sync — worth confirming he's actually dialing in), there's a separate **"Sonar & FieldNation: Pre-trial Sync (Gitar)," 1:30-2:00pm CDT**, invite created 2026-08-07, attendees Jace Key and Gautam Korlam (Sonar/Gitar), Joe (accepted), Chuck (accepted), Cory (needsAction) — and, notably, **Redwan Shahzada (accepted)**, despite having rolled off AI-DLC on 2026-07-14; flagging rather than assuming it's meaningful, could just be a stale invite he never declined. Outcomes for both meetings not yet known (same-day, this sync ran before either happened).

**Update 2026-08-17/18: the Pricing Discussion got moved, the Pre-trial Sync happened — with Redwan actually in the room.** Chuck moved the 10:00am "Gitar Pricing Discussion" (with Cory) to Wednesday 8/19, citing a busy day catching up from the on-site. The 1:30pm "Pre-trial Sync" did happen, with Chuck, Joe, **and Redwan** — so the stale-invite theory doesn't hold; Redwan is still actively participating in Gitar conversations more than a month after formally rolling off AI-DLC (worth noting, not necessarily resolving). Jack Kolis's post-meeting recap sets a concrete action item: **set up Gitar per Sonar's GitHub-connection instructions (docs.gitar.ai/connecting-code/github) by Wednesday morning** (8/19) or sooner, and Chuck still needs to confirm the moved Wednesday pricing-call time with Cory. This is the first concrete technical setup step in the trial plan agreed 8/10.

**Adjacent candidates surfaced via the AI Guild (July 2026):** **CodeRabbit** (paid; Guild wants a decision before going further) and **[PR Agent](https://github.com/The-PR-Agent/pr-agent)** (open source, floated as a GitHub Copilot review alternative). Neither is in the formal O1 evaluation yet.

### SDDF — Spec-Driven Development Framework (Spencer Moulton)
**Status: tentatively adopted as the spec-stack pattern.** The Moissanite Spec artifact (durable execution contract emitted by the Plan stage's Human Plan Gate) is SDDF-style. Per-ticket specs, durable across sandbox restarts, re-runnable, packaged via APM.
- **Repo:** https://github.com/spencemol/sddf
- **What it is:** A framework for spec-driven development with coding agents. Built around the idea that the spec stack should survive tool churn — agents, models, and runtimes can change, but requirements, task contracts, and review gates should not.
- **Key concepts:**
  - **Spec artifacts** as durable contracts between human intent and agent execution
  - **Weight classes** for task sizing: Quick (one-file, compressed review), Standard (2-5 files, full review), Full/Architecture (wave-based multi-task with gates)
  - **Conductor runtime** for orchestrating multi-agent execution with workspace isolation (Git WorkTrees)
  - **Dispatch packets** for parallel agent work coordination (artifact-based, not chat-memory coupling)
  - **Review gates** at task and wave levels
- **Why it matters for us:** Maps directly onto the PEV Loop — the "Plan" phase IS the spec. Formalizes the gate discipline the operating model calls for. Works at both scales: factory uses formal spec stacks for automated pipelines, individual engineers use lightweight spec templates for local Claude Code sessions.
- **Current backends:** Claude Code CLI, Codex CLI. Cursor and Gemini adapters stubbed.

**MOI-49 — SDD framework evaluation on FN Core (in progress).** Chuck opened this ticket 2026-07-02 off the back of an AI Guild discussion; Asad Ali is running it. Evaluating three candidates head-to-head on real FN Core work: GSD (use `open-gsd/get-shit-done-redux`, not the legacy `gsd-core` repo — active development moved after a trust/ownership incident upstream), Fission-AI/OpenSpec (the 52k+-star upstream, not the unofficial `chyiiiiiiiiiiii/openspec-skills` wrapper the ticket had mistakenly linked), and Spencer Moulton's SDDF (barebones repo currently — 3 commits, bare README — but Chuck wants it evaluated anyway). Open methodology questions as of 2026-07-03: which specific completed FN Core ticket(s) to use as ground truth, and who plays "product owner" during each framework's interactive Q&A phase. Chuck's answer (2026-07-06): locally, a developer plays whatever role is needed; for the AISF pipeline itself, the agent should post back to Linear as a comment and wait for human review before proceeding — consistent with the PEV Loop's human-gate pattern. _(added 2026-07-06)_

### Sonar — Verification Layer (POC concluded 2026-06-30/07-01)
**Update 2026-08-06: Launchpad kickoff happening today, not Aug 4 as previously targeted.** "LaunchPad Kick Off <> iTmethods - FieldNation" is on calendar for 2:30-3:00pm CDT today, organized by Shane Nazareth (ITMethods), with Loren Williams, Joe, and the broader ITMethods delivery team (Ivan Lima, Vinicius Melo, Vinod Avinane). No outcome yet.

**Update 2026-08-03: onboarding underway, Launchpad kickoff targeted for Aug 4.** Sonar confirmed 2026-07-16 that they received the signed copy (executed 7/15). Invoice **2601692** issued 2026-07-29; support-account activation email 7/28. Jack introduced the delivery side 7/27: **Loren Williams** (Sonar CSM) plus **Shane Nazareth** and **Vinod K. Avinane** (ITMethods). Joe set **`sonarqube-admins@fieldnation.com`** as the main owner account so org-level permissions aren't tied to an individual. Licenses were slated for delivery the week of 7/27; kickoff was originally targeted for Aug 4, deliberately one day after Chuck's return, and has since slipped two days (see above). Chuck removed Redwan from the vendor thread 7/28: *"he has transitioned off the project and it's just Joe and I now."* Integration tickets MOI-78 (feed-forward MCP) and MOI-79 (PR pipeline) remain Todo, both on Joe.

**Update 2026-07-16: Contract signed.** A SonarQube order confirmation (Order #00011413) was received 2026-07-16 00:03 CDT, addressed to Field Nation accounts payable, Redwan, and Chuck — confirming the order was received and processed, with license key/invoice to follow by email. This closes out the procurement question flagged as unconfirmed on 2026-07-15 (signature had been targeted for July 15 per the 7/7 Tropic timeline). License delivery + Launchpad onboarding start date remains **August 1, 2026** per the earlier timeline.

**Update 2026-07-14:** Two procurement/scoping questions resolved in #fieldnation-sonar: (1) Sonar Vortex (Context Augmentation & Agentic Analysis) is confirmed included in the current proposed contract, as part of the Agent Essentials package (Jack Kolis, confirming Joe's question). (2) SAST/security scanning is not an additional cost — it's part of the core SonarQube platform, scanning both quality and security issues in first-party code (answering a question Travis raised via Chuck).

**Status: moving through procurement via Tropic.** The 2-week Sonar POC (Linear MOI-44) validated successfully. Sonar (Jack Kolis) confirmed the trial result and flagged two new developments: (1) **Sonar Vortex** — Context Augmentation & Agentic Analysis packaged as a lower-cost module distinct from full SonarQube licensing, with Sonar committing to hold overall pricing steady; (2) a premium **Launchpad onboarding program** (~60 hrs: maturity assessment, developer-friction workshops, quality-gate setup) offered at no extra cost if finalized before late July. **The July 7 Sonar sync happened and produced a concrete procurement plan (per Jack Kolis's same-day recap in #fieldnation-sonar):** the deal is routing through **Tropic** (FN's procurement vendor) — Tropic assigns a rep, Redwan sends the intro email, Tropic/Sonar sync, Legal signs off the order form, then FN signs. **Signature target: July 15, 2026. License delivery + Launchpad start date: August 1, 2026** (both reconfirmed). Jack generated a new 1-year-term order form reflecting the Aug 1 start. Jack is OOO July 13–22, so Trey Adamson (trey.adamson@sonarsource.com) is the Tropic-process POC while he's out. As of 2026-07-08, Redwan was still working the Tropic rep intro. _(updated 2026-07-10)_ **Guitar** (Sonar's GenAI PR review add-on) remains a parallel track under discussion (Linear MOI-45).

### Guru — Knowledge Tier 3 candidate (superseded Unblocked, 2026-06-30)
**Status: active pilot, replacing Unblocked as the Tier 3 (Cross-cutting Context) candidate.** Cory evaluated Guru for enterprise-wide use and found it covers everything Unblocked does and more. Chuck and Redwan have taken over the Guru pilot from Cory; the mutual NDA was signed 2026-06-30 (confirmed with Eamon Walsh) and Guru (Cole Sherin) sent over the signed MNDA plus their SOC 2 report the same day. Chuck flagged wanting to add Amplitude as a data source once Guru is set up. **POC details — including spend approval — are still not finalized.** Per Cole Sherin's 2026-06-30 email, those need to be hashed out "when Cory returns, unless someone else has the authority to approve the spend." Cory is out this week, so the POC hasn't formally kicked off yet despite the NDA being in place. _(updated 2026-07-02)_ **Unblocked (Linear MOI-41) is being wound down** — Chuck told Mike Mueller directly it's "likely not" the direction anymore, reversing the 2026-06-02 re-prioritization decision.

**Update 2026-07-08/09:** Cole Sherin followed up with Cory on 7/8 asking for time to hash out POC details and cost. Cory replied same day, confirming a call for Friday afternoon (2026-07-10) to work through scope and spend approval. Chuck and Redwan still own the pilot day-to-day; Cory remains the approver on spend.

**Update 2026-08-03: Guru still not started; Unblocked pilot formally paused and disconnected.** Chuck to Mike Mueller, 2026-07-17: *"We are going to look at Guru when I get back (more enterprise wide but also does what unblocked does)."* Same conversation, he turned off the **Unblocked Slack integration** (it had been posting answers in a few channels), noting "we paused the pilot" (the trial had 8 days left; Mike kept using it personally and reported it giving right answers). No Guru activity of any kind surfaced during the 7/17–8/3 window. Chuck is sole owner of the Guru pilot post-Redwan. **This is now the longest-stalled item in the workspace: NDA signed 2026-06-30, still no POC five weeks later.**

**Glean re-surfaces as a live alternative (2026-08-05).** Glean was last raised by Cory on 2026-06-23 as a Guru/Unblocked alternative worth due diligence, but hadn't had a dedicated point of contact since. On 8/5, **Sandra Rabadi** (Sr. Technical Account Executive, Glean) emailed Cory, Ben Moormeier, and Chuck introducing herself as the new contact (replacing Dan, who left per a 7/22 heads-up from Dylan Kuntz), asking how "the Anthropic rollout is going" and whether Glean can complement it, and proposing times to connect. Not yet acted on. See [program/people.md](../program/people.md).

**Update 2026-08-17: intro meeting now on calendar.** A "*Placeholder* - Field Nation/Glean" meeting (2:00-2:45pm CDT), organized by Sandra Rabadi, invite created 2026-08-11, with Ben Moormeier, Cory, Chuck, and Dylan Kuntz — the first scheduled follow-up since her 8/5 outreach, though still framed as a placeholder rather than a working session. Same day as the Tessl and both Gitar meetings. Outcome not yet known.

**Update 2026-08-17: meeting rescheduled, not held.** Chuck emailed Sandra Rabadi to move the meeting off 8/17 ("need to move our meeting from today to later this week... busy day today getting back from the onsite last week"), landing on **Thursday 2026-08-20, 11:00-11:30am CT**. Sandra confirmed same day. So the four-vendor day was really three-plus-a-reschedule; the Glean intro is now Thursday.

### Spec-Driven Development — Industry Context
The broader SDD movement has gained significant traction in 2025-2026:
- **Thoughtworks** flagged SDD as a key new engineering practice for 2025
- **GitHub Spec Kit** — Open-source toolkit for spec-driven workflows (72k+ stars)
- **AWS Kiro** — Entire IDE built around spec-driven development
- **VentureBeat** reporting SDD as a requirement for enterprise-scale agentic coding
- **Martin Fowler** published on context engineering for coding agents, closely related
- SDD has moved from experimental to emerging industry standard

---

### Gitar Selected as O1 Code-Review Solution (2026-09-14)

**Decision finalized: Chuck confirmed in #moissanite-eng (2026-09-14), "we are moving forward with gitar."** This closes the three-month, four-way evaluation (Gitar vs. Tessl vs. native Copilot review vs. bespoke `fn-review-agent`). Despite the unresolved "how it reviewed" philosophical disagreement noted above (Gitar deliberately omits detailed review methodology, Chuck holds this as table-stakes), the decision to move forward is driven by end-of-September deadline pressure for a live code-review solution. **Next work:** integrate Gitar into the O1 KR3 embed-model rollout; update repo-readiness rubric to measure Gitar review effectiveness (MOI-109, in progress); finalize Gitar repository rules (MOI-98, testing whether custom `.gitar/rules/*.md` rules narrow the "how it reviewed" gap by targeting security-sensitive paths).

**Update 2026-09-18: independent team validation of Gitar review quality.** Chuck conducted a live demo of Gitar's code-review capabilities with Mykhailo Sakhnik, showing real Gitar review output against recent PRs. Mykhailo's assessment (first non-Chuck independent evaluation): *"gitar really provides review like human"* and *"it gives the prompt for an agent!"* — genuine validation that Gitar's output is substantive, agent-friendly, and suitable for downstream agent use. This is material evidence supporting the decision (decision was made despite the "how it reviewed" objection; this call confirms the output quality justifies the trade-off). Harness service account for Gitar CI integration was created by Joe Hoppesch 2026-09-18 (stored in 1Password), unblocking Gitar's integration into Field Nation's CI/CD pipeline for the embed-model rollout.

---

### ADR (Architecture Decision Record) Skill Suite Delivered (2026-09-14)

**New, substantial deliverable — three production skills shipped in ai-toolbox with companion tooling in ai-repo-score.** Asad delivered a complete ADR infrastructure (MOI-62):

**ai-toolbox skills (plugins/fn-software-engineering/skills/):**
- **`fn-adr-create`** — scaffolds a repo's ADR directory and config; bootstraps repos that have no ADR folder yet; includes templates and initial config generation.
- **`fn-adr-enforce`** — CI enforcement: validates ADR immutability (once an ADR is accepted, its decision content cannot change), tracks reviews and due-dates.
- **`fn-adr-review`** — due-date flagging for periodic ADR reviews; surfaces decisions that need re-evaluation against current project state.
- All three ship with SKILL.md, README, references, and invocation evals.
- Plugin bumped to version 1.1.0.

**ai-repo-score tooling (ai-repo-score repository):**
- **`adr-tools/`** — new Python workspace; validates ADRs, enforces immutability, builds the decision index, flags reviews due.
- **`.github/workflows/adr-check.yaml`** — reusable workflow that product repos call to enforce ADR checks in CI.
- **`playbook/adr/`** — playbook step docs, templates, worked example, validation evidence.
- Pinned to Python 3.11 for repo interoperability (installs into Node/PHP repos via uvx).

**Practice split:** Judgment lives in skills (interrogation, scaffolding, review prompting); determinism lives in CLI (validation, immutability enforcement, indexing).

**Organization rollout (once blockers clear):**
- **Once per person:** add the marketplace, install the `fn-software-engineering` plugin (standard org push).
- **Once per repo:** ask the agent to bootstrap (creates docs/adr/, template, config, AGENTS block, CI check — one PR).
- **Every ticket after that:** skills fire on their own when a decision is being made.

**Format decision still TBD:** Two options with identical cost and process:
- **Option A** — numbered: `0001-fanout-activities-are-blocking.md` (matches buyer-reporting and the convention outside FN).
- **Option B** — topic-versioned: `ADR_FanoutFailurePolicy_v1.md` (subject visible in filename, cleaner cross-repo filtering).

**Blockers before org-wide rollout:**
1. No tag cut on ai-repo-score (every @v1 reference currently resolves to nothing).
2. uvx auth against private repos not settled.
3. Cross-repo reusable workflows need org enablement.
4. Filename convention (Option A vs. B) still open pending team feedback post-demo.

**PRs pending:** Demo on day of sync (2026-09-14 standup), then feedback integration before opening PRs on ai-toolbox and ai-repo-score.

**Inspiration source:** adapted Matt Pocock's `domain-modeling` skill (interrogate-before-you-write approach, ADR format, lifecycle); `fn-adr-enforce` and `fn-adr-review` are novel (domain-modeling has no concept of checking changes against existing ADRs or scheduling reviews). The "grill-with-docs" pattern from Matt's skill couldn't be copied directly (ai-toolbox forbids skills calling other skills), so elicitation was inlined.

**Live-demo finding (2026-09-15 standup):** Asad demoed the full flow end-to-end in front of the team. A revert-the-decision attempt appeared to take effect when the ticket's immutability requirement says it shouldn't have — flagged by Asad and Chuck as needing verification/a fix before this ships further, not yet resolved. The demo also reopened (unresolved as of 09-15): whether `fn-adr-enforce` should really be a separate skill from `fn-adr-create` (Mykhailo leans toward merging it in, or into review), and whether a newer ADR should automatically outrank an older one in a conflict — Asad's position: no, it's an audit log, a human picks the winner. Both punted to Slack. Chuck reiterated ADR generation stays out of the repo scorecard until he's reviewed the skills himself via Asad's draft PR. See [knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md](./meetings/2026-09-15-team-moissanite-daily-standup.md).

**PR review and licensing gap (2026-09-18 → 2026-09-21):** Chuck reviewed `ai-toolbox PR #79` (2026-09-18) carrying the three ADR skills in final form. On 2026-09-21, Asad flagged a third-party license compliance issue: `fn-adr-create/references/` contains MIT-licensed code adapted from `mattpocock/skills` with only inline attribution, no SHA, no THIRD-PARTY.md file, and the plugin declares itself proprietary. **Open:** Does FN have a standard intake process (FOSSA/Black Duck/SBOM/legal form) for pulling third-party code into repos? If yes, Asad proposes the notice file should be the *output* of that process rather than a substitute. **Impact:** PR #79 cannot merge until this is resolved; no timeline confirmed. See [program/people.md](./people.md#asad-ali) for Asad contact and [program/references.md](./references.md) for Matt Pocock skill citation.

---

### OpenSpec Standardized for Spec-Driven Development (2026-09-10 – 09-14)

**OpenSpec (openspec.dev) confirmed as the organization standard for SDD.** Resolves the framework-selection question from MOI-49 (GSD vs. OpenSpec vs. Spencer's SDDF).

**Adoption implications:**
- **PR #65 (`ai-toolbox`)** — in progress, needs rework to focus on OpenSpec setup/documentation rather than reimplementing it. MOI-104 (earlier spec-tooling ticket) should close; MOI-60 (playbook step for SDD) needs rework to guide OpenSpec setup and usage.
- **Human spec review retained:** OpenSpec doesn't gate a human review step before implementation starts, but FN's PEV Loop requires it — keep that gate as a separate practice, not part of OpenSpec itself.
- **FN-specific rules:** Vitalii flagged that FN conventions (EARS format, quoting acceptance criteria, requirement citation per task) should stay in `openspec/config.yaml` per repo as local customization, not vanilla OpenSpec. Chuck confirmed that's fine.

**Connection to ADR work:** OpenSpec's native handling of structured specification pairs well with ADRs (both are artifact-driven, version-controlled, gate human review). The two toolchains are compatible, can grow together.

---

### Sonar Integration Enhancements (2026-09-18)

**Risk-assessment dashboard per PR — now in Sonar's roadmap (Chuck, 2026-09-18).** Chuck requested a feature allowing teams to review and adjust risk thresholds dynamically. Gautam Korlam (Gitar/Sonar, now in #fieldnation-sonar regularly) committed to adding a dashboard surface showing risk assessment per PR, enabling granular visibility into how Sonar's risk model is scoring each change. This unblocks the O1 code-review work (MOI-109) by providing concrete feedback on Gitar's effectiveness and risk-adjustment signals.

**Atlassian Confluence connector for design-context-guided reviews (available next week, 2026-09-18).** Chuck asked whether Sonar's Atlassian connector can feed design/architectural context from Confluence into code-review guidance — leveraging Field Nation's TDR/ADR docs as input to review rules. Gautam confirmed the Atlassian connector can do this; it's currently in Sonar's internal dogfood and **will be available to Field Nation next week**. This opens a new pathway for review quality improvement: pulling architectural decisions into the review context automatically, rather than requiring manual rule tuning.

**Blocking context:** MOI-78 and MOI-79 (wire Sonar into the O1 KR3 rollout) now have concrete unblocked momentum, with feature delivery from Sonar happening in parallel. Both enhancements directly improve Gitar trial observability and review-quality guidance, the two open action items for O1 code-review integration.

---

## Harness CI Eval Pipeline (MOI-149) — October 6, 2026

**Status: Near completion, awaiting Bedrock IAM access.** Mykhailo reports MOI-149 (Add non-blocking Harness CI eval pipeline and tune runtime and cost) is almost ready — the pipeline infrastructure is built, but **needs Bedrock IAM role assignment** to run evaluation jobs against AWS Bedrock. This is the final blocker for integrating non-blocking evals into the ai-toolbox CI/CD (MOI-91 work).

**Secondary dependency:** MOI-149 has a partial dependency on `ai-toolbox#90` (PR awaiting review), but Mykhailo can use mock commands as a workaround to unblock progress.

**Related tickets:** MOI-90 (skill-invocation evals), MOI-91 (ai-toolbox CI/CD), MOI-105 (Harness Evals vs. Deep Eval comparison). This work closes the gap on automated eval infrastructure for ai-toolbox skills, enabling continuous validation as skills change.

---
