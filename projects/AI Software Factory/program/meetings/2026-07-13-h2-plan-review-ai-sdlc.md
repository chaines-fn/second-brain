# H2 Plan Review | AI SDLC — 2026-07-13

**Date & Time:** Monday, July 13, 2026 CDT (~70 minutes)

**Attendees:**
- Cory Muir — Sr. Director, Software Engineering; initiative owner (kicked off the meeting, then stepped away partway through)
- Redwan Shahzada — Director of Engineering; presented the H2 plan/roadmap slides
- Chuck Haines — Sr. Principal Software Engineer; Architecture Lead, AI Software Factory workstream
- Joe Hoppesch — Principal Engineering Consultant (invited; no direct speaking lines in the transcript)
- Shane Kinder — EVP Product
- Travis Emslander — CTO
- Kelley Manuel — role not on file; inferred to be OKR/strategy-adjacent (raised the OKR framing question) — **please confirm title**
- Lisa Sacquitne — role not on file; referenced by Chuck as a product-side partner alongside Redwan; no speaking lines captured — **please confirm title**

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1bY0yBSZMHRfpz5v5_Yn7xXVQ3L1gjfcEYKwvk11lQBk/edit)

---

## Overview

This was a leadership review of the H2 plan for what's now being called AI SDLC (formerly AI Software Factory). Redwan opened by reframing the initiative: the Factory answers "how," but the bigger unresolved question is "why" — so the team is expanding scope from a single coding tool to a full AI-driven delivery loop (intake → plan → human gate → execute → verify → human gate → deploy). Chuck defended "AIDLC" as the term the industry (AWS in particular) is converging on, and after some pushback from Travis and Shane on naming and completeness, the group agreed the label is fine as long as the holistic vision — including product/design, not just engineering — gets spelled out clearly to stakeholders.

The most consequential shift in the meeting was strategic, not technical: the group backed away from "build the automated pipeline first" and moved toward "embed with teams, find their friction points, build the missing skills/agents, and let the factory fall out of that." Travis and Chuck both arrived at this independently mid-conversation — Chuck argued that 85% of the codebase lacks the curated, repo-specific skills that make tools like the LTE/Labor Flex workflow fast, and that fixing that is the actual velocity unlock, not an AWS production pipeline. Cory reinforced this by warning against retrofitting AI into existing Scrum process and pushing for a rethink of process end-to-end, using Labor Flex's ship-cadence as the existence proof — while also cautioning (via the Flightboard example) that speed without the right scope just grows the backlog.

Concrete outcomes: the team will target one or two pilot teams rather than rolling out across all 18 in H2, will formalize "PR merged via the factory by mid-August" as a Q3 OKR, will deprioritize the AWS production account build-out in favor of manually building skills/tooling for the top 10 most-committed repos, and will explicitly keep the automated-testing workstream separate but dependent. Resourcing risk was raised directly — Redwan flagged that neither he nor Chuck is fully dedicated to this, and pushed for single-threaded leadership in Q3. The Wednesday demo stays on the calendar regardless of the pivot.

---

## Key Notes

- **AIDLC naming confirmed, scope reframed as holistic:** Chuck's recommendation to use "AIDLC" (vs. Travis's initial read that it should stay "AI SDLC") was accepted, with the caveat from Shane that the framing needs to visibly include product/design phases, not just coding — the group agreed but noted no product/design lead was in the room to help define that piece yet.

- **Major strategic pivot — embed-first over pipeline-first:** Originally the plan was to build the AWS pipeline, then apply it across repos. By the end of the discussion, Chuck, Travis, and Cory converged on the opposite order: pause heavy pipeline build-out, embed with target teams to find why they can't move faster (missing skills, no reliable code-review agent, etc.), fix those specific gaps, and treat the "software factory" as what falls out of doing that repeatedly. Chuck was explicit that work already done (Sonar, architecture decisions) isn't wasted — it's still needed either way.

- **Scaling risk — pilot narrow, not broad:** Cory argued rolling AIDLC out across all 18 teams simultaneously in H2 is too risky and would cause gridlock (Shane's word). Agreed approach: pick one or two core teams, prove measurable velocity improvement (e.g., cycle time from 8 days to 3-4), then expand org-wide starting H1 next year.

- **Product velocity as the north star, with a caution:** Cory used the Flightboard project as a cautionary example — that team had the highest velocity and code coverage of any team, but kept missing scope (micro-interactions, requirements), so speed didn't translate to a good outcome. Shane and Cory agreed there's no perfect single metric, but the group needs to orient around business/product impact, not ticket throughput alone.

- **Automated testing flagged as a hard dependency, not yet merged as one initiative:** Shane pushed hard that none of this scales across the core product surface without automated testing coverage in place. Cory confirmed testing remains a separate workstream for now (open question on whether/how it converges with AIDLC later) — Chuck agreed the verification layer "falls apart" without it.

- **Resourcing / leadership risk raised directly:** Redwan flagged that he and Chuck are both pulled in other directions and not fully dedicated to this initiative, and asked for single-threaded ownership in Q3. Chuck confirmed he's already talking to Cory about getting pulled off Breakfix. Redwan also flagged that contractor capacity (6 people-sprints scaled to 18 for Q3) is an unproven assumption given how recently contractors started.

- **Mid-August OKR locked in conceptually:** Kelley proposed formalizing "a PR merged via the factory process by mid-August" as a Q3 objective; Chuck agreed, Cory directed the team to capture it (plus a Q4 follow-on metric like number of repos with a merged PR) in the OKR tracker.

- **Q3/Q4 plan reworked away from AWS infra buildout:** Travis proposed — and Chuck/Cory agreed — to deprioritize the AWS production-account build-out in favor of immediate impact: identify the top 10 most-committed-to repos (excluding innovation), manually run real backlog bugs through Claude/Copilot in each, find and fix what's missing (skills, code review), and iterate repo by repo. If a full AWS factory still gets built in Q4, this groundwork is directly reusable.

- **Sonar work still considered valuable:** Chuck reiterated that the Sonar verification-layer work isn't wasted under the new plan — still relevant to engineers' agentic development generally, not just the factory pipeline.

---

## Action Items

- **Chuck:** Send Travis the overall architecture diagram showing where Sonar plugs into the software factory / agentic development generally.
- **Chuck:** Follow up in writing on Travis's outstanding Sonar/architecture questions (targeted for today or tomorrow, 7/13–7/14).
- **The group:** Brainstorm a holistic AIDLC framework that explicitly covers product/design phases, not just engineering — Chuck suggested pulling in himself, Redwan, Cory, Shane, and product folks.
- **Chuck, Redwan:** Update the OKR spreadsheet/tracker with the mid-August "PR merged via factory" milestone and a Q4 follow-on metric.
- **The group:** Rework the Q3 plan to prioritize team-based enablement over AWS infrastructure build-out; define velocity/success metrics to track through implementation.
- **Chuck, Redwan, Joe:** Evaluate different team-enablement models (e.g., embedding in an integration/break-fix team vs. running a "paper cuts" initiative through a dedicated team) and bring back pros/cons for group discussion.
- **The group:** Proceed with the previously scheduled Wednesday (7/15) demo showing current progress — Cory confirmed no reason to hold it regardless of the plan rework.
