# Agent registry and definition service: high-level requirements

**Owner:** Chuck Haines
**Status:** Draft. Pending Cory's sign-off on OQ-2.
**Date:** 2026-10-08
**Related:** fai-prd.md (sections 3.3, 4.3, FR-31 to FR-43), 2026-10-07-fai-aws-architecture-draft.md

## Purpose

Give Fai one controlled way to create, store, find, and govern agent definitions. Humans, the UI, and LLM builders all write through the same service. Git stays the system of record.

## Scope

In scope: the definition write path, the definition index and search, access control for edit, invoke, and view, the PR lifecycle, and auto-merge gating.

Out of scope for this document: the AgentCore harness design, the run ledger, the eval engine choice, and skill version pinning (deferred).

## Principles

- Git is the source of truth. The service's database is an index that can be rebuilt from git.
- Every change reaches main through a PR. Nothing writes to main directly.
- The service enforces only what git and branch protection can also enforce.
- Merge happens through GitHub branch protection after gates pass. No AI reviewer and no agent token merges directly.

## Requirements

### A. Definition authoring

| ID | Requirement | Priority |
|---|---|---|
| REG-01 | Definitions live in a dedicated registry repo, one directory per agent, containing agent.yaml, instructions.md, and evals/. | Must |
| REG-02 | The service is the only write path for definitions. The UI, the LLM builder, and the CLI all call it. | Must |
| REG-03 | Each create or edit makes a branch and opens a PR. The service never pushes to main. | Must |
| REG-04 | The service authenticates the requester through Okta and sets the owner from that identity. The owner is never taken from request input. | Must |
| REG-05 | Before opening a PR, the service validates the schema, the required owner, and the requested tool tier against what the requester may approve. | Must |
| REG-06 | A draft is a branch with an open PR. Drafts can be listed and resumed through the service. | Should |
| REG-07 | Changes to tools, tier, skills, or limits require the owner and the tool approval group. | Must |
| REG-08 | The service shows PR state (checks, Gitar result, reviews, merged) back to the requester. | Should |

### B. Index and search

| ID | Requirement | Priority |
|---|---|---|
| REG-10 | Postgres holds an index of agents: id, owner, status, current version, tier, triggers, tools, skills, description, and git SHA. | Must |
| REG-11 | The pipeline populates the index on merge. The whole index can be rebuilt from the registry repo. | Must |
| REG-12 | Search supports filters (owner, tier, trigger type, status, tool, skill), weighted full-text search over name, description, and instructions, and fuzzy matching on names. | Must |
| REG-13 | Each index row records the commit SHA it was built from, so the index can be checked against git. | Must |
| REG-14 | Temporal resolves agent_id through the index at run time and never reads git at run time. | Must |
| REG-15 | Semantic (vector) search is not required for v1. | Later |

### C. Access control

Three actions, one policy model. Policies are written in Cedar to match AgentCore Gateway. Principals come from Okta groups.

| ID | Requirement | Priority |
|---|---|---|
| REG-20 | **Edit:** owner plus CODEOWNERS and the tool approval group, enforced in git. | Must |
| REG-21 | **Invoke:** who may start a run of an agent, by trigger type (Linear label, Slack, CLI, schedule). Checked by the service and by Temporal at run start. The identity model is open (OQ-1). | Must |
| REG-22 | **View:** who may read a definition, its evals, and its run records. Default is engineering. Agents that need restricted visibility are marked in the definition. | Should |
| REG-23 | Tool-call access is enforced at AgentCore Gateway by Cedar regardless of run-start decisions. | Must |
| REG-24 | An agent cannot grant itself tools, a higher tier, or a different identity. | Must |

### D. PR lifecycle and auto-merge

