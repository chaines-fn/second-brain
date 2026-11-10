# Company Brain vendor evaluation

Date: 2026-09-01
Author: Chuck Haines (research pulled via Claude)

## Why this matters

FN already has a live decision touching this space: the Moissanite knowledge plane's Tier 2 (Shared Library) candidate changed 2026-06-30 from Unblocked to **Guru** (see [tech-context.md](../knowledge/tech-context.md)), and that pilot has stalled — NDA signed 6/30, no POC as of this writing. This report is broader than that one decision: it looks at whether any of these products can serve as a general-purpose "Company Brain" — a knowledge layer usable by both AI agents and humans, not just the AGENTS.md/skills knowledge plane for Moissanite specifically.

## Criteria

1. Serves as Company Brain for AI agents and humans — one knowledge layer both can query, not two separate systems.
2. Supports human-readable/editable documentation — people can write and maintain the knowledge base directly, not just have it auto-scraped.
3. Advanced semantic search and/or knowledge graph — understands meaning and relationships, not keyword match.
4. Supports a personal brain — a place for individual context/notes that feeds personal workflows, separate from (but connected to) the company-wide layer.

## The five requested products

### Glean

Enterprise search and knowledge platform built around a permissions-aware **Enterprise Graph** — it indexes content across connected apps (Slack, Google Workspace, Microsoft 365, Salesforce, GitHub, Confluence, etc.) and maps relationships between people, content, workflows, and apps. As of its Fall 2025/2026 release, it added a **personal graph** per employee reflecting their projects, collaborators, and work style, layered under a third-generation Assistant with agents, Deep Research, and an Agentic Engine.

- Company Brain (agents + humans): strong. This is Glean's core positioning — "superintelligent enterprise" via one shared graph queried by chat, search, and agents.
- Human-editable docs: weak. Glean is a search/indexing layer over content that lives elsewhere (Slack, Confluence, Drive, etc.) — it doesn't give people a native place to author and maintain knowledge. You'd still need a docs tool underneath it.
- Semantic search / knowledge graph: strongest of the five. The Enterprise Graph is a genuine knowledge graph, not just vector search, and it's the most mature implementation on the market according to third-party comparisons.
- Personal brain: yes, via the new personal graph, but it's derived automatically from activity rather than something a person curates directly.

Disadvantages: indexing lag (content must be copied/re-indexed, so results can be stale relative to the source), occasional answer/citation inaccuracy inherent to any RAG system, and it stops at search/discovery — it doesn't execute workflows end to end. Pricing is opaque: no public rate card, informally reported around $50/user/month with a ~100-seat minimum, median enterprise contracts around $97,500/year, plus separate indexing/infrastructure costs that scale with data volume.

### Guru

Started as a verified-answers wiki, has evolved into what it calls an "AI Agent Center" — a **Knowledge Agent** that answers questions in plain language (in Slack, Teams, Chrome, or via MCP/API from other AI tools), built on top of "Verified Truth": answers are generated only from content a human subject-matter expert has explicitly reviewed and approved, not from an unfiltered crawl. 2026 additions: Federated Search (indexes Google Drive/Box/Confluence without migrating content in) and a Slack MCP integration (agents can query live Slack conversations).

This is the product FN is already piloting for the Tier 2 shared-knowledge-library slot.

- Company Brain (agents + humans): strong, and it's the only one of the five explicitly built for AI agents to consume via MCP/API, not just a chat UI for humans.
- Human-editable docs: strong — Guru Cards are the native authoring unit, explicitly human-written/editable, with a verification workflow (expiry, owners, review cadence) baked in. This is Guru's real differentiator versus Glean.
- Semantic search / knowledge graph: has agentic/semantic search that reasons over verified cards plus federated content, with cited, permission-aware answers — real semantic search, but not a graph in the Glean sense (no explicit entity-relationship mapping).
- Personal brain: weak/absent in what surfaced. Guru is oriented around team- and org-owned cards, not individual context capture.

