# Review Unisys WO Cap Prototype — 2026-04-15

**Date & Time:** Tuesday, April 15, 2026, 11:44 AM CDT (~29 minutes)

**Attendees:**
- **Linda Lo** — Principal PM, Dispatch/Fulfillment Lead (meeting organizer)
- **Brent Grill** — PM, GARNET Team (presented prototype)
- **Scooter Aspen (Stephanie Aspen)** — CSM, Unisys account

**Note:** Chuck Haines was invited but unable to attend; reviewing the recording after.

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1Yxi6CT4Da7QBRL79IBCqKkWP_4o8eEgnM6OWHOD2duY/edit?usp=meet_tnfm_calendar)

---

## Overview

The team reviewed a lightweight Figma prototype for handling assignment limit overrides in the Unisys smart dispatch workflow. The core design: when all requesting providers in a service territory exceed the max assignment cap (set at 5/day), smart dispatch stops auto-assigning and instead loops a human dispatcher back in via email notification and platform alerts. The dispatcher can then review context (current assignments, distances, provider profiles) and manually override the limit if the situation warrants it.

This is a deliberate strategic pivot from what Unisys originally asked for (bundled dispatching / location-aware grouping) toward a lighter operational workflow improvement. The team aligned that solving for bundling is a fundamentally different capability FN doesn't support today, and the better near-term path is making manual decision-making easier with guardrails. The prototype is intentionally scoped small — no new operating surfaces, just emails, confirmation modals, and a potential flight board status filter — to avoid overcommitting before workshopping with Unisys.

Scooter gave positive feedback on the direction, flagged notification noise as a manageable concern (dispatchers already folder-manage FN emails), and raised a forward-looking point about schedule conflict limitations — the per-day cap helps but doesn't address concurrent same-time assignments, which will matter for buyers like Clear Captions. That's acknowledged as a future phase. This was a pre-meeting ahead of the broader internal Unisys group review on Apr 16, where the team plans to present, get sign-off from Erica, Kaylee, and Scooter, then schedule a dedicated small-group workshop with Matt and two selected AMs.

---

## Key Notes

- **Prototype walkthrough — manual override flow:** When all provider requests on a WO exceed the max assignment limit, the dispatcher gets an email listing the requesting providers (with current assignment counts, distances, phone numbers) plus other service territory providers who didn't request. Email links to the WO detail page where they can assign via pending assignment tab or find providers tab (pre-filtered to the service territory). A confirmation modal warns when assigning an over-limit provider, showing context and allowing override. This doesn't block assignment — it just forces acknowledgment.

- **Notification noise is a known risk, not a blocker:** Some Unisys providers already exceed the daily cap (one had 33 same-day assignments), which would generate many emails. Scooter noted PMs already use dedicated FN email folders and this is standard guidance they give during onboarding. The team agreed to include the ability to disable email notifications per user while keeping platform notifications active.

- **Flight board status filter proposed:** Linda suggested adding a new status filter (e.g., "max assignment exceeded") to the flight board so dispatchers can filter for WOs needing manual intervention. This was preferred over a digest email approach, which wouldn't carry enough detail for decision-making.

- **Schedule conflicts deferred but flagged:** Scooter raised that per-day caps don't solve concurrent scheduling issues — e.g., a provider assigned 5 WOs all at 8 AM has hard-start PSS implications. Clear Captions specifically asked for time-based limits today and was told it's not currently possible. Linda recalled some existing schedule conflict functionality in the system and will recheck. Brent noted manual assignment already shows schedule conflict warnings. This is earmarked for a future phase.

- **Tier-based caps not in scope:** The flat cap of 5 applies uniformly per project/service territory. Unisys had previously mentioned wanting different caps per provider tier (e.g., 2 for new, 5 for experienced), but the team landed last week on starting with a flat cap and evaluating whether tier-based caps are truly needed based on workshop feedback.

- **Credentials/equipment criteria staying in smart dispatch:** Despite concerns about data quality (case sensitivity, inconsistent naming), the team is keeping credentials and equipment as eligibility criteria for specific use cases — DoorDash's provider tool/ID requirement being the primary example. If criteria dramatically shrink eligibility pools, CSMs should help educate buyers.

- **Bundling dispatches remains the elephant in the room:** Unisys's original ask was for FN to understand location proximity and intelligently group dispatches to providers. This is a fundamentally different model FN doesn't support. Linda's framing for the workshop: let's incrementally make operations easier while that bigger capability is evaluated for broader buyer demand.

- **"Product line" mystery:** Scooter tried to track down what Matt meant by "product line" in a prior conversation — no one (including Jack) could figure it out. Parked for the workshop.

---

## Action Items

- **Linda Lo:** Prepare light slides for the internal Unisys group meeting (Apr 16) to orient on the operational-focus decision and walk through Brent's prototype
- **Scooter Aspen:** Check account notification settings to confirm whether users can turn off the new assignment limit emails specifically
- **Linda Lo:** Recheck existing schedule conflict functionality in the system to understand current capabilities
- **Erica, Kaylee, Scooter (implied):** Sign off on the direction after the Apr 16 internal review
- **Erica/Kaylee (implied):** Reach out to Matt to schedule a dedicated small-group workshop with his two selected AMs (one open-minded, one process-attached)
