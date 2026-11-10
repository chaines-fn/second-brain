# Chuck Haines — UI Modernization Workspace Context

## Workspace Structure

```
UI Modernization/
├── CLAUDE.md                    ← You are here
├── program/
│   ├── overview.md              ← Project description, goals, tracks
│   ├── people.md                ← Key people and stakeholders
│   └── references.md            ← relevant documents, slack channels, meetings and resources
├── me/
│   ├── about-me.md              ← Who Chuck is
│   ├── brand-voice.md           ← Voice and tone guide
│   └── working-preferences.md   ← How Claude should work with Chuck
├── knowledge/
│   ├── tech-context.md          ← Tech stack and architecture context
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

Sr. Principal Software Engineer at Field Nation, providing cross-cutting architectural oversight and technical direction. I lead architecture decisions, build POCs, mentor engineers, and review PRs — my influence spans multiple teams rather than leading one directly.

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

Warm and direct. Conversational over formal. Honest about uncertainty. Generous with credit. Uses "y'all", parenthetical asides, and light humor. Avoids corporate buzzwords and over-formality.

→ Full voice guide: [me/brand-voice.md](./me/brand-voice.md)

## UI Modernization Context

A multi-track initiative to modernize Field Nation's web frontend using a strangler pattern. The legacy UI monolith (dating back to 2008, with jQuery, Angular, and outdated React) is being incrementally migrated into Core-UI — a modern mono-repo built on React/TypeScript, MUI, and Vite. The goal is 90%+ of web traffic on an elevated experience with 2-3s page loads and <10m deploys.

→ Full context: [program/overview.md](./program/overview.md)
→ Key people: [program/people.md](./program/people.md)
→ All references (docs, tech context, Slack channels, meetings): [program/references.md](./program/references.md)

**Key things to know:**

- **As of 2026-11-09:** No UI Sync transcript today (sync went async; Maria cancelled the WWT daily for production incident remediation). Andriy opened PRs removing logo cache-busting query strings now that the Nov 7 date (OBS-576) has passed. Review requests open on core-ui #726, #739, #740, #742, #743.
- **As of 2026-11-05:** UI Sync (7:45): PSS on Flightboard needs a bulk-ID endpoint for lazy loading (Miroslav to sync with Fayshal, OBS-638). E2E flaky tests quarantined to unblock merges. Node LTS 26 PR up (Yurii). Harness CI migration on the backlog; Warp factory adoption underway (tour at 9:00). WWT daily: project FB PSS is a fast follow, not a release blocker.
- **As of 2026-11-03:** UI Sync (7:45): new Flightboard is the only supported version, with no old-UI paths. Full E2E suite repair is in progress (critical suite fixed; new FB cut retries ~50%). Ox lint migration started. WWT daily: flightboard redirect fix merges after peak traffic; OBS-690 (Urgent, saved filter drops Created Date) is with Todd. Maria out today.
- **As of 2026-10-30:** No transcript yet for today's UI Sync: Chuck and Andrew were in a TDR, so the 7:45 sync likely went async. Today's WWT daily notes not posted yet (re-check). OBS-662 and OBS-654 closed. New Urgent OBS-673 (mobile schedule date picker rejects future dates). No new decisions found in Slack or Linear.
- **As of 2026-10-29:** UI Sync (7:45, ~16 min): high-severity Node vulnerability fix bumps the Node minor version now (Chuck reviews at home), with a separate ticket for Node 26 LTS. Team agreed to trial the Rust-based React compiler behind a feature flag, adopt Ox lint/format with ESLint rules kept working, and test Bun for UI build/lint only. WWT daily: new Urgent bookmark bug OBS-668 (Andriy, In Review) to be validated by Laura and Todd; Todd's retesting deferred.
- **As of 2026-10-28:** UI Sync (7:45) had no notes at run time. WWT daily notes: school discovery may hit old code paths left beside new code after porting; Ivan's mirroring ticket is open and Andrew owes details. Todd filed a Type of Work list bug. Urgent Project Flightboard list and template tickets (OBS-656/657/658/662/663/664) are In Review with Yurii and Ivan. OBS-661 (project FB past 10k, elasticlist) is Todo with Miroslav.
- **As of 2026-10-27:** UI Sync (7:45, ~16 min): cloud limit pressure (Andriy at 97% of 250, Opus-heavy) led to moving to Sonnet/Haiku from Nov 1. Yurii and Ivan are moving to Field Nation cloud codes so the team can serve as the DX cost-per-PR baseline for the software factory comparison. E2E suite is being scoped to critical business paths only. 8:45 Obsidian/WWT daily: project page fixes merge then retest; external partner banner missing on core UI (owner unknown); PSS ownership (Quartz vs Diamond) pending; refinement cancelled.
- **As of 2026-10-26:** UI Sync (7:45, ~10 min) reported the new Flightboard now toasts all errors, so contract, user context, and reputation timeouts (mostly around noon) are visible to users. ATS Playwright locators broke after Harness deprecation (Andrew fixing). Miroslav on OBS-651, then OBS-650 source maps, then backend pagination. WWT daily (notes only): Todd confirmed deliverables complete and logged more bugs; OBS-652 starts today (Maria wants it first). Amplitude buyer isolation for the projects page is open. Delivery dates to set with Mari this week.
- **As of 2026-10-23:** UI Sync not held (async during TDR). WWT daily: project page ticket closed and handed off for testing; bugs go to the Testing and bug fixes milestone. Sorting persistence bug not reproducible (Yurii). OBS-642 closed. OBS-655 (e2e tests for new Flightboard) started. OBS-558 search_after still awaiting Andrew's approval.
- **As of 2026-10-22:** New Urgent items OBS-651 (contract call error toast, Miroslav) and OBS-652 (Provider Match MUI upgrade and CDN refresh, Andriy). The OBS-558 search_after proposal for project Flightboard pagination is waiting on Andrew's approval because it reverses his earlier backend change. Today's UI sync and WWT daily have no notes yet.
- **As of 2026-10-19:** Project page conversion bug (OBS-631) fixed and deployed early today. Chuck deactivated the Sonar magic-numbers rule to clear CI (scope to confirm). New Urgent mobile Flightboard tickets (OBS-639, OBS-641) and buyer-reported mobile save issue (OBS-642, Triage). OBS-643 (new project list links to old page) is High and open. Project FB PSS approach (index vs. lazy load, OBS-638) still undecided. Andriy has project FB pagination (OBS-558).
- **As of 2026-10-16:** The 7:45 UI Modernization Sync was not held (technical design review); the 8:45 WWT daily covered project handoff. About 15 core-ui PRs are pending review and a routes PR is still needed before the project page is testable (Andrew targets Monday 10-19). New Linear items OBS-637/638 flag a 10,000-row result cap on project Flightboard (possible OpenSearch truncation) and PSS scaling, tied to the scale risk. Maria is OOO through 10-19.
- **As of 2026-10-15:** Maria Saxton is OOO 10-15 to 10-19. Main Flightboard virtualization removal (OBS-629) in review. Bug-closure policy set: customer-visible UI fixes get review, backend fixes can be closed by the author. Dependency policy: fix high and critical only. Sonar Vortex setup rolling out; magic-number false positives go to Chuck as a list. OBS-631 (conversion page not loading in QA) picked up by Miroslav.
- **As of 2026-10-13: project page migration ships as one batch with direct cutover (final ticket OBS-539, ~10-16). Main Flightboard experiment removal proposed for 10-13/10-14, not yet confirmed. Open: OBS-584 disappearing column headers (Spectrotel, not yet reproduced), OBS-579 and OBS-585 new Flightboard reports, PSS on project Flightboard (OBS-583 backend, Miroslav). Karol Tluscik joined this quarter for extra bandwidth.**
- **P0 incident on 2026-10-05 — UI rollback to release 1520.** Early morning Oct 5 (07:59-08:36 CDT): ui-prod pods failed with CrashLoopBackOff after a UI deployment; work-order functionality unavailable, buyer impact. Team rolled back to release 1520. Root cause investigation ongoing; post-mortem planned. E2E tests didn't catch the issue.
- **As of 2026-10-09: logo cache bumps done across 10 repos (OBS-575); reverse PRs drafted until Nov 7 (OBS-576). Cloudflare /logos/* fix done (OBS-567, 10-06). Brand refresh logos appeared updated in Slack on 2026-10-08; favicon fix in review (ui PR #8701).** Earlier, on 2026-10-06, logo deployment was blocked on CDN caching. Andriy Kurenda discovered that Cloudflare is overriding S3's `Cache-Control: public, no-cache` directive for `/logos/*` paths to `public, max-age=2678400` (30 days), defeating the cache-busting strategy needed for brand logo updates. Test PR #589 confirmed the issue. Next step: Cloudflare configuration change to "respect original headers" for `/logos/*` paths. Affects brand launch timing. (PRs in flight: #580 core-ui, #785 herman, awaiting Cloudflare fix.)
- **Flightboard rollout: all remaining buyers moved 2026-10-08.** Andrew submitted final approval at 7:00 CDT, moving high-risk and opted-out buyers too. Feature flag kept on temporarily for power-user feedback before the legacy Flightboard is removed. New CS bug OBS-580: bulk-approving remote WOs on the new Flightboard prompts for a company address (tax). Earlier status (2026-10-02): phase 1 complete. Per #gtm-updates 2026-09-22 (Mari Aune): 40% live as of Sept 28; remaining 60% migrated Sept 29 — Oct 1 (on schedule); High-Risk buyers and ~15 opt-out buyers migrate Oct 8 after CS (Laura Quist) outreach. All build enhancements shipped on schedule (PSS, Pending Assignment, Messaging). Team velocity steady, no blockers reported. Historical data backfill strategy validated (~12 months for WWT PNC projects).
- **Infrastructure and tooling upgrades completed (2026-09-24).** SonarQube/Sonar Vortex fully enabled on core-ui with Okta authentication completed. Brand refresh logo assets consolidation (OBS-518, Miroslav) and CDN deployment fixes (PR core-ui#553, Andriy) shipped 2026-09-18. Packages migration structural phase complete. Core UI architectural refactoring actively advancing with layered architecture implementation (OBS-493): PRs #511 (TurboRepo-based), #503 implementing routes → features → components/shared-context → foundation with per-package npm scopes. Track 2 standardization moving forward with multiple concurrent TDRs. As of Oct 6, brand refresh logo PR #580 undergoing final review with Cloudflare config fix pending; email logo fixes in herman#785 ready. Project Manage page migration (Ivan, PR #571) adding new route /projects/:id/new/manage-project with AES mapping.
- **Active work on remaining enhancements and architectural migration.** **OBS-496/497** (Messages enhancements — scope WO message drawer to assigned-provider conversation, add assigned-count API field, Miroslav/Ivan; logstash#451 ready for review); **OBS-495** (Payment column historical data forward-population, expected forward-only behavior post-approval); **OBS-491** (User Columns Reset consolidation, sprint 26.17 owned ticket addressing recurring column-persistence complaints). **Nav-migration cluster:** OBS-462 (Help menu/Agentforce migration, awaiting backend JWT route per OBS-485); OBS-463/464 Done. **Team status as of Oct 6:** Yurii Dmytriiev returned from sick leave (Oct 4-5); Ivan, Andriy, Miroslav actively shipping. Multiple PRs in flight: #587, #588 (Ivan), #589 (CDN test, Andriy), #580 (brand review, Andriy), #578 (Miroslav).
- **Q4 strategic inflection: GA (general availability) readiness planning for Flightboard.** Maria Saxton asked Chuck 2026-09-03 to estimate/document effort for potential full GA rollout in Q4 — signals potential shift from phased 40%→100% strategy toward considering complete launch. Worth staying alert for follow-up guidance. (See [references.md](./program/references.md) Sync Log 2026-09-09 for context.)
- **Two planned follow-on projects (Maria Saxton):** **Migrate Project Page to Core UI** (target 2026-12-31) and **Core UI Engineering Enablement** (target 2026-12-31). Scope unstated; likely follow Fast Follows completion.
- **5 tracks total** — Flightboard → Single UI Process → Work Order Detail → User Profile → Deprecate Classic Pages
- **Core team:** Andrew Kandels (tech lead, day-to-day), Maria Saxton (leads WWT Phase 1 Fast Follows and two planned follow-on projects, sprint/bug triage), Yurii Dmytriiev (architectural refactor & Track 2 standardization), Ivan Sakhman (frontend), Andriy Kurenda (conversion/infra), Miroslav Isikiyski (backend/elasticlist), Cory Muir (product stakeholder)
- **Name gotcha:** "Laura Douglas" (Linear) and "Laura Quist (Douglas)" (Slack) are the **same person**: Sr. Customer Success Executive, primary WWT voice. Don't treat as two people.
- **Scale risk resolved.** ClearCaptions' ~400K-row project case: root cause traced to undersized Kubernetes resource limits (OBS-442, fixed 07-16) and UI-pattern fix (OBS-443, split data loads, shipped 07-21). No longer blocking; not explicitly load-tested at full scale but stable in production. **Open question:** "acting as"/impersonation architecture — OBS-440 was not the confirmation; root cause was unrelated concurrency bug (OBS-444).
- **Core-UI repo** is the destination for all conversions: `github.com/fieldnation/core-ui`
- **Linear project OBS** tracks all issues. Slack channels listed in [references.md](./program/references.md#slack-channels) — including #proj-ui-modernization (main) and #flightboard-enhancements (WWT focus)
