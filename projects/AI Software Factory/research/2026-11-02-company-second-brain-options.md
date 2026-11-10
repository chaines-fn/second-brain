# Company second brain (companyOS): options and a suggested approach

**Research question:** Can the company second brain (MOI-204, formerly companyOS) still be git-backed and secured, and if not, what should it be built on? Options reviewed: git, OpenWiki, Onyx, self-hosted wikis, knowledge graphs.
**Date:** 2026-11-02
**Scope:** Authored knowledge store for the Fai factory (Warp POC) and Q4 M2 "CompanyOS built for Engineering." Out of scope: index-in-place of Drive/Confluence/Slack (deferred), personal brains, vendor SaaS (excluded by the self-host constraint).

**Requirements from Chuck's interview (2026-11-02):**
- Threats: sensitivity tiers (some content must be hidden from most people and agents) and agent blast radius (a clone gives an agent everything, plus history, with no per-read audit).
- A few tiers (3 to 5, e.g. Company, Engineering, Leadership, Restricted). Folder/space-level boundaries are enough; no per-document ACLs.
- Hybrid access: autonomous agents read with their own scoped identity; interactive runs inherit the human's access for that session (matches the 10/05 vision, Q12).
- Authored store: the brain is the canonical place people and agents write.
- Markdown + PR review is nice to have, not required.
- Self-hosted in FN's cloud, MCP-first, usable for Q4 M2 (readout targeted 12/18).

---

## Summary

The git approach is not the problem. Giving agents git access is. Git's security boundary is the repository, so a single companyOS repo with folders per sensitivity level can't be secured, and any agent that clones it holds the whole thing and its history with no record of what it read. But with only a few tiers, one repo per tier gives clean, coarse boundaries, and if agents never clone those repos and instead read through an FN-run MCP service that checks who is asking, both of Chuck's concerns are addressed: tiers are enforced by the service and repo permissions, and blast radius shrinks to what one query returns, with every read logged.

None of the alternatives replace that design cleanly under these constraints. OpenWiki is an agent-maintained markdown wiki engine with no permission model, so it's a good authoring and staleness engine to use inside the git option, not a security answer. Onyx is the best permission-aware retrieval layer and its MCP server enforces the caller's permissions on every edition, but group-based document access (the thing tiers need) requires a paid Business or Enterprise license, and it is a search layer, not an authored store. Outline is the strongest self-hosted wiki for humans (collection-level permissions, built-in MCP with OAuth or API keys, self-hosted support), but it has no PR-style review for agent writes and its MCP permission behavior isn't documented. Knowledge graphs (Graphiti and similar) are useful for relationships, but the Graphiti MCP server calls itself experimental and documents no authentication, so a graph should be a derived index, not the store.

**Suggested approach:** make the brain MCP service the decision, and treat the store behind it as swappable. For Q4, build "git per tier behind a brain MCP": one private repo per tier, OKF-style markdown maintained by Factory agents (OpenWiki or the existing meeting-synthesis/project-sync agents), agent writes only as PRs the tier owner reviews, and a small FN-hosted MCP service that authenticates the caller, resolves allowed tiers from Okta groups, and searches separate per-tier indexes. Keep Outline as the planned swap-in if non-engineer authors (leadership, product) won't work in GitHub, and keep Onyx Enterprise for when index-in-place of Drive/Confluence/Slack comes back into scope.

---

## Findings

### 1. Why the original git design is hard to secure, and what fixes it

The FAI PRD (2026-10-08) assumes "Git is the source of truth: companyOS repos" with agents writing only through owner-reviewed PRs (FR-25, FR-26). The weak points:

- **Repo is the only access boundary.** GitHub has no folder- or file-level read permissions. Anything in a repo is readable by every principal that can read the repo.
- **History is permanent.** A Restricted doc committed to the Engineering repo by mistake stays in history (and in every existing clone) until someone rewrites history. Removing the file doesn't fix it.
- **Clones are invisible.** Once an agent sandbox has a token and clones, there's no record of which documents it read, what it sent to a model, or where copies went. The Warp sandbox, its model providers, and its logs all become places that content can sit.
- **Tokens leak into sandboxes.** The PRD already flags that the per-run GitHub token's visibility to the agent shell is unverified and assumes it's visible.

What fixes it for a few-tier model:

