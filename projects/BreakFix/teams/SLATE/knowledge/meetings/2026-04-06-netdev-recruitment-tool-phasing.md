# NetDev Recruitment AI Tool — Phased Approach Alignment

**Date:** April 6, 2026
**Source:** [Gemini Notes + Transcript](https://docs.google.com/document/d/1tWxvhbYY1Jdy1XTkcesnq0OO-3acsa1BpTWElcJUnbI/edit)
**Attendees:** Ryan Schulte, John Shaw, Mobarok Hossain, Almansur Siddiqui, Safin Ahmed, Amran Hossain, Cory Muir *(Chuck invited but did not attend)*

---

## What This Meeting Was About

The team aligned on a **4-phase approach** to the NetDev Recruitment tool — abandoning the all-or-nothing framing that had been stalling the project. The core problem: every prior approach was either too big to build or too small to be useful. The new strategy: ship the smallest usable tool first, then layer in automation and AI.

Also covered: engineering estimation review (Safin walked through story points), and a Q&A with Amran on how the agentic AI strategy fits into this new framing.

---

## The 4-Phase Plan

### Phase 1 — Provider Sourcing (in progress, target ~sprint ending Apr 21)
Internal tool for NetDev staff to search for providers by basic criteria. Output is a CSV with provider data including PSS screening and nearby territory/site info. That's it — no outreach, no tracking.

### Phase 2 — MVP End-to-End Recruitment Tool (next up)
Bare minimum for a complete recruitment cycle without ad-hoc spreadsheets or Google Forms:
- Bulk invite providers via ID or CSV (can feed from Phase 1 output)
- Send branded email invites (AI optional for writing/timing, but not required)
- Opt-out management — providers can opt out; signal is stored
- Pass/fail vetting — staff manually reviews email replies and clicks pass or fail; no complex stage ordering
- Providers reply to the email; replies thread into the tool; staff can follow up with questions
- Basic CSV export (email, PSS, invite date, responses) for offline analysis
- **Success criteria:** NetDev team can run a full recruitment end-to-end — from provider outreach through talent pool / service territory placement (via CSV import, not auto-placement)

### Phase 3 — Staff & Buyer Tooling
- Recurring automated reminders (email every 3–5 days, up to ~5 sends, then opt-out)
- Internal stages with sub-statuses (e.g., badge requested → printed → sent within a single stage)
- Coverage targets per geography (track how many providers are being recruited per territory)
- Flexible stage types (quiz, deliverables, manual acknowledgment)
- Enhanced metrics: time-to-complete, territory conversion rates, KPIs
- Buyer access: limited view showing only their recruitment; ability to approve manual stages

### Phase 4 — Provider Experience & Advanced AI
- Mobile-responsive recruitment UI (web + likely mobile app)
- Provider dashboard (recruitment list / tasks — think work order list UX)
- Integrated message center: providers can ask questions, escalate past auto-reply to human staff
- Shared staff inbox for managing provider escalations
- Service company recruitment (reach out to service company admins, not just individual providers — different workflow but same stage/quiz structure)
- **AI features:** conversational email screening (agent reads responses, verifies docs), auto-reply and FAQ handling, drip campaigns across email and in-platform channels

---

## Priority Framing (from Ryan)

Priority 1 is a usable tool that makes the NetDev team more efficient. Priority 2 is AI and automation. Ryan explicitly said — and this aligns with what Chuck has flagged too — **don't overindex on this being an AI tool**. It should be an *automated* tool where AI is the right solution for some parts and regular logic handles the rest.

---

## Amran's Agentic Question (and the Answer)

Amran raised that the original mandate included an agentic AI approach as a primary objective. Ryan's response: you can't have a useful agentic outreach tool if there's no stage management underneath it. Agentic features covering 1/4 of the process and leaving the other 3/4 untouched isn't usable. The foundation has to exist first.

Amran's follow-up: so no AI until Q4? Ryan's answer: roughly yes for the more meaningful AI features, though Phase 2's branded email could optionally use AI for writing/optimization, and AI could be used to summarize or pre-score provider replies — but those are secondary to just getting responses into the platform.

**Bottom line from Amran:** the objective is to solve the NetDev problem with conventional product workflow first, then layer in AI.

---

## Estimation Notes

Safin walked through the current estimation before the phase discussion:
- Campaign build engine: 10 points (5 for wizard-style campaign creator + reshaping DB tables; 5 for rich media support built from scratch with Terraform provisioning)
- Dynamic stage/workflow builder: 5 points for drag-and-drop builder, 8 for advanced stages (unified questionnaire, deliverables, internal processing), 3 for automated if/else logic
- Untimed assessment: **removed from scope** — since this is a brand-new tool, there are no timed assessments to have an "untimed" version of. Just don't build timers at all.

Ryan noted that ~90% of the estimation work Safin has done will still apply — it just needs to be reorganized into the phased structure.

---

## Action Items

- **Ryan Schulte** — Polish the prototype showing the end-state for each phase; share before tomorrow's leadership alignment meeting
- **Safin Ahmed** — Adjust engineering estimation to the phased approach; prioritize a detailed breakdown for Phase 2 first; Phases 3 and 4 can be looser estimates for now

---

## Context: Tomorrow's Meeting

The phased approach has *not yet been presented to leadership* (Travis/Shane). That alignment meeting was originally today but moved to tomorrow. Amran asked if it could proceed without them — the answer was no. Ryan and John need that buy-in before committing the team to this direction.

---

## Notes & Observations

- The project history (HubSpot → managed recruitments → chatbot PC → provider sourcing tool) has been characterized by competing priorities: sometimes "build AI first," sometimes "build for the NetDev team," rarely both. The phased approach is designed to stop that oscillation.
- Chuck's name appears struck through in the attendee list — he was invited but didn't attend. Ryan referenced Chuck's view on AI vs. automation framing during the meeting.
- "Campaign" vs. "Recruitment" terminology note: in this meeting, "campaign" is used to describe the recruitment campaign creation workflow (the tool's internal term), which differs from Jeff Novotny's preference for "recruitment" in buyer-facing UI. Worth tracking as the tool takes shape.
