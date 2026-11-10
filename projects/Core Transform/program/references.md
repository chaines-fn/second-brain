# References Log

Running log of all documents, meeting transcripts, and external resources shared in this project. Updated whenever new links or source material are shared.

---

## Source Documents

| Title | Type | Link | Date Added | Notes |
|---|---|---|---|---|
| Pearl Sandbox State | Google Doc | [link](https://docs.google.com/document/d/11zCZWzkjgDq42euPwcp5fNcqDfFqn6bmbCsu-pnuauU/edit?tab=t.0) | 2026-04-07 | Informal state store for the Pearl Sandbox AWS account — models, IAM policies, cost monitoring |
| CNB Buildpacks Python Engine | GitHub Repo | [link](https://github.com/fieldnation/cnb-buildpacks-python-engine/blob/main/README.md) | 2026-04-07 | Python buildpack for cloud-native builds |
| Enterprise CNB Project | Confluence | [link](https://fieldnation.atlassian.net/wiki/spaces/SEC/pages/4651450384/Enterprise+Cloud+Native+Buildpacks+CNB+Project) | 2026-04-07 | Enablement & Q&A for buildpacks adoption |
| CNB NotebookLM | NotebookLM | [link](https://notebooklm.google.com/notebook/94da4040-a7e2-4005-8d91-a1a1b6221e94) | 2026-04-07 | AI-generated Q&A resource for buildpacks |
| From Twelve to Sixteen-Factor App | Article | [link](https://cloud.google.com/transform/from-the-twelve-to-sixteen-factor-app) | 2026-04-02 | Google Cloud piece on new operational principles for AI apps — prompts as versioned specs, observability for non-determinism |
| PydanticAI Overview | Docs | [link](https://pydantic.dev/docs/ai/overview/) | 2026-04-09 | Framework being evaluated — native Bedrock client, Temporal integration, type-safe, evals built on Pytest |
| AWS Call Talking Points — April 14, 2026 | Google Doc | [link](https://docs.google.com/document/d/1aD4-TaGLfAI0cz6Ag1llM4ncq6354fnr76ppL27sVzY/edit?tab=t.0#heading=h.c417eeuzuqp3) | 2026-04-14 | Spencer's comprehensive AgentCore component evaluation framework for AWS onsite. Component-by-component questions, what's decided vs open, what FN would build without AgentCore. Collaborative doc with Chuck, Mike Mueller, Ian Loverink. |
| PydanticAI + Temporal Integration | Docs | [link](https://pydantic.dev/docs/ai/integrations/durable_execution/temporal/) | 2026-04-08 | Native Temporal integration for PydanticAI — durable execution and state management for agents. Spencer flagged this as a strong combination. |
| Gravitee AI Agent Management | Product page | [link](https://www.gravitee.io/platform/ai-agent-management) | 2026-04-08 | Gravitee's MCP/AI agent gateway capabilities — potential alternative or complement to AgentCore Gateway. Mike Mueller flagged. |
| Advanced Agent Memory Questions (External) | Google Doc | [link](https://docs.google.com/document/d/1HhvtIkIGhDx1VCSwmr5f73rtJVhgdSZDVTHwzmyd9qs/edit) | 2026-04-15 | Spencer's 8-question doc for external AWS specialist on advanced agent memory — batch API hydration, Bedrock KB comparison, neighbor leakage, hooks. Chuck + Mike reviewing before posting (tagging Faren). |
| AWS On-Site AgentCore Call Report | Google Doc | [link](https://docs.google.com/document/d/1O0H0g0mTq4l73oj98KiyY95e1NI8EbJSX9YOH8fporo/edit) | 2026-04-15 | Report from the 2026-04-14 AWS on-site AgentCore call, referenced in the 2026-04-15 Spencer/Chuck/Mike arch discussion. |

---

## Meeting Transcripts

| Meeting Title | Date | Transcript/Link | Key Decisions / Notes |
|---|---|---|---|
| Core Transform Kickoff | 2026-04-08 | [Transcript](https://docs.google.com/document/d/1lYJ6f4-cGv8JZ2e9c_If8Ux8ehQ2c49e2DVWwNsi94I/edit?tab=t.s3icbyh2y7ue) | Mission defined, Now/Next/Later framework, Q2 deliverables (H2 plan as keystone OKR), roles clarified, in-person workshop targeting late May/early June |
| Spencer/Chuck/Mike AWS Arch Discussion | 2026-04-15 | [Transcript](https://docs.google.com/document/d/13gyBbqu4z2iCh98jNS22lRHb3jkiE90UJSSA0-c1EhY/edit) | Leadership split (Chuck = AI dev tooling, Spencer = customer-facing agentic). Context Engine as emerging critical component. Resource Authorization Service as basis for scope isolation. Prioritized specialist questions on Batch API hydration + Bedrock KBs. |
| Chuck / Spencer 1:1 | 2026-04-17 | [Transcript](https://docs.google.com/document/d/14joUw_2p0aJLfB-JPQWflPKhDRt0vhc0_mhxeE3HN74/edit?usp=meet_tnfm_email) | Spencer's master reference architecture: EKS + Temporal + PGVector + Logfire + Bedrock-fronted LLM gateway; AgentCore Memory being ruled out; internal MCP as joint effort with AI SFW; Terraform (not CDK); sandbox provisioning kicks off next week. Chuck's "shield" role reaffirmed; pre-align Mike before principals channel. |

---

## Confluence Pages

| Title | Space | Link | Date Added | Notes |
|---|---|---|---|---|
| Enterprise CNB Project | SEC | [link](https://fieldnation.atlassian.net/wiki/spaces/SEC/pages/4651450384/Enterprise+Cloud+Native+Buildpacks+CNB+Project) | 2026-04-07 | Buildpacks enablement |

---

## Other Resources

| Title | Type | Link | Date Added | Notes |
|---|---|---|---|---|
| Core Transform Kickoff Deck | Google Slides | [link](https://docs.google.com/presentation/d/1w97_RaI0V7Y77KXTpIUUqpnGy-GNKcL_T8LjnKOMKmU/edit) | 2026-04-08 | Shane Kinder's initiative framing deck — mission, now/next/later, Q2 deliverables, strategic considerations |
---

## Slack Channels

### Team Channels (Private)

| Channel | ID | Created By | Purpose |
|---------|-----|------------|---------|
| #pearl-eng | C0AMTVD1M6Y | Maria Saxton | Core engineering coordination and alerts |
| #core-transform-leads | C0ARRLMP58F | Linda Lo | Initiative leads coordination |
| #core-transform-pex-leadership | C0ARMT0KGS2 | Shane Kinder | Product, Design, Engineering leadership |

### Cross-Functional Channels (Public)

| Channel | ID | Created By | Purpose |
|---------|-----|------------|---------|
| #core-transform-xfn-team | C0AR8G288R5 | Linda Lo | Cross-functional team collaboration |
