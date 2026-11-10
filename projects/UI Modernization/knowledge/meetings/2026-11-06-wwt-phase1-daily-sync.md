# Obsidian Daily Sync + Stakeholders (WWT Phase 1 Daily), 2026-11-06

**Date & Time:** Friday, November 6, 2026, 8:44 CST

**Attendees (per Gemini invite list):**
- Yurii Dmytriiev, Contractor, Frontend Engineer
- Andriy Kurenda, Contractor, Frontend Engineer
- Ivan Sakhman, Contractor, Frontend Engineer
- Todd Folie, QA / production testing for WWT
- Andrew Kandels, Engineering lead
- Maria Saxton, Linear project lead, WWT Phase 1
- Chuck Haines, Sr. Principal Software Engineer
- Laura Quist (Douglas), Sr. Customer Success Executive
- Karol Tluscik, joining full-time this quarter (role to confirm). Karol posted in Slack that she would be AFK for about an hour and could not join, so attendance is unverified.

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1CTv2VTiBmQOzer1Lfh8dCC8cgtQ2yDTn2GW6hEIsDMA/edit?usp=drivesdk&ouid=104527139387599317887) (Gemini notes only; no transcript tab)

---

## Overview

The daily covered the stalled E2E test PR review, whether to adopt Warp factories for development work, a new filter bug from Todd, and the Match page rollout. The team agreed to use Warp factories for larger features while keeping the traditional flow for small bugs. Gemini wrote "VARP factories" throughout; this is almost certainly a transcription error for "Warp," given the Warp adoption decision from 11-04.

---

## Key Notes

- **E2E test PR (OBS-670) review is blocked:** The review is waiting on contractor feedback. Adding the contractor to the channel or repo needs admin permissions that current participants do not have, and Paul was named as the person who could grant them. Andrew and Maria agreed to follow up directly on the ticket.
- **Warp factory adoption is feature-dependent:** Andrew said smaller bugs should use the traditional approach to protect productivity. Larger features, such as ETB testing and lift pages, could use Warp to gather feedback. The team agreed on this split.
- **Warp vs. default CI comparison:** Yurii tested both approaches by generating Storybook in CI and comparing them. Storybook integration with Warp works. Yurii will decide on one method after the comparison.
- **Time Awaiting Approval filter vanishes when applied (Todd):** This is a new bug, reported in Slack at 08:27 CST. No ticket existed at the time of the meeting. Maria committed to filing it and linking the Slack post.
- **Triage follow-up:** Maria said Zach (Zach Hohloch, reporter of several tickets) has not responded to triage requests and gets one final notice. Gemini did not record a ticket number.
- **Match page rollout:** Mari and Ashley have not started front-end changes for the Match rollout. Andrew confirmed this makes it a good entry point for Core UI. Maria will ask in the Match rollout chat that they build front-end work in Core UI and help test during migration.

---

## Decisions

- **Aligned:** Adopt Warp factories for larger features, and use traditional methods for smaller bugs.
- **Aligned:** Core UI is the required path for front-end changes in the Match page rollout.

---

## Action Items

- **Andrew Kandels:** Invite the contractor to the communication channel so the E2E test PR can be reviewed.
- **Yurii Dmytriiev:** Compare the default CI approach with the Warp method and pick one based on results.
- **Maria Saxton:** File a ticket for the time-awaiting-approval filter bug and link it to the Slack post.
- **Maria Saxton:** Ask Mari and Ashley in the Match rollout chat to use Core UI for front-end changes and to help with migration testing.
- **Andrew Kandels and Maria Saxton:** Follow up directly on OBS-670 (E2E suite updates) to unblock the review.

---

## Items Not Verified

- Gemini's attendee list includes Karol, but her Slack message says she was AFK during the meeting.
- The "ETB" reference is not clear from the notes and was not expanded.
- "Paul" is named as the admin who can grant repo or channel permissions. His role is not recorded in people.md.
- "Ashley" is named as a Match rollout participant. Not in people.md yet.
