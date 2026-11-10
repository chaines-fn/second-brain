# Key People — UI Modernization

## Core Team

### Chuck Haines
- **Role:** Sr. Principal Software Engineer
- **Function:** Architectural oversight, technical direction, design reviews, PR reviews, estimation for scale enhancements
- **Timezone:** U.S. Central (CDT)
- **Email:** chuck.haines@fieldnation.com
- **Slack:** <@U067T9NK3MF>
- **Note:** Cross-cutting influence across teams. Created the proj-ui-modernization channel. Doesn't lead the team directly but shapes the technical approach.

### Andrew Kandels
- **Role:** Engineering lead (authored the UI Service Modernization Plan)
- **Function:** Day-to-day technical lead. Reviews and merges PRs, files bugs from testing, manages Linear issues, coordinates with support team on testing, drives milestone planning.
- **Timezone:** U.S. Central (CDT)
- **Email:** andrew.kandels@fieldnation.com
- **Slack:** <@U0USKC9CL>
- **Note:** Primary code reviewer for core-ui PRs. Owns the Google Doc that defines the project plan. Very hands-on. Was out 2026-07-30 through roughly 2026-08-06 ("next Thu") — back and actively directing the CI/build-time work in Slack on 08-06, on schedule with his expected return. _(added 2026-07-31; updated 2026-08-07)_

### Travis Lang
- **Role:** Engineer (exact scope not yet confirmed)
- **Slack:** <@U035QRDDF2S>
- **Note:** Named by Andrew Kandels as a point of contact for core-ui code reviews while Andrew is out 2026-07-30 to ~2026-08-06. First appearance in project Slack activity — role and typical function still unconfirmed. _(added 2026-07-31)_

### Yurii Dmytriiev
- **Role:** Contractor — Frontend Engineer
- **Function:** Component conversions, MUI migration, ESLint strictness, Flightboard work. Currently leading the effort to fold MUI components into core-ui and merge/publish storybooks.
- **Timezone:** Poland (CET/CEST)
- **Slack:** <@U0A78AC8RDL>
- **Note:** Active contributor — PRs for bug fixes, env refactoring, ESLint rules. Provides estimates for Flightboard Scale Enhancements.

### Ivan Sakhman
- **Role:** Contractor — Frontend Engineer
- **Function:** Bug fixes, refactoring, component work in core-ui. Working on Flightboard issues, localStorage cleanup, TopNav fixes.
- **Slack:** <@U0A9SHL0JBF>
- **Note:** Consistent contributor. Works closely with Yurii on refactoring decisions.

### Maria Saxton
- **Availability:** OOO 2026-10-15 to 2026-10-19, returning 2026-10-20 (per Slack 10-14). Reachable by phone for urgent items only. _(added 2026-10-15)_
- **Role:** Linear project lead across the WWT/Core-UI portfolio: WWT Phase 1 (Pre-reqs, now closed; Fast Follows, active) plus the two newly-surfaced Planned projects **Migrate Project Page to Core UI** and **Core UI Engineering Enablement** (both target 2026-12-31). _(expanded 2026-08-03)_
- **Function:** Sprint/bug triage for the WWT scale work, coordinates testing with the support team, manages the current-sprint backlog. Joined #flightboard-enhancements 2026-04-28.
- **Slack:** <@U085SGMR7B7>
- **Note:** New key stakeholder as of this sync — leads the project that replaced the old Chuck-led "Flightboard Scale Enhancements" backlog. _(added 2026-07-01)_

### Andriy Kurenda
- **Role:** Contractor — Frontend Engineer
- **Function:** PR contributor alongside Yurii and Ivan — filter bugs, UI fixes, build/CI troubleshooting on core-ui.
- **Slack:** <@U0B09QNQ8G3>
- **Note:** Active since at least mid-June 2026; not previously tracked here. _(added 2026-07-01)_ Posted sick leave for 2026-10-15 in Slack and was active on 10-16 (MUI 9.1 update, OBS-574 In Review). _(updated 2026-10-16)_ Was out sick starting 2026-08-03, duration not stated at the time, but was clearly back and productive by 08-04/05 (OBS-456, MUI 9.x upgrade) and filed + shipped OBS-476 same-day on 08-06, so the absence reads as brief rather than extended. _(updated 2026-08-07)_

