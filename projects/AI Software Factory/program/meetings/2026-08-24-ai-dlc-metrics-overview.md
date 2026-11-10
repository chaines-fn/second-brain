# AI-DLC Metrics Overview — 2026-08-24

**Date & Time:** Monday, August 24, 2026, CDT (start time not stated in the transcript; ~45 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer, AI-DLC (Software Factory workstream lead) — presented the metrics proposal
- Cory Muir — Sr. Director, Software Engineering; AI-DLC initiative owner; Chuck's manager
- Shane Kinder — EVP Product (left ~30 minutes in for another meeting)
- Travis Emslander — CTO

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1uY6G1_7062WQOXdfCaMFGwcjvJDUSQr4XjdbrMflWVA/edit?usp=meet_tnfm_email&tab=t.po01qei7nywx)

---

## Overview

Chuck walked the group through a custom DX-based dashboard he built to track pilot metrics for Emerald, the team on the engineering DBT (data platform) repository — the first of three Q3 pilot repos, alongside Core UI and FN Core. Shane opened by asking for a recap of team/repo selection and how representative these three are of the broader engineering org; Chuck explained the reasoning for each: Emerald/DBT for its data-platform priority, low tech debt, and well-scoped nature; Core UI as the UI-modernization standard and testbed for enhanced code review agents; FN Core last, as the hardest and highest-impact repo, where engineers currently distrust both agent-written code and GitHub Copilot's code review.

Most of the meeting centered on defining the primary performance metric — DX's "True Throughput" (a complexity-weighted PRs-per-engineer-per-week figure, designed to prevent gaming raw PR counts) — and setting a target for it. There was a live correction mid-meeting: Cory initially quoted industry benchmarks for raw PR count rather than True Throughput, then corrected to the real True Throughput benchmarks (P75 ≈ 2.0, P90 ≈ 2.6–2.7). Given Emerald's current trend of roughly 1.3 (versus a rough product-engineering average of 2.1), the group settled on a target of 3.5 — deliberately above the P90 benchmark, with Chuck flagging that hitting it depends on Emerald's backlog staying full enough to sustain a higher merge rate. The group also agreed to add raw PR count and issue cycle time as secondary/contextual metrics, and confirmed revert rate, PR cycle time, review comment density, and reviews-per-PR as quality counter-metrics to make sure throughput gains don't come at the cost of stability.

Two structural decisions came out of a discussion prompted by Cory: metrics will be tracked by repository (aggregated across whichever teams touch a migrated repo) rather than by team, since teams like Emerald already work across multiple repos and this keeps repo-level impact legible as scope expands to Core UI and FN Core. And, at Shane's request before he dropped off, Chuck will run a recurring 30-minute sync every two weeks (weekly if warranted) specifically to surface qualitative learnings that a high-level OKR tracker won't capture. Chuck also flagged — in the context of a conversation about human-vs-agent-created PRs — his continuing view that most current code review agents only report what's wrong with a PR, not what they checked and found fine, which he sees as the reason humans still have to review the whole PR and haven't built trust in the tooling yet.

---

## Key Notes