1. **One repo per tier**, never folders-per-tier in one repo. Repo access maps to Okta groups through GitHub teams.
2. **Agents don't get git credentials for brain repos.** Factory agents (Warp, Claude Code, Copilot) get the brain MCP, not clone access. Only the brain service and human authors touch the repos directly.
3. **Separate index per tier.** Don't rely on a single vector index with a tier filter. Permission-aware RAG guidance consistently says authorization has to happen in trusted retrieval code before content reaches a model, and pre-filtering beats post-filtering. Physically separate indexes per tier make a cross-tier leak structurally impossible rather than dependent on a filter being right.
4. **Writes are proposals.** The MCP exposes `propose_change`, which opens a PR in the right tier's repo for the owner to review (keeps FR-26). Agents can't push.
5. **Every read is logged** with principal, on-behalf-of human (for interactive runs), tier, and document IDs. Ship these to the existing `ai-harness-otel` Datadog index alongside run telemetry.
6. **Pre-commit and CI tier checks.** A CI check on each tier repo flags documents whose front matter declares a higher tier than the repo, plus secret scanning, to stop the "wrong repo, permanent history" failure before merge.

This keeps the parts of the original design that were right (markdown, PR review, owner per OS, agents maintain content on triggers) and moves the security boundary from "who can clone" to "who can query."

### 2. OpenWiki (langchain-ai/openwiki)

An MIT-licensed CLI that writes and maintains a linked markdown wiki for a code repository, with a personal mode that builds a local wiki from sources like Notion, Gmail, Slack, and git repos. Notable pieces: durable page jobs that resume after interruption; "Grounded Claims," which tie each factual statement to versioned source evidence and flag it when the evidence changes; output in Google's Open Knowledge Format (OKF) with provenance front matter; MCP tools for both generation and retrieval (`openwiki_search`, `openwiki_read`). About 17.5k stars; release history and last commit date weren't visible.

What it doesn't do: it describes no user or role permission model. Storage is local files in the repo. The controls are `.openwikiignore`, a loopback-bound visualizer, and guidance to use single-repo tokens in CI.

Fit: strong as the engine that keeps brain content current, and its grounded-claims staleness model lines up with FR-29 (status, re-review dates, non-current treated as a hint). It is not a security layer, and it's designed around repo code, so pointing it at meeting records and decisions would need a spike. Use it inside the git option; don't expect it to solve tiers.

Related: **OKF** was published by Google Cloud in June 2026. It's a directory of markdown files with YAML front matter (only `type` is required) that formalizes the "LLM wiki" pattern. Coverage I found doesn't mention sensitivity or access metadata, so a `tier` field would be an FN extension. Adopting OKF-shaped front matter is cheap and keeps the content portable to Outline, Onyx, or Google's Knowledge Catalog later.

### 3. Onyx (formerly Danswer)

Self-hostable enterprise search and chat. Permissions are group-driven as of v4.7 ("permissions expand": being in more groups only adds access). Documents can be private, public, or auto-synced from source permissions, and access can be restricted to selected groups. Service accounts get access from the groups on their API key.

Two findings matter most:

- **The Onyx MCP server is on all editions, on by default in the Docker Compose and Helm deployments, and enforces the caller's document permissions.** It exposes `search_indexed_documents` (filterable by source, document set, time, and agent), `search_web`, and `open_urls`. Auth is OAuth for interactive clients (15-minute access tokens, 30-day authorization) or a personal access token with `read:search` for bearer auth. That maps well to the hybrid model: OAuth when a human is in the loop, a scoped service account per agent otherwise.
- **Group-based document access is paid.** On Community Edition, documents are visible only to the creator or to everyone if public. Custom groups, group-based document access, and "restrict access to groups" all require Business, Enterprise, or Onyx Cloud. So the free version can't enforce tiers.

Fit: Onyx is the best off-the-shelf answer to "permission-aware retrieval over many sources," and it would be the right layer if index-in-place of Drive, Confluence, Slack, and Linear comes back in scope. It's a retrieval layer, not an authoring tool, so it doesn't satisfy the authored-store requirement on its own. Running it self-hosted with tiers means a license negotiation, which puts it at risk for Q4.

### 4. Self-hosted wikis (Outline, BookStack, XWiki, Wiki.js)

**Outline** is the closest fit. It has had a built-in MCP server in every workspace since February 2026 (search, read, create and edit documents, comments), it works on self-hosted installs at `<your-domain>/mcp` over Streamable HTTP, and it supports OAuth or a named API key as a bearer token. An admin can turn MCP off workspace-wide. Permissions are set per collection, which maps directly to a few tiers.

