# Team Moissanite Daily Standup, 2026-10-22

**Date & Time:** Thursday, October 22, 2026, 8:13 CDT (~20 minutes; the transcript ends at 00:20:32)

**Attendees:**
- Chuck Haines: Sr. Principal Software Engineer, Workstream 1 lead (AI Software Factory); meeting organizer
- Mykhailo Sakhnik: Contractor, Factory engineer (MOI-169 owner); the only other participant who spoke
- Joe Hoppesch: Principal Engineering Consultant, Workstream 2 lead; invited, did not speak in the transcript
- Redwan Shahzada: Director of Engineering; rolled off AI-DLC 2026-07-14, still on the standup invite, did not speak
- Asad Ali: Contractor; invited, did not speak in the transcript
- Vitalii Belinskyi: Contractor; transitioned off the project 2026-09-16, still on the invite, did not speak

**Original Transcript:** [Team Moissanite: Daily Standup, 2026/10/22 (Gemini notes and transcript)](https://docs.google.com/document/d/1EXt4dUwkNnlZavIMdZY1y5sHuAS9H9Z6_a3rpn4KTdg/edit?usp=drivesdk)

---

## Overview

This was a short working standup in which Mykhailo demonstrated the Factory's planner and execution UI to Chuck for the first time in detail. The planner shows run overview, execution progress, a live trace of Step Functions and agent tool calls, and an agent metrics panel with model and token counts. Mykhailo flagged a token accounting problem: output tokens are reported far below input tokens (about 13x less), which he believes is a coder-side bug to fix. Chuck called the demo "really cool" and set direction for the next week.

Chuck designated the core UI repository (a Node-based React TypeScript monorepo) as the Factory's primary test ground, on the view that UI work is the easiest low-hanging fruit for agents. Mykhailo will pull unassigned tickets from the core UI backlog in the Obsidian Linear team. Chuck also proposed wiring in the Sonar MCP server so repo Sonar rules feed forward into agent runs. Mykhailo agreed to investigate.

Chuck set three goals before a Friday (2026-10-23) demo to Cory Muir: integrate an MCP server, complete one core UI backlog ticket through the Factory, and get a baseline cost-per-run number. Chuck said he is not optimizing cost yet, only establishing a baseline. A Warp proof of concept kicks off Monday (2026-10-26) to compare against the Factory. Mykhailo also raised a supply-chain security concern about a LiteLLM package (transcribed as "light LLM" and, in Gemini's notes, "Light Learning Library"; the exact package name needs confirmation). He said it was removed from the harness migration PR but still exists in a development environment. Chuck asked for the advisory link.

---

## Key Notes

- **Planner and execution UI:** Mykhailo showed a run view with a human approval gate (approve starts the coder), an agent metrics panel (model, input tokens, output tokens), and a diagnostics panel that traces Step Functions state and which tools each agent calls. He said AgentCore provides most of this telemetry. Per-run token metrics and traces are now visible in the UI, which Chuck had not seen before.

- **Token accounting bug:** Output tokens appear roughly 13x lower than input tokens in the metrics panel, which Mykhailo judged implausible. He believes the fix belongs in the coder agent. Treat the token numbers as unreliable until this is fixed. This affects the cost-per-run baseline Chuck wants by Friday.

- **Repository scope:** The Factory currently works only on Python repositories because the Docker images cover Python only. Node support requires new images. Repo selection is still manual: the user tells the agent which repo to work in. Core UI is the chosen first Node target.

- **Core UI as testing ground:** Chuck described the core UI repo as a Node/React/TypeScript monorepo and "the easiest low-hanging fruit" for agents. He will tell the wider team that Factory PRs are test activity, so people can ignore them as they arrive. Mykhailo was added to the Obsidian team in Linear for backlog access.

- **Sonar MCP integration:** Chuck proposed connecting the Sonar MCP server (Sonar Vortex) so the Factory receives repo Sonar rules as feed-forward context, making PRs pass the gate before review. He called it a good test of adding an MCP server to the Factory. Mykhailo agreed to investigate and said Sonar access comes through his existing Sonar account. A simple draft PR already showed Sonar green. Related: Sonar MCP is an existing Joe ticket (MOI-78); this would be a Factory-side consumer.

- **LiteLLM supply-chain concern (security):** Mykhailo reported that a LiteLLM package was compromised with injected malicious code, and that the risk applies on install, not only on import. He said the package was removed from the harness migration (the Deep Agents to Harness eval work) but still lives in a development environment, referenced by the skill creator. He recommended removing it wherever it still appears. Chuck asked for the link to read the details. The affected versions, the timeline, and whether any Field Nation environment was exposed are unknown from the meeting. Confirm before drawing conclusions.

- **PHP security context:** Chuck said the team spent the previous day on a PHP security issue, getting a patch out, and that the week had been long. No details were discussed in the standup.

- **Friday demo for Cory:** Chuck plans to invite Cory Muir to the Friday meeting for a live Factory demo by Mykhailo. The three pre-demo targets were the MCP integration, a core UI backlog ticket completed by the Factory, and a cost-per-run figure. Chuck is not concerned about the cost level yet, only having a number to start from.

- **Cost-per-run baseline:** Chuck said early costs will be high, as Warp's team also warned, and the goal is to measure first and optimize later. Mykhailo said the planner can show expected token spend per model, which would support human-in-the-loop estimates. Track cost per PR run (Mykhailo's ticket) as the baseline.

- **Warp comparison:** A Warp proof of concept starts Monday, 2026-10-26. Chuck described Warp as infrastructure-as-code that builds the Factory from YAML, so the team can compare it directly with the AgentCore-based Factory. Outcome of the Warp evaluation will feed the infra choice noted in the 10/13 and 10/16 standups.

- **Mykhailo's transition:** Chuck said Mykhailo will move to the customer-facing agentic solutions team around 2026-11-02 for one to two months, then return to the Factory. This slightly changes the earlier note, which said the week of 10/19. The customer team works on AWS with Bedrock, Pydantic AI, RAG, and Temporal for orchestration. Mykhailo asked whether they use OpenSearch or a vector DB. Chuck does not know yet and will ask the team for their stack.

---

## Action Items

- **Mykhailo Sakhnik:** Send the LiteLLM security advisory link to Chuck (explicit).
- **Mykhailo Sakhnik:** Demo the Factory to Cory Muir at the Friday meeting (explicit).
- **Chuck Haines:** Invite Cory Muir to the Friday meeting (explicit).
- **Mykhailo Sakhnik:** Integrate an MCP server and the Sonar MCP server into the Factory (explicit).
- **Mykhailo Sakhnik:** Pick up an unassigned core UI backlog ticket and complete it through the Factory (explicit).
- **Mykhailo Sakhnik:** Implement tracking of total cost per PR run (explicit).
- **Chuck Haines:** Get the technical stack from the customer-facing agentic team and share it with Mykhailo (explicit).
- **Chuck Haines:** Notify the team that Factory test PRs are in flight, so they do not get confused by them (explicit).
- **Mykhailo Sakhnik (implied):** Remove the LiteLLM package from development environments and the skill creator once the advisory is confirmed.
- **Mykhailo Sakhnik (implied):** Fix the output-token accounting in the coder agent so the metrics panel is accurate.
- **Mykhailo Sakhnik (implied):** Add Node-capable Docker images so the Factory can run on the core UI repo.
- **Chuck Haines (implied):** Follow up on the Warp proof of concept starting Monday, 2026-10-26.
