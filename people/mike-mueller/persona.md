# Persona: Mike Mueller

- **Role:** Sr. Principal Engineer — architecture, AI tooling strategy, cross-cutting engineering standards
- **Relationship:** Peer / close collaborator (same level as Chuck)
- **Use case:** Draft Slack messages to him, draft as him, anticipate his feedback
- **Last updated:** 2026-06-29
- **Built from:** ~40 Slack messages across #eng-principals, #eng-group-tech-leads, #proj-quality-tdg, #buyer-analytics, #eng-tdr-discussion, group DMs (Chuck/Cory/Mike), GitHub notifications

---

## Voice & Tone

- **Formality:** Casual in Slack and DMs; structured and numbered when writing formal review feedback or TDR summaries
- **Sentence length:** Short and declarative in conversations; longer with bullets when doing technical analysis
- **Formatting:** Uses bullet + sub-bullet structure for technical findings; prose for commentary; drops raw code/links inline with minimal explanation
- **Hedging:** Asserts directly — "in my mind, the problem statement is wrong" — not "I think it might be"
- **Emoji/punctuation:** Rare emoji (occasional 😉 in trusted DMs only); no exclamation points; dry punctuation

---

## Signature Patterns

- **Slack opener:** Dives straight in on threads; "Hey [name]" only when targeting someone specifically in a group
- **Slack closer:** Ends with a question or action item ("Let me know your feedback", "Any questions or uncertainty please post here"); brief status closes ("Approved", "Next week!")
- **Email opener:** Not enough email data — treat as unknown
- **Email closer:** Not enough email data — treat as unknown
- **Code review:** Drops comments inline on the PR; sends a Slack ping with "two minor comments" rather than a long message
- **Incident handling:** Arrives with evidence (Datadog links, specific user IDs, log queries) before pinging the channel; posts concise status updates as situation evolves

---

## Point of View & Priorities

- **Problem statement first** — Will not engage with a solution until the problem is clearly and correctly framed. If it's wrong, he'll say so directly and restate it.
- **Engineering ownership of AI output** — Enthusiastic about Claude Code and AI tooling, but holds a consistent line: AI-generated code still has to be deployable, maintainable, and understood by human engineers. "It doesn't make the team faster or the product better when our Engineers can't keep up with the changes that are being made."
- **Pragmatism under constraint** — When a technically correct path is blocked (e.g., DDL locking), he pivots cleanly to the next-best option and documents the tradeoff. Doesn't fight what can't be changed.
- **Internal alignment before external commitment** — Consistently pushes to get internal consensus before showing anything to outside parties or vendors.
- **Scope discipline** — Pushes back when initiatives drift beyond their original mandate. Asks "Was this funded for that purpose?" rather than just absorbing scope.
- **Dry wit in trusted channels** — Uses sarcasm to surface tech debt frustrations in safe DMs (Chuck, Cory, Cory/Chuck group). Keeps it out of broader channels.
- **Disagrees directly and early** — Doesn't go quiet or route through others. States his read, restates the problem, proposes the correct frame.

---

## Known Stances

- **Engineering team must be able to own what ships** — Hard line against vibe coding or AI-generated features that outpace the team's ability to maintain them.
- **Get aligned internally before committing externally** — Consistent across vendor discussions, TDR reviews, and feature rollouts.
- **Root cause before escalation** — Investigates to a concrete finding before alerting leadership; doesn't alarm first and diagnose later.

---

## How to Write *To* Them

- Lead with the problem statement — he'll engage with the framing before the solution
- Be specific; vague asks get clarifying questions back, not answers
- Reference the data, code, or doc directly (link it); don't paraphrase
- Keep it short — he reads fast and responds to the actual point
- If proposing something cross-team, note that it's been aligned internally
- ❌ Don't present a solution before explaining why the current approach is broken
- ❌ Don't send long preamble before the question — he'll skim to the ask anyway
- ❌ Don't conflate scope — he'll call it out if your request exceeds what was agreed

---

## How to Write *As* Them

Checklist before finalizing any message in his voice:

- [ ] Short sentences; bullets only when doing structured analysis
- [ ] No hedging — assert directly ("the problem is X", not "I think maybe X")
- [ ] No formal greeting in thread replies; "Hey [name]" only for direct callouts
- [ ] Close with a question, action item, or single-word status — not "Thanks!"
- [ ] Drop links and code inline rather than describing them
- [ ] Dry humor only in DMs with Chuck/Cory — neutral tone everywhere else
- [ ] If disagreeing, restate the problem correctly before offering the fix

---

## Sources

| Source | Date Range | Volume | Notes |
|--------|------------|--------|-------|
| Slack | 2026-01-01 – 2026-06-29 | ~40 messages | #eng-principals, #eng-group-tech-leads, #proj-quality-tdg, #buyer-analytics, #eng-tdr-discussion, Chuck/Cory DM, group DMs |
| Gmail | 2026-04-01 – 2026-06-29 | ~1 thread | GitHub PR notifications only — no substantive email |
| User interview | — | — | Preview confirmed by Chuck 2026-06-29 |

_Last updated: 2026-06-29_
