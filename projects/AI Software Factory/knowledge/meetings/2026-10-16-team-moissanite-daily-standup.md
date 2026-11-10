# Team Moissanite Daily Standup, 2026-10-16

**Date & Time:** Friday, October 16, 2026, 11:27 CDT (~32 minutes per transcript; rescheduled from the usual 8:15 CDT slot)

**Attendees:**
- Chuck Haines: Sr. Principal Software Engineer, AI-DLC Workstream 1 lead (meeting organizer)
- Mykhailo Sakhnik: Contractor, Fai MVP builder
- Joe Hoppesch, Redwan Shahzada, Asad Ali, Vitalii Belinskyi: invited per calendar; no speech in the transcript
- Maria: mentioned by Chuck as co-lead of the customer-facing project onboarding; last name and title not in the workspace

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/12U947s9n3OWwaLI_lZKqNkdm6sqWAC8-KF8TgWEuf-8/edit)

---

## Overview

Mykhailo ran a live end-to-end demo of the Fai event-driven MVP (MOI-169) on a real Linear ticket. A ticket labeled `ai-factory` flows from a Linear webhook through API Gateway and Lambda into an AWS Step Functions workflow. The workflow selects a planner, validates the workspace in the AgentCore Harness (repo cloned once and persisted for up to 14 days), generates an implementation plan, and waits for human approval in a React UI. After approval, coding and verification run in the harness, state is held in DynamoDB across the wait, and a draft PR is opened. The demo produced a PR. Chuck called the architecture the right direction and asked for the architecture diagrams. Mykhailo's first iteration consumed about 80,000 tokens, so planning is now capped at 16,000 tokens and uses Haiku for the loop.

The meeting also surfaced two open design questions. The first is GitHub authentication: the demo used a personal token, which Mykhailo acknowledged is unsafe, and the team agreed to use the new FN software factory GitHub App instead. The second is repo selection: the system currently supports only one AI repo because it depends on prebuilt Docker images, and Chuck named automatic repo determination as one of the hardest problems in software factories, tying it to ongoing build-versus-buy talks with Warp.

Near the end, Chuck announced a staffing change. Senior leadership prioritized a customer-facing agentic solutions project with a year-end release target. Mykhailo was asked to transition full-time to it for one to two months starting the following week, then return to the software factory. Chuck framed this as a priority call he could not overturn.

---

## Key Notes

- **Fai MVP demo worked end to end on a real ticket:** Ticket labeled `ai-factory` → Linear webhook (added by Chuck) → Lambda → Step Functions (select planner, validate workspace, plan, human approval, coding, verify, draft PR). The PR-opening step ran, and Chuck received the completion notification. Mykhailo noted a review-related issue he still needed to look into. Source: [MOI-169](https://linear.app/fieldnation/issue/MOI-169/build-event-driven-ai-software-factory-mvp-with-human-in-the-loop).

- **Token cost controls:** The first iteration used about 80,000 tokens. Mykhailo capped planning at 16,000 tokens and switched the loop to Haiku. He also noted runaway token burn from recursion loops during development.

- **Serverless over Temporal (for now):** Mykhailo chose Step Functions plus Lambda to avoid always-on cost. He would still like to try Temporal later because workflows live in code and are easier to version. Chuck accepted the POC as built in a few days.

- **GitHub authentication moves to the GitHub App:** The demo used a personal token. Chuck created the "FN software factory" GitHub App, granted Mykhailo access, and will issue client credentials for short-lived tokens. Both parties agreed this replaces personal tokens.

- **Repo selection is the hard problem:** The system needed a `factory context` field on the ticket so it knows which repo to work in. Chuck said every software factory he has seen requires the repo to be specified, and he wants the factory to determine it. Mykhailo said a library or service that resolves the repo is a future option.

- **One-repo limitation:** The harness needs prebuilt Docker images with uv configured, one for Python and one for numpy. Until those exist, only one AI repo is supported. Mykhailo said the harness is simple to extend this way and does not require ECS or Fargate.

- **Infrastructure as code:** All configuration is in Terraform. Mykhailo plans to push it as a PR, to serve as an example for future factories.

- **Harness behavior issues:** The harness eval tried to install packages it was not authorized to install, and a deterministic check blocked the operation. Mykhailo also added the ticket context field because of this.

- **Access issue:** Chuck could not complete Cognito sign-in because the temporary password email was lost. He recreated his user and was added to the approvers group.

- **Staffing (priority shift):** Senior leadership asked for a customer-facing agentic solutions project to ship by year-end. The stack includes Pydantic and Temporal. Mykhailo transitions full-time for one to two months from the week of October 19, then returns to the factory. Chuck, Maria, and Mykhailo will hold an onboarding meeting on Wednesday or Thursday of that week. Chuck said he "tried to fight to keep" Mykhailo on the factory.

- **Direction confirmed:** The team aligned that the POC architecture direction is correct and that the GitHub App should replace personal tokens.

---

## Action Items

- **Mykhailo Sakhnik:** Study GitHub repository cloning and token requirements (GitHub App credentials replace the personal token).
- **Mykhailo Sakhnik:** Push the Terraform changes to the repo as a PR.
- **Mykhailo Sakhnik:** Share the high-level architecture diagrams and flow descriptions with Chuck.
- **Mykhailo Sakhnik:** Investigate factory improvements and draft development tickets for the next phase.
- **Chuck Haines:** Share the recording of the AgentCore meeting with AWS.
- **Chuck Haines and Maria (implied):** Schedule the onboarding meeting for the customer-facing project for the week of October 19 (Wednesday or Thursday).
