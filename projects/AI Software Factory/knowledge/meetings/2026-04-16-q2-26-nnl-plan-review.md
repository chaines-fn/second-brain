# AI Software Factory | Q2 '26 NNL Plan Review — 2026-04-16

**Date & Time:** Thursday, April 16, 2026, 9:55 AM CDT (~57 minutes)

**Attendees:**
- Cory Muir — Sr. Director, Software Engineering (initiative owner; kicked off the meeting)
- Redwan Shahzada — Director of Engineering, Bangladesh team (presenter)
- Chuck Haines — Sr. Principal Software Engineer / Architecture Lead (co-presenter)
- Joe Hoppesch — Principal Engineering Consultant
- Ben Moormeier — Director of Information Security
- Sandro Perez — Information Security Manager
- Stephen Wald — Product Manager (asked most of the probing "what is it / what are we building" questions)
- Shane Kinder — EVP Product
- Ann-Bettina Colace ("ABC") — Product Design Lead
- Travis Emslander — Invited; joined late or intermittently
- Lisa Sacquitne — Invited
- Maria Saxton — Invited

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1cz-K1-WVrIfQsxaZFWlq2BnSblr8uL8SLmy58NW1qOU/edit?usp=meet_tnfm_email)

---

## Overview

This was the Q2 '26 NNL plan review for the AI Software Factory initiative — the first time the broader stakeholder group saw the plan Chuck, Joe, and Redwan had been developing. Cory opened by noting the initiative came up mid-quarter without a proper strategy-setting phase, so the goal was to walk through the high-level plan, northstar direction, and operating model, and surface cross-cutting considerations (design/product inputs, security, human-in-the-loop policy, SOC 2 Type 2 compliance). Redwan drove the deck; Chuck drove the technical architecture discussion.

Two things landed clearly. First, the scope: this is an internal engineering enablement tool for reducing toil (bugs, debugging, build/deploy wait time), explicitly **not** a customer-facing platform — that's the Pearl team's charter. Second, the Phase 1 target: take a single-repository bug from Linear to PR through a Plan-Execute-Verify (PEV) loop, with human-in-the-loop at plan approval and PR review. Chuck framed this as a learning exercise — each failure point tells the team where the harness needs to improve.

The meeting ran long and got into real substance. Stephen (PM) pushed back on whether the devil's advocate/adversarial agent was premature engineering; Chuck defended it with industry research (90% of outcome quality comes from plan quality) and the feed-forward vs. feedback distinction from Martin Fowler. Ben (InfoSec) pressed on where this will run — AWS Bedrock / Agent Core vs. Azure vs. managed third parties — from a security and compliance posture lens, and planted the seed that a safe path to remove human-in-the-loop needs to be designed deliberately so SOC 2 Type 2 isn't violated as the team scales back review gates. Chuck's answer on platform: "we don't know yet, we'll spike it by end of April and prefer off-the-shelf." Cory closed by pushing for concrete next steps: a RACI, the right people (not a 20-person committee), and a parallel track for local-dev considerations. Stephen's most actionable ask: pick a real bug ticket now so everyone can see what "done by end of quarter" actually looks like.

---

## Key Notes

- **Scope clarified — internal tool, not a platform:** Ben flagged an inconsistency in the deck ("not a platform" vs. language about the Field Nation platform). Chuck and Joe clarified: this is an internal engineering enablement tool for toil reduction and bug fixing. It is explicitly distinct from the Pearl team's work on customer-facing agentic experiences. Nothing built here is for expediting feature development on the FN customer platform — though it may accelerate *how* engineers do that work.

- **Phase 1 target — single-repo bug, Linear to PR:** The end-of-quarter success criterion is taking a specifically-formatted bug ticket in Linear, running it through the PEV pipeline, and producing a reviewable PR — all in a single repository. Chuck is deliberately excluding cross-repo scope and UI testing from v1 to keep the harness provable. Target repo is TBD but likely a Go or Node.js microservice (notifications mentioned as a candidate; integrations mentioned but has sprawl across repos). Stephen pushed hard on picking a *specific* bug now so the group has a concrete reference point.