Gaps: Outline's MCP docs don't say how permissions are enforced or what scopes exist. The reasonable reading is that an MCP session acts as the authenticated user, so each agent would need its own Outline user, but that needs to be verified. There's no PR-style approval for agent writes, so an agent with edit rights on a collection writes directly. Search is the wiki's own, not semantic. The license is the Business Source License, which allows internal self-hosting but isn't OSI open source. That may matter to procurement or security review. The source for that comparison is an XWiki hosting partner, so confirm against Outline's own repo.

**XWiki** (LGPL) has the finest permissions (wiki, space, page) and LDAP/AD integration, but it's heavier and I found no first-party MCP server. **BookStack** (MIT) has role-based and some entity-level permissions but no first-party MCP. **Wiki.js 3.0** has been in beta for years with no firm release date, so I'd rule it out.

Fit: Outline is the right answer if most authors are humans outside engineering and a GitHub PR flow will slow them down. It's weaker than git on agent-write governance and auditability.

### 5. Knowledge graphs (Graphiti/Zep, Neo4j, and similar)

Graphiti builds a temporal knowledge graph on FalkorDB or Neo4j and ships an MCP server. Its README calls the server "experimental" and documents no authentication or authorization for the MCP endpoint. Isolation is a `group_id` namespace, and the docs don't say whether retrieval enforces it across groups. Telemetry is on by default.

