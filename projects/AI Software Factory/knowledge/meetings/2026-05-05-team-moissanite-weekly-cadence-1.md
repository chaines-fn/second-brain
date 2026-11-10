# Team Moissanite: Weekly Cadence 1 — 2026-05-05

**Date & Time:** Tuesday, May 5, 2026, 11:00 AM CDT (~40 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer (Architecture Lead)
- Joe Hoppesch — Principal Engineering Consultant
- Redwan Shahzada — Director of Engineering (Bangladesh team)
- Cory Muir — Sr. Director, Software Engineering (joined briefly, dropped early)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1by5wp0QCBNk17Hp8dLJD7hL7gR4E-L6Ir2COWMMG3TQ/edit?usp=meet_tnfm_email)

---

## Overview

First weekly cadence for the newly-named Moissanite squad. The discussion centered on locking down the Q2 platform decisions: what verification tool to use, what "harness" runs the agents, and what infrastructure they run on. Sonar is in as the deterministic verification standard. The bigger open question — harness + infrastructure — got narrowed to a comparative POC between Cursor SDK and Microsoft's Azure AI Foundry, with a hybrid track stitching them together.

The group reaffirmed a strong preference to **buy off-the-shelf rather than build**, both to accelerate Q2 delivery and to keep maintenance costs down. Joe pushed for picking one suite end-to-end (Microsoft or Cursor) to avoid integration glue work; Chuck pushed back that he trusts Cursor's harness more than Microsoft's, but doesn't yet have visibility into how much control Cursor's managed cloud gives over per-agent sandboxing — which is the deciding factor. The team agreed to do real comparative spikes rather than pick on instinct.

In parallel, contractors will continue making the AI toolbox usable inside a real Field Nation repo. The target repo is unselected; FN Core, Provider Match, and Integrations are the candidates, with Joe owning the decision after talking to Mike and Andrew. Unblocked (knowledge store) was explicitly deprioritized for Q2 — interesting eventually, but not on the critical path. Chuck flagged he loses Mon–Wed of next week to travel.

---

## Key Notes

- **Sonar locked in as the deterministic verification standard.** Chuck framed the rationale: AI-reviewing-AI is non-deterministic, and Sonar provides the deterministic "does this meet our standards" check. Bonus: Sonar's existing knowledge base can be fed forward to the coding agents as basic good-engineering-practices context, so the team doesn't have to author that from scratch. Joe later confirmed Sonar does **not** host agents — they only provide an MCP server you point your self-hosted agents at. So Sonar fits cleanly as a computational sensor; it doesn't replace the harness/infra question.

