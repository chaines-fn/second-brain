# Team Moissanite: Daily Standup — 2026-09-16

**Date & Time:** Tuesday, September 16, 2026, 08:15–08:30 CDT (15 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer, Workstream 1 Lead (organizer)
- Joe Hoppesch — Principal Engineering Consultant, Workstream 2 Lead
- Asad Ali — Contractor (ADR skills, agent-hosting)
- Vitalii Belinskyi — Contractor (ai-toolbox infrastructure)
- Mykhailo Sakhnik — Contractor (repo-readiness, harness)
- Redwan Shahzada — Director of Engineering (declined, rolled off 2026-07-14)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1avyN_QgborglTFZ2qUYowxxgXhbwK9_q_1kzVT9HFu8/edit)

---

## Overview

The standup focused on three concrete work streams: repo bootstrap tooling optimization with token-cost reduction, ADR (Architecture Decision Record) skill consolidation strategy, and packaging/distribution readiness for the emerging tool ecosystem. The team discussed whether three separate ADR skills (create, enforce, review) should be consolidated, ultimately deciding to keep them separate given their distinct firing moments and the current AI toolbox architecture constraint that prevents skill-to-skill calling. Packaging of the repo bootstrap and repo scorecard tools as downloadable CLI applications emerged as the next priority, alongside a broader documentation effort to surface all required developer tools. The meeting also surfaced a planned personnel transition: Vitalii will shift off the Moissanite project over the next few days, focusing his final effort on closing the repo bootstrap and Harness integration work.

---

## Key Notes

- **Repo Bootstrap Tool Expansion and Token Cost Reduction**: Chuck presented Vitalii's PR expanding the repo bootstrap tool with deterministic scripts to reduce LLM token costs. The goal is to extract logic that doesn't require an LLM (e.g., file scanning, pattern detection) into standalone scripts, keeping the LLM work for interpretation and planning. Chuck plans to incorporate his own prompt into the bootstrap tool after Vitalii's PR is merged and reviewed, with this becoming a future step after the packaging work is complete.

- **ADR Skills: Consolidation Debate and Decision**: Asad and Mykhailo spent 45 minutes discussing whether the three existing ADR skills (create, enforce, review) should be consolidated into one or two. Asad explained the reasoning: they fire at different operational moments — create fires when a decision is being made, enforce fires when a change is described but not yet written, and review fires when a record review threshold passes. Current AI toolbox architecture prevents a skill from calling another skill, which is a structural reason for keeping them separate. Chuck confirmed that maintaining three distinct skills is acceptable — he views this as a feature, not a problem: each skill serves a distinct purpose and the constraint validates the design. He asked for Mykhailo's architectural diagram to be reviewed and pushed to the toolbox for code review and rollout.

- **AI Repo Scorecard Progress**: Mykhailo reported verifying tickets for adding Gitar to the scorecard and raising the AI repo score from 97 to 100 by factoring in main branch commit history. This scorecard is a central component of the O1 repository readiness KR and is moving toward production readiness.

- **Tool Packaging and CLI Distribution Strategy**: Chuck outlined a packaging initiative to make the repo bootstrap and repo scorecard "just downloadable and runnable" — moving away from requiring repo clones. He proposed GitHub Actions releases as the distribution mechanism, or potentially a private Homebrew tap, with clear installation instructions for end users. This is the priority work after Vitalii's PR is reviewed. Vitalii is asked to scope what this might look like and create tickets for the packaging work.

- **Setup Documentation Initiative**: Chuck proposed creating a Confluence page documenting all required developer tools — OpenSpec, Sonar MCP, AI tools, etc. — with explanations of their roles and how they work together. This became necessary as the agentic tooling ecosystem expands and will be the reference sheet for rolling out the experience to teams. Chuck will own creating a ticket for this work.

- **Harness CI/CD Integration Blocker**: Vitalii flagged that configuring Harness create-trigger for toolbox pipelines requires admin-role access at Harness, which he doesn't currently have. This is blocking MOI-48 work and needs a Harness administrator to unblock.

- **Personnel Transition — Vitalii Rolling Off**: Chuck announced that Vitalii will transition off the Moissanite project over the next few days to another assignment. Mykhailo requested a handover call with Vitalii to review and understand his recent work contributions. Chuck directed Vitalii to focus exclusively on closing out the repo bootstrap and Harness work without taking on new tasks, giving a clear scope boundary for the transition.

- **Claude Analytics Tool for Skill Gap Identification**: Chuck mentioned that Claude recently rolled out an insights analytics tool capable of generating smart reports on skill-gap opportunities across the organization. He identified 3–4 skill-gap opportunities from initial runs that will likely become new skills, pointing to a useful signal source for future Factory work.

---

## Decisions Made

- **Keep the three ADR skills separate**: Create, enforce, and review will remain distinct skills given their different firing moments (decision time, write time, review threshold) and the current toolbox architecture constraint. Each serves a clear purpose.

- **Next priority is tool packaging**: After Vitalii's repo bootstrap PR is reviewed and merged, the focus shifts to packaging both repo bootstrap and repo scorecard as downloadable CLI tools (via GitHub releases or similar).

- **Vitalii transitions off project**: Following final delivery of repo bootstrap and Harness work, expected end of week.

---

## Action Items

- **[Mykhailo Sakhnik]** Review Pull Requests: Review Asad's ADR skill PR and Vitalii's repo bootstrap PR for correctness and readiness for merge. Also review and push Mykhailo's own ADR architecture diagram to the toolbox for code review.

- **[Asad Ali]** Investigate Skill Configuration: Investigate and document the AI toolbox configuration that prevents skill-to-skill calling; report findings to the team on whether it's a configuration, architecture, or design decision.

- **[Asad Ali]** Refresh Pull Request: Clean up boilerplate code from GitHub Copilot in the eval skill PR and refresh it for Chuck's review.

- **[Chuck Haines]** Create Tool Documentation: Create a Confluence page documenting all required tools (OpenSpec, Sonar MCP, AI tools, etc.) and their setup instructions, explaining how they work together.

- **[Vitalii Belinskyi]** Complete Harness Work: Finish the Harness 911 ticket using the newly created spec from the updated STDD call, focusing exclusively on this and repo bootstrap closure without taking on new work.

- **[Vitalii Belinskyi]** Schedule Handover Call: Schedule a demonstration call with Mykhailo to review and explain recent work contributions before transitioning off.

- **[Chuck Haines]** Share Prompt: Send the most recent prompt to Vitalii for incorporation into the repo bootstrap tool (will be integrated after packaging is complete).

- **[Chuck Haines]** Create Deployment Tickets: Create tickets for packaging the repo bootstrap and repo scorecard as easy-to-install CLI tools, detailing the distribution mechanism (GitHub releases, Homebrew, etc.).

- **[Vitalii Belinskyi]** Obtain Harness Admin Access: Coordinate with Harness administrator to get the necessary admin role for configuring toolbox create triggers for pipelines.

---

**Next standup:** 2026-09-17, same time and place.