Fit: graphs are good at the relationship questions the factory will ask (which service owns this entity, which TDR governs this bounded context, who decided this). That's the ground Joe's entity topology and the Workato-POC MCP (Chuck's 10/30 direction: an MCP that knows bounded contexts and entity topology) already cover. A graph should be built from the authored store as a derived index per tier, not be the store, and nothing in this space is ready to be a security boundary today.

### 6. Option comparison against the interview requirements

| Requirement | Git per tier + brain MCP | Outline (self-hosted) | Onyx (self-hosted) | OpenWiki | Graph (Graphiti) |
|---|---|---|---|---|---|
| Few tiers enforced | Yes (repo per tier, separate indexes) | Yes (collection per tier) | Paid plans only | No permission model | Not documented |
| Agent blast radius / per-read audit | Yes, by design of the MCP | Partial (MCP behavior undocumented) | Yes (MCP enforces caller permissions) | No | No |
| Hybrid identity | Build it (Okta groups + agent principals) | Per-user OAuth or API key | OAuth + service accounts | No | No |
| Authored store | Yes | Yes | No (retrieval only) | Yes (agent-written) | No |
| Reviewed agent writes | Yes (PRs) | No | n/a | Via git | No |
| Self-host / MCP-first | Yes / yes | Yes / yes | Yes / yes | Yes / yes | Yes / experimental |
| Q4 M2 feasibility | High; small service on existing pieces | Medium; deploy, SSO, migrate | Low with tiers (license) | Spike needed | Low |

---

## Suggested approach

**1. Make the brain MCP the contract.** Define a small, stable interface that every agent and tool uses: `search(query, filters)`, `read(doc_id)`, `list(tier, path)`, and `propose_change(tier, path, content, rationale)`. This matches the posture from the 10/05 vision (build the differentiating layers, keep vendors swappable behind contracts) and means the store can change later without touching Warp agent definitions or skills.

**2. Q4 store: one private repo per tier.** Start with two tiers that M2 actually needs: Engineering (FNOS for engineering) and Leadership (Engineering Senior Leadership OS). Add Company-wide and Restricted when there's content for them. OKF-style markdown with a required `tier`, `owner`, `status` (current, under review, superseded) and `review_by` in front matter. CI on each repo checks front matter tier against the repo, runs secret scanning, and blocks merge on mismatch.

**3. Brain MCP service in FN's cloud.** Runs in the dedicated factory AWS account (MOI-198). Authenticates via Okta: OAuth for humans and interactive runs (on-behalf-of the human), client credentials for autonomous agent principals with a named owner. Resolves allowed tiers from Okta groups, queries only those tiers' indexes, and logs every call to Datadog. It's the only thing holding read tokens for the brain repos. Rebuilding indexes on merge per tier (pgvector or OpenSearch with one index per tier) keeps it simple.

**4. Agents write, owners approve.** Meeting synthesis, project sync, and the staleness check (the Q4 plan's agent path) write through `propose_change`, so every write is a PR the tier owner reviews. Spike OpenWiki's grounded-claims model for the staleness check rather than building one.

**5. Decide the human front end by watching authors, not up front.** If leadership and product authors are comfortable with GitHub (or with asking Claude to propose changes for them), stay on git. If they aren't, swap Outline in behind the same MCP contract, with collections per tier, and accept that agent writes need a different review mechanism (for example, agents write to a "proposed" collection that owners promote).

**6. Leave index-in-place and the graph for after M2.** When Drive, Confluence, Slack, and Linear need to be searchable, Onyx Business or Enterprise is the strongest self-hosted option and its MCP can sit beside the brain MCP (or be federated through it). The entity graph builds off the authored store plus Joe's topology.

**A measurable KR3 definition this supports** (open item 4 in the Q4 OKR): "Factory agents in the M3 pilot retrieve brain content through the brain MCP on at least N% of runs, with zero cross-tier reads in the audit log, and every brain document has an owner, a status, and a review-by date." Numbers to be set with Cory.

---

## Questions for Chuck before building

1. Who are the expected authors of the Leadership tier, and will they work in GitHub? This is the main thing that decides git vs. Outline.
2. Is any Restricted content (people, comp, customer PII) actually going into the brain in Q4, or can Restricted be deferred? Deferring it lowers risk a lot.
3. Should Warp's own agents call the brain MCP directly, or should brain access go through the planned MCP gateway (Workato or AgentCore Gateway) for tool-tier approval?
4. Is "Fai monorepo vs. warp-engineering-factory" settled enough to say where the brain MCP service code lives?

---

## Key uncertainties

- **Outline MCP permission enforcement and scopes** aren't documented. Verify on a test install before relying on it.
- **Outline's license** comes from a third-party comparison written by an XWiki hosting partner. Confirm in Outline's repo.
- **Onyx pricing** for a self-hosted Business or Enterprise license with a small seat count wasn't found. Get a quote if index-in-place returns.
- **OpenWiki** release cadence and last commit date weren't visible, and it's built around code repos. Fit for meeting records and decisions needs a short spike.
- **Warp sandbox behavior** (what Warp retains from agent runs, egress controls) is still undocumented per the 10/08 Warp research. The brain MCP limits what reaches Warp but doesn't control what Warp keeps.
- **OKF** coverage found doesn't mention sensitivity or access metadata, so `tier` is an FN extension.

---

## Sources

Internal:
- Linear MOI-204 "Company Second Brain" (created 2026-11-02, via #moissanite-eng-notifications)
- [program/OKRs/q4-2026-okrs.md](../program/OKRs/q4-2026-okrs.md) (KR3, M2, open item 4)
- [artifacts/2026-10-08-fai-prd.md](../artifacts/2026-10-08-fai-prd.md) (knowledge row, FR-25 to FR-29, token visibility)
- [brainstorm/2026-10-05-full-software-factory-vision.md](../brainstorm/2026-10-05-full-software-factory-vision.md) (Q7, Q12, Q18)
- [research/2026-09-01-company-brain-vendor-evaluation.md](./2026-09-01-company-brain-vendor-evaluation.md)
- [research/2026-10-08-warp-factories-vs-fai.md](./2026-10-08-warp-factories-vs-fai.md)
- [program/overview.md](../program/overview.md) (Warp POC direction, 10/30 to 11/02)

External:
- [langchain-ai/openwiki (GitHub)](https://github.com/langchain-ai/openwiki/)
- [Onyx: Understanding permissions](https://docs.onyx.app/admins/permissions/understanding_permissions)
- [Onyx: What's changing in permissions (v4.7)](https://docs.onyx.app/admins/permissions/whats_changing)
- [Onyx: MCP server](https://docs.onyx.app/overview/onyx_anywhere/mcp_server)
- [Outline changelog: MCP server (Feb 2026)](https://www.getoutline.com/changelog/mcp)
- [Outline docs: MCP setup](https://docs.getoutline.com/s/guide/doc/mcp-6j9jtENNKL)
- [XWiki vs BookStack vs Outline (MassiveGRID)](https://massivegrid.com/blog/xwiki-vs-bookstack-vs-outline-comparison)
- [Wiki.js v3 release date discussion](https://github.com/requarks/wiki/discussions/4652)
- [Graphiti MCP server README](https://github.com/getzep/graphiti/blob/main/mcp_server/README.md)
- [The Decoder: Google Cloud's Open Knowledge Format](https://the-decoder.com/google-clouds-open-knowledge-format-turns-scattered-docs-into-markdown-files-for-ai-agents/)
- [Wavect: RAG permissions across SharePoint, Confluence, Drive](https://wavect.io/blog/rag-permissions-sharepoint-confluence-drive/)