### Miroslav Isikiyski
- **Role:** Backend Engineer
- **Function:** Monolith-side work supporting Flightboard, e.g. Kafka-based `last_routed` enrichment (schema-registry → logstash → monolith outbox). Previously sole owner of the OBS-434 elasticlist performance investigation (shipped, Done 2026-07-16) and related OBS-379 backend fixes.
- **Slack:** <@U0ASTSC3FRQ>
- **Note:** Bridges backend/monolith changes needed for Flightboard enrichment. _(added 2026-07-01)_ Was on vacation 2026-07-10 through 2026-07-27; **back and active as of 2026-07-28** (PR core-ui#440, picked up OBS-455 CoreUI Storybook work). _(updated 2026-07-31)_

### Fayshal Uddin
- **Role:** Engineer — reviews/releases core-ui changes
- **Slack:** <@UJMH8S140>
- **Note:** Frequently asked to review PRs and handle releases. _(added 2026-07-01)_ Asked questions on Andriy's Provider Match repo bump (approval pending as of 2026-10-29). _(updated 2026-10-29)_

### Todd Folie
- **Role:** Reports production bugs / WWT-facing issues (loom recordings of prod issues)
- **Slack:** <@U13TJ1FS7>
- **Note:** Likely QA or customer-facing contact for WWT. _(added 2026-07-01)_

### Karol Tluscik
- **Role:** Joining the team full-time for extra bandwidth this quarter. Ramping down other projects, expected at 100% allocation on this team in about 1-2 weeks (per Andrew Kandels, 2026-10-14). Confirm role and title.
- **Slack:** <@U0AT74JRDMM>
- **Note:** First appearance in #proj-ui-modernization 2026-10-12. Working on core-ui security dependency updates (OBS-630, core-ui PR #622) and asked for repo write access on 10-15. As of 10-16, also assigned OBS-594 (extend shared analytics and notifications for the Provider Match port, In Progress) and asked the team for more tickets. Role, contract/employee status, and wider workstream still to be confirmed. _(updated 2026-10-16)_ Took the provider availability view ticket on 2026-10-29 after Ivan's overlapping work was folded into another PR. _(updated 2026-10-29)_

## Stakeholders

### Molly Grailer
- **Role:** Unconfirmed — customer-facing (Spectrotel account)
- **Slack:** <@U3Q8K6L8P>
- **Function:** Reports Spectrotel user issues on the new Flightboard (disappearing column headers, custom-field search returning no data; OBS-584). Raised in #proj-ui-modernization-rollout 2026-10-12.
- **Note:** First appearance in this project's tracking. _(added 2026-10-12)_


### Lacey Selvagn
- **Role:** Product Design Lead
- **Function:** Filed all 22 issues in the Flightboard Scale Enhancements project. Driving requirements for WWT and high-volume buyer experience improvements. Providing copy and UX direction for Flightboard column/data enhancements. Partners with Laura Quist on prioritization and UI design for the WWT work.
- **Note:** Primary issue creator for the Scale Enhancements project. Works closely with Chuck on prioritization.

### Laura Quist (Douglas)
- **Role:** Sr. Customer Success Executive
- **Also appears as:** **"Laura Douglas"** in Linear (issue creator on OBS-447/449/450/465/466) and **"Laura Quist (Douglas)"** in Slack (<@U15PH15U4>, laura.quist@fieldnation.com). **Same person**, so do not track as two people. _(clarified 2026-08-03)_
- **Function:** Works directly with customers (WWT) to translate their needs into actionable work for the team. Drives feature requests, priority decisions, and backlog ranking. Files most of the WWT-originated Fast Follows tickets. Runs live production testing sessions with Todd Folie. Partners with Lacey on stack ranking in Linear.
- **Note:** Primary customer-facing voice on WWT Flightboard enhancements. Makes scope-vs-effort tradeoff decisions (e.g., opted for scheduled refresh over real-time messaging based on point estimates).

### Kimberly Zito
- **Role:** Unconfirmed
- **Note:** Filed OBS-462 (migrate Help menu + Agentforce integration to MUI nav) on 2026-07-28. First appearance in this project's Linear activity; role and function not yet established. _(added 2026-08-03)_

### Cory Muir
- **Role:** Product/stakeholder (likely PM or product leadership)
- **Function:** Tracks rollout progress, asks about timelines and customer-facing readiness, interested in storybook and component strategy.
- **Slack:** <@U057QF0Q402>
- **Note:** Asks status check questions in the channel. Wants visibility into when customers will see the new UI and how the component library strategy works.

### Erika Kensington
- **Role:** Sr. Customer Success Executive
- **Function:** Relayed a Flightboard settings-persistence complaint from buyer account ParkPlace Technologies to Chuck on 2026-08-07 (originally raised by ParkPlace's Desiree Larussa on a 2026-07-09 call, escalated by ParkPlace's Alvin Xiong).
- **Email:** erika.kensington@fieldnation.com
- **Note:** First appearance in this project. Distinct from Erika Massopust (also FN, the original recipient of ParkPlace's email) — do not conflate the two. _(added 2026-08-10)_

### Zach Hohloch
- **Role:** Unconfirmed, reporter (CS or product side)
- **Function:** Filed OBS-654 (Done tab due date sort does not persist, Urgent, via Linear Asks, 2026-10-22). Reporter on the location-name search ticket (OBS-640) discussed in the 10-23 WWT daily.
- **Note:** First appearance in this project. _(added 2026-10-23)_

### Talat Anik
- **Role:** Unconfirmed
- **Function:** Filed OBS-478 (Help "?" icon non-functional on new Core UI Flightboard) on 2026-08-10.
- **Note:** First appearance in this project's Linear activity; role and function not yet established. Ticket closed as Duplicate 2026-08-12, confirmed sharing OBS-462's fix as anticipated. _(added 2026-08-11)_

### Sarah Welter
- **Role:** Unconfirmed, customer-facing (CS side)
- **Slack:** <@U027BD329NX>
- **Function:** Likely the "Sarah" named in the 10-16 daily sync, who Andrew will ask to confirm a mobile Save-button overlap with a customer (OBS-634; identity unconfirmed). Forwarded buyer feedback from Nationwide Fixture Installations on the new Flightboard (2026-10-07). Filed OBS-579 ("new Flightboard challenges/bugs," 2026-10-08), still active in #proj-ui-modernization-rollout on 10-13.
- **Note:** First appearance in this project. _(added 2026-10-13)_

### Gina Johnson
- **Role:** Unconfirmed, customer-facing
- **Slack:** <@UEC3YF1LK>
- **Function:** Reports browser-specific issues on the new Flightboard, including Edge behavior with Ctrl+F (2026-10-12, #proj-ui-modernization-rollout).
- **Note:** First appearance in this project. _(added 2026-10-13)_

### Mari Aune
- **Role:** Unconfirmed, GTM/rollout communications (posts the rollout schedule in #gtm-updates)
- **Slack:** <@UBTEW2ZFC>
- **Function:** Reports buyer-facing issues to engineering, such as the chat bubble overlapping paging controls on the new Flightboard (2026-10-12), and requested the 10-12 demo of project page grid changes.
- **Note:** Not previously listed in people.md. _(added 2026-10-13)_

### Ben (last name unknown)
- **Role:** Unconfirmed — CS side
- **Note:** Named by Maria Saxton (Slack, 08-11) alongside Laura Quist as someone to work with on communicating the Flightboard 40%→100% rollout to CS and affected buyer sets (see OBS-482). First appearance in this project; not yet confirmed who this is. _(added 2026-08-12)_

### Adam (last name unknown)
- **Role:** Unconfirmed — CS side
- **Note:** Named by Maria Saxton (Slack, 08-11) as a cc on the Flightboard rollout CS communication (see OBS-482, Ben above). First appearance in this project. _(added 2026-08-12)_

### Lindsay (last name unknown)
- **Role:** Unconfirmed; QA/testing. Owns break-fix testing across roles (OBS-545) with Todd Folie and Laura Quist.
- **Note:** Brought into project testing by Chuck on 2026-10-14 for domain expertise on break-fix. _(added 2026-10-14)_

### Abdullah Reza
- **Role:** Quality team (role title unconfirmed)
- **Function:** Asked Maria Saxton in #proj-ui-modernization-rollout (10-15) about overlap between the Quality team's recent PSS column on Flightboard and the project Flightboard PSS work (OBS-578/583).
- **Note:** First appearance in this project. Overlap question still open. _(added 2026-10-15)_

### Ashley (last name unknown)
- **Role:** Unconfirmed. Named in the 11-06 WWT daily as a Match page rollout participant alongside Mari Aune, who has not yet started front-end work. Maria will ask Ashley and Mari to use Core UI and help test. _(added 2026-11-06)_

## Cross-Team Contributors

_People from other Linear teams (not Obsidian) currently shipping changes directly into Flightboard/elasticlist/core-ui — new as of 2026-08-10._

### Imran Chowdhury
- **Role:** Engineer, Diamond team
- **Function:** Building **DIAMOND-3489** (Provider Success Score column on Flightboard), PR core-ui#454, In Review as of 2026-08-10.
- **Note:** First appearance in this project. _(added 2026-08-10)_

### Monjurul Hoque
- **Role:** Engineer, Marble team
- **Function:** Working **MARBLE-664** (empty Condition column bug on Buyer Flightboard), In Progress as of 2026-08-10, with PRs across the legacy `ui` repo, `elasticlist`, and `fn_ete_playwright`.
- **Note:** First appearance in this project. _(added 2026-08-10)_