Disadvantages: steep initial setup/learning curve for governance workflows, UI has limited customization, weaker for tacit knowledge or deep collaboration (it's built for explicit Q&A, not brainstorming), and a 10-seat minimum makes it costly for small teams (~$1,250/month for 50 seats before negotiation, enterprise pricing fully custom).

### Slite

Positioned as a self-maintaining team knowledge base with an AI layer ("Ask"/"Super") doing semantic search with sourced, cited answers, plus doc verification that flags stale or drifting content (cross-referencing Slack, Linear, GitHub, Intercom, and 20+ tools) and proposes fixes for a human to approve.

- Company Brain (agents + humans): moderate. It reasons about staleness against external tools, but the "Ask" AI historically only searched inside Slite itself — third-party critics noted it couldn't see Google Docs/Confluence/Notion content, limiting it as a true cross-system brain, though the "Super"/federated expansion is closing that gap.
- Human-editable docs: strong — this is Slite's whole premise, a clean editor for team-authored docs, arguably the most writing-friendly of the five.
- Semantic search / knowledge graph: has real semantic search (dense vector embeddings + reranking) with cited answers; no knowledge graph.
- Personal brain: not evident — Slite is team/docs-oriented, no personal-context layer surfaced in research.

Disadvantages: free tier caps at 50 docs; the editor is basic for power users (formatting, tables, LaTeX are weak spots); AI answers on the Standard plan are capped at 30/user/month; pricing recently restructured, pushing real AI/search features into a "Knowledge Suite" tier at $20/user/month (10-seat minimum) versus $8/user/month Standard — a steep jump; no external/customer-facing publishing.

### Superhuman Docs (formerly Coda)

Coda rebranded into Superhuman Docs in July 2026 as part of the Superhuman/Grammarly consolidation. It's a flexible doc/table surface with "Docs AI" (beta) that can read, plan, write, track, and build inside any doc, plus MCP support (beta) to connect to Claude, ChatGPT, Cursor, and external data (Jira, Salesforce). It markets a "living knowledge base" where docs stay current automatically.

- Company Brain (agents + humans): still forming. "Coda Brain," its AI enterprise-search app, is in Private Preview — i.e., the company-wide brain layer isn't GA yet, and the current suite is explicitly aimed at individuals and small teams; larger orgs are on a waitlist for team-plan options.
- Human-editable docs: strong — this is Coda's original strength (structured docs, tables, apps), carried forward.
- Semantic search / knowledge graph: unclear/unproven — no evidence surfaced of a mature semantic search or graph layer; Coda Brain is the relevant effort but is pre-GA.
- Personal brain: plausible fit given the doc-and-table flexibility and MCP hooks, but not yet a named, distinct feature the way Notion's Personal Agent or Glean's personal graph are.

Disadvantages: AI credits are rationed by plan tier with no published counts, making usage hard to forecast; enterprise/team rollout is genuinely behind (waitlist, not GA); post-merger affiliate/partner program quality reportedly degraded. This is the least enterprise-ready of the five for a Company Brain use case today — worth revisiting once Coda Brain leaves Private Preview.

### Notion

Workspace tool (docs, wikis, databases) with Notion AI layered in: Enterprise Search (cross-connected-app retrieval), a personal Notion Agent (must be actively directed) and newer Custom Agents (run in the background, connect via MCP to Linear, HubSpot, Figma, Slack).

- Company Brain (agents + humans): moderate — Enterprise Search is genuinely useful for fragmented-knowledge retrieval, but it's retrieval-only ("what did this thread conclude") rather than action ("send a follow-up in that thread"), and it only searches what's connected, not a unified graph.
- Human-editable docs: strongest of the five for general-purpose team documentation — this is Notion's core identity, and it's the tool most people already know.
- Semantic search / knowledge graph: search yes; graph no — Notion does not build a deep, personal or organizational knowledge graph natively (third-party tools like IVGraph exist specifically to fill that gap), so relationship-mapping is not native.
- Personal brain: yes, most directly of the five — the Personal Agent is explicitly a personal, user-directed assistant over your own workspace content, plus AI meeting notes and personal note-taking are core use cases.

Disadvantages: AI is Notion-workspace-scoped — anything spanning Notion + a CRM + a spreadsheet tool + a design tool still needs external orchestration (Zapier/Make); database performance degrades noticeably above 5,000 records and badly above 10,000; pricing for full AI (Enterprise Search, Personal Agent, Custom Agents) requires the Business tier (~$19.50-20/user/month) or Enterprise, with Custom Agents metered separately by credits.

## Other products worth considering

Based on the criteria, a few categories of alternatives came up that FN doesn't have on the list:

**Onyx (formerly Danswer)** — open-source, self-hostable enterprise search/chat/agent platform: 40+ connectors (Slack, Notion, Confluence, GitHub, Drive, Salesforce, Zendesk, Jira, Linear), permission-aware retrieval, supports Anthropic/OpenAI/Google/Azure OpenAI models or self-hosted Llama/Mistral/Qwen. Notable because it's MIT-licensed and can run entirely inside FN's own infrastructure — relevant if data residency or vendor lock-in is a concern that Glean's opaque, indexed-copy model doesn't address well. Free community tier or $20/user/month cloud. Worth a look purely because it's the one option here that's genuinely self-hostable at low cost, which fits a platform-engineering-minded org.

**Hebbia** — built for deep, multi-step agentic research across large document sets (financial/legal-style workflows) rather than simple Q&A retrieval. Probably overkill/mis-fit for a general Company Brain, but worth naming if there's ever a need for "agent does a 30-step research task across thousands of documents."

**Microsoft 365 Copilot (Copilot Search / Microsoft Graph)** — if FN's collaboration stack leans Microsoft 365 (worth confirming — the enterprise-search plugin here has an `ms365` connector), this is a "already paying for it" option: a universal AI search layer across Outlook, Teams, SharePoint, OneDrive, embedded in tools people already use. Weaker as a distinct knowledge graph/verified-docs product, stronger as a zero-new-vendor baseline to benchmark the others against.

**Moveworks** (now part of ServiceNow, acquired December 2025) — conversational AI assistant + enterprise search with a genuine knowledge graph and connector depth comparable to Glean, paired with an agentic reasoning engine. Relevant if FN's ServiceNow footprint (if any) makes this a natural extension rather than a new vendor relationship.

**Coveo / Elastic Enterprise Search** — Coveo is ML-relevance-tuned for customer support/ecommerce search; Elastic is the infrastructure-control option for engineering-heavy teams that want to run their own search stack. Both are more "search infrastructure" than "Company Brain," but worth naming since they anchor the build-vs-buy spectrum.

**On the personal-brain side specifically** — none of the five requested products (except Notion, partially) are built AI-first around personal knowledge capture. If a true "personal brain" matters as its own deliverable (not just a feature bolted onto a team tool), the more purpose-built options are **Mem** (write freely, AI auto-organizes and links notes), **Reflect** (fast networked notes with built-in AI, individual-focused), and **Rewind** (Mac-based recall search over your own activity/notes by meaning, not exact words). These are individual-subscription tools, not enterprise platforms, so they'd sit alongside a Company Brain rather than replace one — but they're the right reference point if "personal brain" is meant literally rather than as a feature of the org-wide tool.

## Bottom line for FN's criteria

No single product here cleanly wins all four criteria:

- Glean is the strongest on knowledge graph + agent/human brain, weakest on human-authored docs (it indexes, it doesn't author).
- Guru is the strongest on human-editable, verified docs + MCP/agent access — and is already FN's live pilot — but has no real personal-brain layer and no true knowledge graph.
- Slite and Notion are strong writing/authoring tools with real (Notion) or growing (Slite) enterprise search, but neither has a graph, and only Notion has a named personal-agent feature.
- Superhuman Docs is the least mature for this specific use case right now — its Company Brain equivalent (Coda Brain) is still in Private Preview.

Two live tensions worth flagging back to the team: (1) Guru's pilot has been stalled for weeks with no POC — this research doesn't resolve that, but if the stated reason is scope or resourcing, Onyx is a lower-cost, self-hosted way to prototype the "agents + humans query one knowledge layer" question in parallel without a new procurement cycle. (2) If a personal-brain requirement is meant to be substantial (not just "can a person store some private notes"), none of Glean/Guru/Slite/Notion/Superhuman Docs treat that as a first-class feature yet — it may be worth scoping that as a separate, smaller tool decision (Mem/Reflect-class) rather than expecting the org-wide Company Brain vendor to also solve it well.

## Sources

- [Glean Enterprise Graph](https://www.glean.com/enterprise-context/enterprise-graph)
- [Glean fall 2025/2026 release: Enterprise Graph, personal graph, Assistant](https://www.glean.com/blog/live-fall-25-main)
- [Glean knowledge graph guide](https://www.glean.com/resources/guides/glean-knowledge-graph)
- [phData: How to Create a Company "Brain" with Glean](https://www.phdata.io/blog/how-to-create-a-company-brain-with-glean/)
- [Glean pricing breakdown](https://coworker.ai/blog/glean-pricing)
- [Glean review — indexing lag, accuracy, workflow limits](https://www.gosearch.ai/blog/what-is-glean-search/)
- [Guru: AI Knowledge Base guide](https://www.getguru.com/reference/ai-knowledge-base)
- [Guru vs Glean — verified AI vs enterprise search](https://techplustrends.com/guru-vs-glean-2026-enterprise-ai-knowledge-tools/)
- [Guru review — pros/cons/pricing](https://www.siit.io/tools/trending/guru-review)
- [Guru AI review — setup/learning curve](https://bunnydesk.ai/blog/guru-ai-review/)
- [Slite: LLM Knowledge Base guide](https://slite.com/learn/llm-knowledge-base)
- [Slite pricing 2026](https://bunnydesk.ai/blog/slite-pricing/)
- [Slite review — limitations](https://firstsales.io/brand-review/slite/)
- [Superhuman launches Superhuman Docs](https://blog.superhuman.com/superhuman-launches-superhuman-docs/)
- [Superhuman Docs knowledge hubs](https://superhuman.com/docs/use-cases/knowledge-hubs)
- [Superhuman Docs review — Coda Brain private preview, waitlist](https://theaiagentindex.com/agents/coda-ai)
- [Notion AI review 2026](https://www.eesel.ai/blog/notion-ai-review)
- [Notion knowledge graph gap — IVGraph](https://ivgraph.com/journal/ultimate-notion-knowledge-graph-guide-2026/)
- [Notion MCP / Custom Agents 2026](https://kansei-link.com/en/insights/notion-mcp-deep-dive-2026)
- [Onyx (Danswer) — open source Glean alternative](https://onyx.app/insights/glean-alternatives)
- [Onyx overview and connectors](https://medium.com/@airabbitX/powerful-genai-enterprise-search-with-onyx-c38c2e25fac2)
- [Best AI Knowledge Base 2026 — comparison across Glean/Guru/Hebbia/Notion/Coveo/Elastic](https://www.brewsterconsulting.io/best-ai-knowledge-base-2026)
- [Moveworks / ServiceNow acquisition and knowledge graph](https://www.kore.ai/blog/best-enterprise-search-software)
- [Personal Knowledge Management 2026 — Mem, Reflect, Rewind](https://www.remio.ai/post/personal-knowledge-management-guide-2026)
