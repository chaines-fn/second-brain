# AI SF → AI SDLC H2 Vision and Q3 Plans — 2026-07-08

**Date & Time:** Wednesday, July 8, 2026 (time not specified in source; ~55 minutes)

**Attendees:**
- Cory Muir — Sr. Director, Software Engineering; initiative owner (meeting driver)
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead
- Joe Hoppesch — Principal Engineering Consultant
- Redwan Shahzada — Director of Engineering

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1BDkHN7OXlOFH-VALacWTmCbNVGakQcsduBLvnYdAXdk/edit)

---

## Overview

This was the core team's working session to define what "AI SDLC" means as a rebrand of the AI Software Factory effort, and to settle Q3 priorities. Cory framed the rebrand as a shift from "the how" (building a software factory) to "the why" (the outcome): the Northstar is compressing delivery cadence from months/quarters down to days/weeks. He pushed back on Travis's competing Northstar ("nobody writes or reviews a line of code") as beside the point — leadership doesn't care whether AI or humans write code, they care about velocity and features. Cory also cited a stat worth remembering: he estimates ~70% of engineering time currently goes to deciding/debating/documenting what to build, only 20-30% to actually building it, and ~10% to polish — meaning the real bottleneck is planning and requirements churn, not raw engineering throughput or a lack of AI tooling.

The group settled on two distinct but related workstreams: (1) the AI Software Factory itself — the technical build — and (2) the broader AI SDLC, which is the process transformation the factory serves. On pilot selection, they moved away from Breakfix (too many stakeholders/opinions, e.g. John) toward lower-visibility teams like Sapphire or Basalt, where they'd have more control to iterate without political friction. Cory was explicit that "vibe coding" should be reserved for genuinely experimental/high-risk work, not standard production — the factory's job is to produce correctly-integrated, production-ready output the first time.

On infrastructure, the team is leaning toward — and likely headed toward — AWS over GCP (Chuck's recommendation, largely to avoid platform-migration friction), using LangGraph with Bedrock Agent Core initially — with sandboxing benefits and a fallback path to ECS/EKS + Temporal if Agent Core doesn't hold up — and deliberately avoiding lock-in to a single model vendor (keeping both OpenAI and Anthropic in play). This isn't formally finalized yet, but Cory gave unambiguous "air cover" on the decision-making authority itself: engineering owns tool/build decisions, Cloud Ops (Ben) owns governance, billing, and security review, not tool selection. The team also agreed a dedicated new AWS account is needed, classified as OpEx (not COGS), separate from the existing dev sandbox. Resourcing came up directly: nobody has been dedicated to this full-time this quarter (Chuck's bandwidth in particular has been consumed by Breakfix), and Cory wants at least one FTE fully dedicated — likely meaning Chuck gets pulled off Breakfix, backfilled short-term by a contractor until a new principal engineer is hired. A demo for PEX leadership (Shane, Travis, possibly Ben/Kelly) is targeted for July 15, showing a Linear ticket run through the AWS/GCP-evaluated harness (code generation, not yet PR creation).

Note: this resolves an open question flagged in the [2026-07-07 Team Moissanite notes](2026-07-07-team-moissanite-weekly-cadence.md) about whether "AI DLC" was a rename or a superset of AI Software Factory — this meeting confirms it's the latter: AI Software Factory is a deliverable/tool inside the broader AI SDLC process-transformation effort, not being renamed away.

---

## Key Notes

