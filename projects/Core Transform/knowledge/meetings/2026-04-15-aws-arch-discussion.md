# Spencer/Chuck/Mike AWS Arch Discussion — 2026-04-15

**Date & Time:** Wednesday, April 15, 2026, 12:56 CDT (~60 minutes)

**Attendees:**
- Spencer Moulton — Principal Engineer, Pearl team lead (meeting driver)
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead (dropped partway through)
- Mike Mueller — Engineering leadership, infrastructure/platform expertise

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/13gyBbqu4z2iCh98jNS22lRHb3jkiE90UJSSA0-c1EhY/edit?usp=meet_tnfm_email)

---

## Overview

Working session to align on leadership structure, scope boundaries for AI work, and the architectural direction for agent memory ahead of an upcoming external AWS specialist consultation. The group opened with team operating system approaches — Chuck walked through his Cowork-based personal OS (top-level personal assistant plus project-scoped co-work projects with morning briefings that scan relevant Slack channels) while Spencer is spinning up a shared "Field Nation Core Transform Team OS" he hopes can be Git-based and collaborative.

The meatiest strategic thread was the emerging case for a **Context Engine** as the common glue across Pearl's agents — a store of semantic, tenant-isolated context (user preferences, industry knowledge, relational patterns) distinct from structured data served by existing APIs. Spencer framed this as the critical enabler for personalization and explainability across buyer-side use cases, with the open question being not just how to retrieve context but how to source and ingest it well without ending up with "garbage in, garbage out." Mike brought in a pragmatic platform counterweight: FN already has a Resource Authorization Service with an EAV context store driven by Kafka events, which he sees as the foundation for scope isolation and potentially a future shared context service to bootstrap microservices.

They also worked through the eight questions Spencer drafted for the external AWS specialist on advanced agent memory, prioritizing (1) the Batch API for memory hydration — the degree of control FN would have over what goes into long-term memory — and (2) whether Bedrock Knowledge Bases might be a cleaner fit than AgentCore Memory for what they actually need. Mike's challenge to Spencer: if they want this much control, is AgentCore Memory really the right layer or just a wrapper they'll migrate off of? That's the question the specialist session needs to answer.

---

## Key Notes

- **Leadership clarity — Chuck as Architecture Lead, Spencer still runs the show:** Chuck explicitly framed his role as staying informed, asking questions, attending higher-level meetings (OKR, cross-org), and helping influence decisions — *not* being hands-on with Pearl implementation. He noted he'd take a less hands-on role here than with the break-fix team because Spencer is a principal engineer and doesn't need that level of architectural support. Spencer emphasized this isn't about toe-stepping — it's about clarity for people outside the group who need to know who to ask. Chuck has a 1:1 with Cory tomorrow to nail down Cory's vision for the role.

- **AI work responsibility split decided:** Spencer owns customer-facing agentic experiences; Chuck owns AI developer and tooling experiences. Chuck plans to raise this with Cory to get it stated explicitly org-wide so the lines stop blurring. Mike confirmed this was the original intent of Chuck's role. Spencer: "I don't want to be the receiver of all of the things, period."

- **Context Engine as emerging critical component:** Spencer's strongest architectural conviction coming out of his conversation with Cory. Framed as the "common glue" across agents — semantic context (buyer preferences, industry knowledge, relational patterns across buyers/providers) that isn't captured by existing APIs or structured DB queries. Distinguished from structured retrieval: if a work order template exists, use the API; the context engine is for conceptual, semantic, relational representations that enable personalization and explainability. Spencer acknowledged the vision is still vague because they don't yet fully know what unstructured/poorly-governed data already lives in the platform.

- **Sourcing and ingestion is harder than retrieval:** Spencer: "How we source it is the initial question in the first place... we don't want to ingest everything" — framing this like chunking strategy in RAG. The envisioned retrieval pattern: a tool call returning top-N relevant pieces, tenant-isolated, followed by summarization/compression that preserves named entities. Chuck echoed a token-economy angle from the Team OS thread: drive the right initial context upfront, make tool calls for additional context only when needed, rather than starting with broad tool calls and sifting.

