# Chuck Haines — Workspace Context

## Workspace Structure

**CRITICAL** Use this as your guidance for how to find relevant context.

```
PersonalAssistant/
├── CLAUDE.md                    ← You are here — start every session by reading this
├── about-me.md                  ← Who Chuck is, role, strengths, working style
├── brand-voice.md               ← Voice and tone guide for drafting on Chuck's behalf
├── working-preferences.md       ← How Claude should work with Chuck
├── projects.md                  ← Index of all active projects with descriptions
├── memory/
│   └── glossary.md              ← Field Nation terminology and acronyms
├── people/
│   ├── people.md                ← Key people, titles, relationships
│   ├── cory-muir/
│   │   └── persona.md           ← Cory Muir persona
│   └── mike-mueller/
│       └── persona.md           ← Mike Mueller persona
├── projects/
│   ├── AI Software Factory/     ← Agentic software dev pipeline project
│   │   └── CLAUDE.md            ← Start here for AI Software Factory context
│   ├── BreakFix/                ← Break/fix fulfillment engine project
│   │   └── CLAUDE.md            ← Start here for Break/Fix context
│   ├── Core Transform/          ← FN 2.0/3.0 transformation initiative
│   │   └── CLAUDE.md            ← Start here for Core Transform context
│   └── UI Modernization/        ← Strangler-pattern frontend migration
│       └── CLAUDE.md            ← Start here for UI Modernization context
├── templates/
│   ├── External FN Deck Template.pptx
│   └── Internal FN Deck Template.pptx
├── research/
│   └── index.md                 ← Log of every research report (title, date, description)
├── brainstorm/
│   └── index.md                 ← Log of every brainstorm/discovery session (title, date, description)
└── artifacts/                   ← Deliverables and outputs
```

## Projects

**How to navigate project folders:** Always start by reading a project's `CLAUDE.md` first. Look for the **Workspace Structure** section — it describes the folder layout and points to the key files in the project. Don't rely on the CLAUDE.md alone for project understanding — follow its references to load the deeper context files.

## Research & Brainstorm Logs

- **`research/index.md`** — running log of every research report the `research` skill has produced. Check it before starting new research to avoid duplicating work.
- **`brainstorm/index.md`** — running log of every brainstorm/discovery session the `brainstorm` skill has captured. Check it before starting a new session in case a related one already exists.

## Who I Am

Sr. Principal Software Engineer at Field Nation — an on-site labor marketplace connecting field service companies with skilled IT technicians. I report to Cory Muir (Sr. Director, Software Engineering). I don't manage a team directly; my influence is cross-cutting across multiple teams. I focus on architecture, technical design, POCs, mentoring, and code review.

**What makes me different:** Bias to action — I bridge speed and thoroughness. I also excel at translating between technical and non-technical folks.

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

## Key People

| Name                | Title                              | Relationship                                              |
| ------------------- | ---------------------------------- | --------------------------------------------------------- |
| **Cory Muir**       | Sr. Director, Software Engineering | My manager, close collaborator · [persona](./people/cory-muir/persona.md) |
| **Mike Mueller**    | Sr. Principal Engineer             | Peer, close collaborator · [persona](./people/mike-mueller/persona.md) |
| **Spencer Moulton** | Principal Engineer                 | Pearl team tech lead (AI/agents) · [persona](./people/spencer-moulton/persona.md) |
| **Andrew Kandels**  | Principal Engineer                 | Close collaborator, co-leading UI Modernization           |
| **Ridi Haque**      | Software Engineering Manager       | Manages BD teams I work with                              |
| **Amran Hossain**   | Senior Engineering Manager         | Manages BD teams I work with · [persona](./people/amran-hossain/persona.md) |
| **Bilas Sarker**    | Staff Software Engineer            | BD engineering lead I work closely with                   |
| **Mobarok Hossain** | Staff Software Engineer            | BD engineering lead                                       |
| **Lynsay Faucett**  | Product Manager                    | Product coordination · [persona](./people/lynsay-faucett/persona.md) |
| **Linda Lo**        | Principal Product Manager          | Product coordination, critical for cross-stream coherence |
| **Tashfiq Islam**   | Product Owner                      | Product coordination                                      |
| **John Shaw**       | Group Product Manager              | Oversees PMs I work with                                  |
| **Alyssa Eby**      | Senior Product Designer            | Design collaboration                                      |
| **Brent Grill**     | Principal Product Designer         | Design collaboration                                      |
| **Ryan Schulte**    | Principal Product Designer         | Design collaboration, PM Playbook training · [persona](./people/ryan-schulte/persona.md) |
| **Shane Kinder**    | EVP Product                        | Leadership, same level as Cory · [persona](./people/shane-kinder/persona.md) |

→ Full details: [people/people.md](./people/people.md)

## Key Terms (Quick Reference)

- **FN-Core** — PHP monolith (main backend)
- **WO** — Work Order (core platform concept)
- **Provider** — Technicians doing work on the platform
- **Buyer** — Companies posting work for providers
- **BD** — Bangladesh (Dhaka), where much of the engineering team is located
- **PUDO** — Pick-Up/Drop-Off location (Unisys-specific, assigned at part level)
- **Smart Dispatch** — Automated WO-to-provider assignment (Break/Fix)
- **LTE** — Long Term Engagement (buyers to retain)
- **NetDev** — Internal team that vets/recruits providers for enterprise buyers
- **GTM** — Go to Market
- **DARRYL** — CTO's internal AI tool for natural language data queries
- **DX** — Developer experience metrics platform
- **Field Week** — Twice-yearly on-site event (US and BD)

→ Full glossary: [memory/glossary.md](./memory/glossary.md)

## Memory

**Do not use auto-memory.** Store all learned context, preferences, and feedback directly in the relevant `.md` files in this workspace (e.g., `working-preferences.md`, `about-me.md`, project CLAUDE.md files, etc.). No separate memory directory or MEMORY.md index.

## What I Use Claude For Most

1. **Research** — Technologies, patterns, architectural approaches (logged in [research/index.md](./research/index.md))
2. **Architectural validation** — Sanity-checking design decisions and tradeoffs
3. **Slack summarization** — Morning triage of important messages needing attention
4. **Technical design review** — First-pass review of Confluence technical designs
5. **Presentations & documentation** — Accelerating creation of decks and docs
6. **Brainstorming** — Thinking through plans and decisions out loud (logged in [brainstorm/index.md](./brainstorm/index.md))
