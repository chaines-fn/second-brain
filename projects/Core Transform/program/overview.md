# Core Transform (Pearl) — Overview

**Last updated:** 2026-04-09

---

## What Is This Project?

Core Transform — internally codenamed "Pearl" — is a high-visibility, board-driven initiative launched by Field Nation leadership to transform the platform into an autonomous virtual service company. The initiative was formally kicked off on 2026-04-08 with Shane Kinder and Travis Emslander framing the vision.

### Mission Statement

> "Transform the product experience leveraging AI to first automate buyer tasks that are manual today, and then work towards a new buyer experience where Field Nation acts as a virtual service company."

The core insight: Field Nation has a structural cost advantage over W2 and third-party service companies (no markup, no non-utilization costs), but that advantage is hard for buyers to access because of steep learning curves, dispatcher churn, and marketplace complexity. AI can unlock that advantage by making the platform dramatically easier to operate — first through task automation, then through a fundamentally new buyer experience.

## The Vision: Now / Next / Later

**FN 1.0 (Now)** — Three manual stages of a buyer program: program planning, program execution (ticket-by-ticket fulfillment), and program management (long-term performance). All highly manual, high learning curve, subject to dispatcher churn.

**FN 2.0 (Next)** — Automate tasks across the lifecycle, one at a time. Human-in-the-loop initially, progressively moving toward full automation. Build a foundational platform with consistent patterns. Prioritize by impact × solvability. "Aim for 80/20 automation" — move fast, avoid edge case rabbit holes.

**FN 3.0 (Later)** — Virtual service company. All tasks automated and coordinated by super-agents (program planner agent, dispatch agent, program manager agent). Fundamentally different buyer experience — buyers give Field Nation the work like they'd give a service company. Opens new TAM: large buyers who lack dispatchers can now use the platform. Target: virtual service company MVP in 2027 (directional).

## Goals

- **Q2 2026:** Define the H2 plan (this IS the Q2 OKR). Upstream work includes:
  - Activity shortlist with priority order for buyer lifecycle automation
  - FN 3.0 concept vision artifact ("magic marker fidelity" — just enough to ground the team)
  - Technical architecture and foundations (one-way vs two-way door decisions)
- **H2 2026:** Start executing — identify and prove out automations that deliver real buyer value
- **2027:** Accelerate toward virtual service company MVP
- Build a common agentic SDK/platform as the foundation for all AI-driven product use cases
- Evaluate and select the right combination of agentic frameworks, durable execution tech, and runtime environments

## Key Concepts

- **Agentic AI** — LLM-powered reasoning loops that can autonomously make decisions and take actions
- **Durable execution** — Long-running, fault-tolerant workflow orchestration (Temporal is a leading candidate)
- **Pearl Sandbox** — Dedicated AWS sandbox account (400844546951) for experimentation with Bedrock and agent frameworks
- **AgentCore** — AWS service being evaluated as a potential runtime, alongside self-managed EKS
- **Co-Creator Program** — Structured engagement with collaborative buyers operating at scale, embedded in the development process
- **One-way vs Two-way Door Decisions** — Framework for prioritizing architectural decisions. One-way doors (AWS vs Azure) get decided carefully. Two-way doors (framework choice) can be swapped.
- **Escape Hatch Philosophy** — Always use interfaces; never hard-code full adoption of any single technology

## Strategic Guardrails

- **Legal/compliance:** Get as close to a service company as possible WITHOUT taking on compliance risk or outcome ownership. W2 model is not on the table. Buyer must always approve the final action.
- **Speed:** Must move faster than past initiatives. Shane pre-wired the SLT against edge-case-driven delays.
- **Buyer experience is primary objective.** Provider-side automation is in scope when it benefits the buyer, but the north star is making it easy for buyers to go hands-free.
- **Don't over-AI it.** Use heuristics and rules where they suffice. But intentionally find early agent opportunities to build experience.
- **Avoid the "data platform 2.0 spiral."** The concept vision artifact exists to keep the team grounded and prevent losing track of the bigger picture.

---

## How Chuck's Role Fits In

Chuck is the Architecture Lead on Core Transform. Per Cory Muir at the kickoff: Chuck won't be 100% dedicated (also working on other AI projects), but provides cross-cutting architectural support to Spencer and the Pearl team. His job is to help tie the work together across the org, identify one-way door decisions, and ensure the technical foundation supports both the automation and virtual service company stages. Spencer and Chuck are jointly responsible for articulating near-term, mid-term, and long-term technical decisions.

## Key Meetings & Milestones

- **2026-04-08:** Initiative kickoff meeting ([notes](../knowledge/meetings/2026-04-08-core-transform-kickoff.md))
- **Late May / Early June 2026:** In-person workshop in Minneapolis for concept vision sign-off
- **H2 2026:** Begin executing on automations with co-creator buyers
