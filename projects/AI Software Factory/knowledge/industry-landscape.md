# Industry Landscape

Standing home for **external** industry POV relevant to AI-DLC: vendor theses, published case studies, emerging vocabulary, and named practices from other organizations. Distinct from [tech-context.md](./tech-context.md), which covers Field Nation's own stack, tooling decisions, and in-flight evaluations.

**How to use this file:** append a dated entry per source or cluster of sources. Keep entries short - the thesis, the hard evidence, the FN-relevant hook, and a link to the deeper research report if one exists. This file is the map; research reports are the territory.

**When Chuck references "that article" or asks about industry direction, start here.**

---

## Current state of the industry conversation (as of 2026-08-25)

The dominant thesis, converged on by multiple major players in August 2026:

> **Code generation stopped being the bottleneck. Trust became the bottleneck.** Everything downstream of generation - environment setup, CI, verification, review, governance, evidence - is now proportionally more expensive and is where the remaining leverage sits.

Vocabulary that has entered common use and is worth using in FN conversations:

- **Cost per accepted change** (not cost per line) as the economic unit - decomposed across generation, environment, context, verification, review, remediation, governance.
- **Three modes, not one maturity curve** - human-controlled legacy / agentically accelerated / autonomous. The dividing line is whether execution can safely close the loop, *not* greenfield vs. brownfield.
- **"Learning turns into code"** - production failures become regression tests, incidents become policies, compliance obligations become continuous validation, accumulated agent outcomes become an internal eval set.
- **Builder** as a role spanning engineer / designer / PM / domain expert - anyone who can express intent, direct agents, and evaluate the result.
- **PDLC** over SDLC - the boundary between developing software and developing the product dissolves.
- **Context engineering** (Spotify's term) - the discipline of assembling and maintaining what an agent knows about a codebase.
- **Agents don't remove constraints; they expose them faster.** The load-bearing corollary: *"a thirty-minute CI pipeline defeats every model you point at it."*

Three claims that are well-evidenced across independent sources:

1. **Infrastructure before agents.** Every organization that got real results rebuilt dev environments and CI first. Amplitude admits they tried the reverse and it failed.
2. **Standardization is a prerequisite for agent reliability.** Inconsistent frameworks make context unwritable. Spotify dropped a whole framework from a migration over this.
3. **Embed, don't spread.** Amplitude's "peanut butter approach" across the whole org failed; they moved to a tiger team plus embedded engineers. Independent arrival at FN's own 2026-07-13 embed-first pivot.

One claim that is *not* well-evidenced and should be treated carefully: that any of this produces reliable, generalizable percentage gains. Only Amplitude publishes a genuine before/after. Stripe and Spotify publish absolute snapshots with no baselines. Anthropic's playbook has no case studies or statistics at all. And all three case-study orgs had unusually strong pre-existing infrastructure - Stripe's devboxes and 3M-test suite and Spotify's Fleet Management all predate LLM agents.

---

## Sources

### 2026-09-08 (published 2026-09-02): GitLab, "GitLab's internal playbook to foster AI-fluent technical teams"

https://about.gitlab.com/blog/how-gitlab-fosters-ai-fluent-teams/

**Read it as:** a first-party internal-ops post (Manu Narayan, CIO, and Rob Allen, Chief People Officer) about GitLab's own AI-adoption program, not a product launch, though it closes with a "get your AI maturity score" assessment-tool CTA. Same company as the already-logged "When code is abundant" CEO piece (2026-08-24), but this one is about internal culture and enablement rather than platform architecture.

**Named practice worth carrying:** a hybrid governance model splitting the work three ways: a central **"Enterprise AI"** hub (owns vendor strategy, guardrails, security - the only centralized piece), embedded **"AI Transformation Owners"** per business function (senior leaders close enough to the work to spot automation opportunities), and volunteer **"AI champions"** per function (early experimenters who evangelize adoption and support teammates). Paired with an **"AI Literacy Ladder"** - a self-assessment tool that places each engineer on a fluency path and routes them to role-specific training, with four named engineering pathways: planning, code review, fixing a broken pipeline, and security remediation.

**Hard numbers, GitLab's own:** 87% of engineers attending the advanced AI workshops said they learned something immediately applicable; a month after launching the Literacy Ladder, daily interactions with GitLab's primary internal AI coding tool rose 22.3%.

**What wasn't measured:** no productivity or throughput delta tied to the program, unlike Amplitude's before/after. The 22.3% figure is a usage-frequency signal, not an outcome metric - GitLab itself tracks "reach / depth / applied value" as three separate signals rather than claiming one causes the other, which is a more honest framing than most adoption stats in this space.

**FN-relevant hook:** direct hit on [open question 8](#open-questions-this-landscape-raises-for-fn) (is there a cultural track in the AI-DLC rollout) - AI-DLC's rollout so far is marketplace, telemetry, and scorecards, all technical. GitLab's split of a central governance hub from embedded, function-specific enablement owners is a concrete organizational pattern FN doesn't currently name as its own layer, even though the embed model (Chuck + traveling crew) is functionally close to the "AI Transformation Owner" role already. Worth a look for whether FN's embed passes should formally include a training/fluency component (GitLab's Literacy Ladder) rather than just repo-readiness fixes.

### 2026-09-07 (published 2026-09-02): Shopify Engineering, "How River takes security work from a fix to merge"

https://shopify.engineering/river-vulnerability-remediation

**Read it as:** a first-party engineering-blog case study (Kaiyi Li), continuing the River series introduced in "Under the River" (referenced in this piece, not separately logged here). No product to sell - River is Shopify's internal agent, not a vendor offering.

**Hard numbers, Shopify's own:** in the first 11 days of running River's dependency-vulnerability workflow, the open-issue backlog fell about 70% (roughly two-thirds direct merges, the rest confirmed obsolete or already fixed elsewhere by River). Since launch, the share of security fixes merging through Shopify's freshness-gated merge queue rose from about 10% to 80%. One example run: four Slack threads covering 35 findings across several codebases went up within a minute of each other and proceeded independently.

**Named practice worth carrying:** River treats its own tracking ledger as "a claim to be checked, not a fact" and re-validates live repository/PR/tracker state before acting - re-checking whether a vulnerability is still present, whether the underlying dependency lockfile has moved, whether a sibling PR already fixed it - before touching code. After a merge it re-checks the repository head and dependency graph before calling anything "done," explicitly rejecting "PR count and green-branch count measure motion, not remediation" as a success metric. It also draws an explicit line between what belongs in a prompt (judgment calls) versus what must be enforced in code (pagination, dedup, current-head identity, accounting) - "a workflow prompt isn't a control plane."

**What wasn't measured:** no comparison to a pre-River baseline (unlike Amplitude); the 70% and 10%-to-80% figures are both Shopify's own self-reported numbers, not independently audited, and the "11 days" and "since launch" windows aren't dated precisely in the piece.

**FN-relevant hook:** a sixth data point (after Stripe, Spotify, Amplitude, Ramp, and Zalando) for [open question 4](#open-questions-this-landscape-raises-for-fn) - and the cleanest one yet, since River's entire design is "verify before you touch code, verify again before you call it done," applied to a domain (security remediation) where a wrong "done" is expensive. The evidence-bound-to-repository-head discipline and the prompt-vs-code guarantee split are both directly reusable patterns for whatever FN's O1 KR2 code-review/verification step ends up looking like, independent of which reviewer wins.

### 2026-09-07 (published 2026-09-01): AWS, "Amazon Bedrock AgentCore Identity now offers a managed consent portal"

https://aws.amazon.com/about-aws/whats-new/2026/09/amazon-bedrock-agentcore/

**Read it as:** a first-party AWS "What's New" release note, not a case study.

**What shipped:** AgentCore Identity now provides a managed consent portal for OAuth 2.0 three-legged-authorization flows when an AgentCore Gateway connects an agent to a third-party tool (GitHub, Salesforce, and Slack named explicitly). Previously, developers had to build and host their own OAuth callback infrastructure per gateway; now each Gateway gets its own hosted consent web client and credential-provider list, admins can share a portal URL for pre-session consent, end users can self-serve their connection status, and IDE-based agent clients that can't natively present an OAuth consent URL get a dedicated authorization surface. Available in all commercial regions where AgentCore Identity ships.

**What this doesn't do:** it's scoped to agent-to-third-party-tool OAuth consent via AgentCore Gateway specifically. It doesn't address the different shape of problem Zalando's Identity Broker (logged 2026-09-04) targets - delegation chains and a token vault sitting between agents and MCP servers or each other. The two are complementary pieces of the same governance layer, not competing solutions to the same gap.

**FN-relevant hook:** a second concrete, shippable answer (after Zalando's Identity Broker) to O2's still-open, unowned "MCP credential governance" concern, and this one comes from the vendor FN is leaning toward (AWS Bedrock AgentCore). Worth a look before O2 locks in architecture: if FN's agents will call GitHub/Slack/other third-party tools through AgentCore Gateway, this removes a chunk of custom OAuth-callback build-out FN would otherwise own itself.

### 2026-09-04 (published 2026-08-14): Zalando, "Agentic Engineering at Zalando: a snapshot"

https://engineering.zalando.com/posts/2026/08/agentic-engineering-at-zalando-a-snapshot.html

**Read it as:** a first-party engineering-blog retrospective (Bartosz Ocytko, Executive Principal Engineer) covering 2.5 years of practice across 250+ engineering teams - no product to sell, closer to Spotify's Honk series in kind than to a vendor thesis. Caught as a coverage-gap backfill (three weeks old, not previously logged) because it's dense with named practices that map directly onto two of FN's own open questions.

**Two pieces bear directly on live AI-DLC decisions:**

1. **Risk-based PR auto-approval, a second real data point after Amplitude.** Zalando's bot evaluates every PR at creation against a rule set built from analysis of their own production incidents (typo'd config = high risk, backwards-incompatible change = medium, docs-only = low). **33% of PRs come back low-risk and are auto-approved**, which the author reports **cut PR lead time 20-40%** against the full PR population. Unlike Amplitude's model (which the workspace already has in depth), Zalando names a concrete anecdotal behavioral effect: engineers started splitting mixed-risk PRs into a fast low-risk slice plus a slower medium-risk one, rather than shipping them tangled together. This is now a second working example for [open question 3](#open-questions-this-landscape-raises-for-fn) (what class of change can merge without a human), independent of Amplitude's.
2. **Identity Broker - a named, in-progress answer to O2's unowned "MCP credential governance" question.** Zalando is building a broker component that captures delegation chains for on-behalf-of flows, brokers between different OAuth2 infrastructures, and implements a token vault, sitting in the call path between an agent and an MCP server (or between agents) so individual engineers stop generating disparate personal tokens. Not yet shipped - full detail is coming at AGNTCon + MCPCon Europe, Sept 18, 2026, Amsterdam. This is the first named architecture found anywhere in this landscape scan for the exact shape of problem O2 has flagged as open with no owner or timeline.

**Also worth noting, lower priority:** Zalando explicitly uses **AgentsView** (agentsview.io) for cross-tool session-data inspection - the same tool FN ticketed 2026-08-18 as an unevaluated candidate (MOI-96). This is independent third-party validation that it's a real, in-use tool, not just a cold lead. Separately, Zalando's commit-level complexity analysis found agent-adoption inflection points visible in cyclomatic complexity trendlines, and their governance model (a Tech Radar "AI" section plus Docker-image scanning to auto-detect AI model usage) is a lighter-weight alternative to building bespoke governance tooling, worth a look if FN's own repo-readiness governance layer needs one.

**What wasn't measured:** no company-wide productivity delta (unlike Amplitude); the auto-approval and lead-time numbers are Zalando's own report, not independently audited; the Identity Broker has no shipped numbers yet since it's pre-launch.

**FN-relevant hook:** two direct hits on open questions in one post is unusual for this scan - flagging as a candidate for a closer read given how specifically the Identity Broker and auto-approval sections match FN's stated gaps (O2 credential governance; O1 KR2's auto-merge-criteria question). Worth deciding whether this warrants a dedicated research note, especially ahead of the Sept 18 conference talk that will presumably publish more detail on the broker itself.

### 2026-09-03 (published 2026-06-03 and 2026-08-10): Spotify, "Coding Is No Longer the Constraint" (Code with Claude talk) + Spotify Portal, "Introducing Xirp"

[Coding Is No Longer the Constraint](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint) · [Introducing Xirp](https://portal.spotify.com/blog/introducing-xirp)

**Read them as:** the first is a first-party recap, on Spotify's own engineering blog, of a conference talk (Niklas Gustavsson, Spotify's Chief Architect/VP Engineering, at Code with Claude 2026) - real self-reported adoption numbers, not a controlled before/after case study. The second is a Spotify Portal (their commercial IDP spinoff) product-launch post with a "contact sales" / free-trial CTA - a vendor thesis with a product attached, even though the product is built from Spotify's own internal tooling. Caught late as a coverage gap (about three months and three weeks old respectively), not fresh news - flagging because neither had been logged despite being direct continuations of the already-logged Honk series.

**Hard numbers, self-reported, Spotify-wide:** 99%+ of engineers use AI coding tools weekly; 94% report being more productive; PR frequency up 76%. Fleetshift (Fleet Management's automation layer, which Honk plugs into) has merged 2.5M+ automated maintenance PRs to date, the vast majority auto-merged with no human in the loop - the first total-volume figure logged for Fleetshift itself, as distinct from Honk, the newer LLM-driven piece that sits inside it. Xirp: 36,000+ sessions, adopted organically by "thousands" of engineers - an adoption count, not a productivity delta.

**Named practice worth carrying: Xirp.** A vendor-neutral agentic development environment for running 50+ concurrent agent sessions per engineer across multiple harnesses (Claude Code, Gemini CLI, Codex), each session in its own worktree, with context decoupled from any single tool so engineers can switch models or harnesses mid-project without reconstructing state. Session transcripts and metadata flow back into Portal (Spotify's Backstage-based catalog) so any engineer or agent can resume another's work with full organizational context already loaded. This is a different layer than AgentCore or GCP Vertex - a session-orchestration/context layer that would sit above whichever hosting platform wins, not a hosting platform itself.

**What wasn't measured:** no productivity delta for Xirp, same limitation as Ramp's Inspect adoption numbers (logged 2026-08-25). The 76% PR-increase figure is a second independent data point (after DX's Q2-report DXI decline) for "generation sped up, review didn't" - Spotify names the effect directly: *"we now have 76% more PRs to review."*

**FN-relevant hook, two pieces:** First, the review-capacity numbers reinforce [open question 4](#open-questions-this-landscape-raises-for-fn) (whether the code-review bake-off is even the right question) - this is Spotify's own admission that the same review bottleneck FN is fighting shows up company-wide once generation accelerates, not just for one team. Second, Xirp's shape - vendor-neutral, multi-harness, context-portable across sessions - is a concrete named alternative that doesn't map cleanly onto FN's current AWS-vs-GCP AgentCore framing for O2; worth a look given O2's still-open "event-driven vs. continuous-loop architecture" question hasn't yet settled whether FN's agents need this kind of cross-harness portability or will standardize on one.

### 2026-09-02 (published 2026-09-01): GitHub Copilot code review can now approve pull requests

https://github.blog/changelog/2026-09-01-copilot-code-review-can-now-approve-pull-requests/

**Read it as:** a first-party GitHub changelog entry (product release), not a case study. One day old at time of logging.

**What shipped:** every Copilot code review now includes an "approval assessment" in its overview comment, signaling whether Copilot judges the PR ready to approve - on by default, but advisory only ("does not count toward merge requirements" by itself). Separately, and off by default, admins can now let Copilot actually submit an approval that counts toward a repo's required-approvals rule - configurable at enterprise, org, or repo level, with repo admins able to scope it to specific file paths. If new commits land after Copilot approves, the approval is dismissed just like a human reviewer's. Public preview, Copilot Pro/Pro+/Max/Business/Enterprise plans.

**What this doesn't do:** no risk scoring (size, scope, test coverage, API/data-model changes) the way Amplitude's auto-merge model works - it's a binary allow/scope-by-path switch, not a graduated model. Doesn't touch Chuck's "how did it review?" transparency bar either; the assessment is a verdict, not a coverage summary.

**FN-relevant hook:** this is GitHub's own first move on [open question 3](#open-questions-this-landscape-raises-for-fn) (what class of change could merge without a human) - worth watching since it's a live O1 KR2 candidate. If FN ever turned this on for a repo, Copilot could satisfy branch protection directly rather than just parallelizing review, which is a different (bigger) claim than anything else in the current four-way candidate set has made. Also worth flagging alongside the 8/24 `core-ui` finding that Gitar's terse verdicts missed real issues Copilot caught on the same diffs (see the 2026-08-20 entry below) - an "approve" button on a reviewer with unresolved coverage questions is exactly the failure mode Spotify ranked worst ("PR that passes CI but is functionally incorrect... erodes trust in our automation").

### 2026-09-02 (published 2026-03-09): Anthropic's own Code Review product - dogfooding numbers (16% to 54% substantive-review coverage)

https://claude.com/blog/code-review

**Read it as:** a first-party vendor announcement that doubles as a real usage case study, since Anthropic runs the product on its own PRs and reports the before/after. A backfill: nearly six months old, not caught by a prior scan or logged anywhere in this workspace (checked `tech-context.md` too) - flagging now because it's a hard-numbers data point for one of O1's original three code-review candidates ("Claude Code review," alongside Sonar and Copilot, per the [H2 OKR doc](../program/OKRs/h2-2026-okrs.md)) that had gone unquantified while the live evaluation moved on to Gitar/Tessl/bespoke/Copilot.

**Hard numbers, self-reported from Anthropic's internal usage:** before Code Review, 16% of PRs at Anthropic got substantive review comments; now 54% do. On large PRs (1,000+ lines), 84% get findings, averaging 7.5 issues; on small PRs (under 50 lines), 31% get findings, averaging 0.5 issues. Under 1% of findings are marked incorrect by engineers. Average review takes ~20 minutes. Cost runs $15-25/review, billed on token usage, with org-level monthly spend caps and repo-level enable/disable as the admin controls. Stated motivation: code output per Anthropic engineer grew 200% in the last year.

**What it doesn't do:** it does not approve PRs - "that's still a human call," a deliberate design choice, unlike GitHub's new approval feature logged above the same day. No published head-to-head defect-catch-rate benchmark against Gitar/Tessl/Copilot from Anthropic itself (independent third-party benchmarks exist for other tools like CodeRabbit/Greptile but weren't verified here and aren't Anthropic's own data).

**FN-relevant hook:** this is the first hard-number data point found for the original "Claude Code review" candidate, and it lands right in the middle of the live four-way bake-off ([open question 4](#open-questions-this-landscape-raises-for-fn)). The <1% false-positive rate and the 16%→54% coverage jump are stronger self-reported numbers than anything Gitar or Tessl has published; the $15-25/PR cost transparency is also a data point none of the other candidates have given. Still doesn't resolve Chuck's "how did it review?" bar on its own - it publishes outcome stats, not a per-review process/coverage summary - but since the underlying review is already multi-agent (parallel bug-finding, verification pass, severity ranking), a `REVIEW.md`-style pass-tagging retrofit (per Anthropic's own later SDLC playbook, logged 2026-08-21 below) looks like a more natural fit here than on a more opaque reviewer.

### 2026-09-01 (published 2026-08-28): GitHub Copilot code review's default effort level changes from Lite to Balanced (effective Sept 28)

https://github.blog/changelog/2026-08-28-upcoming-changes-to-github-copilot-policies-and-billing/

**Read it as:** a first-party GitHub changelog entry (product/billing policy announcement), not a case study. Bundled with two unrelated billing/policy changes in the same post; only the code-review piece is relevant here.

**What's changing:** starting September 28, 2026, any org or repo using Copilot code review's "Default" effort setting will get **Balanced** (a higher-reasoning model, deeper analysis) instead of **Lite** (the current default, aimed at straightforward changes). Anyone who wants to keep Lite has to explicitly opt out before that date; otherwise the switch is automatic. Effort levels themselves (Lite/Balanced) went GA on 2026-08-07, already logged in `tech-context.md`'s MOI-97 thread contextually but not this specific default-flip.

**FN-relevant hook:** native Copilot review is one of the four live O1 KR2 code-review candidates. This changes the out-of-the-box behavior for any repo that hasn't explicitly set an effort level, on both cost (Balanced uses a "higher-reasoning model") and depth. Doesn't touch Chuck's "how did it review" transparency objection at all, and doesn't affect MOI-97's marketplace-skill-visibility finding (that's about which skills the reviewer can read, not how hard it looks). Worth a one-line check before 9/28: if `engineering-dbt`, `core-ui`, or any MOI-97 probe repo relies on the org/repo default rather than an explicit setting, its Copilot review depth (and spend) is about to change without anyone touching a config file.

### 2026-08-31 (published 2026-08-27): GitHub Copilot code review closes two coverage gaps, adds resolution-reason tagging

https://github.blog/changelog/2026-08-27-copilot-code-review-resolution-reasons-and-expanded-capabilities/

**Read it as:** a first-party GitHub changelog entry (product release notes), not a case study or thesis piece. Bears on the O1 code-review decision because native Copilot review is one of the four live candidates, and MOI-97 (resolved 8/24) already found real limits in how that reviewer works.

**What shipped:** Copilot code review can now fully review two PR types it previously couldn't: (1) PRs authored by bots and auto-requested for review, now billed to the org rather than needing a per-user Copilot license to attribute the review to; (2) PRs opened by Copilot cloud agent itself, which previously fell back to a "limited experience" - these now get a full agentic review. The 300-file/20,000-line size cap on reviewable PRs is also removed. Separately, reviewers can now tag why they resolved a Copilot comment (Addressed / Won't fix / Incorrect) - feedback to GitHub's product team, not reviewer-side transparency.

**What this doesn't do:** none of this touches Chuck's "how did it review?" bar - it's coverage expansion (which PRs Copilot can review) and a human-side feedback signal, not the reviewer publishing its own process or coverage. The resolution-reason field also doesn't map onto per-review-pass tagging the way Anthropic's `REVIEW.md` play or Gitar's `.gitar/rules` do.

**FN-relevant hook:** the "no Copilot-licensed account to attribute the review to" framing on bot-authored PRs is a related but distinct wrinkle from MOI-97's finding (Copilot review runs service-side with no per-user client, so it can't read user-scoped marketplace skill config) - both point at the same underlying architecture: Copilot's automated review path doesn't carry a specific human identity/context forward. Worth a quick check on whether PRs opened by `fn-review-agent` or any future FN bot would now trigger a *full* Copilot review under this org-billed path, which changes the cross-vendor-review math (bespoke agent authors, Copilot fully reviews) versus before this release.

### 2026-08-28 (published 2026-08-06): AWS Bedrock AgentCore ships runtime instances (GA) and temporal policies + rate limiting

[Runtime instances](https://aws.amazon.com/about-aws/whats-new/2026/08/aws-bedrock-agentcore-runtime-instances-generally-available/) · [Temporal policies and rate limiting](https://aws.amazon.com/about-aws/whats-new/2026/08/temporal-policies-agentcore/)

**Read it as:** first-party AWS product announcements (AWS "What's New" release notes), not case studies. Both posted 2026-08-06 - three weeks old, and neither had been caught by a prior scan or logged anywhere in this workspace (checked `tech-context.md` too), so flagging now as a coverage gap rather than breaking news.

**What shipped:** (1) Runtime instances - agents can now run on your own EC2 instances (GPU/memory/compute-optimized families) instead of only the default serverless microVM runtime. This matters because the microVM path caps sessions at 8 hours; runtime instances support sessions up to 14 days, with AWS still handling provisioning, patching, and scaling. Live in 9 regions including US East/West. (2) Temporal policies - stateful authorization rules that evaluate a request against what an agent already did earlier in the same session (enforce step ordering, require a tool argument to match a prior call's output, require human approval before a privileged action, enforce data freshness), plus per-user/group rate limiting on tools, models, and concurrent connections via OAuth or IAM scoping.

**FN-relevant hook:** FN is leaning AWS Bedrock AgentCore for O2 (not yet formally locked in per the 8/4 tracker note questioning AgentCore vs. GCP/Azure). Both capabilities land directly on open O2 gaps: "MCP credential governance" has no owner or timeline today, and temporal policies plus rate limiting is AWS's built-in answer to exactly that shape of problem. Runtime instances' 14-day sessions speak directly to O2's stated goal of *long-running* agents - the 8-hour microVM cap would have been a real constraint for something like Op Cop running continuously. Worth a look before O2's M2/M3 (IaC + CD pipeline, both due Aug 28) lock in an architecture that doesn't use either.

### 2026-08-28 (published 2026-07-22 and 2026-06-09): DX, "State of AI Impact in Engineering: Q2 Report" + AI cost management report

[Q2 report announcement](https://getdx.com/news/dx-releases-q2-2026-state-of-ai-impact-in-engineering-report/) · [AI cost management report](https://getdx.com/blog/meet-the-new-ai-cost-management-report/)

**Read it as:** DX is FN's own licensed developer-intelligence platform (the tool behind O1 KR1's scorecard, TrueThroughput, and PR cycle time). This is a vendor-published aggregate benchmark across DX's own customer base (500+ orgs, April-June 2026, trend data back to Q3 2025) - not a single first-party case study, but real telemetry-plus-survey data, and it's the same vendor whose numbers FN's leadership already cited on 8/24. Both pieces are older than this scan's usual window (July 22 and June 9) and weren't caught by a prior pass or logged here, so this is a backfill, not a fresh launch.

**Hard numbers:** median weekly TrueThroughput rose 37% over four quarters, from 1.42 to 1.94 PRs/engineer/week. Over the same window, DX's Developer Experience Index (DXI) fell from 67 to 65, attributed to declines in incremental delivery, local iteration speed, and review turnaround - DX frames one DXI point as roughly 10 lost engineer-hours/year. Change confidence (developer trust that a change won't break something) fell 6.1%; maintainability rose 3.8%; AI-generated code is now 52.7% of all code, up from 24% two quarters earlier. Small/midsize orgs (15-99 engineers) hit ~2.2 PRs/eng/week vs. 1.2 for 750+-engineer orgs. Median quarterly AI spend grew roughly 28x in a year in the Tech sector even as the innovation ratio (new work vs. maintenance) stayed flat near 57-58%.

**FN-relevant hook, two pieces:** First, FN's own KR1 target is 3.5 TrueThroughput against a stated "P90 industry benchmark of 2.6" (set at the 8/24 metrics meeting) - this report's freshest DX-published baseline is a *median* of 1.94, a different percentile entirely. Worth checking with the DX account rep whether FN's 2.6 P90 figure comes from this same dataset, since median and P90 aren't comparable numbers and this is the only independently-verifiable DX baseline found so far. Second, the DXI decline - specifically review turnaround - is another data point for the "generation sped up, trust/review didn't" thesis already logged here, and lines up with FN's own still-open O1 KR2 code-review decision. Third: DX's **AI cost management report** (a feature in the platform since June, not a new build) already computes an "estimated cost per PR" from AI spend plus merged-PR volume - this may directly answer [open question 1](#open-questions-this-landscape-raises-for-fn) (cost per accepted change, decomposed) without FN building anything, if it's enabled on FN's Data Cloud instance. Worth a five-minute check with the DX rep before scoping any bespoke measurement work.

### 2026-08-20: Tessl launches Tessl Code Review (public beta)

https://tessl.io/blog/launching-tessl-code-review · reported by [DevOps.com](https://devops.com/ai-agents-are-writing-code-faster-than-anyone-can-review-it-tessl-is-betting-on-standards-to-close-that-gap/) (Tom Smith, published 2026-08-20)

**Read it as:** a vendor product launch with a product attached, but one that bears directly on the live O1 code-review decision - Tessl is already one of the four candidates Chuck is evaluating (alongside Gitar, native Copilot review, and `fn-review-agent`). The DevOps.com piece is independent trade coverage, not a Tessl press release, and includes on-record commentary from a Futurum Group analyst (Mitch Ashley) rather than only Tessl's own framing.

**Thesis:** review capacity, not code generation, now caps how fast agent-written code ships. Tessl's answer: standards written once, stored as versioned skill files *in the repo* (not baked into the vendor's product), that the reviewing agent checks the full PR against - surrounding codebase, prior thread discussion, and diff - rather than just the latest diff.

**Named practice worth carrying:** two mechanics stood out. First, review standards as repo-committed, team-owned, portable files - the same "commit review skills per repo" conclusion FN's own MOI-97 A/B test landed on 2026-08-24 for an unrelated reason (Copilot's automated review can't read marketplace-distributed skills, only repo-local ones). Tessl's architecture assumes that constraint by design rather than discovering it. Second, "review memory" across revisions - a second-pass review knows what was already fixed, explained, or explicitly declined, and only flags what's still open, instead of re-flagging the same findings every push.

**What wasn't measured:** no adoption numbers, no before/after, no claim about defect-catch rate. This is a launch article, not a case study - Tessl's own site (checked same pass) shows a separate, non-code-review-specific `tessl review run` skill-quality feature from June 2026, so it's worth not conflating the two Tessl "review" products.

**FN-relevant hook:** doesn't resolve Chuck's "how did it review?" objection (still no published process/coverage summary), but it's a second real-world validation - after Anthropic's `REVIEW.md` play and FN's own MOI-97 finding - that the industry is converging on *repo-committed, team-owned review criteria* as the standard shape, regardless of which vendor wins. Worth factoring into whatever FN lands on for O1 KR2, and worth a direct look given the live Tessl relationship (8/17 "Tessl Code Review overview" call already happened, per [tech-context.md](./tech-context.md)) - Chuck may already have seen this in a vendor briefing rather than as external news.

### 2026-08-25: The Pragmatic Engineer, "Why Ramp built its own in-house coding agent, Inspect"

https://newsletter.pragmaticengineer.com/p/why-ramp-built-inspect

**Read it as:** first-party case study, reported by an independent engineering newsletter (Gergely Orosz, with Jessica Salmon and Ivan Klaric) based on interviews with Ramp's CTO and Inspect's founding engineer - not a Ramp-authored blog post, but sourced directly from Ramp leadership. Paywalled past the free preview: this entry is built on sections 1-3 (what Inspect is, why they built it, how it's used) plus adoption numbers. The architecture/tech-stack section sits behind the paywall and wasn't read - don't treat this entry as covering Inspect's technical implementation.

**Thesis:** Ramp is one of a growing cluster of orgs (Stripe/Minions, Spotify/Honk, Block/Goose, Shopify/River, now Ramp/Inspect) building bespoke in-house coding-agent harnesses instead of buying, because remote sandboxed execution, deep internal-tool integration, and closed-loop verification (tests plus visual screenshots) currently outrun what off-the-shelf harnesses offer.

**Hard numbers, all self-reported by Ramp:** 75% of all company-wide merged PRs now come from Inspect sessions; ~90% of PRs into the Inspect repo itself come from Inspect; sub-5-second sandboxed dev-environment spin-up; 1M+ total sessions as of July; a 5.5-person team (4 engineers, a director, a part-time PM); 150+ engineers have contributed to the Inspect codebase itself.

**Most relevant piece for FN: ReviewBuddy.** A code-review agent built on top of Inspect by a single engineer in a week. Ramp's account is that it beats third-party AI review tools specifically because of context-awareness (same internal data and tool access as a human reviewer, not just the diff). That's a fifth data point, alongside Stripe/Spotify/Amplitude and Chuck's own `fn-review-agent` prototype, for the "bespoke beats vendor on context" side of the open O1 code-review decision.

**Verification pattern matches Spotify's Part 3 finding:** Inspect closes the loop itself before a human looks at the PR - running tests, telemetry, and feature-flag checks on the backend, and literal screenshots/live previews for frontend changes.

**Caveat:** no before/after baseline here, unlike Amplitude. These are adoption-share numbers (how much of Ramp's PR volume runs through Inspect), not productivity deltas - useful as evidence bespoke harnesses get real usage, not evidence they produce more value per PR.

### 2026-08-24: GitLab, "When code is abundant" (Bill Staples, CEO)

https://about.gitlab.com/blog/when-code-is-abundant/

**Thesis:** When implementation becomes abundant, trust becomes scarce. Trust depends on the environment around the model - context, verification, governance, evidence - and that layer must be durable and independent of any single model or vendor. His ownership principle: *"Own the agent. Own its instructions. Own the context it learns from. Keep the models and infrastructure underneath it a choice."*

**Read it as:** a well-argued thesis with a product attached. It concludes with GitLab's four-capability platform architecture (agent platform / execution / context / governance) and "that is the opportunity we see for GitLab." The observations hold up; the architectural conclusion is a sales position. Its evidence is entirely borrowed from Stripe, Spotify, and Amplitude.

**Most useful pieces for FN:** cost per accepted change; the three-modes framing and its three diagnostic questions; "the pipeline runs the inner loop"; the "records, not files" critique of markdown-in-repo practice; the 90-day checklist (measure cost per accepted change, time your CI, write down auto-merge criteria, make context portable, choose which business signals enter the loop).

**Note the naming collision:** Staples uses "software factory" in almost exactly the sense the Moissanite workstream does - *"a system that continuously turns intent and business signals into verified software."*

**Citable line:** *"Software engineering spent sixty years protecting a scarce resource. It will spend the next decade governing an abundant one."*

→ Full analysis, with all figures verified against primary sources: [research/2026-08-25-code-abundance-thesis.md](../research/2026-08-25-code-abundance-thesis.md)

### 2026-08-21: Anthropic, *The AI-Native SDLC Playbook* (Louis Claxton, Applied AI)

https://claude.com/blog/the-ai-native-sdlc-playbook

**The piece GitLab is responding to, and the more directly operational of the two.** Closest thing publicly available to a build spec for what AI-DLC is trying to do. Opens with "Code is no longer the bottleneck."

Six stages as a loop (Plan / Design / Build / Test / Deploy / Maintain), prescribed as modular "plays" with a dependency graph. The load-bearing mechanic is the **committed artifact chain**: `intent.md` → `spec.md` → `plan.md` → diff and tests → PR with review findings → incident record. Each stage ends by writing to version control; the next begins by reading it; accepted artifacts fire the next gate.

**Contains no case studies and no statistics.** Every number in it is a recommendation, not a measurement.

**Most relevant to the open O1 code-review decision:** the `REVIEW.md` play - the review policy becomes a committed artifact with **named passes** (bugs/logic, security, compliance against spec and plan), each finding **tagged with its pass**, "Important" vs. "Nit" defined, nits capped, and a **machine-readable severity tally published by the check run**. Findings never approve or block on their own; branch protection still requires a code owner.

**This is a different answer to Chuck's "how did it review?" problem.** Not reviewer explainability - instead, the organization declares what must be checked and regression-tests the reviewer's compliance via evals. Structurally close to what MOI-98 is testing with Gitar's `.gitar/rules/*.md`. Possibly more achievable than getting a vendor to change its output, and portable across whichever tool wins.

**Also worth carrying:** evals run in CI on a schedule *and on any change to `CLAUDE.md`, skills, or hooks*, with config changes gated on eval pass rate, and every production incident becoming a permanent eval. Skills are explicitly framed as **advisory** controls - *"A skill is a control, though an advisory one… The skill makes violations rare and the hook makes them close to impossible."* That maps precisely onto the MOI-97 finding about which skills actually reach an automated reviewer.

### 2026-02: Stripe, "Minions: Stripe's one-shot, end-to-end coding agents" (Parts 1 & 2)

https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents · [Part 2](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents-part-2)

Fully unattended agents: Slack message in, CI-passing PR out, no interaction in between. **Over 1,300 merged PRs/week are entirely minion-produced** (up from 1,000 ten days earlier - secondary sources still quote the older figure). 3M+ test suite, ~500 internal MCP tools, hundreds of millions of lines of code.

**Two patterns worth stealing:**

- **"Blueprints"** - workflows defined in code as a state machine where each node is either deterministic code or a scoped agent loop. Agentic: "Implement task," "Fix CI failures." Deterministic: "Run configured linters," "Push changes" (no LLM call at all). *"Putting LLMs into contained boxes compounds into system-wide reliability upside."*
- **The environment is the containment boundary, not the merge gate.** Pre-warmed EC2 "devboxes" in the QA environment, no production access, no real user data, no arbitrary egress - and *because* of that isolation, minions run with **full permissions and no confirmation prompts.** A materially different safety model from tool-level restriction.

**Don't misattribute:** Stripe has **no** risk model, **no** auto-merge, and **no** SOC 2 discussion. Every minion PR is reviewed by a second engineer. Their North Star is a PR with no human *code*, not no human *review*. Their pre-push checks are **linters, not tests** - under 5 seconds, backed by a caching daemon; tests run in CI, capped at two rounds before it goes back to the human.

**Their failure modes:** global unconditional rule files filled the context window before the agent started (fix: conditionally scope everything to subdirectories and file patterns, auto-attached during traversal). Tool sprawl hurt - ~500 tools exist, minions get a small curated subset, because *"agents perform best when given a 'smaller box.'"* They sync one rule source across Cursor, Claude Code, and minions.

**The caveat that matters:** devboxes, the 3M-test suite, the lint daemon, and Sourcegraph all predate LLM agents. This is a story about an org that already had the substrate.

### 2025-11 → 2026-04: Spotify Engineering, "Background Coding Agents" / Honk (4 parts)

[Part 1](https://engineering.atspotify.com/2025/11/spotifys-background-coding-agent-part-1) · [Part 2 - context engineering](https://engineering.atspotify.com/2025/11/context-engineering-background-coding-agents-part-2) · [Part 3 - feedback loops](https://engineering.atspotify.com/2025/12/feedback-loops-background-coding-agents-part-3) · [Part 4 - dataset migrations](https://engineering.atspotify.com/2026/4/background-coding-agents-dataset-migrations-honk-part-4)

**The most useful of the three case studies for FN, because it's the most honest about failure.** 1,500+ merged AI-generated PRs (a Nov 2025 floor, never updated), 60–90% time saving on migrations, ~50 migrations run with Claude Code.

Honk is **not a new system** - it's an extension of Spotify's existing Fleet Management, which already automated roughly half of all Spotify PRs pre-AI. Honk replaced only the transformation-definition step: a deterministic migration script became an agent taking a natural-language prompt.

**Part 3 is the piece to study - abstracted verification.** Design principle, verbatim: *"the agent doesn't know what the verification does and how, it just knows that it can (and in certain cases must) call it to verify its changes."* Individual verifiers are never exposed; they auto-activate on repo contents (Maven verifier activates on a root `pom.xml`), behind a single MCP tool. Verifiers regex out only the relevant error lines and return a terse success message otherwise, for context economy. **All relevant verifiers run before a PR can be opened, via the Claude Code stop hook** - if one fails, no PR. An LLM-as-judge runs last on diff + original prompt, added because agents got *"a bit too 'ambitious', trying to solve problems that weren't strictly in their prompt, like refactoring code or disabling flaky tests."* Judge vetoes ~25% of sessions; the agent then course-corrects half the time.

**Their ranked failure modes:** no PR produced ("a minor annoyance") < PR that fails CI ("frustrating") < **PR that passes CI but is functionally incorrect - "the most serious error, as it erodes trust in our automation."**

**Context engineering: prompt-first, tool-light, on purpose.** They prefer large static prompts because *"you can version-control the prompts, write tests, and evaluate their performance,"* and reject thin-prompt-plus-many-tools because *"the more tools you have, the more dimensions of unpredictability you introduce."* Exactly three tool grants: verify, Git (selective subcommands, never push), Bash with a strict allowlist. Code search and documentation tools deliberately withheld. Their recommendation is to push context **into the repo** - tests, linters, API docs - because that works for all prompts and all agents thereafter. Best single prompting tip: **state preconditions, i.e. tell the agent when *not* to act.** Second best: ask the agent to critique its own prompt afterward.

**The standardization finding (Part 4), and its nasty corollary.** Three pipeline frameworks. The two consistent ones (BigQuery Runner, dbt) worked; Scio's per-team variability meant *"this lack of standardisation… made it hard to write all-in-one prompts for Honk that could truly capture all available permutations."* **They dropped Scio entirely.** But the two standardized frameworks *rarely used build-time unit testing* - so verification was unavailable and all 240 PRs shipped on manual downstream testing. **Standardization bought context, not verifiability. They're separate prerequisites and they don't arrive together.** Directly relevant to Emerald: `engineering-dbt` is a dbt repo, and dbt is exactly the consistent-but-untested case.

**Their admission worth remembering:** four parts into a public series about a production agent, *"we are still flying mostly by intuition. Our prompts evolve by trial and error. We don't yet have structured ways to evaluate which prompts or models perform best."*

### undated (six-month window, published ~2026): Amplitude, "Speedrunning Software: How We 3x'd PRs in 6 Months"

https://amplitude.com/3x

**The only one of the three with a genuine before/after dataset, a real auto-merge risk model, and a SOC 2 story.** All figures verified:

| Metric | Before → After |
|---|---|
| PRs shipped | **3×**, same headcount, six months |
| PR cycle time | 5.2 hrs → **44 min** |
| Frontend CI | ~30 min → **3–4 min** |
| Bug reports | 715/month → **319** (↓55%) |
| Non-engineer share of PRs | 0% → **~5%** |
| Newcomer full-stack setup | 1–4 days → 10–15 min |

**What they rebuilt, in order:** a custom local dev CLI on devbox/Nix (per-service containers rejected - machines ran out of memory); CI via three levers, of which **swapping JS tooling for Rust/Go natives was "the single biggest lever"** (~10× on tsc→tsgo, ESLint→oxlint, Prettier→oxfmt); then **automated legacy migrations aimed specifically at the patterns that confused agents most** - three design systems collapsed to one, three translation systems to one - with the results codified as **agent rules and skills backed by lint checks** so nothing drifts back. These were migrations they'd *"written off for years."*

**Their auto-merge risk model** scores size, scope, area of codebase, test coverage, and whether public APIs or data models change. Low risk + comments resolved = auto-merge; medium and above routes to a code owner. And the gate has its own feedback loop: *"When someone catches a change that shouldn't have merged, it goes into an eval suite, and we tune the risk model."*

**Their SOC 2 paragraph is the most quotable thing in any of these sources:** *"SOC2 doesn't need a human to click approve. What it does need is demonstrable controls: documented criteria, logged decisions, and a way to override."* Approval and validation are logged as **separate signals**. Compliance was brought in early rather than retrofitted.

**"CI as our new dev environment."** Below five minutes, local stopped being the fast option. Engineers push and review in GitHub - and being cloud, they aren't limited to one change at a time: *"They can kick off 20 PRs before logging off and review them the next morning."* That five-minute threshold produced a qualitative behavior change, not a marginal one.

**Their admitted mistakes, the most valuable part for FN:**

- **"The peanut butter approach" failed** - spread thin across the org. Moved to a tiger team plus embedded "AI-pilled engineers." Same conclusion as FN's embed-first pivot, reached independently.
- **They tried to build agents before the infrastructure existed.** *"There was no need for us to jump to the end. Prioritizing our foundational DevEx is what ultimately helped us move faster."*
- **The hard part was cultural.** Wade Chambers (Chief Engineering Officer): *"AI progression is largely not a technical issue. It's a cultural one."* Two waves - AI fluency, then workflow disruption - with leaders made to **vibe code live on stage in front of the whole EPD org.**
- Self-aware on their own headline metric: *"PRs can be artificially inflated, and more code isn't inherently valuable."*

**Non-engineers shipping code is Amplitude-only among the three.** Designers, PMs, and marketers at ~5% of PRs. Named case: a principal designer went from a button-alignment PR to reorganizing product navigation and sunsetting low-usage sections - a change the team wanted but never prioritized. Enablers: fast setup, preview links so nothing runs locally, per-PR deep links to the changed page, real training, and **an internal marketplace for shared skills that anyone can access.**

### 2026-07-16 (shared internally; published 2026-07-07): Warp, "A Guide to Cloud Software Factories for Engineering Leaders"

https://www.warp.dev/blog/a-guide-to-cloud-software-factories-for-engineering-leaders

Shared by Chuck in #moissanite-eng with the framing *"we aren't building out the full factory"* but relevant background. Logged here for continuity; predates the August thesis cluster above.

### 2026-08-14 (shared internally; publish date not confirmed): Augment Code, "Solving Code Review with Cosmos"

https://www.augmentcode.com/blog/solving-code-review-with-cosmos

Shared by Chuck in #eng-news as closely matching his own vision for agentic code review (interactive pair-review pattern). Partial inspiration for the `fn-review-agent` prototype. Kept here as the code-review-specific industry POV; the FN-side evaluation state lives in [tech-context.md](./tech-context.md#gitar-ai-code-review-candidate-sonar-live-as-of-2026-07-27). *(URL carried over from `program/references.md`; could not be re-verified as live during the 2026-08-25 pass.)*

---

## Conventions for this file

- **Section headers use the date the source entered FN's orbit** (published date where that's also when we read it, share date otherwise, noted inline when they differ).
- Every entry should say what to *read it as* - neutral analysis, vendor thesis with a product attached, or first-party case study - because the mix here skews heavily toward vendors with something to sell.
- Distinguish what a source *measured* from what it *recommends*. Anthropic's playbook has zero measurements; Amplitude has the only real before/after. Keep that legible so nobody cites a recommendation as evidence.
- Link out to a research report for anything that warranted primary-source verification, and keep the entry here short enough to skim.

---

## Open questions this landscape raises for FN

Tracked here so they don't get lost between research reports. Full reasoning in [research/2026-08-25-code-abundance-thesis.md](../research/2026-08-25-code-abundance-thesis.md) §5.

1. **What is FN's cost per accepted change, decomposed?** KR1 measures the aggregate (True Throughput, cycle time, revert rate) but not the breakdown across environment / CI / review wait / remediation / governance. Both GitLab and Amplitude say the breakdown is where the actionable finding lives. **2026-08-28 addition:** DX's AI cost management report (in-platform since 2026-06, see the DX Q2-report entry above) already computes an estimated cost per PR from AI spend and merged-PR volume - worth checking with the DX rep whether it's enabled for FN before building anything bespoke.
2. **What are Emerald's, Core UI's, and FN Core's full CI pipeline durations?** Not currently an AI-DLC metric. Amplitude's five-minute threshold was a phase change, not an increment. Cheap to measure, plausibly reorders the KR2 playbook.
3. **What class of low-risk change could merge without a human, and what are the documented criteria?** Nothing in O1 does this. It's the only lever that *removes* human review time rather than accelerating it, and Amplitude's risk model plus SOC 2 framing is a working template. **2026-09-02 addition:** GitHub Copilot code review can now submit an actual approval that counts toward required-approvals (off by default, admin-scoped) - a first vendor move on this question, but a binary switch, not a risk-scored model like Amplitude's. **2026-09-04 addition:** Zalando's risk-based PR approval bot (33% of PRs auto-approved, built from their own incident analysis) is a second working risk-model example alongside Amplitude's - see the Zalando entry above.
4. **Is the four-way code-review bake-off the right shape of question?** None of these three organizations solved trust at the review layer. Spotify and Amplitude both invested in verification-before-PR first. "What does the agent get to verify before it opens a PR on this repo" may be the more consequential question. **2026-08-25 addition:** Ramp's ReviewBuddy (built on their in-house Inspect harness by one engineer in a week, reportedly beating third-party review tools on context-awareness) is a fifth data point for the bespoke side of this question - see the Pragmatic Engineer/Ramp entry above. **2026-09-02 addition:** the "bake-off" is arguably five-way, not four - Anthropic's own Claude Code Review (one of O1's original three candidates) has real self-reported numbers (16%→54% substantive-review coverage, <1% false-positive rate) that were never weighed against Gitar/Tessl in this workspace. Worth a deliberate decision on whether it's back in scope or was ruled out for a reason not captured here. **2026-09-07 addition:** Shopify's River security-remediation workflow (published 2026-09-02) is a sixth data point, and the sharpest yet - its whole design is revalidate-before-acting and verify-again-before-calling-it-done, with an explicit rule for what must live in code vs. what can stay a prompt. See the Shopify entry above.
5. **Should skill and `CLAUDE.md` changes be gated on an eval pass rate in CI?** Anthropic says yes explicitly. FN has DeepEval in ai-toolbox and evidence that skills change behavior (Mykhailo, 8/17) but no CI gate - and MOI-47 now force-pushes the marketplace to every engineer's machine, which raises the stakes.
6. **Does `repo-readiness` score consistency and verifiability independently?** Spotify's Part 4 shows a repo can be strong on one and useless on the other. Conflating them would mis-rank repos.
7. **What's the answer to "records, not files" at FN scale?** For `.ai-factory/`, committed skills, and any future `intent.md` chain. "Linear plus git history" is a defensible answer but should be a deliberate one.
8. **Is there a cultural track in the AI-DLC rollout?** Current rollout is technical-first - marketplace, telemetry, scorecards. Amplitude's strongest claim is that the technical half wasn't the hard part. **2026-09-08 addition:** GitLab's own internal AI-fluency playbook (hybrid governance: central hub + embedded "AI Transformation Owners" + function-level "AI champions," plus an "AI Literacy Ladder" self-assessment routing engineers to role-specific training) is the first concrete external template found for what a named cultural track could look like - see the GitLab entry above.
