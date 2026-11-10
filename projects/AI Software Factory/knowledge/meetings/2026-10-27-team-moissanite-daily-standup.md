# Team Moissanite Daily Standup, 2026-10-27

**Date & Time:** Tuesday, October 27, 2026, 8:15 CDT (~9.5 minutes; transcript ends at 00:09:30)

**Attendees (per invite):**
- Chuck Haines: Sr. Principal Software Engineer, Workstream 1 lead (AI Software Factory); meeting organizer
- Mykhailo Sakhnik: Contractor, Factory engineer; the only other participant who spoke
- Joe Hoppesch: Principal Engineering Consultant, Workstream 2 lead; invited, did not speak in the transcript
- Redwan Shahzada: Director of Engineering; invited, did not speak in the transcript
- Asad Ali: Contractor; invited, did not speak in the transcript
- Vitalii Belinskyi: Contractor; invited, did not speak in the transcript

**Original Transcript:** [Team Moissanite: Daily Standup, 2026/10/27 (Gemini notes and transcript)](https://docs.google.com/document/d/1GDgLOFKpb2MRCRfuu9YXZJwWePjuPT4KbojWUx8KXOo/edit?usp=drivesdk)

---

## Overview

This was a short check-in between Chuck and Mykhailo on the Factory's Docker-based harness work. Mykhailo built a single Docker image for the harness (Python and NumPy installed) and asked whether the AI software factory workspace label was meant only for Moissanite. Chuck clarified that the long-term goal is for other teams and repositories to use the Factory, but that rollout will be staged, starting with the core UI repository (TypeScript/Node.js, newer codebase, expected to need little setup). Chuck then created a workspace-wide "AI software factory" label and merged it with the existing Moissanite label, so it is now global. Mykhailo raised that some backlog tickets contain only a single sentence, which is not enough for the Factory to run against; Chuck asked him to forward those tickets to him for more context.

The main news is a team transition. Mykhailo will begin working with the parl team on Monday (the transcript is ambiguous about who on that team reached out; Mykhailo said a colleague named Maria set up a chat with Sandro and Spencer and shared a Confluence page and a technical design review document). Chuck expects to take over some of Mykhailo's Factory work during the transition. Mykhailo is off Friday (approved), and he will send Chuck a Slack status download on Thursday before logging off. The Factory code will stay on a branch for now while it is being tested.

---

## Key Notes

- **Repository scope:** Chuck framed the Factory as eventually serving any repository and team, but not switched on everywhere at once. Core UI is the starting repo. The Factory code remains on a branch during testing; Chuck agreed that is fine until it is no longer needed on a branch.

- **Workspace-wide label:** Chuck created an "AI software factory" label at the workspace level and merged it with the existing Moissanite version. Mykhailo had seen a label ID mismatch when adding it to other projects through the UI, so the ID consistency is not yet confirmed. Mykhailo plans to test the global label on another project.

- **Ticket quality for the Factory:** Some backlog tickets have one-sentence descriptions. Chuck's guidance: if Mykhailo is unsure how to proceed or the ticket lacks information, send it to Chuck, who expects to be able to add the context.

- **Sonar investigation (open):** Mykhailo's next step is to research Sonar for both the planner and coder roles. He described his knowledge as theoretical so far. This continues the Sonar MCP integration Chuck proposed on 10/22.

- **Parl team transition:** Mykhailo starts with the parl team on Monday (Nov 2). He was asked to review the Confluence page and TDR shared by that team and fill gaps in the TDR documentation. Chuck confirmed the plan. Transition hand-off details are not yet defined in the transcript.

- **Handoff timing:** Mykhailo is off Friday, 10/30. Chuck asked for a Slack progress download before Mykhailo logs off Thursday 10/29, since Chuck will likely take over some of the in-flight work next week.

- **Carry-forward from 10/22 and 10/26 (no update in this meeting):** the Friday 10/23 demo to Cory has no recorded outcome; the Warp PoC was set to start 10/26; the LiteLLM advisory link is still open; the Sonar MCP integration is still open.

- **Transcript caveat:** Gemini notes and the transcript contain transcription errors, for example "Michael" for Mykhailo, "Good loot" and "AI-actory" for labels, and "parl team" (possibly a team name). Treat exact names and labels as needing confirmation.

---

## Action Items

- **Mykhailo Sakhnik:** Investigate Sonar for planner and coder use in the Factory workflow (from 10/22 and reaffirmed today).
- **Mykhailo Sakhnik:** Review the parl team's Confluence page and TDR, and address gaps in the TDR documentation.
- **Mykhailo Sakhnik:** Add the global "AI software factory" label to other projects to verify it behaves correctly.
- **Mykhailo Sakhnik:** Create a ticket to resolve the space issue that blocked a merge (merging requires a ticket).
- **Mykhailo Sakhnik:** Send Chuck a Slack status update on Factory work before logging off Thursday 10/29.
- **Mykhailo Sakhnik:** Finish formatting checks and the final project diagram.
- **Mykhailo Sakhnik:** Forward backlog tickets with insufficient descriptions to Chuck for added context.
- **Chuck Haines (implied):** Provide context on tickets Mykhailo forwards; likely take over some of Mykhailo's in-flight Factory work during the week of 11/2.
