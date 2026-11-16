# B/F PEX Lead Sync — 2026-06-04

**Date & Time:** Thursday, June 4, 2026, 10:28 CDT (~60 minutes)

**Attendees:**
- **Lynsay Faucett** — PM, GARNET (meeting organizer)
- **Chuck Haines** — Sr. Principal Engineer (dropped at 11:00 CDT)
- **Amran Hossain** — Engineering Manager, BD teams
- **Ridi Haque** — Engineering Manager, GARNET + MARBLE (transcribed as "Rei" in places)
- **Mobarok Hossain** — Engineering Lead, SLATE
- **Bilas Sarker** — Engineering Lead, GARNET (email bilas.chandra@)
- **Alyssa Eby** — UX, GARNET (left after SLA data capture topic)
- **Ryan Schulte** — PM, SLATE (joined late / audio issues)
- **John Shaw** — Group PM *(declined / absent)*

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1_NKeqpxhLaJHTC-8pnCa5EKmIodhm96HBH1W-Pmr-Ek/edit)

---

## Overview

This was a fast-moving lead sync covering near-term delivery status and upcoming planning for the Break/Fix program. On delivery: SLA Data Capture is code-complete and in testing, targeting a Monday (June 8) production release — Amran was firm about avoiding a Friday deploy given ongoing migration-related incidents, so there will be no release tomorrow, not even for Dunder Mifflin demo purposes. Service Territories / auto-applying pay rates is on track, gated mainly on two open `fn core` PRs (Jack involved). Smart Dispatch needs additional Marble support for the assignment drawer and surfaced a new DoorDash request to allow manual assignment while Smart Dispatch is running.

A recurring theme was edge-case discipline. Chuck pushed on error handling for pay-rate funding failures — the team has no good in-platform mechanism to surface these errors today. The group aligned on starting with Datadog monitoring, but Chuck flagged the risk that "just log to Datadog" means only engineering sees it (requiring a page or investigation). The agreed path: test these conditions during fit-and-finish to learn the actual error behavior before deciding on in-platform alerting. Chuck explicitly tied this to prior burns (service territories breaking under auto-dispatch) and argued fit-and-finish edge-case testing should become standard practice.

The second half shifted to upcoming planning. Lynsay announced a documentation philosophy change — moving from solution-first to problem-first framing for Break/Fix project setup (Ryan reworked the doc; Chuck strongly endorsed). She introduced three product briefs (Break/Fix project setup & management, Smart Dispatch talent group, and a new Break/Fix program build) and noted each may need a dedicated tech lead. Amran closed with a strong ask: reserve engineering bandwidth for ad-hoc data requests via CLI scripts, and stop having engineers log into production as users to change customer data.

---

## Key Notes

- **Location standardization = tech debt (confirmed):** Mike asked whether location standardization/deduplication should be folded into the current body of work. Amran and Mobarok confirmed it's already in that bucket but is classified as tech debt. Mobarok clarified to Mike that the initiative did not originate from the product team. Consensus: regardless of bucket, it gets solved within the existing location-standardization initiative.

- **SLA Data Capture — release Monday June 8, not Friday:** Dev complete, testing ongoing. Ridi targeted a release as early as tonight (BD time) / 11am CDT, with afternoon BD time as fallback. Amran overrode any Friday release — including refusing a Dunder Mifflin production enablement for demo — citing current migration-related incident risk. Alyssa confirmed QA4 is resolved and she no longer needs it live there; her demo date is June 8.

- **Service Territories / auto-applying pay rates — on track, gated on PRs:** Mobarok reported two open PRs in `fn core` (Jack involved) as the main concern; Chuck offered to help facilitate review. Implementation otherwise on track.

- **Pay-rate funding-failure error handling (edge case):** Triggered by a Matul/Mike/Chuck side conversation. Today pay can only change up to publish state; in the async Smart Dispatch eventing flow there's a risk the WO publishes at the old/wrong value if funding fails and Smart Dispatch isn't aware. Bilas noted ~6–7 specific funding errors already surface in worker details. Agreement: monitor in Datadog first, then add in-platform error handling/alerts after observing real cases in fit-and-finish. Chuck's caveat: Datadog-only means no one but engineering sees it — not sufficient long-term; consider blocking Smart Dispatch on funding failure rather than publishing bad data.

- **Service territory pay rates vs. private networks (clarification):** Lynsay clarified pay rates apply based on the service territory attached to the WO, regardless of which provider/network gets assigned. This differs from private networks, where rates change based on the assigned provider's network. Confirmed: you can only set *service fees* (not pay rates) for private-network WOs — so private-network pay rates are a future, not now, problem. Chuck noted the "service territory pay rate" label is loose since it's reused when expanding to talent pools/marketplace; he wondered why templates weren't used (parked).