- **PEV loop with two human-in-the-loop gates:** Plan (agent-authored, devil's-advocate-reviewed, human-approved) → Execute + Verify (agent-led) → PR (human-approved). Chuck: "agents executing, humans authorizing." The two human gates exist deliberately for this phase as the primary learning signal — rejections tell the team where the harness is failing so they can improve it. Ben planted the seed that the team needs a safe path to eventually remove human review, or engineers like Chuck will be buried in code review; Chuck agreed and said he has ideas based on industry direction for an offline discussion.

- **Devil's advocate defended as non-negotiable:** Stephen questioned whether the adversarial review agent was premature — maybe better models in a month make it unnecessary. Chuck pushed back firmly: industry research is unambiguous that multimodal adversarial review improves first-time-right results (different models trained differently see different things). He cited the feed-forward vs. feedback frame from Martin Fowler — feed-forward is everything you pre-load to prevent errors, feedback catches what slips through despite that. 90% of outcome quality comes from plan quality, which is why most companies keep humans in the loop at the plan stage even with fully agentic pipelines downstream.

- **Observability and eval framework are hard requirements:** Stephen asked how failures will be captured and learned from. Chuck's answer: a strong eval framework constantly testing prompt/model changes, plus full lifecycle telemetry (OpenTelemetry-style) baked into whatever platform is chosen. If they go with something like Langmith's deep agents, that observability is built in. Non-negotiable for platform selection.

- **Verify step — what it looks like is still open:** Chuck was candid that verify-step specifics are TBD (the team has had about a week on this). Ideal components: security review, acceptance criteria check, engineering best practices, linting, unit tests — all agent-driven, plus deterministic checks. For UI work, open question whether to spin up an ephemeral QA env with Playwright or simulate. Ben suggested capturing screenshots/video as verification artifacts. This whole step is expected to be figured out during phase zero architecture.

- **Platform choice — TBD, prefer off-the-shelf:** Ben (from a security / compliance posture lens) pushed on where this is going to run — AWS Bedrock / Agent Core vs. Azure vs. managed third parties like Claude or Langmith deep agents — and wanted Ian involved in the spikes. Chuck's position: don't rule anything out yet; all options on the table. Goal is to spike and decide by end of April. Strong preference for off-the-shelf components ("people way smarter than us have already figured a lot of this out") glued together, even if we run on our own infra. Ben agreed Ian should be in the spike discussions.

- **One investment, two delivery channels:** Chuck emphasized this is a strategic win beyond the factory itself. ~80% of what they build (skills, context, best practices, code reviews, repo AI enablement) is immediately usable by engineers using AI locally in the same codebases. The factory isn't a walled garden — the infrastructure the factory needs is the same infrastructure that makes every engineer using AI faster.

- **Research agent may be needed earlier than expected:** Cory asked if the monorepo UI decision complicates things (back-end + front-end in one repo means bug tickets may cross that boundary). Chuck acknowledged a research/routing agent may need to go in front of `plan` to split tickets into distinct back-end and front-end work items to avoid context rot across agents. Not in Phase 1, but on the radar.

- **The "agent vs. Mike" draw — future policy question:** Ben recounted a recent incident where an OPCop agent and Mike Mueller debated whether an issue needed fixing and landed at a draw (both agreed it was a moot point, but the agent was hellbent). Raised the open question of when/whether the org will trust the agent's judgment over a human's. Also raised the principle: "does the fix do harm?" — the recommendation mattered less than whether acting on it would make things worse.

- **Cory's close — stop debating implementation, get organized:** Cory's push at the end: the harness concept is 6 months old and evolving rapidly; don't spend 3 months debating implementation of a box that'll be replaced. Align on *what is non-negotiable* (is this the right flow? what's absolutely critical in the first slice?) and move. Avoid a 20-person committee. Set up a RACI so it's clear who's R/A/C/I and move fast. Consider bringing in a security consultant (as they're bringing in contractor engineers).

- **Parallel track — local dev is still wild west:** Joe (or Cory) flagged that even as the team races on local-development-based prototyping, there's a parallel thread about where the factory runs long-term and how engineers contribute to it from their own machines without having to deploy somewhere. This needs to be part of phase zero planning, not deferred.

---

## Action Items

- **Redwan Shahzada:** Schedule a RACI meeting to organize the right personnel for the initiative — confirm who is R/A/C/I on each workstream. Include Ian from infrastructure.
- **Chuck Haines:** Select a specific target bug from the backlog that meets the single-repo criteria (likely a Go or Node.js microservice) and format the ticket for factory processing. Pull a concrete example the group can all point to as "this is what success looks like by end of quarter."
- **Chuck Haines (implied):** Follow up with Ben offline on the safe path to removing human-in-the-loop over time — Chuck said he has ideas from industry direction.
- **Chuck Haines (implied):** Lead phase zero — produce a high-level architecture for infra, harness, and the flow through a mini-TDR process (Ben's group + principal engineers as reviewers, not the full org). Target end of April.
- **Ben Moormeier (implied):** Get Ian involved in the platform/infra spikes (Bedrock, Agent Core, Langmith, Claude managed agents, etc.).
- **Cory Muir (implied):** Evaluate whether to bring in a security consultant alongside the contractor engineers already being sourced.
- **Group (implied):** Share the Martin Fowler article on feed-forward vs. feedback that Chuck referenced.
