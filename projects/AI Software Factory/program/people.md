# Key People — AI-DLC

*AI-DLC is the umbrella initiative (rebranded 2026-07-09 from "AI Software Factory" — see [program/overview.md](./overview.md#rebrand-to-ai-dlc-2026-07-09)); most people below are described in terms of the **AI Software Factory** workstream specifically, since that's where the work has concretely happened so far.*

## Core Team

### Chuck Haines
- **Role:** Sr. Principal Software Engineer
- **Function:** Architecture Lead for the AI Software Factory workstream of AI-DLC. **As of 2026-07-14, sole lead of Workstream 1 (AI Software Factory / Embed Model)**, working with contractors — Redwan rolled off the project the same day, and Joe shifted to leading Workstream 2.
- **Responsibilities:** Architecture validation, technical design, POCs, mentoring, code review — cross-cutting across all teams
- **Note (2026-07-16):** Pushing the H2 OKRs into concrete Jira/Linear epics and tickets ahead of a **2-week vacation starting 2026-07-20** (Mykhailo covers day-to-day, per the 2026-07-14 note below). Extending the 7/17 standup to walk the team through the full Q3 plan before he's out. Also organized today's "AI-DLC H2 Regroup" to review redrafted OKRs with leadership — see [program/overview.md](./overview.md#sonar-contract-signed-ai-dlc-h2-regroup-convenes-on-objective-2--team-selection-2026-07-16).
- **Note (2026-08-03):** **Back today** from vacation (calendar OOO block ran 2026-07-20 → 2026-07-31; he stayed partially engaged: kept up with Slack/email flagging replies for his return, attended AI Guild 7/23 + 7/30 and the 7/22 OKR Review, declined Moissanite standups). **Now formally full-time dedicated to AI-DLC**: Redwan's 7/30 Break/Fix-reallocation post treats this as settled fact, closing the 2026-07-08 resourcing-gap thread. Queue on return: Gitar demo + call with Ali Adl-Tabatabai, Sonar Launchpad kickoff (8/4), ai-toolbox PR #51 review/merge, Guru pilot, the Atiq-on-Break/Fix pushback, three unanswered OKR-tracker update reminders, and four on-hold AI Guild items. See [program/overview.md](./overview.md#vacation-window-sync-telemetry-workstream-lands-gitar-enters-as-code-review-candidate-tamvir-reassigned-2026-07-17--2026-08-03).
- **Note (2026-08-04):** Redwan's Break/Fix reshuffle proposal evolved again (see Tamvir entry below) — Chuck confirmed he's glad to dedicate most of his time to AI-DLC regardless of how the Tamvir/Atiq allocation lands. Also directed Vitalii toward a marketplace-based (non-APM) distribution POC for MOI-47, to demo with a small force-installed group before taking it to the AI Guild. See [program/overview.md](./overview.md#resourcing-re-proposed-apm-distribution-reconsidered-aws-partner-allcloud-surfaces-2026-08-04).
- **Note (2026-08-05):** Confirmed the MOI-47 APM-removal direction and is actively reviewing the renamed PR #51. Read AllCloud's "AI-DLC Offering Phase 0" as validation of FN's existing plan rather than a new direction. Opened two self-assigned skill-builder tickets (MOI-88, MOI-89). Setting up a Tuesday 2026-08-11 review meeting with Cory, Ben Moormeier, and Mike Mueller on the engineering-wide plugin force-install mechanism. Still hasn't followed up on Ian Loverink's Agent Architecture Deep Dive; hasn't yet picked a Gitar demo slot. See [program/overview.md](./overview.md#moi-47-apm-removal-becomes-the-working-direction-skill-builder-tooling-started-allcloud-read-as-validation-not-new-direction-2026-08-05).
- **Note (2026-08-07):** Fielded a new tooling question from Vitalii about Tessl (tessl.io, surfaced at the 8/6 AI Guild meeting) — has a Tessl meeting Monday 2026-08-10; told the team to proceed on the current path in the meantime, with a quick pivot if they decide to adopt it. Also has a Gitar technical call today (12:00pm CDT, with Ali Adl-Tabatabai and Jack Kolis) and a 1:1 with new hire Ehteshamul/Ehtasham Tamvir (9:30am Dhaka time) — both outcomes not yet known. See [program/overview.md](./overview.md#tessl-surfaces-as-a-new-evaluation-candidate-gitar-and-tamvir-meetings-land-today-2026-08-07).
- **Slack:** U067T9NK3MF

### Joe Hoppesch
- **Role:** Principal Engineering Consultant
- **Function:** **As of 2026-07-14, leads Workstream 2 (AI-DLC Process Transformation)** — see [artifacts/2026-07-14-q3-implementation-plan.md](../artifacts/2026-07-14-q3-implementation-plan.md). Previously a general partner on the AI Software Factory workstream alongside Chuck; that partnership continues informally (e.g. courtesy sign-off on Workstream 1 contractor staffing), but Chuck is now sole lead of Workstream 1.
- **Timezone:** America/Chicago
- **Email:** joe.hoppesch@fieldnation.com
- **Slack:** U06C4QGDRTM
- **Note (2026-07-10):** Has built "Op Cop," a custom agent that uses Datadog to investigate production issues. Surfaced during a brainstorm on the Architecture doc's open items — flagged as the comparison point once the Factory's own post-production incident/root-cause loop (see Architecture & Overview.md open item 12, AWS DevOps Agent) is evaluated post-implementation. Not otherwise documented in this workspace yet — worth a fuller note from Joe.

### Redwan Shahzada — rolled off AI-DLC (2026-07-14)
- **Role:** Director of Engineering
- **Function:** Was a partner on the AI Software Factory workstream through 2026-07-13 (co-presented the H2 plan, drove OKR/enablement-model brainstorms). **Rolled off the AI-DLC project as of 2026-07-14** — no longer holds any ticket ownership or decision authority in [artifacts/2026-07-14-q3-implementation-plan.md](../artifacts/2026-07-14-q3-implementation-plan.md). Historical notes elsewhere in this workspace describing his prior involvement are left as-is (accurate record of what happened at the time).
- **Timezone:** Asia/Dhaka
- **Email:** s.redwan@fieldnation.com
- **Slack:** U086ZBC2UL9
- **Note:** Bangladesh + US teams together = ~100 engineers; Redwan's team was a significant part of the engineering capacity being designed for. Whether/how his prior responsibilities (Bangladesh team leadership) get reassigned outside of AI-DLC is not tracked in this workspace.
- **Update (2026-08-17):** Still showing up in Gitar trial conversations more than a month after rolling off — attended the "Sonar & FieldNation: Pre-trial Sync (Gitar)" call (1:30pm CDT) alongside Chuck and Joe, an accepted (not just stale-invite) attendance. Not yet clear whether this reflects an informal advisory role Chuck still wants him in, or just inertia.
- **Update (2026-08-19):** Accepted (optional) on today's "Sonar & FieldNation: Trial Kickoff Call - Gitar" (1:00-2:00pm CT) — third consecutive Gitar meeting attended post-rolloff. Separately relayed a request from Cory (8/18 DM) about using the Sept 13 BD Field Week AI Hackathon for AI-DLC-connected repo work; still acting as an informal go-between on AI-DLC topics more than a month after formally rolling off. See [program/overview.md](./overview.md#gitar-trial-kickoff-today-bd-hackathon-repurposed-toward-ai-dlc-repo-work-copilot-mdm-push-hits-a-snag-2026-08-1819).

### Ehtasham Tamvir — Principal Engineer, Bangladesh (started 2026-08-03)
- **Role:** Principal Engineer (new hire, BD team)
- **Function:** **Correction 2026-07-30: start date and role assignment both changed from the 2026-07-15 record.** Redwan announced in #eng-corys-directs that the new BD Principal is **Ehtasham Tamvir**, starting **August 3, 2026** (not August 17 as relayed 7/15). He is **not** the Breakfix backfill: Redwan's proposal is Tamvir → **Granite** (strong fintech background), with **Atiqul Haque** moving off Granite to take over Break/Fix instead, framed explicitly as necessary "since Chuck's now dedicated full-time to AI-DLC and we're short on principal bandwidth there."
- **Status: unresolved.** Chuck pushed back the same day (group DM with Cory and Redwan): *"Atiq isn't strong enough for break/fix so I don't believe that a good idea. I can give some more details when I'm back next week."* Mike Mueller agreed Tamvir fits Granite (Abu Sayed's team moves fast, would benefit from stronger guidance) but hedged on Atiq for Break/Fix, noting Bilas and Mobarok are solid and mainly need best-practices help, which is Atiq's strength. Redwan floated the allocation for a gut check, not as a locked decision. **Expect Chuck to reopen this his first week back.**
- **Background:** Chuck was an enthusiastic advocate during the interview process (2026-07-08 hiring-panel Slack thread with Andrew Kandels and Atiqul Haque); panel consensus was a strong hire, with Andrew flagging a minor "answers seemed too polished" gut check that Chuck and Atiqul felt was outweighed by Tamvir's demonstrated depth (e.g., saga pattern implementation experience).
- **Not the same person as** `ahtasham.billah.himel@fieldnation.com`, who appears on AI Guild and EDA Guild invites: similar-looking name, different individual.
- **Possible name-spelling correction (flagged 2026-08-07):** a calendar invite ("Connect with Ehteshamul Tamvir") and its linked Confluence onboarding doc ("Ehteshamul Tamvir - Principal Software Engineer Onboarding") both spell the name **Ehteshamul Tamvir**, and his email is `ehteshamul.tamvir@fieldnation.com` — not "Ehtasham" as recorded in this section's heading and prior notes. Flagging rather than renaming outright since the two forms haven't been reconciled with Chuck; worth confirming the correct spelling next sync.
- **Update (2026-08-04): allocation reopened again, third version.** Redwan (group DM with Chuck and Cory): Tamvir starts on **Break/Fix** (not Granite as proposed 7/30), mentored by Chuck, with a possible later move to Granite given his fintech background. **Atiqul Haque now proposed for Platinum** (not Break/Fix as floated 7/30) — Platinum works the PHP monolith, fits Atiq's PHP strength, and gives Mike Mueller extra monolith support. Chuck is open to multiple configurations (Tamvir on both Break/Fix + Granite, or Chuck managing Break/Fix directly) and confirmed he's glad to focus most of his time on AI-DLC either way. **Still not locked in** — this is the third distinct proposal for this allocation in as many weeks (7/15 → 7/30 → 8/4); treat as evolving, not settled, until it sticks. New context surfaced in the thread: Break/Fix is narrowing to two teams as **Marble moves to the Data Platform**.
- **Update (2026-08-14): strong early performance, allocation itself still unresolved.** Tamvir is already reviewing Technical Designs and giving substantive feedback — e.g. inline Confluence comments on the "Work Order Invoice in NetSuite" TDR catching a cross-system clock-skew bug (proposed a monotonic sequence number instead of comparing NetSuite vs. AccountingDB timestamps) and flagging a missing rate-limit on an fn-core query API. Chuck relayed this to Cory and Mike Mueller: "Tamvir is already in there reviewing TDs and providing good input… I have really high hopes for this guy." Doesn't resolve the Break/Fix/Granite/Platinum allocation question, but is the first concrete evidence of his technical quality since starting. **New calendar item:** "Mike & Chuck connect with Tamvir," Fri Aug 21, 10:30-11:00am CDT — an overview of Field Nation's engineering architecture and technical vision.
- **Update (2026-08-17):** Slack displays his name as **"Ehteshamul Haque Tamvir"** (looped into a Netdev TDR review thread today by Safin Ahmed, alongside Chuck) — consistent with the 8/7 spelling flag, still not reconciled directly with Chuck. Continues the pattern of him actively reviewing TDRs outside his own team.

### Cory Muir
- **Role:** Sr. Director, Software Engineering
- **Function:** Initiative owner; Chuck's manager
- **Note:** Owns AI-DLC at the leadership level; authored the core operating model document. **2026-07-08:** In response to Shane's "Paper-Cuts" tiger-team proposal (which Chuck opposes — see Shane Kinder note below), Cory countered with routing the work through AI SDLC instead: bring problems in and solve them via the owning team(s), with Tashfiq as a dedicated intake PM. Idea stage only.

---

## Key Stakeholders (Broader)

### Mike Mueller
- **Role:** Sr. Principal Software Engineer
- **Function:** Likely the most significant internal skeptic on AI-DLC
- **Timezone:** America/Chicago
- **Email:** mike.mueller@fieldnation.com
- **Slack:** U0713V2HBKN
- **Note:** Peer-level to Chuck. His buy-in (or at least neutralization of skepticism) matters for org-wide adoption. Winning Mike over means the approach needs to be demonstrably grounded and quality-first, not hype-driven.
- **Note (2026-09-17):** Joined Chuck's embed-pilot feedback session with the engineering-dbt (Emerald) team. Argued agents could help with DBT layer-placement decisions via guiding questions (pushing back on Imtiaz Chowdhury's view that it's purely human judgment), and separately flagged that DBT lineage tracing may need to be a cross-repo capability rather than engineering-dbt-specific. Also revealed he's been covering the team's recurring Thursday data-governance meeting in Chuck's place for the past year. See [knowledge/meetings/2026-09-17-ai-enablement-engineering-dbt.md](../knowledge/meetings/2026-09-17-ai-enablement-engineering-dbt.md).

### Spencer Moulton
- **Role:** Principal Software Engineer
- **Function:** Working on agentic experiences for Field Nation consumers; has hands-on experience in the AI/agentic space
- **Timezone:** America/Denver
- **Email:** spencer.moulton@fieldnation.com
- **Slack:** U0ANJRRLEV6
- **Note:** Has practical agentic experience from the consumer product side. Author of SDDF (Spec-Driven Development Framework — https://github.com/spencemol/sddf). Chuck has been chatting with Spencer and Mike about how skills/context/specs developed by the AI Software Factory can also be used by engineers locally — Spencer sees this as a huge value add. His SDDF work is a strong candidate for adoption as the spec-driven development standard within the initiative.

### Ian
- **Role:** TBD (moved from TBD Stakeholders section — now actively relevant)
- **Function:** Has been working with Microsoft's APM (Agent Package Manager). His APM work is directly applicable to the AI Software Factory workstream as the packaging and distribution standard for skills, context files, and spec templates.
- **Note:** Cory wants him looped into the Software Factory (as of 2026-04-10). Chuck flagged Ian's APM work in the #ai-software-factory channel on 2026-04-15 as something the initiative should adopt — using APM as the standard means it works in both the factory harness and for engineers locally. See: https://github.com/fieldnation/ai-toolbox/pull/3. Ben confirmed in Q2 NNL review (2026-04-16) that Ian should be involved in platform/infra spikes (Bedrock, Agent Core, Langmith, Claude managed agents).
- **Open flag (2026-07-28): possible duplicate agent-platform effort.** Chuck spotted a rescheduling request for an **"Internal Agent Architecture Deep Dive"** Ian is running (moved to Aug 5, 1:30–2:30) and raised it to Cory and Joe: *"it sounds like Ian is working on some kind of internal agent thing? That seems to have a lot of cross over with the AI-DLC work for running agents so just calling it out as maybe something worth asking WTF he's working on unless anyone knows."* Joe didn't know either. Directly overlaps O2 (long-running agent platform); unresolved, worth Chuck closing out. **Still unresolved as of 2026-08-04** — Chuck told Cory he "still hasn't dug into that message from Ian."

### Ben Moormeier
- **Role:** Director of Information Security
- **Function:** Owns security posture for the AI Software Factory workstream — SOC 2 Type 2 compliance framing, security review of the harness, platform selection from a security lens (AWS Bedrock / Agent Core / Azure / managed third-parties all seen through that filter). Responsible for looping Ian in on platform spikes.
- **Note:** Planted the "safe path to remove human-in-the-loop" seed at the Q2 NNL review (2026-04-16) — flagged that without it, senior engineers like Chuck will be buried in code review. Also raised SOC 2 Type 2 as a framing constraint throughout. Sandro Perez (InfoSec Manager) reports into this function.

### Paul Wochnick
- **Role:** Owns MDM (mobile device management) for Field Nation engineering, per Ben Moormeier
- **Function:** Executing the Copilot-side marketplace rollout for MOI-47 — pushed a test MDM profile (`com.github.copilot` / `extraKnownMarketplaces`) to Chuck's machine on 2026-08-18 to validate the mechanism ahead of wider rollout. First test didn't surface the marketplace in Copilot despite a correctly-typed, confirmed-installed profile; troubleshooting ongoing. See [knowledge/tech-context.md](../knowledge/tech-context.md).
- **Note:** New contact as of 2026-08-18, surfaced via a Chuck/Ben Moormeier/Cory group DM.
- **Update (2026-08-20):** Chuck put a "Sonar SSO Setup" meeting on calendar with Paul today — outside his usual MDM/Copilot scope, plausibly to help troubleshoot a SonarSource support ticket (#73101, opened 8/19) about a team member unable to log into the US Sonar instance via GitHub. Connection between the two not confirmed.

### Sandro Perez
- **Role:** Information Security Manager
- **Function:** Security stakeholder for the AI Software Factory workstream. Works with Ben on security implications of the harness. Ben referenced him by name at the Q2 NNL review (2026-04-16) when raising security vulnerabilities as a candidate intake domain for the factory.

### Stephen Wald
- **Role:** Product Manager
- **Function:** PM-side stakeholder for the initiative. Asked the sharpest framing questions at the Q2 NNL review (2026-04-16) — "I don't understand what we're building," challenged whether the devil's advocate was premature engineering, and pushed hardest for picking a *specific* bug ticket as a concrete reference point for end-of-quarter success.
- **Note:** Good source of "keep this concrete" pressure. Treat his questions as a useful proxy for what broader PM audience will ask.

### Shane Kinder
- **Role:** EVP Product
- **Function:** Executive product leadership. Attended Q2 NNL review (2026-04-16). Asked about the relationship between this initiative and the Pearl team's customer-facing agentic work, and pushed for a concrete end-of-quarter demonstration (ticket → PR → prod).
- **Note:** Senior exec audience — framing/communication around this initiative needs to land cleanly with product leadership, not just engineering. **2026-07-08:** Proposed standing up a dedicated "Paper-Cuts" team — a VOC-style tiger team that would jump into different parts of the platform fixing operational inefficiencies. Chuck is against running this as a dedicated tiger team (worried it bogs down and pulls focus from teams' own product work, and scope often spans multiple teams anyway). Cory countered with routing the work through AI SDLC instead — see note under Program Overview and under Cory Muir above. Not yet resolved with Shane.

### Tashfiq
- **Role:** TBD
- **Function:** Proposed by Cory (2026-07-08) as the dedicated intake PM if the Paper-Cuts idea gets folded into AI SDLC instead of run as a standalone tiger team — would own bringing operational-inefficiency problems in and routing them to the appropriate owning team(s) to be solved through the AI SDLC pipeline.
- **Note:** Idea stage only — not yet discussed with Shane, Redwan, or Joe.

### Travis Emslander
- **Role:** CTO
- **Function:** Active voice in H2/Q3 planning — pushed back on the "AIDLC" naming (2026-07-13) until Chuck explained the industry-convergence reasoning, and was one of two people (alongside Chuck) who independently proposed pivoting to embed-with-teams-first over building the AWS pipeline first.
- **Note (2026-07-13):** Proposed deprioritizing the AWS production-path account build-out in favor of immediate impact via Claude/Copilot-driven skill-building across the top 10 most-committed repos — Chuck and Cory agreed. See [program/overview.md](./overview.md#embed-first-pivot--pilot-scope-narrowing-2026-07-13).

### Ann-Bettina Colace ("ABC")
- **Role:** Product Design Lead
- **Function:** Design leadership stakeholder for the initiative. Attended Q2 NNL review (2026-04-16). Limited speaking role in that meeting but should be considered the design voice as the factory expands to UI-affecting work.
- **Nickname:** ABC

---

## External Partners — AWS

### Lauren Hood
- **Company:** Amazon (AWS)
- **Email:** lauhood@amazon.com
- **Note (2026-07-14):** Organized a call titled "AWS/Field Nation: AI-DLC Discussion" (2026-07-14, 12:30-1:00pm CDT) with Chuck, Cory Muir, and Folarin Alamu (Amazon) — Chuck accepted, invite created 2026-07-13. First documented direct AWS-side engagement specifically framed around AI-DLC; likely connects to the open M3 platform decision (AWS Agent Core / Bedrock vs. GCP) given the timing, but the specific agenda wasn't confirmed from the invite alone. Outcome not yet known as of this sync.

### Folarin Alamu
- **Company:** Amazon (AWS)
- **Email:** alamuf@amazon.com
- **Note (2026-07-14):** Attendee (needsAction as of invite) on the same "AWS/Field Nation: AI-DLC Discussion" call above.

### Monica Zamora · Daniel Beachnau
- **Company:** AllCloud (AWS partner)
- **Emails:** monica.zamora@allcloud.io · danny.beachnau@allcloud.io
- **Note (2026-08-03):** Cc'd on Lauren Hood's "AllCloud & AI-DLC" email to Cory Muir, forwarding an "AllCloud - AIDLC Offering Phase 0" PDF ahead of a Thursday (2026-08-06) follow-up. First appearance of AllCloud in the workspace — exact role/offering not yet clear (contents weren't accessible this sync).

---

## External Partners: Warp

*Added 2026-10-30. Warp is running a Factories POC with Field Nation, kicked off 10/29. See [tech-context](../knowledge/tech-context.md#warp-factories-poc-kicked-off-2026-10-29).*

### Killian Jackson
- **Company:** Warp
- **Email:** killian@warp.dev
- **Slack:** #ext-fieldnation-warp
- **Note (2026-10-29):** Posted the POC kickoff recap and next steps; organized the Wed 11/04 working session.

### Ian McConchie
- **Company:** Warp (organizer of the Warp Factories kickoff invite)
- **Email:** ian.mcconchie@warp.dev
- **Note (2026-10-29):** Explained the per-run cost breakdown in the Warp platform and proposed a budgeting-scenarios call for the week of 11/02.

---

## External Partners: Sonar / Gitar

*Added 2026-07-27/28 as the Sonar relationship moved from procurement into delivery, and as Gitar (Sonar's AI code-review product) became a live candidate for the open O1 code-review decision.*

### Jack Kolis
- **Company:** SonarSource, account lead for Field Nation
- **Slack:** U08LQH69UMR (#fieldnation-sonar)
- **Note:** Drove the trial, business case, Tropic procurement path, and contract close. Handed off to Loren Williams for onboarding 2026-07-27 while staying in the thread. Is also the one pushing the Gitar conversation.

### Ali Adl-Tabatabai
- **Company:** Sonar, **EVP, and founder of Gitar**
- **Slack:** U0B4PNJKVN2 (joined #fieldnation-sonar 2026-07-28)
- **Note (2026-07-28):** Looped in by Jack specifically to talk through Chuck's objection to how agentic code review presents its findings. Opened with: *"I'd love to hear more about your thoughts on how we can improve signal in our code reviews."* A call with him is pending Chuck's return; this is the decision-shaping conversation for O1's code-review tool pick.

### Loren Williams
- **Company:** SonarSource, customer success / onboarding
- **Email:** loren.williams@sonarsource.com
- **Note (2026-07-27):** Introduced by Jack as the Launchpad + support owner. Kickoff being scheduled for **Aug 4** (Joe deliberately left Chuck a catch-up day after vacation).

### Gautam Korlam
- **Company:** Sonar, part of the Gitar founding team
- **Note (2026-08-11):** Added to #fieldnation-sonar by Jack Kolis to assist with the Gitar trial. Also on the invite for the newly-surfaced 2026-08-17 "Sonar & FieldNation: Pre-trial Sync (Gitar)" (1:30-2:00pm CDT), alongside Jace Key.

### Shane Nazareth · Vinod K. Avinane
- **Company:** ITMethods, Sonar's delivery team
- **Emails:** shane.a.nazareth@itmethods.com · vinod.avinane@itmethods.com
- **Note (2026-07-27):** Brought in by Loren for license delivery and Launchpad execution.

### Daniel Goldman
- **Company:** ITMethods, Sonar's delivery team
- **Note (2026-08-17):** New ITMethods contact — organized a "Launchpad Session: Fieldnation/iTmethods" call for 2026-08-18 (2:00-3:00pm ET), goal stated as getting Chuck logged in so he can enable his team and set up SSO. First appearance of this name in the workspace.

*Account note: `sonarqube-admins@fieldnation.com` is the main SonarQube owner account (set by Joe, 2026-07-27) so org-level resource permissions aren't tied to an individual.*

---

## External Partners: Glean

### Sandra Rabadi
- **Company:** Glean, Sr. Technical Account Executive
- **Email:** sandra.rabadi@glean.com
- **Note (2026-08-05):** New Glean point of contact for Field Nation, replacing Dan (who left, per a 2026-07-22 heads-up from Dylan Kuntz, Glean Solutions Engineer). Emailed Cory, Ben Moormeier, and Chuck introducing herself and asking how "the Anthropic rollout is going" and whether Glean can complement it. Glean was last raised in this workspace by Cory on 2026-06-23 as a Guru/Unblocked alternative worth due diligence; this is the first dedicated contact/thread since. Not yet acted on.

---

## External Partners: Tessl

*Added 2026-08-10 — Tessl (tessl.io) surfaced 2026-08-06 via the AI Guild as a new evaluation candidate (see [program/overview.md](./overview.md#tessl-surfaces-as-a-new-evaluation-candidate-gitar-and-tamvir-meetings-land-today-2026-08-07)); these are the vendor contacts for the intro call.*

### Adrian
- **Company:** Tessl
- **Email:** adrian@tessl.io
- **Note (2026-08-10):** Set up and rescheduled the intro call with Chuck (moved from 11:00 to 11:30am CT on 2026-08-10 for a scheduling conflict). Attending today's call along with Shawn.

### Amy W.
- **Company:** Tessl, leads Europe business
- **Email:** amyw@tessl.io
- **Note (2026-08-06):** First response to Chuck's inbound interest, introducing herself and looping in Adrian.

## External Partners: DX (getDX)

*Added 2026-08-10 — new contacts tied to a "Field Nation | DX - AI DLC Sync" meeting on calendar today, timing suggesting it's the venue for Shane Kinder's 8/6 question on KR1 repo-scorecard readiness (see [program/OKRs/h2-2026-okrs.md](./OKRs/h2-2026-okrs.md)).*

### Taylor Brown
- **Company:** Atlassian / getDX — email domains for both (tbrown3@atlassian.com, taylor.brown@getdx.com) appear on the invite; relationship between the two not yet confirmed
- **Note (2026-08-10):** Organizer of today's "Field Nation | DX - AI DLC Sync" meeting.

### Cheng Tang
- **Company:** getDX
- **Email:** cheng.tang@getdx.com
- **Note (2026-08-10):** Optional attendee on the same DX/AI-DLC sync meeting.

---

### Jobaer Ahamed - Engineer (Moissanite ticket owner)
- **Function:** Owns MOI-163 (file drift between ai-toolbox and other repositories, Todo) as of 2026-10-08. Chuck first assigned the same work as MOI-107 on 2026-09-10; MOI-107 was canceled and re-filed as MOI-163 on 10/08. Also active in other FN engineering channels (#eng-help API philosophy work, #basalt-eng Workato integration), so Moissanite is one of several commitments.
- **Source:** Linear (MOI-107, MOI-163) and Slack search, 2026-10-12. Role and title not confirmed in the workspace.

## TBD Stakeholders

The following roles will be involved but specific individuals are not yet confirmed:
- **Ben Moormeier** — Moved to Key Stakeholders section above (Director of Information Security, actively engaged as of 2026-04-16)
- **Ian** — Moved to Key Stakeholders section above (working with APM, now actively relevant to the initiative)
- **Arefin** — Leads teams with Opal and Basalt; had AI demos presented at TDR (2026-04-09). **Increasingly relevant as of 2026-07-08** — the AI SF → AI SDLC alignment session named Basalt (alongside Sapphire) as the leaning candidate for the first AI SDLC pilot team, specifically because it's lower-visibility and gives full product+engineering control. If Basalt is confirmed as the pilot, Arefin becomes a direct stakeholder, not just TBD. Worth confirming whether Arefin's team is the same "Basalt" referenced in the pilot discussion before looping him in.
- Group Product Leads (PM stakeholders)
- Group Design Leads (design stakeholders)
- Engineering Team Leads (implementation stakeholders)
- **Security consultant** — Cory floated bringing in an external security consultant (parallel to contractor engineers) at Q2 NNL review (2026-04-16)

---

### Tashfiqul Islam (possible intake PM, unconfirmed)
- **Function:** Asked on 10/30 to join the factory after Chuck said a VOC list will run through it. Chuck said he will be added to the list once more of the factory is locked down. May be the same person as Tashfiq, the intake PM Cory proposed on 2026-07-08. Not confirmed.
- **Slack:** UCSBYB1MM

---

## Contractors

Contractors are being sourced through **Svitla** to help build out the platform.

### Active Contractors

**Vitalii Belinskyi**
- **Started:** Mid-June 2026
- **Focus:** ai-toolbox infrastructure — completed the bundle/auto-update system that propagates skills to consumer repos whenever ai-toolbox changes (MOI-47, In Review). **MOI-48 event-skill split resolved 2026-07-16/17:** recast from `fn-event-design` to **`fn-event-integration`** (layers B+C — integration reliability + per-language recipes — in ai-toolbox; PRs #52+#55, In Review, pending principals approval). Layer A (contract authority) moved to `fieldnation/schema-registry` as a separate ticket (MOI-52, In Progress). Eval results: Opus-graded 100% (63/63) with-skill vs. 70% baseline, +30pp. See [knowledge/tech-context.md](../knowledge/tech-context.md). **As of 2026-07-06, also weighed in on the M3 hosting-platform decision** — Joe asked him to review Asad's AWS Bedrock AgentCore vs. GCP Vertex Agent Engine research and give his own opinion on which platform makes sense.
- **Focus update (2026-07-23 → 08-03):** Now leading **MOI-80: Claude Code execution telemetry (OTel)**, the instrumentation layer under the O1 repo scorecard. Ran real Claude Code sessions end-to-end into Datadog, verified both metric and event layers against actual captured telemetry, and authored the layer-1→getDX / layer-2→Datadog recommendation. Also closed out **MOI-47** with the *vendored-with-automation* delivery decision (Renovate auto-merge + `apm-refresh` workflow; fetch-on-use spiked and rejected). [ai-toolbox PR #51](https://github.com/fieldnation/ai-toolbox/pull/51) is waiting on Chuck's review and merge. MOI-52 (schema-registry contract authority, [PR #163](https://github.com/fieldnation/schema-registry/pull/163)) moved to In Review 7/23. See [knowledge/tech-context.md](../knowledge/tech-context.md).
- **Update (2026-08-04):** Proposed dropping APM as the Tier 2 distribution mechanism entirely in favor of native Claude Code/Copilot marketplace-plugin support (one manifest serves both) — Chuck independently reached the same conclusion on the same call. PR #51 (now branch `feat/MOI-47-ai-toolbox-integration-standard`) has both tools resolving skills locally; force-install/auto-update via managed settings is the untested piece, pending Chuck's Admin Console access. See [program/overview.md](./overview.md#resourcing-re-proposed-apm-distribution-reconsidered-aws-partner-allcloud-surfaces-2026-08-04) and [knowledge/tech-context.md](../knowledge/tech-context.md).
- **Update (2026-09-15 standup):** Merged the harness PR Joe requested and finished the repo-SDK bootstrap work (initializes OpenSpec in a repo, generates ticket specs); confirmed the "red validate run" CI check correctly flags a broken pipeline (earlier flakiness fixed). Kafka PR is Chuck-approved, pending outdated-comment cleanup before merge. See [knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md](../knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md).
- **Update (2026-09-16 standup):** **Transitioning off the project effective 2026-09-16.** Will focus exclusively on closing out repo bootstrap PR and Harness 911 ticket work through end of week (target ~2026-09-20). Handover call with Mykhailo scheduled to review contributions and document his work. Currently blocked on Harness admin access to configure toolbox create-trigger for pipelines. See [knowledge/meetings/2026-09-16-moissanite-standup.md](../knowledge/meetings/2026-09-16-moissanite-standup.md).
- **Update (2026-09-18 DM with Chuck/Joe):** Joe may pull Vitalii next week for a separate project, but **split-time arrangement confirmed available** — Joe indicated willingness to let him split his hours between AI-DLC wrap-up work and the new project. Vitalii had been working compressed hours (evenings/nights) this week for family care (grandmother's Parkinson's), no timeline impact expected. This keeps MOI-118/119/124 (macOS packaging for ai-repo-readiness) unblocked through end of week.
- **Update (2026-09-21):** Harness CI setup still blocked on three items Vitalii flagged 9/18 — PR trigger UI config (YAML ready, needs admin), GitHub connector secrets (for AI Guild), commit-status write on GitHub App. None resolved yet; Joe owns likely. Split-time arrangement with new project still stands. Family care situation ongoing (grandmother's Parkinson's flare-up).
- **GitHub:** vitaliibelinskyi
- **Email:** vitalii.belinskyi@fieldnation.com
- **Slack:** U0BARRWR2AG

**Asad Ali**
- **Started:** Early June 2026 (already active during June 2 meeting)
- **Focus:** Agent-hosting POC — running parallel platform comparisons (AWS Agent Core / Anthropic Managed Agents, Gemini/ADK on Vertex AI Agent Engine, Azure AI Foundry). Has deployed the AWS POC with guardrails, orchestration, and observability benchmarking. Also owns evals tooling (promptfoo). **As of 2026-07-10, tentatively also owns the FN-Core-parity sandbox image spike** (open item 1 in the Architecture & Overview doc — the single largest infrastructure lift in the build), assigned as a working assumption pending confirmation it doesn't conflict with his agent-hosting POC load.
- **Focus update (2026-07-21 → 08-03):** Moved onto **MOI-81: Copilot execution telemetry (OTel)**, the Copilot-side counterpart to Vitalii's MOI-80. Confirmed layer 1 is already covered by DX's GitHub Copilot adoption report (no build needed) and proved the layer-2 pipeline end-to-end (VS Code Copilot Chat 0.58.0 → local OTel collector → Datadog, tagged by repo). Content spot-checked twice: no prompt/code content in exported data. Still open on his side: a few layer-2 signals only verified synthetically, the `.vscode/settings.json` rollout mechanism unvalidated on a clean checkout, JetBrains/CLI coverage untested, and DX AI Code Insights not yet checked for layer-2 overlap. Also **MOI-53 (reconcile DX metric definitions against FN's usage)**, In Progress. Argued (correctly, citing Datadog guidance) for separate log indexes per service; Joe overruled and consolidated into one, a reversible call.
- **Focus update (2026-09-14 → 09-15): moved onto the ADR (Architecture Decision Record) skill suite (MOI-62).** Delivered three ai-toolbox skills — `fn-adr-create`, `fn-adr-enforce`, `fn-adr-review` — plus companion Python tooling in `ai-repo-score` (see [knowledge/tech-context.md](../knowledge/tech-context.md)). Live-demoed the full flow to the team 09-15; the demo surfaced a real bug (a revert-the-decision attempt appeared to succeed when the ticket's immutability requirement says it shouldn't have) and left the create/enforce/review skill-count question and conflict-precedence question open for Slack discussion. Next: open a draft PR (repo-scorecard first, ai-toolbox later) for Chuck/Vitalii/Mykhailo to review. Also confirmed with Chuck that Copilot code review is now off org-wide, resolving his earlier friction with Copilot's iterative issue-flagging. See [knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md](../knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md).
- **Email:** asad.ali@fieldnation.com
- **Slack:** U0B552YMK9A

**Mykhailo Sakhnik**
- **Started:** 2026-07-06
- **Focus:** Made his first substantive technical contribution on 2026-07-09 — proposed adding a task-type-specific pre-planning validation gate (reproducer-first for bugs, acceptance-test-first for features, validation/safety-first for chores) between INTAKE and the Planner in the LangGraph PEV POC architecture. Chuck agreed and is incorporating it into the diagram/doc. See [program/overview.md](./overview.md#pre-planning-validation-gate-proposed-2026-07-09). **Getting a real workstream as of 2026-07-09:** Redwan asked Chuck to assign him an actual ticket so he can start contributing, and to set up 1:1 time with Chuck to go deeper on the architecture ideas he raised in standup. Mykhailo agreed to write up his ideas/questions as a Confluence page ahead of that conversation. **As of 2026-07-10, that workstream is the OpenAI-on-Bedrock integration** (Architecture & Overview doc, open item 2) — wiring the Responses API model calls (GPT-5.5, Codex) through LangGraph and confirming tool-calling behavior per agent. **2026-07-16:** shared a 90-day repo-activity Google Sheet in #moissanite-eng — directly useful groundwork for the O1 repo scorecard/DX-metrics work in the [H2 OKRs](./OKRs/h2-2026-okrs.md). Also received the "almost exactly my vision for the AI-DLC" video from Chuck the night before, alongside Joe. **Assigned MOI-57 (define v1 of the AI Repo Readiness Score)** as part of the newly-ticketed O1 KR2 work (Linear project "Engineering Velocity via Repo Enablement," created 2026-07-16).
- **Focus update (2026-07-20 → 08-03): biggest output of the vacation window.** Built the entire repo-readiness toolchain himself, creating and closing his own tickets: **MOI-82 `repo-scanner`** (rank org repos by recent activity), **MOI-83 `repo-passport`** (static technical passport: languages, runtime, build/test/lint/type-check commands, structure, CI config, Docker, docs, frameworks), **MOI-84 `repo-health`** (remote GitHub/CI health: branch protection, required/optional checks, workflow runs, maintenance signals), **MOI-85 `repo-readiness`** (combined score, category scores, blockers, warnings, missing capabilities, remediation, evidence), **MOI-86** SQL-repo support (relevant to `engineering-dbt` as Repo 1), **MOI-87** sandbox-tooling investigation. All artifacts write to `.ai-factory/` on a shared YAML contract. Added a `--sandbox` flag with `host`/`docker` execution modes so a repo's own tests run isolated; Go working, Node/TS next. **MOI-57 (AI Repo Readiness Score v1) moved to In Review 7/30**; also picked up **MOI-54** (scorecard success metrics / target impact magnitude), pulling data via the DX API. Raised Docker Sandboxes ([docs](https://docs.docker.com/ai/sandboxes/)) as a candidate 7/22.
- **Focus update (2026-09-15 standup):** Finished bringing the repo scorecard template to ~100%, removing all Copilot-specific checks now that Copilot is out of the code-review evaluation; next up is running it across multiple repos and then a harness-vs-baseline comparison. Spent the back half of the 09-15 standup (after Chuck dropped off) pressure-testing Asad's ADR skill design with him — arguing enforce may be redundant with create/review, and that the newest ADR should carry more conflict weight than Asad's audit-log-only framing allows; unresolved, moved to Slack. See [knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md](../knowledge/meetings/2026-09-15-team-moissanite-daily-standup.md).
- **Update (2026-10-13 standup):** Now owns the two new AgentCore spikes, **MOI-167** (AgentCore Harness for autonomous coding) and **MOI-168** (event-driven human-in-the-loop). Ran a real repo fix through the harness sandbox on 10/12-13. Chuck gave him a green light to explore quickly with sandbox access (tear down when done). Also owns the security/prompt-injection backlog work raised 10/13 and the Harness CI DeepEval-removal PR. See [2026-10-13 standup note](../knowledge/meetings/2026-10-13-team-moissanite-daily-standup.md).
- **Update (2026-10-14):** Owns **MOI-169** (Fai event-driven MVP build, In Progress; first PR open at ai-software-factory#1) and **MOI-170** (`instruction_hygiene` fix in repo-agent-context). In DM with Chuck on 10/13 and 10/14 he asked for a Linear webhook on the Moissanite team (Issue events) and an `ai-factory` label, with the signing secret stored in AWS Secrets Manager. Both need Chuck or a workspace admin.
- **Update (2026-10-16):** Owns the MOI-169 milestone tickets created 10/14: M0 (MOI-171) and M1 (MOI-172) Done, M1.1 React approval UI (MOI-175) Done; M2 (MOI-173, AgentCore execution), M2.1 (MOI-176, repo agentification) and M3 (MOI-174, quality gates) Todo. See [tech-context](../knowledge/tech-context.md#fai-mvp-milestones-and-infra-benchmark-2026-10-16).
- **Update (2026-10-16 standup): staffing change.** Chuck announced senior leadership prioritized a customer-facing agentic solutions project (Pydantic, Temporal; year-end release target). Mykhailo is to transition full-time to it for one to two months starting the week of 2026-10-19, then return to the factory. Chuck and Maria to hold an onboarding meeting Wed or Thu that week. Chuck said he "tried to fight to keep" Mykhailo on the factory. Impact on the Fai MVP M2 to M3 timeline is not yet assessed. Source: [2026-10-16 standup](../knowledge/meetings/2026-10-16-team-moissanite-daily-standup.md).
- **Update (2026-10-19 standup):** Verifying updated GitHub App permissions (MOI-169 path); drafting an "open router" model-selection proposal for discussion; waiting on Joe for the Bedrock test harness (Chuck to nudge). Source: [2026-10-19 standup](../knowledge/meetings/2026-10-19-team-moissanite-daily-standup.md).
- **Update (2026-10-22 standup):** Demoed the planner UI (approve gate, execution trace, token metrics) to Chuck and owns the next Factory targets: MCP and Sonar MCP integration, one core UI backlog ticket via the Factory, and cost-per-run tracking, all ahead of a Friday 10/23 demo to Cory. Also owns the LiteLLM security follow-up. Customer-team transition now around 2026-11-02 for 1-2 months (Chuck's latest read; earlier note said week of 10/19). Source: [2026-10-22 standup](../knowledge/meetings/2026-10-22-team-moissanite-daily-standup.md).
- **Update (2026-10-27 standup):** Transition confirmed for Monday 2026-11-02 to the "parl" team (Maria set up the onboarding chat with Sandro and Spencer and shared a Confluence page and TDR; transcript is ambiguous on names, so confirm). Chuck expects to take over some of his in-flight Factory work that week. Mykhailo is off Friday 10/30 (approved) and will send Chuck a Slack status download before logging off Thursday 10/29. Open items he carries: Sonar research for planner and coder roles, TDR gap review, global label test on other projects, a space-issue ticket. Source: [2026-10-27 standup](../knowledge/meetings/2026-10-27-team-moissanite-daily-standup.md).
- **Update (2026-10-20):** Reports the Fai factory now runs end-to-end (Linear ticket to draft PR) after the GitHub App integration. MOI-173 (M2) and MOI-176 (M2.1) moved to Done 10/20; M3 (MOI-174) was Canceled the same morning, reason not recorded. Flagged two Harness CI blockers: LAZ-717 (Bedrock connector) and LAZ-718 (scoped GitHub App for PR comments). See [tech-context](../knowledge/tech-context.md#fai-mvp-scope-update-m2-and-m21-done-m3-canceled-2026-10-20).
- **Update (2026-11-02):** Chuck cancelled the Team Moissanite Daily Standup series after the DST shift and expected Mykhailo to move to another team. Mykhailo replied that he has already started the new product work (project details from Spencer) and is writing a Confluence page on Harness eval/CI. The move off the factory workstream is happening, but the in-flight factory handover is still unconfirmed. Source: Slack DM, 2026-11-02.
- **Access:** Linear board access confirmed (2026-07-07 weekly cadence); GitHub repo access still pending as of 2026-07-07.
- **Title:** Contractor
- **Timezone:** Asia/Jerusalem
- **Email:** mykhailo.sakhnik@fieldnation.com
- **Slack:** U0BFK48EYJ0

### Earlier Candidates (Not Hired)
- **Daniil** — Passed. More of an executor, not the independent problem-framer needed.
- **Orfeas** — Passed. Confirmed no-go by Cory (2026-04-14).
- **Taras** — Passed. "Book smart" without grounded implementation experience.


## Embedded Teams

### engineering-dbt (Emerald) Team

Repo 1 for the O1 embed-model pilot (`fieldnation/engineering-dbt`, confirmed "Emerald" at the 2026-08-24 AI-DLC Metrics Overview meeting — see [program/overview.md](./overview.md)). Invited as a group via engineering-emerald@fieldnation.com. First direct feedback session with team engineers held 2026-09-17; none of the individuals below had a workspace entry before that meeting, so roles below are inferred from the transcript, not confirmed.

**Rakibul Hoque**
- **Team:** engineering-dbt (Emerald)
- **Note (2026-09-17):** Raised that product-side gaps (missing ERDs, no data dictionary) slow onboarding into the repo, and proposed a governance-side skill to auto-approve low-risk data-governance-spreadsheet changes so only ambiguous ones need manual review.

**Amran Hossain**
- **Team:** engineering-dbt (Emerald)
- **Note (2026-09-17):** Reframed the team's throughput problem as an issue-cycle-time question (time is healthy once a ticket is picked up) versus pre-ticket friction (where the actual time is going); noted the team was recently recognized as best team of the quarter.

**Imtiaz Chowdhury**
- **Team:** engineering-dbt (Emerald)
- **Note (2026-09-17):** Argued DBT layer-placement decisions (base/staging/reporting/intermediate) require human judgment about future reuse and aren't well suited to agent assistance; Mike Mueller pushed back on this. Referred to by teammates as "MT"/"MTS" in the transcript.

**Tanin Rahman**
- **Team:** engineering-dbt (Emerald), works with the Marble team
- **Note (2026-09-17):** Gave the strongest positive signal in the session — existing repo-standards/layer-placement skills worked well for a full sprint. Flagged cross-team migration debt (e.g. invoices moving from fn-core to a new accounting microservice) as an ongoing documentation burden.

**Ruzlan Karim**
- **Team:** engineering-dbt (Emerald)
- **Note (2026-09-17):** Flagged that SQL doesn't capture business logic, forcing manual validation loops with product/business teams, and that column-naming collisions (same name, different meaning or vice versa) require manual cross-checking. Mentioned the team is moving toward a semantic layer to help enforce this, not yet built. Also confirmed the data-governance definitions live in a Google Sheet, not Excel or a schema file.

**Rezaul Karim**
- **Team:** engineering-dbt (Emerald)
- **Note (2026-09-17):** Described an ML-team hackathon project (the weekend prior) building a knowledge graph linking database schemas, product definitions, and Jira/migration data to help cross-teams find data sources. Agreed to send Chuck the (rough) project link via Slack for async review.
