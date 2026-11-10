# Spec-driven development vs. architecture decision records (ADR)

**Research question:** How does spec-driven development (SDD) compare to architecture decision records (ADR) as software engineering practices?
**Date:** 2026-07-13
**Scope:** General practice comparison — definitions, what each is for, where they overlap or compete, documented criticisms of each, and how teams are combining them. Not scoped narrowly to Field Nation's AI-DLC build (though the comparison is obviously relevant background for it).

---

## Summary

These two practices aren't really competitors, even though the question "SDD vs. ADR" gets asked constantly right now because both terms exploded into everyday use in 2025-2026 alongside AI coding agents. ADR is a fifteen-year-old practice (Michael Nygard, 2011) for recording *why* a team made a significant, hard-to-reverse architectural choice — one short markdown file per decision, with status, context, decision, and consequences. Spec-driven development is a much newer, still-unsettled term for writing a structured spec of *what* should be built before an AI agent generates code from it — a response to "vibe coding" producing plausible-looking code that drifts from what was actually intended.

The cleanest way to hold the distinction: ADR answers "why did we choose this path," SDD answers "what are we building and how do we know it's right." They operate at different altitudes — ADR is about a single durable decision that outlives any one feature; a spec is about the shape of one feature or change, and in most current tooling gets discarded once that change ships. Several sources converge on this same framing independently, which is a reasonably strong signal it's correct rather than one blogger's opinion.

Where it gets genuinely interesting is that spec-driven development, as a term, is not one thing — it's semantically diffused across at least three different tools (Kiro, GitHub Spec Kit, Tessl) with real differences in how "spec-anchored" (kept and maintained) versus "spec-first" (used once, then discarded) each one is. And both practices have well-documented failure modes in practice: SDD tools in hands-on testing produce absurd amounts of process overhead for small changes and agents that still ignore their own specs; ADRs go stale, get treated as an "any decision record" dumping ground, and become actively misleading documentation years after the fact. The most credible synthesis found across several sources — including two independent frameworks (OpenSpec's `spec-driven-with-adr` schema and a broader "Decision-Driven Software Engineering" proposal) — is that ADRs and specs are complementary, addressing different questions (why vs. what/how) at different timescales (durable vs. per-change), and mature AI-development setups increasingly use both rather than picking one.

---

## Findings

### What each practice actually is

An **architecture decision record** is a short document — typically one or two pages of markdown, stored in the repo (commonly `docs/adr/` or `docs/decisions/`) — that captures a single significant, hard-to-reverse technical decision. The format popularized by Michael Nygard in 2011 has four core parts: status (proposed, accepted, deprecated, superseded), context (the problem being solved), decision (what was chosen and why), and consequences (the trade-offs, both positive and negative). ADRs are meant to be an immutable log — once accepted, a decision isn't edited, it's superseded by a new ADR that explicitly references the old one, preserving the full history of how the team's thinking evolved. The practice predates AI-assisted development entirely; it exists to solve a much older problem, which is that six months or two years after a decision, the person who made it is often gone and nobody else knows why the system is shaped the way it is.

**Spec-driven development** is a much newer and still-contested term. As one Thoughtworks engineer who did hands-on testing of three different SDD tools put it, the definition is "still in flux," and she's seen the word "spec" used as a near-synonym for "detailed prompt" by some practitioners. The common thread across every source: write a structured description of intended behavior *before* an AI coding agent generates code, so the spec — not an ad hoc back-and-forth chat — becomes the source of truth both the human and the AI work from. IBM and several other sources frame this explicitly as a reaction to "vibe coding": agents producing confident, plausible code that quietly solves the wrong problem, hallucinates APIs, or decays as a codebase grows, because nobody grounded the work in an actual specification first.

### SDD is not one thing — three real implementations, three different philosophies

This matters more than it might seem, because a lot of "SDD vs. ADR" confusion actually comes from people describing different SDD tools as if they were the same practice. A hands-on comparison from a Thoughtworks distinguished engineer (published on martinfowler.com, October 2025) is the most rigorous independent source found and is worth taking seriously — she actually used all three tools rather than summarizing marketing copy.

She identifies three levels of SDD maturity, each building on the last:

1. **Spec-first** — a spec is written up front and used for the task at hand, then effectively discarded. All three tools support at least this level.
2. **Spec-anchored** — the spec is kept after the task completes and continues to be updated as the feature evolves.
3. **Spec-as-source** — the spec becomes the primary artifact a human edits; code is generated from it and marked "do not edit by hand."

