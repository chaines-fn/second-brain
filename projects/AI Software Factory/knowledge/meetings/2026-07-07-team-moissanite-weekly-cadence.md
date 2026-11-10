# Team Moissanite: Weekly Cadence — 2026-07-07

**Date & Time:** Tuesday, July 7, 2026 (time not specified in source notes)

**Attendees:**
- Redwan Shahzada — Director of Engineering (meeting organizer)
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead
- Joe Hoppesch — Principal Engineering Consultant
- Cory Muir — Sr. Director, Software Engineering; initiative owner
- Asad Ali — Contractor, agent-hosting POC
- Vitalii Belinskyi — Contractor, ai-toolbox infrastructure
- Mykhailo Sakhnik — Contractor, joined 2026-07-06, focus TBD

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1y3MXmeew5H6b8X3BguDuB2Ledqp1jNhBsmdZSrMefz4/edit)

---

## Overview

Team Moissanite's weekly cadence covered three threads: onboarding new contractor Mykhailo Sakhnik, Q3 planning ahead of the SDLC transformation, and status updates on active POCs (Sonar, Guru, and Asad's agent-hosting/platform work).

Redwan welcomed Mykhailo to the team and confirmed his Linear board access, though he flagged a pending GitHub repo access issue. Mykhailo also raised a question about OS requirements — he runs Ubuntu Linux while some project docs reference Windows/macOS — Chuck clarified this isn't a blocker since nothing in the project is Mac-specific, and the team doesn't need VPN access.

On Q3 planning, the near-term goal is defining a "Q3 punch list" and aligning on transforming the SDLC into agent-first workflows; Chuck suggested aligning with Cory and Joe first before bringing it to leadership. Milestones discussed include moving the software factory from POC to production, standardizing the code review agent (currently GitHub Copilot), scaling the triage intake agent, and making the factory multi-repo aware. The team reaffirmed pausing Unblocked in favor of Guru (funding to be finalized with Cory), and discussed Sonar: the trial period itself lapsed, but the POC was successful — the team is now moving into contract negotiation, with Sonar's actual implementation/rollout targeted to start August 1. On infrastructure, Asad's agent-hosting POC (called the "Harness POC" in this meeting) remains the lead demo candidate, pending the final AWS-vs-GCP platform decision. Chuck also flagged the AI Toolbox's Kafka skill PR for review and committed to an updated architectural diagram unifying system components.

One open item: the notes mention "AI Software Factory rebranding to AI DLC" — it's genuinely unclear whether AI Software Factory becomes part of a broader AI-DLC workstream or is being renamed to it. Leaving this as an open question, not a settled decision.

---

## Key Notes

- **Mykhailo onboarding:** Welcomed by Redwan. Linear board access confirmed; GitHub repo access still pending. OS-requirement concern (Ubuntu vs. Windows/macOS docs) resolved — not a blocker, no VPN required.
- **Q3 planning / possible AI DLC rebrand:** Group wants a Q3 punch list, aligning on agent-first SDLC transformation. AI DLC relationship to AI Software Factory is unresolved — flagged, not decided. Chuck suggested a Cory/Joe alignment session before leadership.
- **Software factory milestones:** POC-to-production is the primary Q3 milestone. Also: standardizing the code review agent (GitHub Copilot), scaling the triage intake agent, multi-repo awareness.
- **Sonar:** Trial period lapsed, but the POC was successful. Moving into contract negotiation; implementation/rollout targeted to start August 1, 2026.
- **Guru over Unblocked:** Reaffirmed. Funding still to be finalized with Cory.
- **Demo/infra decision:** "Harness POC" = Asad's existing agent-hosting POC. Still pending the AWS-vs-GCP platform decision (AWS Agent Core vs. GCP Vertex Agent Engine, per Asad's existing comparison). Asad sharing the side-by-side Confluence doc.
- **AI Toolbox / Kafka skill PR:** Chuck to review — continuing the design-skill (schema registry) vs. implementation-skill (toolbox) distinction.
- **Architecture diagram:** Chuck committed to an updated diagram unifying components for team + leadership.

## Action Items

- **[The group]** Define the Q3 punch list — align on Q3 objectives for the AI Software Factory / SDLC transformation.
- **[Chuck]** Lead the demo presentation — run Asad's agent-hosting POC locally, manage the end-to-end session.
- **[Asad]** Share the Confluence doc — AWS vs. GCP side-by-side platform comparison.
- **[Chuck]** Review the AI Toolbox PR — comment on the Kafka skill pull request.
- **[Chuck]** Create an updated architectural diagram unifying system components.
- **[Mykhailo]** Explore the POC repository, download it, and practice running it locally (source said "PC repository" — likely the agent-hosting-poc repo, unconfirmed).
- **[Asad]** Answer the Linear approval question in Slack — whether plans post automatically to the issue tracker.
- **[Asad]** Share demo steps in Slack for running the POC.
