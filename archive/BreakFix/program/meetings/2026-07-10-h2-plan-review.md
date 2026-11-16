# H2 Plan Review | Break/Fix — 2026-07-10

**Date & Time:** Friday, July 10, 2026 CDT (exact start time not in transcript; ~53 minutes based on recording timestamps)

**Attendees:**
- John Shaw — Group PM, cross-stream alignment (led the walkthrough, organizer)
- Shane Kinder — EVP of Product (set meeting flow/format up front; drove most of the questioning on program ops/reporting)
- Cory Muir — Sr. Director, Engineering (Chuck's boss)
- Amran Hossain — Engineering Manager (BD teams, oversees all three team deliveries)
- Lisa Sacquitne — Sr. Director of Product (John Shaw's boss)
- Travis Emslander — CTO
- Kelley Manuel — Data Platform/warehouse team (role inferred from context — owns data-warehouse conversations with Cory/Mike Mueller; not currently in `program/people.md`, flagging for you to confirm)
- Ann-Bettina Colace — PM (SLATE) — invited, no lines attributed in transcript
- Chuck Haines — Sr. Principal Engineer — invited but did not join (John and Shane briefly debated whether you'd joined; you hadn't)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1Dr8g4di-5XopTAmeim_fHD4t_xIW0HIk3ZhvhnbpsH8/edit)

---

## Overview

This was John Shaw's H2 plan walkthrough for Break/Fix, structured around three objectives: network development automation, program operations/reporting, and (woven throughout) security and technical governance. The network development piece is the most concrete commitment in the meeting — 100% of new recruitment campaigns onto the automated tool by end of Q3, a targeted 90% reduction in manual effort, and full elimination of the ~25 spreadsheets the net-dev team currently runs on. Shane pressed to confirm the scope of that commitment (new campaigns only, not mid-flight ones) and it held up. Alongside that sits the still-open question of when and how the network development effort — which has been run as an innovation project — graduates into a standard engineering process; Cory, Amran, and Lisa flagged real concerns about mobile app coupling, security review, and engineering rigor, and that review is explicitly assigned to Chuck, Amran, Shaf, and Jamu this quarter.

The program operations and reporting discussion was the longest and most contested part of the meeting. John walked through a granular list of buyer-driven VOCC/BC items (Door Dash, Clear Captions, Unicus) and Cory pushed hard on how the team measures whether any of it moves the needle — John and Shane's answer was that most of these items aren't meant to have a 1:1 revenue correlation; they're about building success-team confidence to bring bigger programs onto the suite. Shane separately pushed John on whether several of the Smart Dispatch asks are tactical patches versus opportunities to fix things structurally in the dispatch logic itself — landing on "a couple are patches, two or three are worth solving structurally," to be worked through in a dedicated session next week with Mia, Chris, and Lindsay. Reporting automation surfaced as a bigger and messier problem than expected: today's break/fix reporting runs through a 15-step manual process (Excel, Jupyter notebooks, Mia) before it becomes an HTML file distributed via Google Sites (soon Seismic). The team committed to daily automated distribution in Q3, but the real fix — productized metrics living in the data platform — isn't landing until Q4, so Kelley and Cory agreed the data warehouse stays the near-term home for experimental reporting.

Security and governance concerns ran under both threads. Cory was direct that Field Nation's Quicksite governance is thin and that recent months have already seen data co-mingling incidents (wrong-account SSO access, wrong-customer integrations) — he wants a real QC process before more buyer reports move through Seismic, and wants Ben and Sandro looped in before any broader rollout. The meeting also closed two smaller threads: John announced a new charter for a Provider Experience team (formed as the parts logistics team winds down and partners with LTE/network development on the provider-side recruitment experience — mobile-vs-desktop build tradeoffs still to be worked out), and Amran flagged that any agentic-based solution in network development this quarter would be happening without an established org standard, with the group agreeing to at least align directionally with the Pearl team rather than lock in a divergent stack.

---

## Key Notes

- **Network dev automation: 100% of new recruitments on the tool by Q3, 90% effort reduction, 100% spreadsheet elimination.** John confirmed with Shane this applies to new campaigns only (in-flight work stays as-is). The team timed and got sign-off from Jeff's team on current manual hours per recruitment as the baseline for the 90% figure. One recruitment is live on the new tool now, two more start next week. ~25 support spreadsheets are targeted for full elimination by end of Q3 — Shane called this out as the more exciting number, buried under the automation framing.

- **Innovation-to-product transition remains unresolved, explicitly assigned to Chuck, Amran, Shaf, and Jamu this quarter.** Cory framed the core question as "is this program ready for graduation" — concerns center on tighter mobile app integration (is it needed yet, and what does "tightly integrated" even mean here), security/performance risk from bumping into the core platform, and the fact that Field Nation has never moved a project from innovation-speed into standard engineering process before, so there's no existing playbook for what that transition costs in time or rework. Travis noted the flip side: if the automation succeeds, this becomes a feature critical enough that any outage is a fire drill, which is its own argument for engineering rigor sooner rather than later.

- **Data platform / eventing gap flagged for anything graduating out of innovation.** Cory pointed out the team isn't yet publishing much of this work's activity data into the data platform, and that buyers will expect effectiveness reporting once features graduate. Kelley confirmed she and John have already started a conversation about pulling this data into the warehouse, but "there's more to do there."

- **VOCC/BC prioritization list is buyer-tied, not aspirational, and order is fluid, not a fixed sequence of dependent work.** Shane confirmed each program-ops item traces to a specific buyer or CSM ask (e.g., Door Dash wants to take action during Smart Dispatch runs; Clear Captions wants per-territory Smart Dispatch enablement and schedule-conflict avoidance). Cory pushed on prioritization risk — since items are independently valuable but the list is granular, partial completion could still miss the intended impact if the "right" combination isn't hit. John's position: the current order reflects today's priority but shifts constantly with buyer signal (auto-accept counter offers for RIMG dropped from top to bottom and could snap back up if RIMG re-prioritizes).

- **Structural vs. tactical framing for Smart Dispatch improvements — Shane's push, still being worked out.** Shane's concern: some buyer asks (override buttons, manual batching) may be papering over deeper shortcomings in the dispatch model itself rather than solving the real problem. John agreed a couple of items are genuinely tactical patches, but 2-3 (notably schedule conflict avoidance and the broader "dispatch expansion intelligence" item) are real opportunities to solve structurally in the backend rather than adding buyer-facing controls. A dedicated working session with Mia, Chris, and Lindsay is scheduled for next week to dig into this.

- **Unicus dispatch relationship still lacks a clear driver; deep-dive session with new contact "Sean" set for August.** Chris Broveleit, previously the closest working contact, is reportedly moving off Unicus. Adam has been pushing to get Erica engaged. A punch list of Unicus needs arrived the day before this meeting (Jul 9) via the break-fix biweekly go-to-market sync — Shane asked John to connect with Adam on it.

- **Trunk stock (parts logistics investment) flagged as a recurring buyer ask with no committed scope.** Adam has heard the term from multiple buyer conversations but no buyer has yet stated it as a hard blocker. Shane asked John to prepare slides scoping what a real trunk-stock investment (inventory management + chain of custody) would require, distinct from the current "provider added parts" capability, which isn't sufficient for true chain of custody.

- **Roadmap already reviewed with Dana and Adam at the strategic level; granular VOCC list goes to them next week.** Two items they've raised outside this list: work-order execution automation (Dana) — confirmed out of scope, belongs to Core Transform, not Break/Fix; and trunk stock (Adam, see above).

- **Daily automated Break/Fix reporting committed for Q3, but the real blocker is unproductized metrics, not the distribution mechanism.** Current process: ~15 manual steps (Excel exports, Mia's Jupyter notebook work) to produce an HTML report pushed to Google Sites and soon Seismic, refreshed roughly weekly today. The team commits to Q3 daily/near-daily refresh via more automation (they've been experimenting with "Halal" borrowed from Jeff's team, a trailing-7-day metric, and exploring Zapier and cloud automation with Daryl), but full in-platform Quicksite reporting depends on metrics being productized in the data platform, which is Q4 work.

- **Experimental reporting stays in the data warehouse near-term (Kelley/Cory/Mike agreement).** Rationale: lets the team validate which metrics actually resonate with buyers before investing in full data-platform productization — avoids building infrastructure for reports that turn out not to be valuable. Cory flagged this as worth a dedicated follow-up conversation on whether the "temporary" manual pipeline is actually costing more effort than building an intermediate solution would.

- **Multi-SLA capture experience gets a half-sprint on GARNET in Q3.** Needed because some buyers (Blackbox, BOA, one other named buyer) have different SLAs for different work streams within a single program.

- **Quicksite multi-dashboard support is planned by the platform team this quarter (Amran), pending confirmation.**

- **Buyer appetite for dashboards vs. raw data is still an open question for Break/Fix specifically.** Amran raised that on the cost/quality dashboard, sophisticated buyers have told Ryan they'd rather pull raw data into their own tools than use Field Nation's dashboard. John doesn't have a clear read yet for Break/Fix buyers specifically — Clear Captions is the closest example of a buyer pulling data for their own analysis. Shane's view: Break/Fix reporting is inherently simpler than project-based reporting (tickets cost roughly the same vs. project's variable cost phases), so there's likely more durable need for Field Nation's own standardized reports paired with actionable recommendations, even as some buyers still want raw data too.

- **Data security/governance concerns are serious and rooted in recent incidents, not hypothetical.** Cory cited multiple actual incidents in the last two months — wrong-account SSO access grants, integrations turned on for the wrong customer — as reasons to get ahead of QC for the manual Seismic upload process before scaling it to more buyers. He also flagged that Quicksite's existing governance reflects speed-over-rigor tradeoffs made to ship it originally, and that risk grows as more people/programs get added. Action: check with Ben and Sandro on whether the current QC/security posture is sufficient before broader rollout.

- **Provider Experience team charter announced — new formation, not yet widely discussed.** As the parts logistics team ramps down in July/August, John has Sadat (Sedat) and team partnering closely with LTE and the network development team to build the provider-side recruitment experience, primarily on mobile. Open question flagged by John: build once for mobile, or duplicate effort across desktop and mobile — to be worked through as the team stands up.

- **Agentic solution work in network development has no established org standard yet.** Amran flagged that if the team pursues an agentic-based solution in Q3 (not yet decided), it would happen without Field Nation having a standard framework for building these — the Pearl team is working on one but hasn't published it. Cory's guidance: treat it as innovation-speed work for now, but collaborate directionally with Pearl so the team doesn't end up on a completely different tech stack that needs to be reworked later. This connects to an existing action item to loop in Linda and others on whether to tackle this in Q3 or wait for Core Transform's platform work.

## Action Items

- **Chuck Haines, Amran Hossain, Shaf, Jamu:** Review the innovation-to-product transition — determine technical design and security requirements for moving network development from an innovation initiative to standard engineering process.
- **John Shaw:** Prepare for the Unicus deep-dive session with new contact Sean in August.
- **John Shaw:** Connect with Adam on the Unicus punch list of needs (received Jul 9 via the break-fix biweekly go-to-market sync).
- **John Shaw** (with Mia, Chris, Lindsay): Meet to review Smart Dispatch expansion/intelligence logic — work through structural vs. tactical framing for the outstanding buyer asks.
- **John Shaw:** Present the granular VOCC/prioritization list to Dana and Adam at the weekly sync next week; confirm alignment on sequencing.
- **John Shaw:** Prepare slides scoping a trunk stock (parts logistics) investment — inventory management and chain of custody requirements.
- **John Shaw / Lisa Sacquitne (implied):** Schedule a dedicated conversation on intermediate reporting-automation solutions and resourcing support (raised by Cory, agreed to by John).
- **Amran Hossain:** Check in with Ben and Sandro on Quicksite implementation security/governance ahead of wider rollout.
- **John Shaw:** Define and document a QC process for manual Seismic report uploads to prevent buyer data co-mingling.
- **Amran Hossain, Cory Muir (group):** Collaborate with the Pearl team to align on technical stack/framework direction for any agentic-based solution work, even while it stays innovation-speed.
- **John Shaw (implied):** Loop in Linda and relevant stakeholders on whether agentic work proceeds in Q3 or waits for Core Transform.

No other explicit deadlines were set beyond "next week" references (Mia/Chris/Lindsay session, Dana/Adam roadmap review) and "August" for the Unicus/Sean session.