- **Northstar reframed:** Cory's Northstar is compressing delivery cadence from months/quarters to days/weeks — not "AI writes all the code" (Travis's framing, which Cory and Chuck both rejected as beside the point). Leadership/board only cares about velocity and features, not who or what writes the code.
- **70/20-30/10 time split (Cory's estimate, not measured data):** ~70% of time spent deciding/debating/documenting what to build, 20-30% building, ~10% polishing. The bottleneck is planning and requirements churn, not engineering throughput — echoed by Chuck citing Breakfix as a live example of "build the wrong thing, then spend most of the time re-litigating it."
- **Vibe coding stance:** Cory explicitly does not want uncontrolled AI-generated ("vibe coded") output treated as production-ready. Vibe coding is reserved for high-risk/experimental exploration; the AI SDLC/factory should produce correctly-integrated, production-ready work directly, with guardrails letting product teams prototype safely before handoff.
- **Pilot team selection:** Breakfix ruled out as first pilot — too many stakeholders (PMs, John, others) who'd interfere with process changes. Sapphire or Basalt favored — full control over both product and engineering side, low visibility, room to iterate without political noise. Diamond mentioned as another low-visibility option.
- **Two-workstream framing (Chuck, confirmed by Cory):** (1) AI Software Factory — the technical build/tool, and (2) AI SDLC — the broader process transformation it serves. The factory is a deliverable in service of the SDLC outcome, not the outcome itself.
- **Platform direction — leaning AWS over GCP:** Chuck recommended AWS to avoid the friction/migration cost of GCP. The team is leaning this way and it's likely where they land, but it has not been formally finalized. Cory gave explicit, unambiguous authority to engineering on this decision regardless of which way it lands — Cloud Ops (Ben) owns governance/billing/security review, not build-tool selection. Cory flagged frustration with Ben pushing tool choices (e.g. Whiz) that aren't his call to make.
- **Architecture approach:** LangGraph on AWS, starting with Bedrock Agent Core (for sandboxing), with a contingency to move to ECS/EKS + Temporal if Agent Core fails to scale. Deliberately avoids full Anthropic-managed lock-in so both OpenAI and Anthropic models stay usable.
- **New AWS account:** Team agreed a new, separate production AWS account is needed for AI SDLC work, classified as OpEx rather than COGS. Current dev/sandbox account continues for iteration; promote to the new production account once ready. Chuck and Joe to raise with Ben and Ian.
- **Resourcing gap:** No one has been dedicated full-time this quarter; Chuck's Breakfix load in particular has eaten his bandwidth. Cory wants at least one FTE 100% dedicated to driving this — likely means pulling Chuck off Breakfix, with a contractor backfilling Breakfix short-term until a new principal engineer is hired for that area.
- **Contractor feedback:** Group is generally satisfied with Asad Ali, Vitalii Belinskyi, and the new hire (Mykhailo Sakhnik — Chuck is optimistic about his pace). Asad executes well with clear direction but moves slower without it, and tends to surface open questions without a recommended answer. New team expectation: contractors should pair any question with a proposed solution/recommendation, not just raise the question.
- **Milestones:** M1 — propose an updated SDLC process for Field Nation, circulate to senior leadership for feedback. M2 — identify a pilot team, execute and iterate, with the goal of onboarding additional teams by end of quarter.
- **Factory technical path:** Continue single-repo work (e.g., a bug fix) as the near-term proof point; in parallel, pursue a dual path of (a) cross-repository work and (b) integrating Triage's intake agent as the factory's first external-service intake.
- **Redwan's process point:** Rather than waiting for the SDLC/architecture to be fully designed before engaging a pilot team, start the pilot team's involvement immediately so their feedback shapes the process design in real time.
- **Demo planning:** Targeted for July 15 (Wednesday) for PEX leadership — will show a Linear ticket run end-to-end through the harness (AWS vs. GCP infra comparison, code generation as the evaluation metric — no PR creation yet). Shane and Travis are the primary audience; Cory suggested including Ben and Kelly if Ben is included.

## Action Items

- **Chuck Haines, Joe Hoppesch:** Finalize the AWS vs. GCP infrastructure decision (currently leaning AWS, not yet formally locked).
- **Chuck Haines, Joe Hoppesch:** Talk to Ben and Ian about standing up a new, separate AWS production account for AI SDLC, classified as OpEx (not COGS).
- **Chuck Haines:** Share the markdown research notes and architectural plan (AWS/GCP harness comparison) in the team channel.
- **Chuck Haines:** Present the demo to PEX leadership on July 15 — Linear ticket run through the harness, AWS vs. GCP evaluation.
- **Chuck Haines, Redwan Shahzada:** Define resourcing — get at least one FTE fully dedicated to AI SDLC; work out the Breakfix transition/backfill plan.
- **Chuck Haines, Redwan Shahzada:** Draft the updated SDLC process document for Field Nation and circulate to senior leadership for feedback.
- **Cory Muir:** Meet with Ben to align on roles/responsibilities — engineering owns build/tool decisions, Cloud Ops owns governance/billing/security.
- **The group:** Set the expectation with contractors that any question raised should come with a proposed recommendation attached.
- **Redwan Shahzada:** Schedule the July 15 demo; include Ben and Kelly if Ben is invited.
- **Redwan Shahzada (with Cory):** Coordinate a plan review with Cory before Tuesday, July 14, so he can give feedback ahead of the leadership demo.
