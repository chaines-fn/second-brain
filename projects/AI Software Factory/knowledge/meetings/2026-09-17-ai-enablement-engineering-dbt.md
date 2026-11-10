# AI Enablement (engineering-dbt) — 2026-09-17

**Date & Time:** Thursday, September 17, 2026 (exact start time not in the transcript, likely morning) CDT (~30 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer, AI-DLC lead (meeting organizer)
- Mike Mueller — Sr. Principal Software Engineer, Team Moissanite
- Rakibul Hoque — engineering-dbt (Emerald team) engineer *(role not on file, inferred from transcript)*
- Amran Hossain — engineering-dbt (Emerald team) engineer *(role not on file; spoke with some authority about team performance, possibly a lead)*
- Imtiaz Chowdhury — engineering-dbt (Emerald team) engineer *(role not on file; referred to by teammates as "MT"/"MTS")*
- Tanin Rahman — engineering-dbt (Emerald team) engineer *(role not on file; works with the Marble team)*
- Ruzlan Karim — engineering-dbt (Emerald team) engineer *(role not on file)*
- Rezaul Karim — engineering-dbt (Emerald team) engineer *(role not on file)*

Invited as a group: engineering-emerald@fieldnation.com

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1sTHPfj6_2akcW7-_PYVRRf45Yqpgs5-Ka9JKIIZvHiU/edit)

---

## Overview

Chuck sat down with the engineering-dbt (Emerald) team, the first repo picked for the AI-DLC embed pilot, to find out why true throughput hasn't moved despite the repo scorecard work done a couple weeks earlier. He opened by walking through the AI-DLC vision (human-in-the-loop gates around agent-led development, with the embed model targeting specific teams to find and fix what's actually slowing them down) and framed engineering-dbt as a test bed, not a performance review of the team.

The bulk of the conversation surfaced where time is actually going: not in development once a ticket is picked up (issue cycle time is healthy) but before a ticket can even start. Engineers described spending most of their time finding the right data source across multiple warehouse layers, deciding which DBT layer a model belongs in, chasing down undocumented business logic with product and business teams, and manually checking for duplicate or conflicting column definitions across models. Existing repo skills for layer placement and repo standards got a genuine endorsement from the team working with Marble, but the harder problems (business logic validation, data source discovery, cross-team data governance) were consistently flagged as things agents can't easily solve because the knowledge lives in people's heads or in a manually maintained spreadsheet.

The session converged on two candidate next steps: a skill that cross-references the team's data governance spreadsheet to speed up (or auto-approve) column/model validation, and picking up a rough hackathon project from the ML team that prototypes a knowledge graph connecting schemas, product definitions, and Jira/migration data to answer "where should this data come from" questions. Chuck closed by reiterating this wasn't a knock on the team and that he's collecting the same kind of feedback across other embed teams (he mentioned talking to "Basalt" later that same day).

---

## Key Notes

- **True throughput is flat despite the scorecard work.** Engineering-dbt sits around 1.4 PRs/engineer/week; Chuck expected the initial scorecard pass (run ~2 weeks prior, scoring CI/CD, unit tests, agent MD file, rules, and skills on a 0-100 scale) to push it toward 1.8-1.9 on the way to the 3.5 goal, but it hasn't moved. This is the reason for the session: figure out whether the bottleneck is even something agentic development can fix.

- **The real bottleneck is pre-ticket, not in-development.** Once a ticket is picked up, issue cycle time is strong (Amran: the team was recently recognized as best team of the quarter). The time sink is upstream: identifying the correct data source across warehouse layers and determining the right domain before work can start. Chuck called this an important distinction to bring back to Cory and Travis, since it changes what "fixing engineering velocity" actually means here.

- **Layer-placement decisions consume real time, and there's disagreement on whether agents can help.** Imtiaz said deciding which DBT layer (base, staging, reporting, or an intermediate layer) a model belongs in requires human judgment about future reuse. Mike disagreed that this is purely human territory, arguing agents could at least prompt engineers with the right guiding questions. Chuck noted a step-by-step layer-placement skill and ADR guidance were already built from the team's own documentation, but no one confirmed they'd actually tried it, so its effectiveness is still unverified.

- **Mike separately flagged that DBT lineage tracing may need to be a cross-repo capability, not an engineering-dbt-specific one**, since a lot of the code these engineers write already exists upstream in the DBT repo populating the warehouse tables, and tracing a column's lineage through several layers has been a long-standing struggle across teams (not just this one).

- **Business logic validation is described as essentially unsolvable by AI as things stand.** Ruzlan said the SQL itself doesn't capture business rules (his example: travel expense calculations), so engineers have to go back to product and business teams to validate conditions, and that validation loop, not the coding, is what takes time.

- **Column naming collisions are a real, distinct pain point.** Ruzlan described the same column name meaning different things across models, and different names meaning the same thing, both requiring manual cross-checking to avoid duplicate or conflicting work. He suggested this is something an agent could plausibly help catch by checking whether a concept/table already exists, though it wouldn't catch the "different name, same meaning" case. He also mentioned the team is separately moving toward a semantic layer that would enforce this more directly, but that's not built yet.

- **The team already has a validated positive result to point to.** Tanin, working with the Marble team, said the existing repo-standards and layer-placement skills worked well for a full sprint. He was clear that the parts remaining hard (business logic, data source discovery) are the same ones everyone else raised.

- **Data governance definitions live in an informally maintained Google Sheet, not a documented system.** Ruzlan and Mike described a spreadsheet of dimension/measure/column definitions that the team discusses and updates roughly weekly; there's no automated check against it today. Rakibul suggested a skill that could auto-approve low-risk changes against it and route only ambiguous ones for manual review, to cut wait time. (Side note: Mike ribbed Chuck for having skipped this recurring Thursday governance meeting for the past year and sending Mike instead, which is also why Chuck didn't already know the format of the doc.)

- **Cross-team migrations create ongoing documentation debt.** Tanin gave a concrete example: invoices currently live in fn-core but are migrating to a new accounting microservice, which means the DBT team has to track that migration and update their own models and docs accordingly, something that depends on staying in close touch with the domain team rather than anything an agent can infer on its own.

- **A rough hackathon project may be a starting point for the data-discovery problem.** Rezaul described a weekend hackathon effort (ML team) prototyping a knowledge graph linking database schemas, product definitions, and Jira/migration data, aimed at answering "what should I do to build X" questions and flagging missing tables/columns in the landing zone. Chuck asked to see it and was clear it's fine that it's rough since it doesn't need to be reinvented from scratch.

- **Chuck was explicit that this session isn't a critique of the team.** He repeated twice that engineering-dbt was picked as the embed test bed because of its healthy backlog and multi-team contribution pattern, not because of a performance concern, and that the same conversation is happening with other teams (he mentioned "Basalt" later that day) so learnings can be applied broadly.

## Action Items

- **Rezaul Karim:** Send Chuck the link to the ML team's hackathon knowledge-graph project via Slack.
- **Chuck Haines:** Review the hackathon project (docs and code) to assess whether it can be integrated into the AI enablement toolchain or pushed across the finish line.
- **Chuck Haines:** Build a repository skill that cross-references the data governance spreadsheet definitions for column/model validation, with an eye toward auto-approving low-risk changes.
- **Chuck Haines (implied):** Follow up on whether anyone on the team has actually tried the existing layer-placement/naming guidance skill, since no one confirmed testing it during the call.
