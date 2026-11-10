# Chuck Haines — Core Transform Workspace Context

## Workspace Structure

**CRITICAL** Use this as your guidance for how to find relevant context.

```
Core Transform/
├── CLAUDE.md                    ← You are here
├── program/
│   ├── overview.md              ← What Pearl is about, goals, Chuck's role
│   ├── people.md                ← Team members and stakeholders
│   └── references.md            ← relevant documents, slack channels, meetings and resources
├── me/
│   ├── about-me.md              ← Chuck's professional profile
│   ├── brand-voice.md           ← Voice and tone guide
│   └── working-preferences.md   ← How Claude should work with Chuck
├── knowledge/
│   ├── tech-context.md          ← Tech stack and evaluation status
│   └── meetings/                ← Dated meeting notes
├── research/
│   └── index.md                 ← Log of research reports done in this project's context
├── brainstorm/
│   └── index.md                 ← Log of brainstorm/discovery sessions done in this project's context
└── artifacts/
    └── (deliverables go here)
```

**Research & brainstorm logs:** kept local to this project — check before starting new work: [research/index.md](./research/index.md) · [brainstorm/index.md](./brainstorm/index.md)

## Who I Am

Sr. Principal Software Engineer at Field Nation, a field service labor marketplace. I lead architecture decisions, build POCs, mentor engineers, and drive technical alignment across teams. My influence is cross-cutting — I don't lead a team directly.

→ Full profile: [me/about-me.md](./me/about-me.md)

## How I Want Claude to Work

### Communication

- **Concise and to the point by default.** I'll ask for more detail if I need it.
- Match my conversational, warm tone. Be direct.
- Don't over-format with bullets and headers unless the content requires it.

### Safety Rules

- **Read operations: always OK** — Search Slack, read Confluence, check calendars, pull Linear data, web searches, file creation in workspace.
- **Write/delete operations: NEVER without permission** — No sending messages, creating issues, updating pages, posting comments, or modifying anything without explicit approval.

→ Full preferences: [me/working-preferences.md](./me/working-preferences.md)

### Voice

Warm and direct. Conversational over formal. Honest about uncertainty. Generous with credit. Shares early and invites feedback. Uses "y'all" naturally. No corporate buzzwords.

→ Full voice guide: [me/brand-voice.md](./me/brand-voice.md)

## Core Transform (Pearl) Context

Core Transform — codenamed "Pearl" — is a board-driven, high-visibility initiative to transform Field Nation into an autonomous virtual service company. Mission: "Transform the product experience leveraging AI to first automate buyer tasks, then work towards a new buyer experience where Field Nation acts as a virtual service company." The team is building a common agentic SDK/platform as the foundation, currently in an evaluation and upskilling phase targeting real product use cases in H2 2026. Initiative formally kicked off 2026-04-08.

→ Full context: [program/overview.md](./program/overview.md)
→ Key people: [program/people.md](./program/people.md)
→ Tech context: [knowledge/tech-context.md](./knowledge/tech-context.md)
→ All document/meeting references: [program/references.md](./program/references.md)

**Key things to know:**

- Chuck is the Architecture Lead on this initiative (not 100% dedicated — also working other AI projects)
- Core team: Spencer (team lead/implementation), Linda (product), Brent (design), Maria (eng manager), Kimberly, Max. Operates in #pearl-eng on Slack.
- Leadership sponsors: Shane Kinder, Travis Emslander, Cory Muir. CEO Mynul Khan wants to be brought along.
- Vision follows Now/Next/Later: FN 1.0 (manual) → FN 2.0 (task automation, human-in-loop) → FN 3.0 (virtual service company with super-agents, new buyer experience)
- Q2 OKR is "define the H2 plan" — upstream deliverables include activity shortlist, FN 3.0 concept vision, and technical architecture decisions
- In-person workshop in Minneapolis targeting late May / early June for concept vision sign-off
- Tech stack is under active evaluation: PydanticAI is the leading framework candidate, Temporal for durable execution, AWS Bedrock for LLM, AgentCore vs EKS runtime TBD
- Reference tracking is mandatory — log all shared docs, transcripts, and links in program/references.md
