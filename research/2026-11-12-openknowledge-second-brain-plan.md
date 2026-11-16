# OpenKnowledge as a second brain: research and migration plan

**Research question:** Can we convert the PersonalAssistant folder into OpenKnowledge (OK) and use it as Chuck's second brain, and if so, how?
**Date:** 2026-11-12
**Related:** [2026-11-09-second-brain-wiki-and-graph-options.md](../projects/AI%20Software%20Factory/research/2026-11-09-second-brain-wiki-and-graph-options.md) (company-wide brain, a different problem)

---

## Summary

OpenKnowledge is a good fit for a personal second brain. The folder is already plain markdown with relative links, so there is little to convert. OK does not need a database, indexes live files directly, backlinks are computed for you, and its MCP server gives Claude the same folder with graph tools on top. It is not a fit for the company brain. It has no OIDC login, no server-enforced tiers, and no approval queue for agent writes, which the November 9 research requires. Keep the two problems separate.

The main risks are not technical. They are (1) the Cowork skills and CLAUDE.md files depend on exact relative paths and index file formats, (2) OK is young (launched around June 2026), and (3) this folder holds internal Field Nation material and colleague persona files, so it must stay local.

## What OK is (from its docs and README)

- A markdown editor and knowledge-base tool with agent integrations. Source of truth is plain `.md` or `.mdx` files in a folder. "OpenKnowledge has no database dependency."
- Opening any existing folder of markdown works. The docs say it works with existing wikis and Obsidian vaults.
- Backlinks and a graph view are computed automatically. Links use standard markdown or wiki-style `[[path]]`, resolved by a documented order.
- Every change is tracked in git, with per-burst diffs and selective rollback.
- Search is agentic: the agent greps, searches, and follows backlinks over live files. No vector database.
- `ok init` scaffolds an `.ok/` folder and wires up Claude Code and Claude Desktop, among others. `ok seed --pack knowledge-base` installs skills into `.claude/skills`.
- The MCP server (named `open-knowledge`) exposes `exec`, `search`, `write`, `edit`, `links` (forward, dead, orphans, hubs, suggest), and `audit`.
- Platforms: desktop app for macOS 13 or later (Intel Macs use the web app or CLI), Windows, Linux. CLI and web path need Node.js 24 or later and git.
- License: GPL-3.0 or later. Local-first. Git sync and team sharing are optional.

## Fit against the current folder

The folder is about 300 files and 38 MB. Most are markdown: project CLAUDE.md files, meeting notes, research, brainstorms, and people files. A few are binaries: `.pptx`, `.docx`, `.xlsx`, `.drawio`, `.svg`, and one `.html`. Cross-file references already use relative links such as `[about-me.md](./about-me.md)`, which OK resolves natively.

Three things need attention before migration.

1. **Duplicated profile files.** `about-me.md`, `brand-voice.md`, and `working-preferences.md` appear at the root and again in several project `me/` folders. OK's backlinks will make this visible. Pick one source of truth per file and link to it.
2. **Paths that agents depend on.** `CLAUDE.md` at the root and in each project, `research/index.md`, `brainstorm/index.md`, and `people/people.md` are read or written by name by Cowork skills. The research and brainstorm skills append table rows to index files. Do not rename or move these without updating the skills and CLAUDE.md. Frontmatter added to index files could also break their tables, so skip frontmatter on index files.
3. **Junk files.** `.DS_Store`, `.fuse_hidden*`, a `.~lock` Excel file, and `.$*.drawio.bkp` backups should be excluded from any OK project and from git.

Binary deliverables (decks, Word files, Excel files) should stay where they are. The docs do not mention docx, pptx, or xlsx import, and the Assets page was not read. Link to them from notes rather than converting them.

## Plan

**Phase 0: Scope and decision (this week).** Decide whether OK covers the whole folder or only the personal layer (root files, `people/`, `research/`, `brainstorm/`, and the project index files). My recommendation is the whole folder, since the project knowledge is what makes it a second brain, but pilot on a copy first.

