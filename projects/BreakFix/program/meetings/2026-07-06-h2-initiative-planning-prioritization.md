# H2 Initiative Planning & Prioritization — 2026-07-06

**Date & Time:** Monday, July 6, 2026, CDT (exact start time not in transcript; ~90 minutes based on recording timestamps)

**Attendees:**
- Ridi Haque — Engineering Manager (GARNET + MARBLE) — organizer, initiated the meeting
- Amran Hossain — Engineering Manager (BD teams, oversees all three team deliveries)
- Lynsay Faucett — PM (GARNET)
- Ryan Schulte — PM (SLATE)
- Ann-Bettina Colace — PM (SLATE)
- Mobarok Hossain — Engineering Lead (SLATE)
- Bilas Chandra ("Sarker") — Engineering Lead (GARNET)
- John Shaw — Group PM, cross-stream alignment
- Alyssa Eby — UX (GARNET) — absent; Lynsay flagged at the top that Breakfix reporting needs might be under-represented without her
- Chuck Haines — Sr. Principal Engineer

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1p1vYSdIGK2-eaATlWDStcJjclIXNE_i86rYFB6Uw3FE/edit)

---

## Overview

Ridi called this session to finalize the Q3 development sequence for GARNET and SLATE ahead of the leadership planning submission (Amran and John report this plan up to Shane and Travis each quarter). The bulk of the meeting was Lynsay walking a shared spreadsheet top-to-bottom, sequencing items by urgency and checking each against capacity, while Amran and Bilas worked SLATE and GARNET capacity math in parallel. MARBLE was mentioned only in passing (Smart Audit) and had no dedicated planning time in this session — consistent with the gap flagged back on July 2 that MARBLE still needs its own dedicated Q3 planning meeting.

The throughline was reprioritizing around urgent buyer/sales feedback: service territory configuration jumped to the top of SLATE's list to unblock Clear Captions and Interface, while several GARNET items (cancel-in-progress, availability configuration for Unicus/Clear Captions, auto-accept counter offers for RMG/DoorDash) got sequenced ahead of longer-term work because of live customer pressure. Lower-priority or longer-horizon items — PSS score/driving distance display, public API conversion, full network expansion, deep talent-pool-to-territory mapping — were explicitly cut or pushed to Q4 to make room. The group closed by locking two Q3 OKRs (buyer adoption, standardized service territories) and deciding to record the H3 demo asynchronously rather than fight everyone's calendars.

## Key Notes

