# B/F PEX Lead Sync - Jul 28, 2026

**Organizer:** Lynsay Faucett · 8:30 AM CDT
**Attendees:** Ryan Schulte, Mobarok Hossain, Alyssa Eby, Bilas Sarker, Ridi Haque, Lynsay Faucett, Amran Hossain, Sadat Haque, John Shaw
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1Xw0e9fxkygzitXLw8XFpHC0m9hpE5J7YRjG9UjiWsBc/edit)

> Chuck on vacation. Highest-signal meeting of the two-week window.

## Key Decisions

| Decision | Detail |
|----------|--------|
| **Service territory tagging now requires a formal TD** | Direct reversal of the Jul 21 "lightweight comma-separated field, no TD needed" alignment. Alyssa has a light requirements doc plus prototype in shared Drive. |
| **Move forward on placeholders rather than wait for Jeff Novotny** | With Jeff OOO, the team decided to proceed on the enclosing-zip-code and service-territory-list placeholders. Applied as a general principle: blocked items proceed on placeholders rather than waiting for stakeholder feedback. |
| **Historical work order data stays in the data warehouse only**, not at the service level | H3 cell resolution data already exists in the DW, so they will map there. Proposal to **add an H3 resolution-8 column to existing DW tables**. Strongest signal yet that H3 is the operative geo primitive - but the decision is still not formally closed on the record. |
| **RabbitMQ removed from the parts and shipment service** | Event consumption converted to Kafka. Bilas scoped it as a minor task, bundled into the Aug 31 parts release. |
| **MARBLE release dates locked: Aug 31** | Parts return + provider-added parts ship together Aug 31; internal work complete Aug 25. Includes sunsetting experiments. Suggestion to dip into the KTLO bucket to hit the date. |
| **Territory config releases to all customers** once fit-and-finish completes | Safe because it's opt-in. Framed as a permanent, necessary feature for buyers to trust Smart Dispatch alongside auto-dispatch. |
| **In-platform territory editing deliberately deferred** | Territories continue to be imported via CLI script from CSV. An in-platform update tool was discussed and explicitly ruled out of the current TD - consistent with "Standardize, Don't Customize" holding by default. |
| **Capacity now expressed as 85% dev / 15% KTLO**, not story points | Supersedes the SLATE 95 pts / GARNET 110 pts figures locked Jul 6. |
| **Map view + filter fit-and-finish put on hold** to protect service territory config | Reverses the Jul 21 "no carryover, done by end of sprint" position. |
| TD review process change | The platform-service-recreation TD goes out as documentation mocks to the broader engineering team rather than a synchronous review session. Shared Friday Jul 31 with Mike Pash and the internal team. |

## Shipped

- **Cancel Smart Dispatch is live** - the first delivered H2 item. Minor UX tweaks remain.

## Buyers

- Smart Dispatch live with **3 buyers**; **6 buyers** in reporting.
- **Shift** wants territory-level pay rates and auto-accept counter offers.
- **DoorDash and Shift** both report the ranking window causing assignment delays and duration-setting problems - flagged as a potential **Q4** item.
- Tony is presenting Smart Dispatch use cases to the CX org.

## Sequencing

Next initiatives: schedule conflicts, then auto-accept counter offers. Lynsay owns the **dispatch network expansion** TD - expansion breadth varying by territory type (wider in rural, narrower in metro). Needs a Q3/Q4 delivery roadmap before the TD can start.

## Watch Items

- **"Marble team project departure scheduled for August 31"** in the quick notes - reads as the team rolling off the project at Aug 31, lining up exactly with the parts release date. Corroborated by Mike Mueller's Jul 30 remark that "Marble is also transitioning to Data Platform work." Worth confirming what happens to MARBLE's Q3 provider-recruitment / BF WO visibility focus.
- Team members hitting Claude usage limits; Ryan built the in-platform territory prototype in Gemini instead to work around context limits.