- **Distinction between harness and infrastructure is now the operating mental model.** Chuck named two layers: (1) the *harness* — how agents connect, orchestrate, and execute their work (Cursor SDK, Copilot, Claude Code); (2) the *infrastructure* — what they run on (AWS, Bedrock, Azure, Cursor's managed cloud). Azure AI Foundry tries to be both bundled together; Cursor SDK is more clearly just a harness, with their managed cloud as one infra option among several.

- **Chuck trusts Cursor's harness over Microsoft's, but the Cursor managed-cloud control story is the open question.** His specific concern: in an ideal world, you spin up a per-agent sandbox restricted to "read this GitHub repo, view this knowledge base, write nothing else" — granular, token-cost-aware, scope-limited. Azure AI clearly offers that level of control. Whether Cursor's managed cloud does — or whether you'd need to use the Cursor SDK on your own compute to get it — is unknown and is the main thing the spikes are meant to answer. Cursor's docs are thin; Chuck wants a dedicated cursor deep-dive on the SDK and cloud agents to get clarity.

- **POCs/spikes defined as three parallel tracks.** Comparison is Cursor SDK vs Microsoft (Azure AI Foundry), plus a hybrid:
  1. **Cursor SDK on AWS** (Chuck) — Cursor SDK deployed to AWS-hosted containers (likely EKS or equivalent lightweight container hosting). Deliberately worded as "sandbox cloud VMs" rather than self-managed Kubernetes to avoid getting shot down on AWS-native concerns.
  2. **Azure AI Foundry out-of-the-box** (Joe) — use Microsoft's stack as it ships, including the agent framework.
  3. **Hybrid: Cursor SDK on Azure AI Foundry** (Chuck + Joe) — push Cursor-SDK-driven agents up to Foundry's runtime to get Foundry's context store and infra control with Cursor's harness ergonomics.

  **Bedrock AgentCore was dropped from the active spike set.** Chuck doesn't see a compelling AWS-native offering in this space, and Joe noted even AWS engineers seem lukewarm on Kiro. Joe initially suggested simplifying to just "go with Cursor and figure out the rest later," but the group agreed the comparative due-diligence is worth it; if the hybrid blows up that's also valuable knowledge.

- **All-in-on-one-suite vs. mix-and-match tension.** Joe's strong preference: pick a single suite (Microsoft OR Cursor) so you minimize the integration tax. Chuck agrees in principle ("if cursor can do all that, no reason not to go all in") but is unwilling to commit until the per-agent control question is answered. The hybrid spike is the hedge.

- **Buy vs. build is now a stated strategic principle.** Chuck: "I'm very much buy whatever we can off the shelf because that way six months from now when it's no longer the right thing, we just buy the next thing and migrate it." Joe agreed. This will be added explicitly to Redwan's deck as a guiding principle for Q2. Maps directly to the existing "use Claude/etc. and build a harness on top" architectural direction — not a bespoke end-to-end build.

- **Contractor work is the AI toolbox in a real repo, regardless of harness/infra outcome.** Chuck's framing: the toolbox work (skills, bundles, deployment into a repo, keeping it up to date) is independent of the harness/infra decision and is a clear path forward. This is what the contractors should drive while the principals run the spikes.

- **Target repo for the AI toolbox is unselected — Joe owns the choice.** Candidates: **Provider Match** (high volume) and **Integrations / API** (also high volume). FN Core came up in conversation but is not back on the table — the 4/16 NNL pivot to a Go/Node.js microservice still stands. Joe ran some analysis previously and saw the volume concentrated in those repos; he'll talk to Mike and Andrew for input before selecting.

- **Sonar will use whichever target repo gets selected.** Two-birds-one-stone: same repo answers Joe's "where do we point the toolbox" question and Chuck's "where does Sonar kick the tires" question, which also satisfies Ben's earlier ask for restricted-scope access for Sonar.

- **Unblocked (knowledge store) deprioritized for Q2.** Chuck's read: not on the critical path for Q2 deliverables. Where it eventually matters: when the system becomes multi-repo-aware and architecture-aware and needs cross-cutting context. Possibly also relevant for the long-term evolution of engineering standards (codified in knowledge store vs. continuously codified in skills) — but that's not a today decision. Chuck will start a conversation with Unblock anyway, with the explicit caveat "this is not a Q2 implementation."

- **Cost/co-pilot constraint surfaced.** Chuck flagged that Foundry uses Codex / Copilot under the hood by default, which raises the same cost-model concerns Ben has been flagging. Joe clarified you can swap pieces — even if Copilot's cost model is a deal-breaker, you can still use Cursor tools locally and push artifacts up to Foundry. That flexibility is part of what makes the hybrid track interesting.

- **Travel constraint.** Chuck loses Monday–Wednesday next week to travel; will work on the plane where possible but commits cautiously on deadlines. Joe reinforced that this is exactly why keeping the contractor toolbox track moving in parallel matters.

- **Ben sign-off check.** Chuck mentioned wanting to confirm with Ben that the proposed plan is acceptable before kicking off — particularly the Cursor engagement and the spike scope. Worth flagging back to Ben before too much spike work lands.

---

## Action Items

- **The group:** Get the AI toolbox up, running, and applicable for general use in a target repo (skills, bundles, deployment, freshness mechanism).
- **Chuck:** Schedule a team deep dive with Cursor on the Cursor SDK and cloud agents — needed to answer the per-agent control question.
- **Chuck:** Tailor next week's training content to potentially include Cursor information (depending on direction).
- **Chuck:** POC Cursor SDK deployed to sandbox cloud VMs / lightweight container hosting on AWS.
- **Chuck:** Initiate conversation with the Unblocked team to understand capabilities — explicitly framing this as *not* a Q2 implementation.
- **Chuck (implied):** Confirm with Ben that the proposed harness + spike plan is acceptable before kicking off the Cursor engagement.
- **Joe:** Talk to Mike and Andrew to gather suggestions for potential target repositories.
- **Joe:** Select the target repository for the AI toolbox / Sonar bootstrap and report back to Redwan.
- **Joe:** POC Azure AI Foundry out-of-the-box (full Microsoft agent framework).
- **Chuck + Joe:** POC the hybrid approach — Azure AI Foundry as runtime/infra with Cursor SDK as the harness.
- **Ian:** Spin up an AWS sandbox environment for the Cursor SDK spike.
- **Redwan:** Update the presentation deck on toolings and standards to reflect today's conversation.
- **Redwan:** Add the "prioritize buying over building" strategy to the deck as an explicit guiding principle.