- **Q3 capacity locked:** SLATE targets 95 points (107 raw, minus 30% of Mobarok's time for design/planning, minus a 10% buffer). GARNET targets 110 points against 112 total team capacity. Both numbers were arrived at live in the meeting via Amran and Bilas's math.

- **Service territory configuration escalated to SLATE's #1 priority (27 points):** Lets a buyer enable Smart Dispatch for specific work orders/territories rather than a whole project, so the rest of their work keeps running through standard process. Already in technical design; escalated in priority over the prior week due to sales and internal stakeholder feedback. Lynsay and Ryan to finalize the design together.

- **PSS score and driving distance dropped from Q3 entirely:** Mobarok asked whether these two service-territory-enhancement line items should be included; Ryan said neither is needed, and the group agreed to cut both rather than defer them.

- **GARNET's near-term sequence driven by live customer asks, not roadmap order:** Cancel Smart Dispatch while in progress (lets buyers cancel a run but preserve active requests) is next sprint's priority. Smart Dispatch availability configuration (max assignments/day by tier for Unicus; double-booking prevention for Clear Captions) follows. Auto-accept counter offers for RMG/DoorDash is sequenced after those specifically because RMG's August go-live target pushed several other items ahead of it in the queue. A callout from a recent PR session (Shane Kinder, relayed by Lynsay): stop adding more eligibility-criteria/config knobs and instead get smarter about automated assignment — this shaped how the availability-configuration item was framed.

- **In-platform service territory creation is SLATE's big Q3 swing, scope still being drawn:** Two pieces — standardized service territories (backend: default territory definitions buyers can copy from) and an in-platform creation workflow (a "setup wizard," per Ryan, though he noted it's really something you'd touch at project start or when adding new territories mid-project). Ryan and Mobarok agreed this could be built simply or built elaborately, and the team needs to explicitly decide where the Q3 line is. Ryan is writing a doc breaking down major vs. minor features to support that call. Lynsay's read: standardized service territories is the must-have by end of Q3; the in-platform creation UX itself can be lightweight since staff will be the first users.

- **Talent pool mapping to service territories is the logical next step, but most of it is out of scope for Q3:** Long-term vision (Ryan): talent pools become the single source of truth for providers, with service-territory-level whitelist/blacklist overrides layered on top — a big shift from today's folder-style manual provider-to-territory assignment. Bilas asked pointed technical questions about how this interacts with automatic geographic attribution and flagged he needs to sync with Mobarok on the data model. Bilas explicitly confirmed the whitelist/blacklist and talent-pool-source-of-truth pieces are out of scope for Q3.

- **Public API conversion deferred to Q4 (Phase 2):** Bilas raised converting three internal microservice APIs (currently proxied through the monolith) to public status, estimating 10-12 points. Lynsay recommended deferring — not urgent enough to compete with the Q3 list.

- **Smart Dispatch Network Expansion confirmed as a Q3 non-starter:** Depends on standardized service territories being in place first; Bilas described the work (linking providers, territories, and the work-order layer) as necessary but not something the team has room for this quarter.

- **Breakfix reporting: 10-point placeholder, not a real scope yet.** With total capacity at 112 points, the group set aside 10 points as a placeholder for Breakfix reporting support on each team, explicitly contingent on Lynsay confirming actual requirements with Alyssa (who was out for this meeting). Amran's proposed principle: whichever team builds a feature also owns reporting for it, to avoid duplicated ramp-up cost. Which team (GARNET or SLATE) ultimately owns it was left open pending better scope/sizing on everything else.

- **Smart Audit confirmed for Q3 completion:** Lynsay's rationale — it's a small piece of work driven directly by customer feedback, so finishing it now closes out that body of work cleanly. Amran to tell Sed; Lynsay to separately follow up with Sed on provider experience project status.

- **Service territory enhancements bucket capped at 20 points (down from a broader ask), freeing 12 points for in-platform creation.** This is a continuation of Q2 work (table filtering, pagination, drawer history) that didn't make the Q2 capacity line. Lynsay proposed the cap specifically to redirect capacity toward the bigger in-platform-creation initiative.

- **Estimation confidence spot-checked, not re-litigated:** Bilas gave 80% confidence on a 20-25 point item; Lynsay opted to keep the estimate at the high end (25) as a buffer, with the idea that any slack could roll into network expansion later if the item comes in under. Mobarok separately raised service-configuration confidence to 90% since requirements are settled even though the technical design is still in progress — confirmed this doesn't change the 30-point estimate, since the added confidence is about buyer trust-building, not new technical scope.

- **Two Q3 OKRs locked: buyer adoption and standardized service territories.** Lynsay's framing: the features being built this quarter all exist to unlock buyer adoption. Ryan owns drafting the actual OKR wording and milestones.

- **H3 demo moved to a recorded, async format.** Scheduling conflicts (particularly with Jeff) made a live session impractical: Lynsay proposed recording it for asynchronous stakeholder review, and Ryan agreed it lets the team keep moving without needing everyone in the room at once.

## Action Items

- **Lynsay Faucett:** Finalize the technical design for service territory configuration with Ryan.
- **Lynsay Faucett, Ryan Schulte:** Evaluate service territory creation and talent pool mapping in the PEX lead session the next day (July 7).
- **Mobarok Hossain:** Produce a limited/scoped version of the in-platform service territory creation UX to support engineering estimation.
- **Bilas Chandra:** Sync with Mobarok on technical design and data mapping for in-platform service territory creation.
- **Lynsay Faucett:** Consult Alyssa Eby on required bandwidth for Breakfix reporting support.
- **Lynsay Faucett:** Verify the Q3 Smart Audit plan with Sed.
- **Lynsay Faucett:** Check in with Sed on provider experience project status.
- **Ryan Schulte:** Draft OKR wording and milestones for the two locked Q3 OKRs.
- **Lynsay Faucett:** Reschedule the H3 standardized-service-territory demo (recorded format).
- **Group:** Fill in the missing estimation columns in the shared planning spreadsheet.
