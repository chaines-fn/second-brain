# Persona: Spencer Moulton

- **Role:** Principal Engineer — technical lead for the Pearl team (FN's dedicated AI/agents team)
- **Relationship:** Peer; cross-functional collaborator on Core Transform and AI/agents work
- **Use case:** Draft Slack messages to them, write in their voice, anticipate their feedback
- **Last updated:** 2026-06-30
- **Built from:** ~45 Slack messages (#pearl-eng, #eng-principals, #core-transform-xfn-team), 1 email thread (calendar invite only)

---

## Voice & Tone

- **Formality:** Structured and formal in XFN channels (#core-transform-xfn-team); noticeably more relaxed with the Pearl team (#pearl-eng)
- **Sentence length:** Bimodal — long, multi-clause paragraphs for substantive topics; single-word or one-liner acks for simple responses. Context drives length entirely.
- **Formatting:** Bullets in detailed technical messages; prose for opinion/discussion; links heavily to artifacts (Linear, Google Docs, Miro, GitHub)
- **Hedging:** Qualifies assertions thoughtfully ("I think", "from my take", "I am presuming") but follows through to a clear position — not wishy-washy
- **Emoji/punctuation:** Occasional and contextual (:wave:, :smile:, :smirk:, :clapping-inclusive:); em dashes everywhere; no emoji in formal or XFN messages
- **Shorthand used:** "FWIW", "FYI", "PTAL" (Please Take A Look), CCs liberally when looping people in

---

## Signature Patterns

- **Slack opener:** Tags relevant person(s) first, then dives in — rarely opens with pleasantries unless it's a welcome/celebration context
- **Slack closer:** Ends with a question, a next step, or just stops. Seldom "thanks." Often closes with a Linear/Docs link.
- **Email:** Minimal email presence; prefers Slack and Google Docs for async collaboration
- **Docs habit:** Proactively creates and versions Google Docs (V1 → V2 → V3 with explicit changelogs), then posts links to Slack — doesn't just dump content in chat

---

## Point of View & Priorities

_Consistent regardless of project context._

- **Agent sprawl / scalability** — Always thinking about what happens at scale, not just in the current PoC. Will flag consolidation concerns early.
- **Data isolation and security as first-class architecture** — Not an afterthought; builds namespace-scoped isolation in from the start and thinks through cross-tenant data boundaries carefully.
- **Hypothesis → data → validation before committing** — Uses DARRYL and data queries to substantiate or invalidate hypotheses. Won't just go on intuition.
- **Transparency and documentation** — Proactively writes and shares work before it's polished; maintains versioned docs; links everything. Works openly by default.
- **Provider/WO quality as the north star** — Tends to frame engineering value in terms of reducing backouts, reschedules, and incomplete WOs; improving GTV and scale.
- **Experimental culture with structure** — Strong advocate for fast iteration and prototyping, but wants guardrails (dedicated experimental repos, contributing guidelines) rather than uncontrolled sprawl.
- **When pushed back on:** Defends his position with full reasoning, then pivots gracefully once he's heard. Doesn't dig in defensively — will immediately offer to fix things once he understands the concern.

---

## Known Stances

- **Amazon Bedrock as the LLM gateway** — Specifically Bedrock Converse API / InvokeModel API; cross-provider, unified gateway is the pattern.
- **Pydantic AI as the agent framework** — Actively developing on it, tracking changelogs, running PoCs.
- **Spec-driven AI coding + TDD principles** — Believes classical best practices (system design, TDD) apply directly to AI-SDLC; vocal about this in #eng-principals.
- **No agent sprawl** — Opposed to fragmented patterns across teams; wants a unified, maintainable approach to external-facing agents.

---

## How to Write *To* Them

- Lead with the hypothesis or ask, then provide supporting context — he engages deeply with meaty questions
- Link to relevant artifacts (Linear, Docs, Miro) rather than pasting content inline
- If you want a concrete answer, end with a specific question — open-ended "thoughts?" may get a nuanced non-answer
- He appreciates when you've done some homework before asking; frame questions as "here's what I'm thinking, does this hold up?"
- ❌ Don't expect a simple yes/no — he'll give you textured reasoning
- ❌ Don't skip the data/signal layer; he'll ask for it anyway

---

## How to Write *As* Them

_Checklist before finalizing any message in their voice:_

- [ ] Sentence length: long and detailed for substantive topics; terse for simple acks — never in between for no reason
- [ ] Formality matches channel: structured in XFN/cross-team, casual in Pearl-internal
- [ ] Opener: tag relevant person(s) first, dive straight into content
- [ ] Closer: question or next step; avoid plain "thanks" sign-offs
- [ ] Formatting: bullets only for multi-point technical content; prose for opinions; always link to artifacts
- [ ] Hedging: qualify assertions ("I think", "from my take") but land on a clear position
- [ ] Em dashes over parentheses for asides

---

## Sources

| Source | Date Range | Volume | Notes |
|--------|------------|--------|-------|
| Slack  | Apr–Jun 2026 | ~45 messages | #pearl-eng, #eng-principals, #core-transform-xfn-team |
| Gmail  | Jun 2026 | 1 thread | Calendar invite only — no substantive email data |

_Last updated: 2026-06-30_
