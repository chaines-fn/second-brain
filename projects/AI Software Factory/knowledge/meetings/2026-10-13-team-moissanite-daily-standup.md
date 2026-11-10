# Team Moissanite: Daily Standup, 2026-10-13

**Date & Time:** Tuesday, October 13, 2026, 08:13 CDT (~18 minutes)

**Attendees:**
- Chuck Haines: Sr. Principal Software Engineer (organizer)
- Mykhailo Sakhnik: Contractor (presented status)
- Joe Hoppesch, Asad Ali, Vitalii Belinskyi: invited (no speaking on the transcript)
- Redwan Shahzada: invited, struck through in the invite (rolled off AI-DLC 2026-07-14)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1_9d5NbBPajq3YxkgZdTkDmQCkjdxgvU-zd6FVsnWOFg/edit?usp=drive_web&tab=t.gtuxyhoorjrt)

---

## Overview

Mykhailo used the standup to walk Chuck through two spikes he opened today, MOI-167 (evaluate the Amazon Bedrock AgentCore Harness for autonomous coding) and MOI-168 (evaluate event-driven, human-in-the-loop execution on AgentCore). He ran a real repository issue through the AgentCore Harness built-in sandbox and it fixed the bug, updating the same two files as his local run. The harness default sandbox lacks Node, NumPy, uv, and tar, so environment prep is the next open question. Chuck gave Mykhailo a green light to move as fast as needed, with a standing instruction to tear down any sandbox resources when done.

Harness CI remains blocked on permissions: Mykhailo cannot write commands into pull requests, and he filed two tickets for Ian to resolve this and a separate AWS connector. Mykhailo also has a ready PR that removes DeepEval from ai-toolbox. Chuck committed to reviewing it and the spike write-up the same day.

Chuck flagged that the standup was in the middle of Q4 planning and that he is back at work after being unwell. He also announced a planned webinar with Sonar and Gitar on December 2. New external meetings were added: an AWS expert call on the AgentCore Harness (Wed 10/14, 12:00 CT) and a Warp call about "Warp Factories" (Fri 10/16, 11:00 CT, with Cory and Mykhailo invited). Chuck framed Warp as a possible infrastructure alternative to build on and asked that the AgentCore work continue so the two can be compared.

---

## Key Notes

- **AgentCore Harness spike (MOI-167):** Mykhailo built the harness environment by hand and ran a real repo issue (a RepScore bug) through it. The bug was fixed and the two changed files matched his local run. The default sandbox has no Node, NumPy, uv, or tar. Mykhailo sees two paths: use the built-in harness environment with pre-prepared images, or run a custom runtime (e.g., LangGraph or another AgentCore Runtime setup) that carries its own dependencies. Chuck's concern is that the default image would need rebuilding every time for Node or PHP. He proposed an S3 hot cache of a cloned repo (repo cloning is slow) or pre-built container images in ECR as options to explore.

- **Sandbox access and tooling:** Mykhailo could not enable X-Ray tracing in the UI because his user lacked permission, though logs are downloadable. Chuck granted full sandbox use, including applying Terraform locally, with the instruction to tear down anything created once work is finished. Eventually the infrastructure needs to be in Terraform for deployment to the real environment.

- **Human-in-the-loop design (MOI-168):** Mykhailo is evaluating an event-driven pattern with "ambient" agents that listen for external signals, using DynamoDB to store run status for idempotency. The AgentCore Harness includes an `ask human` tool, which he thinks makes the pattern usable. He wants to create SQS and Lambda components in the sandbox to test it, starting from a GitHub issue rather than a Linear ticket. Chuck said to start with simple AWS services and defer Temporal until there is a reason for it. Mykhailo's plan is to replace temporary functions with Temporal later if needed.

- **Architecture mapping:** Chuck asked Mykhailo to begin sketching the architecture in Lucid or draw.io using simple AWS services, and said he will create the Lucid board himself. Chuck also suggested comparing other harnesses (open-code and pi), which overlaps the Fai harness spikes (MOI-153 to MOI-155) on 2026-10-08.

- **Security of skills (prompt injection):** Mykhailo raised that skills can carry prompt-injection content (e.g., "ignore previous instructions") and that the current toolchain does not check for it. Chuck confirmed that backlog tickets should be created. He said he first looked at Whiz but no longer thinks it fits, and asked Mykhailo to look for an off-the-shelf option before writing a custom one. Mykhailo will evaluate deterministic checks and Gitar as an LLM judge. Chuck will review the security skills proposal submitted this morning.

- **Harness CI and DeepEval removal:** Harness CI progress is blocked on PR command write permissions (two tickets filed for Ian; the second covers a scoped AWS connector, which Mykhailo said is not blocking because he can still proceed). A PR removing DeepEval from ai-toolbox is ready for review. This is consistent with MOI-151 (In Review) and MOI-165 (In Progress) in Linear.

- **External context:** Chuck announced a webinar with Sonar and Gitar on December 2. He described Gitar as a tool that has worked well for him. (The transcript spells it "Guitar"; the vendor name is Gitar.)

- **Blockers and open items:** The Harness PR-command permission gap is the only stated blocker. The AWS expert call and the Warp call are scheduled but their outcomes are unknown as of this note.

---

## Action Items

- **Chuck Haines:** Review Mykhailo's security skills proposal (today).
- **Chuck Haines:** Review the AgentCore Harness work from the spike and open a ticket for it.
- **Chuck Haines:** Create the architecture diagram (Lucid board) for the AgentCore/AWS build.
- **Chuck Haines:** Send Mykhailo the Warp documentation link.
- **Mykhailo Sakhnik:** Create backlog tickets for the prompt-injection and skill security issues.
- **Mykhailo Sakhnik:** Evaluate off-the-shelf security tooling (Whiz was the first candidate, Chuck has doubts) and deterministic checks, with Gitar as an LLM judge.
- **Mykhailo Sakhnik:** Research custom architecture options (LangGraph and alternative platforms) and draft the initial AWS services needed for the project foundation.
- **Implied, Chuck:** Attend or follow up on the AWS expert call (Wed 10/14, 12:00 CT) and the Warp call (Fri 10/16, 11:00 CT).
- **Implied, Ian:** Resolve the two Harness and AWS connector tickets Mykhailo filed (write access to PRs is the blocking one).
