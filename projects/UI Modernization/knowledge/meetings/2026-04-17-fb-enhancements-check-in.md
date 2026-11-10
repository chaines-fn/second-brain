# FB Enhancements / Check In — 2026-04-17

**Date & Time:** Friday, April 17, 2026 (~27 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer, Field Nation
- Andrew Kandels — Engineering lead, UI modernization
- Cory Muir — Product/stakeholder
- Laura Quist — Sr. Customer Success Executive (translating WWT customer needs into actionable work)
- Lacey Selvagn — Product Design Lead *(invited; did not speak in transcript)*

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1xWun1zge66OLeuUEsWde92XL2KAv7bY7NVD7mTTU5oo/edit)

---

## Overview

This was a working check-in on the Flightboard Scale Enhancements backlog, focused on scoping and prioritizing a handful of higher-impact features ahead of a new contractor (Miro) joining Monday. Laura walked the team through her priority asks — real-time message alerts, project grouping/segmentation, a default global configuration setting, and case integration into the flightboard sidebar — and Chuck and Andrew sized each one in rough story points to inform what's realistic given current staffing.

The biggest decision was to abandon real-time messaging in favor of a scheduled refresh approach (every ~5 minutes). The real-time path would have required Kafka event wiring across FN Core plus consumer work — roughly 10+ story points — while the scheduled refresh is essentially just SQL queries against the replica, estimated at 3–5 points. Laura explicitly called that a "hockey stick" vs. linear tradeoff and opted for the simpler path. The team also resolved the case-integration question inline: Chuck pulled up the mobile app's API calls during the meeting and confirmed the `/api/cases/:id/messages` endpoints already exist, so no separate spike is needed — it's a UI build-out.

On staffing, Cory redirected Miro (PHP backend contractor) from the AI Software Factory project to Flightboard enhancements for the next couple weeks. Chuck was supportive because AI Software Factory is still in early architecture/exploration mode and isn't ready for a productive PHP contributor anyway — the two-week delay actually helps. Miro and Oleg will join the team's existing standup on Monday to get bootstrapped. Chuck is moving Monday's standup earlier to accommodate a mockup walkthrough with the new contractors, and Laura will be added as an optional attendee going forward.

---

## Key Notes

- **Real-time messaging rejected in favor of scheduled refresh:** Laura wanted real-time new-message alerts on Flightboard. Chuck's initial sizing was ~10+ story points for real-time — requires firing Kafka events from FN Core, consuming them, and wiring into OpenSearch (the infra for OpenSearch to consume Kafka was recently added by Slate or Garnet, which makes real-time *possible* but still nontrivial). Andrew proposed a scheduled SQL-query approach against the replica (similar to how LogStash already works), which he estimated at 3–5 points. Laura made the call to go with scheduled, accepting ~5-minute latency.

- **Case integration → UI build-out, no spike needed:** Laura wants clicking a case reference from the new exceptions column to open the case in the flightboard side drawer (view + reply). Chuck confirmed during the meeting that the mobile app already calls `/api/cases/:id/messages` and `/api/cases/:id` endpoints, and providers can both read and send messages from mobile. That means the backend APIs are there — this becomes a pure UI effort to collapse the cases-page functionality into the drawer footprint. Andrew estimated 3–5 points (interpreted as "a few days"). The originally-planned spike was effectively completed live during the meeting.

- **Side drawer messaging (view + reply inline) is the biggest UI lift:** Laura raised high-priority request to view/reply to messages directly in the side drawer when clicking a message alert. Andrew confirmed APIs exist (not PHP pages), so it's a UI problem — but specifically a big one, because the challenge is collapsing the full messaging-page functionality into a smaller drawer footprint while maintaining feature parity. Andrew pushed back on doing a *different* implementation (e.g., a trimmed-down version) because feature-gap drift creates ongoing QA churn. Next step is design work with Lacy on how to collapse functionality; lazy-loading may help. Andrew compared the eventual effort to "implementing the drawer itself was the largest single chunk of the flightboard project" — so this is sizable.

- **Global default configuration is small (3–5 points):** Andrew confirmed that if the global config is a single global default (not user-specific, not scoped to "apply-this-one-to-my-view" options), it's a minor API update + page awareness, ~3–5 point story.

- **Project grouping/segmentation still unresolved:** Laura asked about the status of investigating grouping/segmenting projects or flight boards to isolate them from the main one. Andrew confirmed this has not been looked into yet. Andrew flagged this as the thing he's "most concerned about" from a staffing perspective — unclear who has the business context to drive it.

- **Miro (PHP contractor) redirected from AI Software Factory to Flightboard backend:** Cory was planning to put Miro on AI Software Factory, but pulled him over for ~2 weeks of Flightboard backend work. Chuck supported this because AI Software Factory is still in early architecture exploration and a productive PHP contributor would be underutilized. The deferral also gives Chuck time to better define onboarding tasks for Miro when he rejoins AISF. Miro + Oleg will connect with the team Monday morning.

- **Ongoing cadence — Linear-first, meet only when needed:** Andrew recommended Laura put items directly in Linear and tag Andrew/Chuck to point-estimate. Call a meeting only when items need further discussion (e.g., "we need more data" or "we have options"). Chuck offered Laura an optional invite to the daily 7:45 AM standup for informal sync. Andrew added her as optional.

- **Stack ranking coming from Laura + Lacy in Linear:** Chuck asked Laura to work with Lacy to rank the backlog in Linear most-to-least important. This helps the team focus given the limited contractor bandwidth.

---

## Action Items

- **Chuck Haines:** Move Monday's standup to an earlier time to accommodate the mockup walkthrough with the new contractors. Notify Gary and Ivon (Yurii) about the time change. Invite Laura.
- **Chuck Haines:** Research via Claude/Copilot how providers access case messages in the mobile app (partially done during meeting — API endpoints identified). Confirm the full action-taking flow.
- **Cory Muir:** Coordinate a Monday morning connection meeting for Miro + Oleg to join the team and get bootstrapped on Flightboard enhancement work.
- **Andrew Kandels:** Add Laura Quist as an optional attendee to the daily 7:45 AM standup invite. *(Confirmed done during the meeting.)*
- **Laura Quist + Lacey Selvagn:** Stack rank the Flightboard Scale Enhancements backlog in Linear from most to least important.
- **Laura Quist:** Talk to Lacy about the side drawer messaging UI initiative — complexity, lazy loading approach, design collapse strategy.
- **Laura Quist (implied):** Confirm whether a case-opening message is posted in the work order messages thread when a case is opened (minor clarification mentioned but not resolved).
