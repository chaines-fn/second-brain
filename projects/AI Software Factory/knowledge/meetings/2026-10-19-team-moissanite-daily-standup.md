# Team Moissanite Daily Standup, 2026-10-19

**Date & Time:** Monday, October 19, 2026, 8:15 CDT (~9 minutes per transcript)

**Attendees:**
- Chuck Haines: Sr. Principal Software Engineer, AI-DLC Workstream 1 lead (meeting organizer)
- Mykhailo Sakhnik: Contractor, Fai MVP builder (spoke)
- Joe Hoppesch, Redwan Shahzada, Asad Ali, Vitalii Belinskyi: invited per calendar; no speech in the transcript. Redwan declined the calendar invite.

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1uKEMZsZQmY961k4Mk6FrEmiWRVms_z1ftCG1khauRcg/edit)

---

## Overview

Short standup, mostly between Chuck and Mykhailo. The main thread was GitHub access for the Fai factory. Chuck accepted a permission request from the software factory GitHub App, which asked for additional content access beyond the read-only scope Mykhailo had originally requested. Mykhailo will retrigger the flow to confirm the permissions work, and he has updated the pull request and pushed fixes.

The second thread was model and gateway strategy. Mykhailo proposed an "open router" that picks the best model for each task, and he described a token-price monitoring queue from his personal projects that could pick the cheapest suitable model dynamically. Chuck said the team is evaluating AI gateways, including Workato, which Field Nation already uses in part. Chuck highlighted Workato's single-plane MCP gateway as a way to keep agents from loading every MCP server into memory at once. Mykhailo is writing up the router idea for discussion.

Mykhailo is still waiting on Joe for the Bedrock test harness. Chuck agreed to nudge Joe the same day.

---

## Key Notes

- **GitHub App permissions:** The software factory GitHub App requested additional permissions, including content access. Chuck approved the request during the call. Mykhailo will retrigger the process to verify. Source: [Fai MVP, MOI-169](https://linear.app/fieldnation/issue/MOI-169/build-event-driven-ai-software-factory-mvp-with-human-in-the-loop).

- **Open router for model selection:** Mykhailo proposed routing each task to an appropriate model, with a possible token-price monitor that favors the cheapest option. Chuck said the team is already looking at AI gateways, so the proposal is an input to that work, not a decision.

- **AI and MCP gateway evaluation (Workato):** Chuck said the team has some Workato product in house and is talking with Workato about its AI gateway and MCP gateway. The single-plane MCP gateway lets an agent request a tool and receive it on demand, which avoids loading all MCP servers into context. Chuck described this as the direction many companies are moving.

- **Bedrock test harness is blocked on Joe:** Mykhailo is waiting on Joe for the Bedrock test harness. Chuck will nudge Joe the same day. This is the dependency for the harness work under MOI-167 and MOI-168.

- **CI and ticket status:** Mykhailo said he has not checked the CI status for two tickets and has not received notifications about them. He will review open tickets.

---

## Action Items

- **Mykhailo Sakhnik:** Verify the updated GitHub permissions by retriggering the process and confirming it works.
- **Mykhailo Sakhnik:** Draft a detailed description of the open router concept for team discussion.
- **Chuck Haines:** Nudge Joe Hoppesch on the Bedrock test harness.