- **DoorDash manual-assignment request (Smart Dispatch) — ~8 pts, needs design:** DoorDash wants to manually assign while Smart Dispatch is running (currently locked until the dispatch window ends; only revert-to-draft is possible). This would cancel Smart Dispatch and must *not* count as an automatic Smart Dispatch assignment. Bilas swag'd 8 points but hadn't validated with Jack and flagged it needs new status logs, escalation/TUPI notifications, and downstream reporting changes. Chuck and Amran both warned against communicating 8 points to John as firm — treat it as a swag with no date, give Bilas time to dig in and produce a technical design. Chuck noted the team "pointed this out early and was told to ignore it." Applies to all customers, not just DoorDash. Tied to the June 23 timeline pressure.

- **Smart Dispatch reporting in the data warehouse:** Current schema mirrors auto-dispatch (basic: providers matched, assignment=true). This was intentional to allow asking more questions later. New statuses needed (e.g., manual assignment after window). Josh Regan has time allocated later this month; Alyssa and Lynsay to meet on which tables/fields to add. Broader B/F reporting ownership (a team vs. a single resource for Alyssa) still being planned by John.

- **Documentation shift to problem-first framing:** "Default service territories" doc renamed to "Break/Fix project setup and configuration," reframed from proposing a solution to framing the problem (faster/easier project setup). Ryan led the rework; Chuck strongly endorsed. Same step-back rework planned for the Smart Dispatch talent group / prioritized-criteria doc.

- **Three product briefs introduced (in a shared "Break/Fix Management Suite" doc):** (1) Break/Fix project setup & management [SLATE], (2) Smart Dispatch talent group [GARNET — needs rework], (3) new Break/Fix program build [team TBD — buyers want multiple clients per project, which doesn't fit current hierarchy; built from Andrew's input]. Briefs organized by initiative theme; each may need a dedicated tech lead. Follow-up review set for next week's lead sync (after taking to PR).

- **Prioritized-criteria business cases (Bilas's ask):** Bilas wants explicit scenarios documented for ranking criteria conflicts (e.g., PSL vs. distance vs. certifications vs. work completed, and ordering permutations). Lynsay agreed to add business use cases ("buyer does X → Y happens") to the PRD, but first wants to step back to the core problem: buyers want control over assignment, and FN must preserve its north star of best-match providers / positive outcomes without letting buyer control undermine it.

- **Break/Fix project setup pain points (problem framing):** Today setup requires heavy FN assistance — no self-service, dependence on FN to manage/finalize site lists, manual mapping of talent-pool providers into service territories, two separately-maintained provider lists (talent pool vs. service territory) that don't sync, service territories not reusable across projects (tied to one project), unmapped sites/WOs hurting Smart Dispatch & reporting, and no metro-vs-rural territory segmentation. Jeff's zip-code→territory CSV is FN-owned and must be requested. MVP (per Ryan): map sites → service territories, generate default lists; UI will evolve but core mapping functionality won't. Long-term goal: move all of this in-platform.

- **Reserve bandwidth for ad-hoc data requests / CLI scripts (Amran):** Ad-hoc requests (mass edits, zip-code/provider corrections, associating service territories to approved/paid WOs, per-territory weekly-volume attributes across ~1,200 territories with differing values) keep interrupting the team. Amran's position: reserve dedicated bandwidth and use CLI scripts — engineers should *not* log into prod as users to change customer data. Mobarok noted some cases genuinely require CLI (e.g., differing values per territory that mass-edit can't handle). Ryan countered that product already does in-prod provider/zip edits as standard workflow, and that the planned Q2 territory enhancements (mass edit/delete/create, view-all providers/zip codes) should eliminate most of these requests once shipped. Ridi to estimate ad-hoc bandwidth from Linear history.

- **Meeting logistics:** Current slot conflicts with other commitments. Ridi to reschedule. Chuck proposed folding this into the Tuesday Break/Fix edge leads meeting (extend to 45 min), possibly Tuesday 8:00 CDT — acknowledged as early for Lynsay. Tentative move to Tuesday 8:00 CDT.

---

## Action Items

- **Ridi:** Reschedule this meeting to a non-conflicting slot (tentatively Tuesday 8:00 CDT, possibly merged with the Tuesday edge leads meeting extended to 45 min).
- **Lynsay & Alyssa:** Meet to review existing Smart Dispatch data-warehouse reporting; determine tables/fields to add or port (for Josh Regan to implement later this month).
- **Bilas Sarker:** Validate the DoorDash manual-assignment request with Jack and write a technical design doc before committing to an estimate (treat 8 pts as a swag only).
- **Lynsay:** Add business use cases / expected outcomes to the PRD (both the manual-assignment and prioritized-criteria scenarios Bilas raised).
- **The group:** Read the service territory expansion document and provide feedback ahead of next week's follow-up discussion.
- **Lynsay:** Distribute the product briefs to participants (shared Google Doc / Slack).
- **Mobarok:** Review the briefs (especially project setup) and post questions directly in the doc before next week's sync.
- **Ridi:** Audit Linear to estimate how much bandwidth ad-hoc requests have consumed (last ~quarter).
- **Ryan:** Expand the service territory expansion spec to cover both algorithmic and manual territory modifications.
- **Chuck (implied):** Help facilitate review of the two open `fn core` PRs blocking service territories.
- **Lynsay / Ryan (implied):** Take the project-setup brief to PR (leadership) and bring an update to next week's sync.
