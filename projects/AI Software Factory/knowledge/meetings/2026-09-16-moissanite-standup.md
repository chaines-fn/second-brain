# Team Moissanite: Daily Standup — 2026-09-16

**Date & Time:** Tuesday, September 16, 2026, 08:15 CDT (~18 minutes)

**Attendees:**
- Chuck Haines — Workstream 1 Lead (meeting organizer)
- Joe Hoppesch — Workstream 2 Lead
- Asad Ali — Contractor (ai-toolbox/skill architecture)
- Vitalii Belinskyi — Contractor (repo bootstrap/harness)
- Mykhailo Sakhnik — Contractor (ai-repo-score/skill design)
- Redwan Shahzada — _(declined; formerly contributed, now in supporting capacity)_

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1avyN_QgborglTFZ2qUYowxxgXhbwK9_q_1kzVT9HFu8/edit?usp=meet_tnfm_calendar)

---

## Overview

The team discussed repo bootstrap tool optimization and skill architecture standardization, with a focus on packaging tools for easy distribution and reducing LLM token costs through deterministic scripting. Vitalii's new repo bootstrap implementation was highlighted as a strong direction. The team debated whether to consolidate three existing skills (ADR create, ADR enforce, ADR review) into fewer skills but ultimately kept the current three-skill structure due to distinct execution moments and AI toolbox limitations. Key decision: package repo bootstrap and repo scorecard as downloadable CLI tools for easier adoption. Significant personnel news: Vitalii is transitioning off the project within days to focus on other work; Mykhailo will conduct a handover call to document his contributions. The team also learned Claude rolled out a new insights analytics tool for identifying org-wide skill gaps.

---

## Key Notes

- **Repo bootstrap token cost optimization:** Chuck wants to extract deterministic logic from Vitalii's repo bootstrap PR into scripts to reduce LLM token usage. The goal is to preserve human-driven prompts where needed while automating repetitive checks (e.g., "is AI toolbox installed?"). This will be incorporated as a follow-up after the initial PR merges.

- **Skill consolidation debate resolved:** Mykhailo and Asad spent ~45 minutes discussing whether to consolidate three skills (create, enforce, review) into fewer. Asad explained that each fires at different operational moments: create runs when a decision is made, enforce runs when a change is described, review runs when a record review date passes. Additionally, the AI toolbox currently prevents skills from calling other skills. After discussion, Chuck approved keeping all three skills—no consolidation needed. Asad will test alternative structures if Mykhailo proposes a diagram or prototype, and will investigate the AI toolbox constraint on skill-to-skill calls.

- **Repo scorecard nearing completion:** The AI repo-score is now at 97 and will reach 100 soon. Mykhailo is accounting for commit history on the main branch in the calculation. PRs for both Gitar integration (MOI-109) and repo-score finalization (MOI-110) are in review.

- **Tool packaging and distribution strategy:** Chuck wants to package repo bootstrap and repo scorecard as standalone downloadable CLI tools so users don't need to clone the repository. Discussed publishing via GitHub action releases or a private Homebrew tap. This will be a new set of tickets for Vitalii and Mykhailo to execute after the initial PRs land. Clear install instructions will accompany the packaged tools.

- **Setup documentation initiative:** Chuck proposed creating a Confluence page documenting all required developer tools (OpenSpec, Sonar MCP, AI toolbox, etc.) with explanations of why each is needed and how they work together. This will be distributed to engineers to ensure they have the optimal agentic experience. A ticket will be created; work likely starts after repo bootstrap lands.

- **Prompt incorporation into bootstrap:** After the repo bootstrap PR lands and packaging is set up, Chuck's most recent prompt will be integrated into the bootstrap tool. This combines deterministic scripts with Chuck's context-setting prompt to streamline the initial repo assessment.

- **Harness/toolbox CI configuration gap:** Vitalii needs someone with Harness admin role to configure toolbox create triggers for pipelines. This is a blocker for completing the CI/CD integration work.

- **Personnel transition — Vitalii leaving:** Chuck announced that Vitalii will transition off the project within days to another assignment. Priority: finish repo bootstrap and Harness work without taking on new tasks. Mykhailo requested a handover call to understand what Vitalii built.

- **Claude analytics insights tool:** Chuck noted Claude recently rolled out an insights analytics tool that can generate smart reports on org-wide skill gaps. He has identified 3-4 gaps from initial reports that will likely turn into new skills.

---

## Action Items

- **Mykhailo Sakhnik:** Review Vitalii's repo bootstrap and Gitar PRs; provide feedback before merging.
- **Mykhailo Sakhnik:** Review repo-score PR (MOI-110) to verify scoring logic and main branch commit history factoring.
- **Asad Ali:** Investigate AI toolbox skill-to-skill call configuration and report findings on the constraint preventing skills from invoking other skills.
- **Asad Ali:** Refresh evaluation PR by removing boilerplate code and cleaning up test cases; prepare for Chuck's review.
- **Vitalii Belinskyi:** Complete repo bootstrap work and get it merged within the next few days (priority focus before transition).
- **Vitalii Belinskyi:** Complete 911 ticket using the newly created spec and updated STDD call.
- **Vitalii Belinskyi:** Schedule handover call with Mykhailo to demonstrate and explain repo bootstrap and Harness work.
- **Chuck Haines:** Send most recent prompt to Vitalii Belinskyi for documentation.
- **Chuck Haines:** Create Confluence page documenting required setup tools (OpenSpec, Sonar MCP, AI toolbox, etc.) and why each is needed.
- **Chuck Haines:** Create tickets for packaging repo bootstrap and repo scorecard as downloadable CLI tools (likely GitHub releases or private Homebrew tap).
- **Chuck Haines (implied):** Incorporate personal prompt into repo bootstrap tool after packaging work is complete.
