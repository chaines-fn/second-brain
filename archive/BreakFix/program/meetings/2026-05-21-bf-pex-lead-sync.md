# B/F PEX Lead Sync — 2026-05-21

**Date & Time:** Wednesday, May 21, 2026, 10:30 AM CDT (~43 minutes)

**Attendees:**
- **Lynsay Faucett** — PM, GARNET (meeting organizer)
- **Bilas Sarker** (Bilas Chandra) — Engineering Lead, GARNET
- **Alyssa Eby** — UX, GARNET
- **Ridi Haque** — Engineering Manager, GARNET + MARBLE
- **Mobarok Hossain** — Engineering Lead, SLATE
- **Amran Hossain** — Engineering Manager (BD teams)
- **Ryan Schulte** — PM, SLATE
- **Chuck Haines** — Sr. Principal Engineer *(invited, not present)*
- ~~John Shaw~~ *(invited, not present)*

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1mNDB7By0svbDv8MEPIMV6MAKjpAyAATAg_9PJCPfiTI/edit)

---

## Overview

This was the first combined Slate + Garnet sync — Lynsay kicked off by announcing these teams' Thursday meetings are merging into one recurring session (10:30 AM CDT, 45 min). The intent: first 10–15 minutes on delivery status, then the remaining time on upcoming quarterly planning and technical design reviews. This format starts after Eid (week of June 2).

The meeting covered three delivery threads: SLA data capture (nearly done, targeting Tuesday merge), the talent pool expansion / service territory work (a new complexity just surfaced around service company inheritance that may require scope cuts), and SLATE's smart dispatch + map view (on track, but Amran flagged the timeline as tight). There was also a quick discussion on the custom field time zone experiment, which is already live and just needs a GTM plan before opening it up broadly.

The most significant risk discussed was the service company inheritance problem in the talent pool eligibility work — Marble team is working through a technical design, but Bilas flagged that some edge cases around managed providers being removed/re-added to service companies weren't originally scoped. Lynsay set a Tuesday deadline for Bilas to clarify exactly which eligibility criteria are causing the delay, so a scope cut decision can be made by end of sprint.

---

## Key Notes

- **Meeting restructure:** Slate and Garnet Thursday meetings merged into one. 10:30 AM CDT, 45 minutes. First ~15 min = delivery check-in; rest = planning / technical design review. Takes effect after the Eid holiday (week of June 2). John Shaw was invited but absent.

- **SLA Data Capture — shipping Tuesday:** Bilas has 3 PRs pending approval. Lower env testing is underway. Habib deployed; Alyssa sent to QA and will test this week; Tahsin will verify the tracked data points are correct. They're avoiding a Friday push to production to dodge payment-processing window conflicts. Target: all merged by Tuesday May 26, then release to prod.

- **Chuck's UX feedback on Ascend by Target:** Alyssa acknowledged the feedback on UX issues but explicitly decided *not* to address them in Phase 1. It'll ship behind a staff flag as-is. Any redesign is deferred to Phase 2.

- **SLA Technical Design close-out:** No open technical questions remain. The "business days vs. business hours" question was clarified as a product-side concept, not a technical requirement. Ridi confirmed the TD doesn't need a formal presentation — just the final communication posted to close the loop.

- **DoorDash hardcoded SLAs — not Slate/Garnet's problem:** Lynsay asked about the transition plan for DoorDash's hardcoded SLA metrics. Alyssa confirmed that transition is owned by the reporting team (her and Tahsin) post-Phase 1 — Slate and Garnet don't need to plan for it.

- **Custom field time zone — staying under experiment for now:** The new date-time + time zone custom field is already live in production as an experiment. Team agreed to keep it under the experiment flag until Lynsay and Alyssa put together a GTM plan. Bilas raised eventually deprecating the old `date time` field, but Alyssa pumped the brakes — too risky to force migration before people are on the new one. They'll coexist for now; deprecation is a future-quarter conversation.

- **Talent pool / service territory — new complexity discovered:** The biggest risk flag of the meeting. The open-source provider model doesn't have a service company concept, which creates data inconsistency scenarios when managed providers are added/removed from service companies. Marble is working through a TDD covering schema changes, outbox pattern, and Kafka event production. Bilas couldn't give a clear timeline estimate — it's more work than originally scoped. Insurance sync will be real-time; equipment is considered simpler. Lynsay wants a list of which specific eligibility criteria are problematic by Tuesday (May 26), so a scope cut decision can be made before end of sprint.

- **Smart Dispatch config (SLATE) — on track:** Mobarok's team finished the configuration work last sprint. Current sprint: provisioning Redis and background jobs in the display automation service. "Talent pool coverage counts" ticket was deferred to next sprint but doesn't threaten the June 23 delivery target.

- **Map view capacity (SLATE) — tight but manageable:** With 4 engineers at 85% and vacation, Mobarok estimated ~19.5 points per sprint / 29 points total for the remaining two sprints. Above the capacity line (map view + metric view + territory side drawer + "fit and finish") ships in June. Below the line (PSS score, driving distance, etc.) ships July. Amran flagged the timeline as tight overall. Ryan noted PSS and driving distance alone are ~10 points that could be deprioritized to buy room. Lynsay will review the spreadsheet and share notes with Ryan.

- **Planning conversations starting early June:** Amran pushed for starting Q3 planning discussions early rather than waiting for the formal quarterly cadence. Ryan agreed — early June, plan first 2–3 sprints. Lynsay noted there's already a meeting planned between Chuck and Ryan on talent pool architecture / service territory.

- **Admin:** Team (BD engineering) off May 27 – June 1 for Eid. Meeting recurring time moved to 10:30 AM CDT to avoid conflict with the Net Dev tooling meeting, extended to 45 minutes.

---

## Action Items

- **Bilas Sarker:** Post communication closing out the SLA data capture TD *(by end of current test phase)*
- **Bilas Sarker:** List the specific eligibility criteria and constraints causing delays in the talent pool service company work *(by Tuesday, May 26)*
- **Lynsay Faucett + Alyssa Eby:** Develop GTM plan for the custom field time zone feature
- **Lynsay Faucett:** Review Mobarok's map view spreadsheet and share notes with Ryan
- **Tahsin Jahin Khalid (implied):** Verify tracked SLA data points match implementation requirements *(next morning after Habib's deploy)*
- **Chuck + Ryan (implied):** Architecture session on talent pool / service territory — Lynsay referenced this as already scheduled
