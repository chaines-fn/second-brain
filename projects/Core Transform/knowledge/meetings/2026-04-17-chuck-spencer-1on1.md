# Chuck / Spencer 1:1 — 2026-04-17

**Date & Time:** Friday, April 17, 2026, 3:29 PM CDT (~37 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead on Core Transform
- Spencer Moulton — Principal Engineer, Core team lead on Core Transform (Pearl)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/14joUw_2p0aJLfB-JPQWflPKhDRt0vhc0_mhxeE3HN74/edit?usp=meet_tnfm_email)

---

## Overview

Spencer walked Chuck through the "master horizon" reference architecture he's been developing over the past ~2.5 weeks for Core Transform (Pearl). The big convergences: EKS (not ECS, not serverless) for agent hosting given long-running and background workloads and Temporal fit; Temporal as the durability layer for short-term agent state, displacing the need for AgentCore Memory short-term; Postgres PGVector as the leading candidate for long-term and semantic memory (versatile, role-level security gives tenant isolation); Logfire for LLM observability with a dev→prod eval feedback loop, while standard APM still goes to Datadog; and a Bedrock-fronted LLM gateway so non-agent LLM use cases can share guardrails, SOPs, and cheaper fallback models. Terraform is the infra-as-code standard (not CDK), and the team will start provisioning a sandbox early next week with Maria.

The other thread was organizational alignment. Chuck reiterated his role as a "shield" — he wants to be informed of major decisions and their rationale so he and Mike can redirect misaligned conversations externally, but he's not looking to be a voting voice on Spencer's team's technical choices. Spencer agreed to DM Chuck and Mike first with the rationale behind big calls (like moving off AgentCore Memory to EKS + Temporal + PGVector) before widening those conversations to the principals channel, where Ian in particular is expected to push back on anything that deviates from "how we've always done it." Chuck also flagged he likely can't make the June Minneapolis on-site due to his BD travel.

---

## Key Notes

- **EKS over ECS/serverless — decided:** Spencer is "leaning heavy heavy EKS by the day." Reasoning: anticipated workload is long-running background agents and job-triggered work, which fits Kubernetes + Temporal Cloud; serverless isn't Temporal-compatible; ECS is optimized for bursty interactions. This is one of the architectural calls Spencer wants to align Mike and Chuck on before deploying, so there's no shock when AgentCore Memory gets ruled out.

- **Temporal as the durability layer for short-term agent state:** Agent short-term messages and consolidated scope state live in Temporal. This removes the need for AgentCore short-term memory. Temporal will also be "more pivotal than we think" in managing scheduled memory operations and translating short-term → long-term memory. Mike is increasingly bullish on Temporal — now the question is cost profile and usage patterns, not whether to use it.

- **Long-term / semantic memory — PGVector leading:** Evaluating Postgres PGVector as the primary option. Strong points: versatile, mature community, role-level security gives free tenant isolation for vector spaces. Aurora MySQL has new vector support but immature docs/community. OpenSearch Serverless always relevant but high base cost — not worth it until scale is guaranteed. Nice bonus of Postgres: joins against structured provider data (L3 structured preferences, L5 long-term) if FN Core / microservices allow RDS access.

- **AgentCore Memory being ruled out — need to pre-align Mike:** Spencer stressed the importance of "dispelling the idea that agent core memory is necessary" before deploying to EKS. Chuck agreed the three of them (Chuck, Spencer, Mike) need to be on the same page on the reasoning so they can redirect external conversations as the "agent core memory" murmurs surface. Thursday's AWS session is expected to give definitive answers without needing a PoC.

- **LLM Gateway + Bedrock — not just for agents:** Proposed gateway wraps a Bedrock provider to enforce guardrails/SOPs, enable future injection of buyer- or industry-specific SOPs/compliance docs, and support non-agent internal LLM use cases. Rationale: lots of LLM need is coming — don't want every team assuming they need an "agent" for it; shared fallback to cheaper models (Nova Lite, Nova Pro) for basic use cases. Portkey is the leading wrapper candidate; LiteLLM explicitly ruled out due to supply-chain-attack history. LangChain technically can act as a gateway. Spencer aligned with Ian's preference for going through Bedrock for internal LLM apps vs. buying more seats/licenses.

- **Context Engine as a master-horizon (not foundation) piece:** Has tenant-isolated knowledge bases plus general internal knowledge bases feeding internal LLM calls. Bedrock Knowledge Bases being considered as the internal general-platform SOP store — Spencer flagged this as potentially relevant to Chuck's AI Software Factory stream too. Deferred from foundation phase: complex memory management, episodic memory distillation, formal dedicated HIL approval service (distinct from one-off HIL prompts via PydanticAI).

