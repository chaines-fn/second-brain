# Chuck Haines — Break/Fix Program Workspace

## Workspace Structure

**CRITICAL** Use this as your guide for finding relevant context.

```
BreakFix/
├── CLAUDE.md                              ← this file
├── about-me.md                            ← Chuck's role and profile
├── brand-voice.md                         ← voice and tone guide
├── working-preferences.md                 ← how to work with Chuck
├── program/
│   ├── overview.md                        ← vision, roadmap, status, beta customers
│   ├── people.md                          ← key people across all teams
│   ├── slack-channels.md                  ← all Slack channels
│   ├── architecture.md                    ← services, Kafka, events, DB patterns
│   ├── references.md                      ← index of durable cross-team resources (briefs, master docs, dashboards)
│   ├── proposals/                         ← outstanding proposals (not-yet-adopted designs)
│   │   └── topic.md
│   └── meetings/                          ← program-level meeting notes
│       └── YYYY-MM-DD-topic.md
├── research/
│   └── index.md                           ← Log of research reports done in this project's context
├── brainstorm/
│   └── index.md                           ← Log of brainstorm/discovery sessions done in this project's context
└── teams/
    ├── SLATE/                             ← Network Development
    │   ├── team.md                        ← members, roles, channels
    │   ├── projects.md                    ← active and recent projects
    │   └── knowledge/
    │       ├── knowledge.md               ← key decisions, gotchas, open questions
    │       └── meetings/
    │           └── YYYY-MM-DD-topic.md
    ├── GARNET/                            ← Smart Dispatch & Fulfillment
    │   ├── team.md
    │   ├── projects.md
    │   └── knowledge/
    │       ├── knowledge.md
    │       └── meetings/
    └── MARBLE/                            ← Parts Management
        ├── team.md
        ├── projects.md
        └── knowledge/
            ├── knowledge.md
            └── meetings/
```

**Research & brainstorm logs:** kept local to this project — check before starting new work: [research/index.md](./research/index.md) · [brainstorm/index.md](./brainstorm/index.md)

## Who I Am

Sr. Principal Software Engineer at Field Nation. Cross-cutting role focused on architecture, technical design, POCs, mentoring, and code review. Bias to action — I bridge speed and thoroughness.

→ Full profile: [about-me.md](./about-me.md)

## How I Want Claude to Work

### Communication

- **Concise and to the point by default.** I'll ask for more detail if I need it.
- Match my conversational, warm tone. Be direct.
- Don't over-format with bullets and headers unless the content requires it.

### Safety Rules

- **Read operations: always OK** — Search Slack, read Confluence, check calendars, pull Linear data, web searches, file creation in workspace.
- **Write/delete operations: NEVER without permission** — No sending messages, creating issues, updating pages, posting comments, or modifying anything without explicit approval.

→ Full preferences: [working-preferences.md](./working-preferences.md)

### Voice

When drafting messages or documents for me: warm, casual, direct. "Hey y'all" energy. No corporate buzzwords. Honest about uncertainty. Share early, invite feedback.

→ Full voice guide: [brand-voice.md](./brand-voice.md)

---

## Working with This Workspace

All program knowledge lives in markdown files here — **do not use auto-memory for B/F context**.

- **Program-level meetings** go in `program/meetings/YYYY-MM-DD-topic.md`
- **Team-level meetings** go in `teams/<TEAM>/knowledge/meetings/YYYY-MM-DD-topic.md`
- **knowledge.md** files are structured with: Key Decisions (table), Integration Patterns & Gotchas, Open Questions
- For **technical/architecture** questions, load `program/architecture.md` alongside any team-specific files
- **Durable cross-team resources** (master requirement docs, briefs, dashboards) go in `program/references.md` — add a resource only when it's referenced repeatedly across sessions/teams. One-off meeting transcript links stay in their meeting note, not here.