**Phase 1: Pilot install on a copy (about 1 day).** Copy the folder to a sibling folder, `PersonalAssistant-OK-pilot`, excluding junk files. Install Node 24 or later and git, run `npm install -g @inkeep/open-knowledge`, and run `ok init` in the copy only. Before running it, back up the copy's `CLAUDE.md` files and check whether `ok init` overwrites anything or adds a conflicting `.claude/` folder. Read the pages that were linked but not fetched: Obsidian migration, Assets and embeds, Remote control, Share, and Syncing. Then check the desktop app and the web editor on the copy.

**Phase 2: Inventory and cleanup (2 to 3 days).** Produce a file list with sizes and types. Remove the junk files. Consolidate the duplicated profile files. Mark archive and draft material. Initialize a local git repository in the pilot with a `.gitignore` for the junk files. Do not add a git remote yet.

**Phase 3: Conventions (2 days).** Add lightweight frontmatter to content notes only: `type` (meeting, research, brainstorm, project, person, decision), `project`, `date`, `status`, and `sensitivity` (internal, confidential). Use a script with a dry-run mode, not hand edits, and keep existing filenames. Add `.ok/frontmatter.yml` folder descriptions for the main folders so agents know what each folder is for.

**Phase 4: Link repair (1 day).** Run `links` with `dead` and `orphans` to find broken and unlinked notes. Fix broken relative links. Review wiki links after any folder move, since OK's docs warn about folder-prefixed and case-sensitive links. Use `audit` to confirm.

**Phase 5: Agent wiring and write rules (1 to 2 days).** Connect the `open-knowledge` MCP server to Claude. Keep write approvals on, since your working preferences say no writes without permission, and review agent edits through the timeline or git diff. Confirm that the Cowork skills still find and update `research/index.md` and `brainstorm/index.md` correctly in the pilot. Test a retrieval question such as "who owns the service territory work" and a graph question such as "which notes are orphaned."

**Phase 6: Two-week trial.** Use the pilot daily for research and brainstorms. Track retrieval quality, broken links, any Cowork skill breakage, and performance at 300 files. Keep a short log of what failed.

**Phase 7: Cutover or stay.** If the trial passes, point the desktop app at the real folder and keep the same Cowork connection, since the folder path does not change. Run `audit` weekly and commit to git after each working session. Turn off any sync or share feature for this folder unless it has been reviewed.

## Security and confidentiality

The folder contains internal Field Nation material and persona files for named colleagues. Keep the OK project local. Do not enable git remotes, GitHub sync, OK share links, or team sharing for this folder without review. Use Claude as the in-app agent rather than an external app, so content goes only to the same Claude path the Cowork session already uses. Do not use the Docker or remote-control deployment options for this folder. The company org instructions also say uploaded customer data is confidential, so do not move any customer data into the pilot.

## Risks and open questions

- OK is young. The docs list no limitations section, and pricing and hosting tiers were not visible in the pages read.
- Docx, pptx, and xlsx handling is unverified. Binaries stay as linked files for now.
- `ok init` behavior on an existing CLAUDE.md is unverified. Test on the copy.
- The docs recommend auto-approval for OK tool calls. Do not adopt that for write tools on this folder.
- Search quality and speed at 300 files are untested.

## Sources

- [OpenKnowledge overview](https://openknowledge.ai/docs/get-started/overview)
- [OpenKnowledge quickstart](https://open-knowledge-docs.preview.inkeep.com/docs/get-started/quickstart)
- [OpenKnowledge core concepts](https://open-knowledge-docs.preview.inkeep.com/docs/reference/core-concepts)
- [OpenKnowledge LLM wiki workflow](https://open-knowledge-docs.preview.inkeep.com/docs/workflows/karpathy-llm-wiki)
- [OpenKnowledge README](https://cdn.jsdelivr.net/gh/inkeep/open-knowledge@main/README.md)
- [OpenKnowledge on GitHub](https://github.com/inkeep/open-knowledge)