- **Observability = Logfire for LLM tracing + Datadog for standard APM:** Logfire (Pydantic partner, OTEL-span based, mature since before PydanticAI existed) gives granular LLM tracing that feeds an observability→eval feedback loop: production traces inform/expand the golden set in dev, which ties to CI — effectively programmatic reinforcement learning on the agent. Datadog's LLM observability is "crazy expensive" and immature. Standard app-perf metrics still flow to Datadog; plan may be to conditionally forward some OTEL signals from Logfire → Datadog. Spencer to collaborate with Ian and the APM focus group. (Side note: APM is now overloaded — Application Performance Monitoring vs. Agent Package Manager.)

- **Internal MCP — joint effort with AI Software Factory:** Spencer wants to start developing an internal MCP paradigm/pattern, collaborating with Mike's AI SFW team but with Spencer's team doing the hands-on work. Scope: API client wrappers on FN Core plus supporting microservices (geospatial, parts, orders, etc.). This standardizes agent access to internal services across both streams.

- **Terraform is the infra-as-code standard (not CDK):** Chuck confirmed Terraform for everything. Relevant repos: `FN Terraform` (more AM/Cloudflare — Ian has context) and `FN Terraform Dev` (maps to existing platform services — Cognito, Daryl, dynamic talent pools, etc.) — `fn-terraform-dev` is the right starting point for Spencer's sandbox work. Sandbox provisioning kicking off early next week with Maria's help.

- **Product Discovery Group doesn't need new Terraform:** Their work is a single Go binary deployed to Kubernetes via the Golden Go image — already has Helm charts, Tilt, Dockerfiles, everything. Quirk: bundles UI inside the Go binary (not what FN would normally do in product realm, but fits this innovation context).

- **Bedrock SDK / PydanticAI SDK already in flight with Max as first customer:** Spencer has a Bedrock-backed PydanticAI agent SDK in development. Max is already a customer. Planned approach: get a "hello world" work-order agent running on EKS + Temporal behind a load balancer, then extend from there.

- **Chuck's role as "shield" — explicitly framed:** Chuck reiterated he's not looking to vote on Spencer's team's technical decisions — he wants to be looped in so he can fight organizational battles (e.g., on Temporal with Travis) and shield the team from external friction. He's already bandwidth-constrained. Cory's ask for Chuck to stay tangentially involved was a little surprising to Chuck, but the framing is protective, not directive.

- **Pre-align before the principals channel — Ian dynamic:** Spencer agreed to DM Chuck and Mike with the rationale for big architectural calls (EKS, no AgentCore Memory, etc.) *before* posting in the principals channel (Ian and Andrew are there). Chuck's flag on Ian: "If I don't know it and I don't understand it or it's not how we've always done it, I'm going to immediately say no." Same friction expected on any non-Datadog observability choice. Historically Travis wasn't anti-Temporal — that pushback was mostly Ian. Travis's only negative history was on LangGraph.

- **AI Software Factory ≠ customer-facing:** In Thursday's NNL presentation, Chuck was explicit with Shane/Travis/leadership that the AI Software Factory is about enabling engineers to build faster — *not* building any customer-facing agentic functionality. Initial audience didn't get it on first pass; Chuck had to restate.

- **Chuck's June on-site attendance unlikely:** Between BD travel and turnaround time, Chuck probably can't attend the Minneapolis in-person in early June. He'll let Linda know (she's coordinating with Maria since she's farthest from Minneapolis).

---

## Action Items

- **Chuck Haines:** Let Linda know he likely can't attend the June Minneapolis on-site due to BD travel.
- **Spencer Moulton:** Start the UI / front-end conversation with Brent and Linda for Pearl.
- **Spencer Moulton:** Collaborate with Ian and the APM focus group on LLM observability implementation (Logfire + Datadog bridging).
- **Spencer Moulton:** Kick off the internal MCP paradigm/pattern work, coordinating with Mike's AI Software Factory team.
- **Spencer Moulton:** Start reviewing Terraform (new to him vs. CDK) — begin with `fn-terraform-dev` for sandbox provisioning early next week.
- **Spencer Moulton:** DM Chuck and Mike with the rationale behind key architectural decisions (EKS over AgentCore Memory, PGVector, Logfire, etc.) *before* posting in the principals channel.
- **Spencer Moulton (implied):** Get a hello-world work-order agent deployed to the Pearl sandbox (EKS + Temporal + load balancer) once infra is provisioned.
- **Chuck + Spencer (implied):** Continue weekly touchpoint to coordinate AI Software Factory ↔ Pearl boundaries.
