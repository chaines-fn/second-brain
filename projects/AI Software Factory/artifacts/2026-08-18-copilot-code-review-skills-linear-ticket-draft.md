# Linear ticket draft — Copilot code review + agent skills

**Status:** Draft, not yet created in Linear (connector not authorized in this session, 2026-08-18)

---

**Title:** Evaluate GitHub Copilot code review with agent skills (marketplace + repo-local) — test in core-ui

**Project:** Engineering Velocity via Repo Enablement (O1 KR2 — Repo-readiness playbook)

**Priority:** No urgency, normal backlog

**Assignee:** Unassigned

**Labels:** (suggest) code-review, copilot, o1-kr2

---

### Context

O1 KR2 lists a code review agent as a standard playbook step, evaluated across three vendor candidates: Claude Code review, Sonar/Gitar, and Copilot. Gitar is mid-trial and Tessl is in eval; this ticket covers the Copilot leg.

GitHub's docs confirm Copilot code review can use agent skills and MCP servers configured in the repo ([Using GitHub Copilot code review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review)):

- Repo-local skills live in `.github/skills`; a review-focused directory name like `code-review` makes Copilot more likely to use it during review.
- MCP servers come from repo Copilot settings (GitHub + Playwright MCP enabled by default); a repo setting controls whether MCP tools are available to code review specifically vs. only to the interactive agent.
- Each review comment shows an attribution footer naming which skill or MCP server produced it — this is how we verify a skill was actually used, not just installed.

### What this ticket covers

1. **Read the docs and understand the mechanism.** Confirm how Copilot code review discovers and prioritizes skills vs. MCP servers, and what makes it "more likely" to use a given skill (naming, explicit instruction, PR description references).
2. **Verify marketplace-distributed skills reach Copilot code review, not just the interactive agent.** This is the open question: skills pushed org-wide via the Copilot marketplace bundle (MOI-47, `fn-platform`, shipped 2026-08-17) need to actually surface during automated PR review, not just when a dev invokes Copilot chat/agent directly. Confirm this before assuming marketplace distribution covers the review use case.
3. **Stand up and test in `core-ui`.** Add a `.github/skills/code-review` skill to the repo, open test PRs, and confirm via the review-comment attribution footer that both the repo-local skill and the marketplace-distributed skills are being picked up.

### Acceptance criteria

- Written confirmation (yes/no + evidence) on whether marketplace-installed skills are visible to Copilot code review.
- `core-ui` has a working `.github/skills/code-review` skill; at least one test PR shows an attributed review comment referencing it.
- Findings documented and fed into the O1 KR2 code review vendor decision, alongside the Gitar and Tessl trial results.

### References

- [Using GitHub Copilot code review — GitHub Docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review)
- MOI-47 (ai-toolbox marketplace delivery mechanism, Done 2026-08-17)
- `program/OKRs/h2-2026-okrs.md` — O1 KR2
