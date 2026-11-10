# Team Moissanite: Weekly Cadence 1 — 2026-06-02

**Date & Time:** Tuesday, June 2, 2026, ~10:30 AM CDT (~35 minutes)

**Attendees:**
- **Chuck Haines** — Sr. Principal Software Engineer; Architecture Lead, AI Software Factory
- **Joe Hoppesch** — Principal Engineering Consultant; partner on the initiative
- **Redwan Shahzada** — Director of Engineering (Bangladesh team); ran the agenda
- **Asad Ali** — invited; *out today* (AI toolbox owner)
- **Cory Muir** — Sr. Director, Software Engineering; initiative owner. *Invited but not in attendance — referenced heavily throughout.*

**Original Transcript:** [Notes by Gemini](https://docs.google.com/document/d/1_VdUDVDGNobPT3SFrhmM885_8JU1_xzlWxHyProUt90/edit)

---

## Overview

The first weekly cadence for Team Moissanite covered logistics (operational guidelines, the June 4 AI Guild kickoff, OKR status) before turning into a substantive strategic debate about the *scope* of the AI Software Factory. Redwan ran the agenda; Asad was out. The headline tension: Joe wants to look at AI enablement holistically — arguing Field Nation has no centralized governance and everyone is "doing their own thing," which will become chaos — while Chuck holds that the Software Factory's charter is narrower: building production-ready features for the FN platform from specs, *not* solving the org-wide problem of letting product teams deploy prototypes and internal agents.

They didn't fully resolve the philosophical disagreement, but landed in a pragmatic place: they agree on the underlying need (shared governance, observability, deployment, evals, consolidated MCP connections) and agree it's *nobody's* job today — which is why both keep lobbying Cory for a "COO-type" owner. For the Factory specifically, they aligned that buying third-party tools (Unblocked, possibly Sonar) beats building, since Unblocked solves the cross-cutting context problem without internal maintenance burden.

The most concrete outcome was a sharpened POC plan: rather than comparing whole agent stacks abstractly, Joe will build the *same* use case — a planning agent connected to Linear — across platforms, separating the "agent stack" layer (Deep Agents / Microsoft Agent Framework / Google ADK) from the "operational platform" layer (Azure AI Foundry / Gemini Enterprise / managed Deep Agents) for an apples-to-apples comparison. They also aligned on a hosting mandate: agents default to one of three approved platforms (Gemini, Azure, or managed Deep Agents) unless there's a specific reason to go custom.

---

## Key Notes

- **Software Factory scope debate (the core tension):** Joe argued for a holistic view — FN lacks centralized governance for deploying agents and managing connections, and fragmentation across Pearl, the Factory, and the business ("the Ryans and Stephens") will create chaos. Chuck pushed back that the Factory's charter is specifically building production-ready platform features from specs; enabling product folks to deploy prototypes/vibe-coded things is a *separate* problem that should be solved by buying a tool, not built into the Factory. They converged: it's not the Factory's job, but it *is* a real org gap nobody owns.

- **Four distinct problem spaces (Chuck's framing):** (1) customer-facing agentic solutions → Pearl's team; (2) a software factory that builds production-ready platform features end-to-end → us; (3) enabling product folks to get prototypes deployed → buy a tool (e.g., revisit Replit); (4) internal-tooling agents → an enterprise solution like Glean/Guru. One tool won't solve all four; maybe one spans two.

- **Shared foundation is real (Joe's point Chuck conceded):** Auth, governance, observability, deployment, evals, consolidated MCP connections are the same building blocks regardless of purpose. The problem is everyone's duplicating them. Both want a dedicated "COO-type" owner to bring all AI efforts together; they've raised this with Cory repeatedly.

- **Buy over build — Unblocked:** Consensus that Unblocked effectively solves the cross-cutting context problem. Joe: "we write a check and it solves all these problems... even if we throw it out in a year, it's so much better than what we have." Chuck already sold. Unblocked solves *context*, not how agents spin up / sandbox / connect to repos — that's where the POC work and Deep Agents come in.

- **POC plan sharpened (most actionable decision):** Build the same scenario — a planning agent connected to Linear that writes a plan back — on each option. Separate two layers: **agent stack** (Deep Agents vs. MS Agent Framework vs. Google ADK) from **operational platform** (Azure AI Foundry vs. Gemini Enterprise vs. managed Deep Agents). Joe's been comparing Google ADK vs. MS Agent Framework via a rubric (posted in Slack); says they're "fundamentally the same." Chuck wants Deep Agents added — partly because its recent coding/long-running-agent improvements are compelling, and it parallels Pearl's Pydantic/LangChain work.

- **Hosting mandate aligned:** Agents run on one of three approved platforms — Gemini, Azure, or managed Deep Agents — by default, unless there's a specific reason to roll your own. Both less concerned about standardizing the *framework* (Pydantic/Mastra/etc.) than the *hosting* platform.

- **Headwinds flagged:** Chuck noted both MS and Google are "uphill battles" for adoption — Travis (BD) was "basically hell no on Microsoft." Worth selecting the right tool but eyes open on the political fight.

- **OKR status — trending red:** Chuck candidly owned being the bottleneck: BD travel, upcoming PTO, and an explosion of break-fix work ("every time I think I have something situated, something else comes up and I need to tell product why they're wrong"). Expects the next OKR review to read yellow-trending-red.

- **Governance is the real worry (Joe):** Echoing an AWS conference and the Microsoft Build talks — the message is "how not to go out of business with AI": use eval frameworks, get your data right, configure MCP servers to be accessed only when needed so context (and cost) doesn't balloon. Joe's fear: picking five different hosting platforms = a mess that sets FN back years.

- **Logistics:** Operational guidelines doc is out for review with placeholders to fill (purpose: document how the team collaborates with the AI Guild + define success criteria). AI Guild kickoff is **June 4** (calendar confusion over duplicate invites — same meeting, Redwan to verify attendee list). Joe closed the repo-selection / provider-match ticket — those two will be piloted. Sonar PC track has a timeline from Jack; prep call June 4.

- **Schedule:** Chuck is **out June 8–12**. Plan is to hit both Unblocked and Sonar hard the week of **June 15–19** when he's back, and make go/no-go calls then.

---

## Action Items

- **The group:** Review the operational guidelines doc and fill in empty/placeholder sections.
- **Redwan:** Verify the AI Guild kickoff (June 4) meeting details — resolve the duplicate-invite / attendee-list discrepancy.
- **The group:** Review Asad's PR (an M3 goal).
- **Joe:** Build the planning-agent POC (connected to Linear) comparing hosting platforms + integration — target a reviewable result the Monday Chuck is back (~June 15). Chuck is taking Sonar off Joe's plate to free him up.
- **Chuck:** Own third-party assets — drive the Unblocked evaluation/implementation, and keep the AI toolbox balls moving while Asad continues that work.
- **Asad:** Continue AI toolbox efforts.
- **Chuck & Joe (implied):** Reconvene week of June 15–19 to review the POC and Unblocked/Sonar and make go/no-go decisions.