- **Long-term memory is the target; short-term is Pearl's problem:** Spencer explicitly does not want to rely on AgentCore's short-term memory — he views that as persistent state / checkpointing that the Pearl team should own separately. The interest is in long-term memory: ideally a pipeline that auto-updates long-term memory in the background from agent interactions (not manual cron-driven indexing).

- **Context isolation will be multiple stores, keyed off Resource Authorization Service:** Mike raised buyer vs provider context needs. Spencer confirmed: multiple conceptual stores, starting with buyer-company-based tenant isolation. Mike shared that the Resource Authorization Service (EAV context store, updated by Kafka events) is the authoritative answer for "does this user have access to this resource" and will be the basis for any scope isolation / namespace customization — including agent memory namespacing. The group bookmarked this for deeper exploration.

- **Bias toward cheaper LLMs over classical ML for classification:** Reviewing Linda's use-case document, Spencer noted that anywhere Linda tagged "ML" (recommendation, smart defaults, classification), Pearl will bias toward a cheaper LLM with good context over a traditional model — LLMs are strong at classification given the right framing. Mike surfaced the NetDev team's FNQL rules engine (regex + proximity for work order categorization, ~4 FTE maintaining it) as a prime candidate for this approach and something Steven has considered.

- **Agentic interactions as lineage/explainability capture:** Spencer's thesis: if decisions flow through agentic interactions, Pearl gets semantic understanding of how decisions were reached without having to retroactively structure/codify everything on the backend. Cory previously flagged a lineage problem in provider match decisions — Spencer sees agentic capture as a partial answer.

- **DW/reporting is a downstream constraint, not Pearl's problem to solve directly:** Mike cautioned that stakeholders will assume "if the agent knows it, we should be able to report on it." Spencer acknowledged this may eventually require change-data-capture from vector memory into DBT, but it's not a launch requirement. Short-term surfacing is via agent tracing in observability (with production constraints on what can be traced). Mike's framing: "Don't over-promise that this is going to solve the full lineage problem."

- **Portability fear is real — Batch API and Bedrock KBs are the hedge:** Chuck surfaced the portability concern (Chuck's framing: "if we realize agent core memory doesn't work and we got to scale or... we don't want to have to rewrite it"). Spencer's hedge: if they can use Batch API hydration to control what goes into long-term memory, they can also replicate/copy that data for internal FN use. If AgentCore can't give them that level of control, they may end up rolling their own stack (Neo4j/LangChain, GraphRAG, Pinecone/Weaviate) — and they want to avoid a messy migration project after they're already at scale. This is why Bedrock Knowledge Bases warrant comparison: if KBs offer better control and dedicated per-buyer isolation, they may be the cleaner answer than AgentCore Memory.

- **Neighbor leakage risk with shared indexes:** Spencer's concern about AgentCore Memory's isolation strategy — if AWS is doing metadata filtering over shared indexes rather than dedicated indexes per scope, they risk returning adjacent vectors from other tenants (and the filtering becomes slow). A key question for the specialist.

- **Steven sidebar (off-topic but tracked):** Chuck confirmed from a Cory sidebar that Steven has been trying to reframe "let me vibe code to production" requests after being told no. The upcoming Steven meeting will be about establishing boundaries. Cory reportedly said, "People need to leave Spencer alone." Maria wants Spencer to attend to set team boundaries; Spencer reluctant but agreed it's worth setting expectations.

---

## Action Items

- **Chuck Haines:** Raise the customer-facing vs AI-dev-tooling role delineation with Cory at tomorrow's 1:1 and push to have it stated explicitly org-wide.
- **Mike Mueller + Chuck Haines:** Review Spencer's "Advanced Agent Memory Questions" doc and give a thumbs up / thumbs down / "hold" before Spencer posts it in the external affiliation channel and tags Faren. Mike committed to getting to it later today.
- **Spencer Moulton:** Reorder the external specialist questions to lead with (1) Batch API for memory hydration and (2) Bedrock Knowledge Bases as an alternative to AgentCore Memory, then move namespace customization lower.
- **Group (implied):** Bookmark the Resource Authorization Service as the basis for agent scope isolation / memory namespacing and explore further in a follow-up.
- **Chuck Haines (implied):** Attend the Steven meeting tomorrow alongside Spencer to help establish boundaries on the "vibe code to production" asks.