- **Q3 repo scope confirmed: Emerald (DBT), Core UI, FN Core.** Chuck plans ~10 repos across roughly two more teams by end of Q3, with full-engineering rollout targeted by end of year (Chuck thinks it'll land sooner). Shane specifically asked whether this set is representative of the org's mix of backend/data vs. front-end/feature teams; Chuck's answer was that Emerald covers data platform, Core UI covers UI/front-end, and FN Core covers the hardest backend case.

- **True Throughput target set to 3.5, and the benchmark numbers were corrected live.** Cory first cited P75/P90 figures that turned out to be for raw PR count, not True Throughput; the real True Throughput benchmarks (for companies with engineering teams under 100) are P75 ≈ 2.0 and P90 ≈ 2.6–2.7 PRs/week/engineer. Emerald currently sits around 1.3 (some sources in the conversation cited 0.9 as the trailing-4-week figure), well below the product-engineering average of ~2.1. The group picked 3.5 as a "strive high" goal — splitting the difference between the P90 benchmark and a more aggressive stretch Travis proposed — while flagging that Emerald's backlog needs to stay full enough to sustain that rate once the team gets faster.

- **True Throughput is a complexity-weighted metric, not a raw PR count** — DX applies an (undisclosed) complexity algorithm so that, e.g., one engineer merging a single large PR can register the same throughput as another merging four small ones. This is meant to prevent gaming the metric by splitting work into trivial PRs. A concrete example from the live dashboard: a README update carried a 0.25 complexity weight versus a 1.5 weight for a 9-commit, 35-file PR.

- **Raw PR count and issue cycle time added as secondary metrics.** Cory wants raw PR count alongside True Throughput because the normalized number is harder to explain to the board/SLT than a plain count. Travis wants issue cycle time (distinct from PR cycle time) specifically to catch pre-coding delay — the work that happens before code gets written — which he expects to be significant for Emerald.

- **Quality counter-metrics:** revert rate (expected to stay at/near zero — a rise indicates a production issue), PR cycle time (a rise suggests review bottlenecks or PRs arriving low-quality and needing rework), review comment density, and reviews-per-PR (a rise in either suggests PRs are needing more revision cycles before approval). The intent is to make sure throughput gains aren't coming at the expense of quality.

- **Tracking will be repository-based, not team-based.** Cory raised this because teams like Emerald already work across multiple repos, and as data-platform work spreads to other teams (Chuck noted Break/Fix teams are already starting to build data products into the DBT repo), team-level attribution would get muddy. The group agreed to measure by aggregate of migrated repositories instead, which also lets them compare migrated vs. non-migrated repos later and avoid one team's numbers skewing a shared repo's picture.

- **Biweekly deep-dive learnings sync established.** Shane's ask, made just before he left the call: the OKR tracker is too high-level to carry the qualitative learnings coming out of this work, so he wants a standing 30-minute sync every two weeks. Chuck agreed, noting he may need to bump it to weekly if the volume of learnings justifies it.

- **Agent-created PRs are effectively zero right now.** Cory asked about tracking the human-vs-bot PR creation ratio; Chuck said engineers (including himself) currently write/review code locally and create PRs by hand rather than letting an agent open the PR directly. Travis framed the desired end state as engineers managing AI the way a manager works with an engineer — clear instructions, verification questions — rather than writing code themselves. Chuck connected this to a broader trust gap: current code review agents typically report only what's wrong with a PR, not what they checked and found acceptable, so a human still has to review the whole thing and trust in the tooling doesn't build. He described a planned future metric, "human turn rate" (how many times a human has to reprompt or clarify with an agent per session), as a way to quantify that trust-building over time, though it isn't tracked yet.

- **Live dashboard walkthrough showed real volatility.** Trailing-4-week True Throughput for Emerald/DBT was reviewed live: a dip to roughly 0.4–0.6 during a planning week and Field Week, rebounding to 1.8–2.0 in the most recent week, with a 4-week rolling average of 0.9. Cory suggested this rolling 4-week window is the number that should get reported going forward.

- **FN Core's trust gap flagged as a known headwind.** Chuck reiterated that FN Core currently has roughly half the raw PR throughput of the DBT repo, and attributed this partly to engineers not trusting agent-generated code or GitHub Copilot's code review there — Copilot review has reportedly missed real issues on that repo. This reinforces why FN Core is being sequenced last and expected to need the most iteration.

---

## Action Items

- **Chuck Haines:** Set the True Throughput target to 3.5 in the dashboard/tracker for Emerald.
- **Chuck Haines:** Add raw PR count to the dashboard as a secondary metric alongside True Throughput.
- **Chuck Haines:** Add issue cycle time as a tracked metric on the dashboard.
- **Chuck Haines:** Schedule a recurring 30-minute biweekly sync with Shane (and likely Cory/Travis) to review learnings and dashboard metrics.
- **Chuck Haines:** Update the custom DX report to track a rolling 4-week True Throughput window for the engineering DBT (Emerald) repo.
- **Chuck Haines:** Report the aggregate True Throughput metric for migrated repositories weekly on the OKR tracker.
- **Chuck Haines (implied):** Confirm with DX whether the True Throughput calculation is affected by vacations/time off — raised as an open question by Cory but not resolved on the call.
