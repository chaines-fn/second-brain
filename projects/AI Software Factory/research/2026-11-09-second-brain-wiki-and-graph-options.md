# Second brain, part 2: self-hosted wiki and knowledge graph options

**Research question:** What can we self-host in FN's AWS account, secured with OAuth/OIDC (Okta), to build the wiki and knowledge-graph part of the company second brain (MOI-204) first, with Onyx possibly in front later?
**Date:** 2026-11-09
**Builds on:** [2026-11-02-company-second-brain-options.md](./2026-11-02-company-second-brain-options.md)
**Requirements carried over:** a few sensitivity tiers (3 to 5, space or folder level), hybrid identity (agents use their own identity; interactive runs use the human's), authored store, MCP-first, reviewable agent writes preferred, self-hosted, usable for Q4 M2.
**Scope:** Wikis and graph stores/frameworks. Onyx, index-in-place, and personal brains are out of scope.

---

## Summary

The wiki market changed in the last two weeks in a way that helps us. Two fully open-source wikis now ship first-party MCP servers that authenticate through the wiki's own OAuth and run as the calling user, so the wiki's permissions apply to agents too. **Wiki.js 3.0** was tagged 2026-11-01 (I confirmed the tag and its MCP spec in the repo). **XWiki's MCP Server 1.0** came out at the end of October. Both map Okta group claims to wiki groups, both support tiers as path- or space-level permissions, and both have a review queue for changes from people (or agents) without write access. That means the wiki can be the secured store and the agent interface, without a custom brain MCP service for v1.

**Outline** still has the best editor and a polished MCP server that enforces collection permissions. But I confirmed in its source that self-hosted installs are permanently on the "Community" plan with no entitlements. That rules out Okta group sync. It's also BSL-licensed and has no approval flow. **Docmost** gates SSO, MCP, and page permissions behind its paid tier. **BookStack** has solid permissions and Okta group mapping but only community MCP servers.

For the knowledge graph, nothing open source gives you OIDC, server-enforced tier isolation, and an authenticated MCP server out of the box. Neo4j and Memgraph do, but only on their paid Enterprise editions. Graphiti's MCP server has no auth and its `group_id` is not a security boundary. So the graph should be a **derived index** rebuilt from the wiki, with **one physical graph per tier**, sitting behind **AgentCore Gateway** with an Okta JWT authorizer. That gateway is already the PRD's working assumption for tool access.

**Suggested approach:** run a one-week spike of Wiki.js 3.0 against XWiki, using Okta, two tiers, and Claude Code over MCP. Lean toward Wiki.js if it holds up, because it's markdown-native and has a git storage module, and its MCP design matches what we'd have built ourselves. Fall back to XWiki if Wiki.js 3.0 is too new to trust. For M2, treat the wiki's links plus typed front matter as the v0 graph. Build the real graph (Neptune Analytics per tier, compiled deterministically from front matter) after M2.

---

## Part 1: Wiki options

### Comparison

| Tool | License / paywall | Okta OIDC + group mapping | Tier boundary | First-party MCP | Reviewable agent writes | Storage | AWS shape | Maturity |
|---|---|---|---|---|---|---|---|---|
| **Wiki.js 3.0** | AGPL-3.0, nothing paywalled | Yes: `groupsClaim` + map groups, optional unassign | Groups + page rules by path (tier = path prefix) | **Yes.** OAuth 2.1 or personal token; acts as the token's owner; read + write | **Yes.** `suggest_edit` into approvals queue; writes require a reason and are marked "via MCP" in history | Markdown in Postgres; **git storage module** | Node app + Postgres (RDS) on ECS | **3.0.0 tagged 2026-11-01** |
| **XWiki** | LGPL-2.1, core + OIDC + MCP + Change Request all open | Yes: `oidc.groups.claim`, `oidc.groups.mapping` | Wiki / space / page rights, inherited (strongest model) | **Yes, 1.0 (late Oct 2026).** Uses wiki auth (OIDC, dynamic client registration); enforces user rights; read, create, edit | **Yes.** Change Request app with multi-approver merge | XWiki syntax in DB; markdown export partial | Java/Tomcat + Postgres + Solr | Very mature core; MCP brand new |
| **Outline** | BSL 1.1 (Apache-2.0 on 2030-10-23) | OIDC login yes; **group sync unavailable self-hosted** | Collections | **Yes.** OAuth or API key; enforces collection permissions in tool code; ~20 tools | No (drafts + history only) | Postgres + Redis + S3; MD export | ECS + RDS + ElastiCache + S3 | v1.12.0, very active |
| **Docmost** | AGPL core + commercial EE | **EE only** | Spaces (page permissions EE) | **EE only** | Page verification EE | Postgres + Redis + S3 | ECS + RDS + ElastiCache + S3 | v0.97, active |
| **BookStack** | MIT, nothing paywalled | Yes: groups claim to roles | Shelf / book / chapter / page | Community only, token-based | No | HTML in MySQL; MD export | PHP container + RDS MySQL | Mature, active |
| **MediaWiki + Semantic MediaWiki** | GPL-2.0+ | OIDC via PluggableAuth; group sync unverified | Namespace via Lockdown (weak for read restriction) | Third-party (ProfessionalWiki), OAuth as user | Via extensions | Wikitext | PHP + RDS | Mature |
| AFFiNE, SilverBullet, Basic Memory | Various | No multi-user Okta story | Workspace or none | AFFiNE static token, read-only | No | Varies | n/a | Not a fit as system of record |

### Notes on the front-runners

**Wiki.js 3.0: closest fit on paper.**
- Its MCP spec says the token "carries their groups and permissions," so page rules, locked pages, and unpublished drafts all apply to an agent the same way they apply to its owner.
- It ships its own OAuth 2.1 authorization server for remote MCP clients (discovery, dynamic registration, consent screen). Grant tokens are confined to the `/_mcp` route, and access is gated by a `use:mcp` group permission.
- `suggest_edit` turns an agent edit into an approval request when the caller lacks write access. That's the reviewable-write behavior from FR-26, without git.
- Content is markdown, and the git storage module can mirror pages to a repo. That mirror is useful for backups and for feeding the graph build. The repo then holds every tier, though, so lock it to the brain service account, or skip it for Restricted.
- **Risks.**
  - The release is 8 days old, after a four-year rewrite, and third-party coverage suggests waiting for patch releases.
  - Tiers are page rules inside one instance, not separate stores, so a bad rule leaks content. If a Restricted tier ever lands in the wiki, give it its own instance.

**XWiki: the mature, fully open choice.**
- It has the best permission model here (rights at wiki, space, and page level, with inheritance), long-standing Okta-compatible OIDC group mapping, and a real Change Request approval flow.
- Its structured objects and classes give a typed, semantic layer that Wiki.js lacks.
- **Trade-offs.**
  - It's heavier to run (Java, Solr) and the UI is dated.
  - Content isn't markdown-native.
  - The MCP server is as new as Wiki.js 3.0. Its docs warn that admin tokens inherit script rights, so agents must use least-privilege accounts.

**Outline: best UX, but the wrong license and identity story for self-hosting.**
- I confirmed in the source that `Team.plan` always returns the default `Community` plan, which has no entitlements. `SyncUserGroupsTask` checks the `GroupSync` entitlement before syncing, so a self-hosted Outline never syncs Okta groups.
- You'd have to manage groups in the app or script them through the API.
- No approval flow for agent writes.
- If the editor experience for leadership authors ends up mattering more than anything else, it's still a workable option, but it's the most friction for our security requirements.

---

## Part 2: Knowledge graph options

### Comparison

| Option | License / paywall | Auth | Tier isolation (server-enforced?) | MCP | How the graph is built | AWS shape |
|---|---|---|---|---|---|---|
| **Graphiti** (Zep CE is deprecated) | Apache-2.0 | None in the MCP server | `group_id` namespaces, **not enforced** | Reference server, no auth | LLM extraction; custom entity/edge types | Neo4j, FalkorDB, or **Neptune** |
| **Cognee** | Apache-2.0 | Built-in users/tenants/roles; OIDC unverified | Dataset-level permissions with separate DB per dataset | Yes, but API mode uses one token per instance | LLM extraction | Neo4j, Postgres, Neptune Analytics |
| **Neo4j** | Community GPLv3 / Enterprise commercial | **RBAC, label/property access, OIDC are Enterprise-only** | Enterprise: multiple DBs + fine-grained privileges | First-party; HTTP passes the bearer token to Neo4j (needs Enterprise/Aura) | Explicit modeling (Cypher) | EC2/EKS or Aura on AWS Marketplace |
| **Memgraph** | BSL / Enterprise | RBAC, label access, **Okta OIDC are Enterprise** | Multi-tenant DBs (Enterprise) | **First-party; validates the IdP's JWT and routes to a tenant DB from a claim** | Explicit + GraphRAG tooling | Self-managed |
| **Amazon Neptune** (DB / Analytics) | AWS managed | IAM SigV4 only | No label ACLs; **tier = separate graph**, controlled by IAM | AWS Labs MCP server (stdio, IAM) | Explicit (openCypher) | Fully managed |
| **Bedrock KB + GraphRAG** | AWS managed | IAM | One KB per tier | AWS Labs MCP server (stdio, IAM) | Automatic, opaque extraction from S3 | Managed, on Neptune Analytics |
| LightRAG, MS GraphRAG, FalkorDB, TrustGraph | MIT / MIT / SSPL / Apache | API keys at best; OIDC paid or absent | Weak or not documented | Community or unverified | LLM extraction | Self-managed |

### What this means for us

**The graph should be derived, not the store of record.** LLM-extracted graphs are non-deterministic and hard to review. The wiki is reviewable, versioned, and permissioned. For the core entities (service, owner, team, repo, bounded context, decision/ADR), put explicit typed fields and link conventions in page front matter. Compile them into the graph deterministically. Add LLM extraction (Graphiti or Cognee) only for softer relationships, and tag each extracted edge with the source page it came from.

**Isolate tiers physically: one graph per tier.** Label-level security exists only in the paid Neo4j and Memgraph editions, and Neptune has none. Separate graphs also stop extraction from creating edges across tiers. Building each tier's graph from that tier's pages alone guarantees nothing crosses.

**Front it with AgentCore Gateway.** The research confirmed that AgentCore Gateway supports Okta as an inbound JWT authorizer, can require specific claims (such as a group), and can aggregate several MCP servers into one endpoint. Per-caller authorization runs through Lambda interceptors that can read JWT claims, or through AgentCore Policy (Cedar, GA since March 2026). Whether Cedar rules can reference arbitrary JWT claims like groups is **unverified**, so plan on an interceptor for claim-to-tier routing. This matches the PRD's existing "AgentCore Gateway with Cedar policy" assumption for tool access.

**The AWS-native shape:** Neptune Analytics with one graph per tier and IAM roles per tier. The AWS Labs Neptune MCP server runs behind AgentCore Gateway, which validates the Okta token and routes each caller only to the tier graphs their claims allow. **The buy shape:** Memgraph Enterprise, whose MCP server already does Okta JWT validation and claim-based tenant routing, but that's a new vendor and license.

---

## Suggested approach

1. **Spike Wiki.js 3.0 and XWiki side by side (about one week).** Deploy each in a sandbox in the factory AWS account (MOI-198). Then for each:
   - Wire Okta OIDC with group claims and create two tiers (Engineering, Leadership).
   - Connect Claude Code over MCP using OAuth, once as a human and once as an agent service user.
   - Pass/fail checks:
     - An Engineering-only identity cannot search or read Leadership pages through MCP.
     - Agent writes from a read-only identity land in the approval queue.
     - Every agent write shows in history with its identity.
     - The tier is preserved when you export.
     - A Warp cloud agent can reach the MCP endpoint.
2. **Pick based on the spike.** Wiki.js if it passes and stability looks acceptable (better fit: markdown, git mirror, cleaner MCP). XWiki if Wiki.js 3.0 shows rough edges, since it's the lower-risk platform with the stronger permission model.
3. **Define the page schema now, whichever wiki wins.** Front matter fields: `tier`, `owner`, `status` (current, under review, superseded), `review_by`, plus typed relationship fields (`owns`, `owned_by`, `governs`, `decided_by`, `supersedes`). This covers FR-29, and it's the input the graph compiles from later.
4. **For M2, the wiki is the graph v0.** Links, backlinks, and typed front matter answer most "who owns / what governs" questions through MCP search and read.
5. **After M2, build the graph layer.** Compile front matter into one Neptune Analytics graph per tier on each page change. Expose it through the AWS Labs Neptune MCP server behind AgentCore Gateway (Okta JWT authorizer, plus an interceptor for claim-to-tier routing). Add Graphiti extraction per tier later if the explicit graph isn't enough.
6. **Onyx later** can index the wiki through its connectors and sit beside or in front of the wiki MCP. Keeping tiers as separate spaces or paths keeps that mapping simple.

This updates the 11/02 recommendation. A wiki with a permission-enforcing first-party MCP replaces "git per tier behind a custom brain MCP" for v1. Keep the brain MCP contract idea for when Onyx and the graph need to sit behind one endpoint, which AgentCore Gateway can provide.

---

## Key uncertainties

- **Wiki.js 3.0 stability.** It's 8 days old. The spike is the real test.
- **XWiki MCP maturity.** 1.0 is also brand new. Script-rights inheritance for admin tokens needs care.
- **Wiki.js 3.0 runtime requirements** (reported as PostgreSQL 17+ and a recent Node) come from a third-party article. Confirm in the install docs. Wiki.js 3.0 search quality (keyword vs. semantic) wasn't checked.
- **Whether Cedar policies in AgentCore can use group/tier claims directly.** Assume a Lambda interceptor until confirmed.
- **Cognee OIDC support**, **MediaWiki Lockdown read restrictions**, and **Neptune Database's one-graph-per-cluster model** were not confirmed from primary sources.
- **Warp's handling of what agents read** is still undocumented. The wiki MCP limits what reaches Warp, not what Warp retains.

---

## Sources

Wikis:
- [Wiki.js repo](https://github.com/requarks/wiki): tag `3.0.0` (commit dated 2026-11-01), `dev/specs/mcp.md`, OIDC and storage modules, verified by clone
- [Wiki.js 3 release coverage (byteiota)](https://byteiota.com/wikijs-3-release/)
- [Outline repo](https://github.com/outline/outline): `LICENSE` (BSL 1.1, change date 2030-10-23), `shared/utils/PlanHelper.ts`, `server/models/Team.ts`, `server/queues/tasks/SyncUserGroupsTask.ts`, verified by clone
- [Outline MCP docs](https://docs.getoutline.com/s/guide/doc/mcp-6j9jtENNKL), [Outline OIDC docs](https://docs.getoutline.com/s/hosting/doc/oidc-8CPBm6uC0I)
- [XWiki MCP Server extension forum post](https://forum.xwiki.org/t/xwiki-mcp-server-extension-available-for-testing/18720), [XWiki MCP Server docs](https://playgroundtemplate.xwiki.org/xwiki/bin/view/documentation/extensions/user/llm/mcp-server/), [XWiki MCP launch coverage](https://aitoolly.com/ai-news/article/2026-10-31-xwiki-launches-open-source-mcp-server-enabling-secure-ai-agent-integration-with-enterprise-knowledge)
- [XWiki OpenID Connect Authenticator](https://extensions.xwiki.org/xwiki/bin/view/Extension/OpenID%20Connect/OpenID%20Connect%20Authenticator/), [XWiki Change Request](https://extensions.xwiki.org/xwiki/bin/view/Extension/Application%20Change%20Request%20-%20UI/)
- [Docmost editions](https://docmost.com/docs/editions), [Docmost pricing](https://docmost.com/pricing)
- [BookStack OIDC auth](https://www.bookstackapp.com/docs/admin/oidc-auth/)
- [ProfessionalWiki MediaWiki MCP server](https://www.professional.wiki/en/mediawiki-mcp-server), [PluggableAuth](https://www.mediawiki.org/wiki/Extension:PluggableAuth)
- [AFFiNE MCP](https://affine.pro/mcp), [Basic Memory](https://github.com/basicmachines-co/basic-memory), [SilverBullet](https://github.com/silverbulletmd/silverbullet)

Graphs and gateway:
- [Graphiti](https://github.com/getzep/graphiti), [Zep CE deprecation](https://help.getzep.com/ce-deprecation-notice)
- [Cognee permissions](https://docs.cognee.ai/core-concepts/multi-user-mode/permissions-system/overview), [Cognee MCP](https://docs.cognee.ai/cognee-mcp/)
- [Neo4j authentication and authorization](https://neo4j.com/docs/operations-manual/current/authentication-authorization/), [Neo4j licensing](https://neo4j.com/licensing/), [Neo4j MCP authentication](https://neo4j.com/docs/mcp/current/authentication/)
- [Memgraph MCP](https://memgraph.com/docs/ai-ecosystem/mcp), [Memgraph auth](https://memgraph.com/docs/database-management/authentication-and-authorization)
- [Neptune fine-grained access control](https://aws.amazon.com/blogs/database/fine-grained-access-control-for-amazon-neptune-data-plane-actions), [AWS Labs Neptune MCP server](https://awslabs.github.io/mcp/servers/amazon-neptune-mcp-server)
- [Bedrock KB GraphRAG](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-build-graphs.html), [AWS Labs Bedrock KB MCP server](https://awslabs.github.io/mcp/servers/bedrock-kb-retrieval-mcp-server)
- [AgentCore Identity with Okta](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/identity-idp-okta.html), [Gateway interceptors](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-interceptors.html), [Gateway fine-grained access control](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-fine-grained-access-control.html), [AgentCore Policy GA](https://aws.amazon.com/about-aws/whats-new/2026/03/policy-amazon-bedrock-agentcore-generally-available)
- [LightRAG](https://github.com/hkuds/lightrag), [MS GraphRAG](https://github.com/microsoft/graphrag), [FalkorDB MCP](https://docs.falkordb.com/genai-tools/mcpserver.md), [TrustGraph workspaces](https://docs.trustgraph.ai/overview/workspaces)
