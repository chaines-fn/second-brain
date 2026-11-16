# B/F PEX Lead Sync — 2026-06-30

**Date & Time:** Tuesday, June 30, 2026 (~47 minutes)

**Attendees:**
- Lynsay Faucett — PM, GARNET (meeting organizer)
- John Shaw — Group PM (cross-stream)
- Amran Hossain — Engineering Manager (all BD teams)
- Ridi Haque — Engineering Manager (GARNET + MARBLE)
- Mobarok Hossain — Engineering Lead, SLATE
- Bilas Sarker — Engineering Lead, GARNET (email: bilas.chandra@fieldnation.com)
- Ryan Schulte — PM, SLATE
- Alyssa Eby — UX, GARNET
- Chuck Haines — Sr. Principal Engineer (cross-cutting)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1gDp4adWqdU7jCXsolukMnUA-J3ftAfcrYXaq9zMZ03Q/edit?usp=meet_tnfm_email)

---

## Overview

The meeting opened with Lynsay confirming that Smart Dispatch went live the prior week and the team is on track to close Q2 strong — map view and single service territory drawer are releasing end of month. Planning documentation is migrating from the existing Google Doc to a new spreadsheet (Ryan helped build) for better visibility into sequencing and status.

The biggest shift out of this meeting: service territory configuration for Smart Dispatch was elevated to the top priority for GARNET's next sprint. Clear Captions — a high-volume buyer currently running autodispatch — wants to test Smart Dispatch on a single service territory rather than flipping it on program-wide. The lack of this capability is a real blocker to onboarding risk-averse, high-volume buyers. John made clear they need this quickly to get Clear Captions and similar buyers actually running Smart Dispatch at scale. This trades off against the view-all service territory drawer enhancement, which slides down the queue. For Mobarok's SLATE work, the in-platform service territory creation POC (H3 model) continues as the current sprint priority, with configuration picking up next sprint.

The second half focused on standardized service territories and the H3/Uber hex model. Mobarok walked through the technical design: moving from zip-code-based definitions to Uber H3 hex cells, using isolines and centroid/anchor points, with Reswan from the data science team as a key collaborator. Jeff from the data team raised concerns about the hex model's ability to track driving distances — Chuck and John pushed back clearly: the H3/isoline model *specifically accounts for drive times* and is strictly better than zip codes for this. Lynsay is setting up a session with Jeff, Jake (data), Chuck, and Mobarok to get everyone aligned. A buyer customization thread also emerged: Unicus needs a 25-mile radius buffer around service territories, which doesn't fit the "combine defaults" model Ryan had proposed. Chuck flagged this as a significant open design question — customization opens a real wormhole around historical reporting, versioning, and how to handle changes to master territory definitions after buyers have copied them.

Q3 planning closed out the meeting. The OKR will likely center on Smart Dispatch adoption. Lynsay asked Mobarok and Bilas to provide high-level estimates for Q3 initiatives. For large, vague items like network expansion, Ridi suggested collaborative problem-framing sessions to produce workflow diagrams before attempting estimates — Lynsay adopted this approach for those items.

---

## Key Notes

- **Smart Dispatch service territory configuration — top priority shift:** Elevated above all other GARNET work. The core need: buyers like Clear Captions want to pilot Smart Dispatch on one territory while autodispatch continues on everything else. Without this, high-volume risk-averse buyers can't adopt. Current workaround (creating a separate project) is impractical. This is a sprint 1 priority; Mobarok can start reviewing the TD this sprint if capacity allows.

- **Trade-off: view-all service territory drawer enhancements deprioritized:** The planned "view all territories" drawer enhancement is being pushed down to accommodate service territory configuration. This was the last planned Q2 item for SLATE's map view work.

- **Counter-offer ranking conflict flagged by Bilas:** Bilas raised that the proposed logic — lowest counter-offer gets priority — may conflict with the existing ranking mechanism when multiple providers have counter-offers. He posted specific questions in the doc. Lynsay to review and respond. This is pre-technical-design; nothing to build yet.

- **H3/Uber hex model: direction set, data sourcing in progress:** Mobarok is building the technical design around H3 cells + isolines. Needs two additional data points: centroid points and anchor points for zip codes. Reswan (data scientist/strategy) has done similar work in the data warehouse using H3; Mobarok is syncing with him today. Chuck confirmed Reswan's prior work is relevant.

- **Jeff's driving-distance concern is a misunderstanding:** Jeff from the data team believes the hex model can't track driving distance. Chuck and John are confident it's the opposite — isolines are *defined* by drive time and are strictly more accurate than zip codes. Lynsay scheduling a readout with Jeff + Jake to build confidence before this concern becomes a blocker.

- **Buyer customization: unresolved design question, needs a dedicated session:** Ryan's original vision for buyer customization = combining default templates. Unicus is asking for something different: 25-mile radius buffers around specific territories, effectively custom-built. Chuck flagged these are two different use cases and both open hard questions around historical reporting (what territory was a WO in if the territory definition changes mid-flight?), versioning, and revert-to-default behavior. Need everyone in a room for 1–1.5 hours to work through this.

- **Reporting resource alignment:** Coverage scores across talent pools depend on standard service territories being in place — Alyssa and John already aligned on this with the business. Engineering resourcing for reporting productization in Q3 is unresolved: Emerald won't share with Break/Fix (confirmed by both Alyssa and Amran), so it'll come from within the org. Alyssa estimated 1-2 engineers needed. She's meeting with John today to solidify the ask. Amran wants reporting line items folded into Lynsay's master planning spreadsheet rather than tracked separately.

- **Q3 OKR direction:** Lynsay said the OKR will likely be a Smart Dispatch adoption metric — e.g., number of buyers running Smart Dispatch. Several sequenced items (service territory config, counter-offers, availability) directly feed this.

- **Problem framing before estimating vague items (Ridi's approach, adopted):** For large ambiguous initiatives like network expansion, the team will do collaborative workflow-charting sessions first, then estimate. Lynsay adopted this for items that don't yet have a clear solution path. Smaller, concrete items get estimated directly.

- **AWS consultant meeting:** Chuck committed to rescheduling the AWS geospatial consultant meeting (had been forgotten during time off). Lynsay flagged it as still valuable, though Chuck's view is it likely won't change the direction they're already heading.

---

## Action Items

- **Mobarok:** Review the two shared documents (service territory creation and interface design)
- **Lynsay:** Respond to Bilas's questions about counter-offer assignment logic in the shared doc
- **Mobarok:** Sync with Reswan on centroid and anchor point data for the H3 service territory model
- **Chuck:** Schedule meeting with the AWS consultant to discuss geospatial tech direction
- **Lynsay:** Schedule meeting with Jeff, Jake (data team), Chuck, and Mobarok to walk through the H3 model and address driving-distance concerns
- **Alyssa:** Meet with John today to discuss Q3 reporting needs and engineering resource requirements
- **Alyssa:** Merge reporting spreadsheet into Lynsay's master planning document (Lynsay to send the doc)
- **Mobarok + Bilas:** Provide high-level estimates for Q3 initiatives listed in the planning spreadsheet
- **Lynsay + Ridi (implied):** Set up collaborative problem-framing session for network expansion and other vague Q3 items
- **Mobarok:** Review service territory configuration details this sprint if capacity allows; full sprint focus begins next sprint