| ID | Requirement | Priority |
|---|---|---|
| REG-30 | Every PR, whether from the factory or from the service, runs Gitar and Sonar. | Must |
| REG-31 | The agent addresses review feedback by pushing commits. The loop is capped at three rounds, then hands off to a human with the thread history. | Must |
| REG-32 | The agent does not resolve review threads or decide whether its fix addressed a finding. The re-review on push is the check. | Must |
| REG-33 | A PR auto-merges only when all of these hold: Gitar approves, no review threads are open, CI and Sonar pass, CODEOWNERS and required reviewers are satisfied, the change falls in an allowed lane, and no sensitive path is touched. | Must |
| REG-34 | Auto-merge is armed through GitHub's native auto-merge, so branch protection performs the merge. No token grants direct merge. | Must |
| REG-35 | Sensitive paths always require a human reviewer: authentication, authorization, payments, secrets, schema and migrations, infrastructure, and data deletion. The list is set with security (OQ-5). | Must |
| REG-36 | The registry repo, the Gitar rules files, and the gate policy are never in the auto-merge lane. The gate policy cannot be auto-approved by any rule that it defines. | Must |
| REG-37 | The first auto-merge lane is docs and generated files. Product code and registry changes stay human-merged until revert data supports widening. | Must |
| REG-38 | Every auto-merge decision is logged with the criteria that passed and the Gitar result. | Must |
| REG-39 | Gitar rules files (`.gitar/rules/*.md`) are committed per repo and changed only through a human-reviewed PR. | Should |
| REG-40 | Review-relevant skills are committed in each repo, not delivered only through the marketplace, because Copilot's automated review does not read marketplace skills (MOI-97). | Should |

### E. Security

| ID | Requirement | Priority |
|---|---|---|
| REG-50 | One GitHub App for Fai, with contents and pull-requests write only. No merge permission. | Must |
| REG-51 | The App private key is in Secrets Manager, readable only by the service and the token-minting workflow, behind a narrow IAM role. | Must |
| REG-52 | Agents receive only their per-run installation token, never the service's credentials. | Must |
| REG-53 | Agent inputs (Linear issues, review comments, diff contents) are untrusted. Agents may act on review findings but may not widen scope or permissions because of text in those inputs. | Must |
| REG-54 | Every agent has its own identity and a named owner. | Must |

### F. Operations

| ID | Requirement | Priority |
|---|---|---|
| REG-60 | Writes are audited with requester, owner, agent id, branch, PR number, and SHA. | Must |
| REG-61 | Read queries are served from the index without calling GitHub. | Should |
| REG-62 | A rebuild from git runs on demand and on a schedule, and reports any drift from the index. | Should |
| REG-63 | The service is small: one API, one Postgres instance on RDS, deployed with OpenTofu. | Should |

## Done when

- A new agent definition is created from the UI, goes through a PR, passes Gitar, works through at most three feedback rounds, and auto-merges in the docs lane.
- A PR that touches an authentication path never auto-merges, even when Gitar approves it.
- A rebuild from git produces an index that matches the live index.
- A search with a misspelled agent name returns the intended agent.

## Open questions and decisions

- **OQ-1 Invoke identity.** Do runs act as the agent's own identity or as the human who invoked them? If the human, per-user JWT propagation is needed and invoke ACL becomes a question of delegation. This blocks the REG-21 design. Owner: Chuck. Decide before build.
- **OQ-2 Q4 scope change.** This document changes PRD FR-32 (no agent merge permission in Q4) and moves FR-35 (auto-merge, currently "Later") into Q4 scope. Needs Cory's sign-off.
- **OQ-3 Database engine.** Recommendation is Postgres on RDS for weighted ranking, fuzzy matching, and JSONB indexing. Confirm it is an approved engine. The fallback is MySQL FULLTEXT, with weaker ranking and fuzzy matching.
- **OQ-4 Lane and cap ownership.** Who approves changes to the allowed lanes, the feedback-round cap, and the Gitar rules?
- **OQ-5 Sensitive path list.** The definitive list needs agreement with security before REG-35 can be built.
- **OQ-6 Gitar capability.** Can Gitar's auto-approve and auto-merge express the path, severity, and thread conditions in REG-33 natively? If not, a wrapper must enforce them. This feeds the O1 evaluation.