**Kiro** (AWS) is the lightest of the three: a three-document workflow (Requirements → Design → Tasks), mostly spec-first, no clear story for spec-anchoring over time. **GitHub Spec Kit** adds a "constitution" file of immutable principles and a heavier Specify → Plan → Tasks workflow, producing many more markdown files per change; it aspires to spec-anchored (per GitHub's own blog framing specs as "living, executable artifacts") but in practice creates one branch per spec, which behaves more like a per-change artifact than a durable one. **Tessl** (private beta) is the only one of the three genuinely attempting spec-as-source, with a 1:1 mapping between one spec file and one generated code file, code explicitly marked `GENERATED FROM SPEC — DO NOT EDIT`.

This is directly relevant background if you're evaluating or building any of this at Field Nation — the earlier 2026-07-09 research note in this workspace already flagged the same Thoughtworks source and its critique of the PRE-VERIFY problem (one-size-fits-all spec workflows are a bad fit for small changes).

### ADR vs. SDD, side by side

Multiple independent sources land on essentially the same axis of comparison:

| | ADR | SDD (spec) |
|---|---|---|
| Answers | Why did we choose this path? | What are we building, and how do we verify it? |
| Timing | Written after (or as) a decision is made | Written before implementation |
| Format | Lightweight — context, decision, consequences | Heavier — requirements, design, acceptance criteria, tasks |
| Lifecycle | Write once, supersede rather than edit; meant to be durable | Varies by tool — often discarded after the change ships (spec-first), sometimes kept (spec-anchored) |
| Scope | A single significant, hard-to-reverse technical choice | A feature or change |
| Audience | Architects, engineers, future maintainers | Primarily the AI coding agent, secondarily the human reviewing it |

One useful mental model that recurs across sources: ADRs are about the "shape" of the system (what data structures, what fundamental algorithms, what's costly to reverse), while specs are about the behavior of one particular feature or change within that shape. A spec for a two-factor-auth feature describes which flows it supports and how it integrates with the session layer; a separate ADR would document why TOTP was chosen over SMS as the underlying mechanism — the "what" and the "why" living in two different, appropriately-scoped documents.

### Where they genuinely compete versus where they're just misapplied

Strictly, they don't compete for the same job — an ADR can't replace a spec (it doesn't describe feature behavior in enough detail for an agent to build against) and a spec can't replace an ADR (once the feature ships and the branch is merged, most SDD tooling discards the reasoning, so nobody can later ask "why did we build it this way"). The apparent competition comes from a specific gap: some SDD frameworks generate a `design.md` that *contains* architectural reasoning during a change, but that file gets archived along with the rest of the change once it ships — so the "why" temporarily exists, then disappears. That's a real design flaw in some tooling, not an inherent conflict between the two practices.

A more substantive alternative framing, called **Decision-Driven Software Engineering (DDSE)**, argues that decisions — not specs — should be the system of record, and proposes a whole typed hierarchy of decision records (ADR, plus newer categories like Contract Decision Records for API/interface contracts) with required traceability from decision to code and CI-enforced compliance checks. This is a more opinionated, heavier-weight answer than either plain ADR or plain SDD, and it's explicitly positioned as "SDD's governance is optional and team-defined; ours is mandatory." Whether that's worth the overhead depends heavily on team size and regulatory/audit needs — it reads as aimed at larger, more governance-conscious enterprises rather than small teams moving fast.

### Documented failure modes — both practices break in predictable ways

**SDD failure modes**, per the Thoughtworks hands-on testing: workflows that don't scale down — asking an SDD tool to fix a small, well-understood bug produced four "user stories" and sixteen acceptance criteria, a sledgehammer for a small nut. Verbose markdown review overhead — one tool generated so many repetitive, overlapping files that reviewing them took longer than just writing the fix directly would have. And critically, a "false sense of control" — even with all the structure, the agent in testing sometimes ignored its own generated notes (treating an existing class as new and duplicating it) or over-applied instructions too literally. There's also an open, unresolved question about who the target user even is — much of the tooling assumes a developer will do product-style requirements analysis, but doesn't say whether that's realistic or where a real PM/product function fits in.

**ADR failure modes**, per an InfoQ piece from two architecture practitioners and a more recent (2026) critique found during this research: staleness is the big one — an ADR written years ago that no longer reflects the current system doesn't just become irrelevant, it actively misleads whoever reads it and assumes it's still accurate. There's a structural argument that this is baked into the format itself: an ADR is fundamentally a point-in-time snapshot being asked to behave like a living, current artifact, and nothing in the format enforces that it stays accurate as the system evolves — unlike, say, a test suite, which fails loudly when it's wrong. The other common failure is scope creep — teams without a clear definition of "architectural" start dumping every decision (including trivial ones) into ADRs, which dilutes them into what one source calls an "Any Decision Record," making the genuinely significant decisions harder to find. The practical guidance to avoid this: an architectural decision is one that's both costly to reverse *and* changes the fundamental "shape" of the system (its core data structures, algorithms, or concurrency/messaging model) — not just anything that happens to be expensive or long-lived. Swapping a SQL vendor is a significant decision; changing from row-based to tree-structured data representations is architectural.

### The emerging consensus: use both, at different layers

The most consistent, and most convincing, synthesis across sources is that ADRs and specs are complementary rather than competing, and the AI-development tooling ecosystem is actively building for this combination rather than treating it as a hypothetical. One concrete example: a newer OpenSpec schema variant (`spec-driven-with-adr`) deliberately keeps ADRs *outside* the per-change spec folder specifically so they survive after a change is archived — while `specs.md` (current feature behavior) also persists, `design.md` (the reasoning) normally wouldn't, so this schema promotes the durable "why" reasoning into a standing ADR precisely to fix the gap described above. The stated logic: specs represent the current state of *what the system does*, ADRs represent the current state of *why and how it's built that way* — both are meant to be read by future change proposals, while the proposal/plan/tasks scaffolding for any single change is disposable once that change ships.

This also shows up in how AI agent context files are being organized in practice: a repo-level `AGENTS.md` or similar file increasingly acts as a table of contents pointing an agent toward both specs and ADRs, while a separate "constitution" or rules file holds a smaller set of non-negotiable, always-applicable principles. The practical shape that's emerging, in other words, is a three-layer stack — durable non-negotiable principles, durable architectural decisions (ADRs), and per-change specs — rather than a single artifact type doing everything.

---

## Key uncertainties

Spec-driven development is a genuinely unsettled term as of mid-2026 — sources explicitly disagree on scope, and the space is moving fast enough that any specific tool description here may already be stale by the time this is read. Treat the Kiro/Spec Kit/Tessl comparison as a snapshot from around September-October 2025 testing, not a current state of those products.

The DDSE framework is a much less independently validated concept than ADR or the mainstream SDD tools — it comes primarily from a single foundation's own site and spec, without independent case studies or adoption data found in this research. It's a reasonable framework to know about, not something with a track record yet.

The claim that ADRs are "structurally" doomed to go stale (as opposed to just poorly maintained in practice) is one author's framing (Java Code Geeks, 2026), not a broadly cross-validated finding — it's a compelling argument but worth treating as one perspective rather than settled consensus.

This report deliberately did not search Field Nation's internal Slack, Confluence, or Linear for how (or whether) ADRs and specs are already being discussed internally — this was scoped as general external practice research per your framing, and several of those connectors also weren't authorized in this session. If you want the internal angle (e.g., whether Moissanite's PRE-VERIFY or any TDR conventions already touch this), that would be a natural follow-up pass.

---

## Sources

- [What is Spec-Driven Development? — IBM](https://www.ibm.com/think/topics/spec-driven-development)
- [Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl — Birgitta Böckeler, Thoughtworks/Martin Fowler](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html) — independent hands-on comparison, the strongest single source in this research
- [Spec Driven Development: When Architecture Becomes Executable — InfoQ](https://www.infoq.com/articles/spec-driven-development/)
- [ADR vs Spec-Driven Development: Why, What, and Using Both — Ceyhun Aksan](https://ceaksan.com/en/adr-openspec-decision-spec-management)
- [Architectural Decision Records with Spec-Driven Development using OpenSpec — intent-driven.dev](https://intent-driven.dev/blog/2026/04/29/spec-driven-development-with-adr/)
- [Decision-Driven vs. Spec-Driven Software Engineering — Mahmudur R Manna, Hugging Face](https://huggingface.co/blog/mrmanna/ddse-vs-sdd)
- [bliki: Architecture Decision Record — Martin Fowler](https://martinfowler.com/bliki/ArchitectureDecisionRecord.html)
- [Master architecture decision records (ADRs): Best practices — AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/master-architecture-decision-records-adrs-best-practices-for-effective-decision-making/)
- [Has Your Architectural Decision Record Lost Its Purpose? — Pierre Pureur & Kurt Bittner, InfoQ](https://www.infoq.com/articles/architectural-decision-record-purpose/)
- [The Reason Most ADRs Get Written and Never Read Is Architectural, Not Cultural — Java Code Geeks](https://www.javacodegeeks.com/2026/05/the-reason-most-architecture-decision-records-get-written-and-never-read-is-architectural-not-cultural.html)
- [Architectural Decision Records (ADRs) — adr.github.io](https://adr.github.io/)
- [Constitution vs AGENTS.md — github/spec-kit discussion #2476](https://github.com/github/spec-kit/discussions/2476)
- Prior internal research: [2026-07-09-ai-dlc-industry-benchmarking-aws.md](./2026-07-09-ai-dlc-industry-benchmarking-aws.md) (already cites the same Böckeler source in the context of FN's AI-DLC architecture)
