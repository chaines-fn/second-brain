# SmartDispatch Deep Dive with Shyft — 2026-04-15

**Date & Time:** Tuesday, April 15, 2026, ~2:55 PM CDT (~60 minutes)

**Attendees:**
- **Linda Lo** — Principal PM, Dispatch/Fulfillment (FN; meeting co-lead)
- **Brent Grill** — PM, GARNET / Smart Dispatch (FN; prototype walkthrough lead)
- **Laura Quist** — Account/Program Lead (FN)
- **Chuck Haines** — Sr. Principal Engineer (FN)
- **Bree O'Hara** — Shyft Services (operations leadership)
- **Benjamin Ramirez** — Shyft Services (maintenance operations)
- **Stephanie Owen Bland** — Shyft Services
- **Tim Draxten** — Shyft Services (project dispatch — "Timmy")

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1y17daXLQMSWnYso0d7EWmOdjW1m_Wbl07hfaNaOvRBQ/edit?usp=meet_tnfm_email)

---

## Overview

This was the first dedicated Smart Dispatch preview session with the Shyft program team. Linda Lo and Brent Grill walked Shyft through the upcoming V3 Smart Dispatch model — which represents a fundamental shift from the existing auto-dispatch paradigm of "granular-to-broad" (tiered cascading routes) to a "broad-to-narrow" approach where all eligible providers in targeted segments see the opportunity simultaneously, and the system ranks incoming requests to find the best fit. The session covered general dispatch settings, segments and criteria, eligibility floors, counter-offer thresholds, reporting targets with priority-based overrides, and automatic redispatch on provider backout.

The Shyft team's biggest concern was speed-to-assign for maintenance/break-fix work. Bree O'Hara was direct that their current auto-routing fills tickets in under 5 minutes, and any model that introduces a ranking window could blow up their time-to-assign metrics — especially given that tickets often hit Field Nation with limited SLA time remaining. Linda confirmed the ranking window can be disabled entirely, making Smart Dispatch behave as "first qualified request wins," which addressed the concern. The team converged on the idea that project work benefits from the ranking/round-robin model while break-fix/maintenance needs the fastest-to-claim path.

The session also surfaced strong enthusiasm for three features: automatic redispatch on provider backout (Tim and Bree both flagged this as a huge win for project work), counter-offer acceptance thresholds for travel (Benjamin was visibly excited, Bree noted it could drive positive provider behavior changes), and service territories as a vehicle for market-level pricing. Bree flagged that Shyft already has service territories defined in Salesforce and sees a one-to-one mapping as a reporting and pricing opportunity. The group agreed to prioritize break-fix configuration first despite initial plans to pilot with a fresh project, given the volume and operational pain on the maintenance side.

---

## Key Notes

- **Broad-to-narrow dispatch paradigm:** Smart Dispatch V3 flips the model — instead of cascading through tiers (Tier 1 → Tier 2 → Tier 3), it publishes to all targeted segments at once and uses floor-based eligibility (pass/fail minimums like PSS ≥ Good, background check within 2 years, distance ≤ 50mi) plus ranking to sort qualified requests. Eliminates the need for granular point-based custom selection rules. Linda framed this as easier for buyers to manage.

- **Ranking window is optional:** The ranking window kicks in at 60% of the dispatch duration by default, but can be fully disabled. When off, the first qualified request gets assigned immediately — functionally equivalent to an auto-route. This was the key concession that got Shyft comfortable with the model for maintenance work. Brent showed the toggle in the prototype ("disable ranking window" setting, staff-enabled).

- **Break-fix speed-to-assign concern:** Bree and Benjamin were emphatic that maintenance work needs instant assignment. Benjamin noted that by the time tickets reach Field Nation, SLA time is already partially consumed by internal Shyft processes. Their current auto-dispatch tiers complete in ~40 minutes total. The group agreed that for break-fix, first-to-claim with no ranking delay is the right mode, while project work can benefit from the pooling/ranking model.

- **Automatic redispatch on provider backout:** Smart Dispatch will listen for provider cancellations/backouts and automatically redispatch to all eligible requesting providers — no human touch needed. Configurable lead-time cutoff (e.g., don't redispatch if backout happens <30 min before scheduled start). Tim was "shocked into silence" per Bree — this eliminates a major manual pain point for project dispatch.

- **Counter-offer acceptance thresholds (travel):** Buyers can set auto-acceptance rules for travel counter-offers — e.g., accept up to $50 for providers >100 miles, or as a percentage of WO total. Can be scoped to specific provider segments (e.g., only platinum talent pools). Benjamin wanted it "now." Bree predicted providers will adapt by proactively adding travel to requests to get faster assignment, which she sees as a positive behavior shift. Limitation: doesn't cover providers who bake travel into base labor rates.

- **Talent pool tiers in Smart Dispatch:** Talent pools can be ranked (Tier 1 / Tier 2) with assignment balancing (e.g., 80/20 split). The ranking cascade is: service territory requests > talent pool requests > marketplace. Marketplace expansion can be set on a timer or immediate. Bree liked the ability to give the certified network first look before expanding to marketplace — addresses the promise they make to platinum providers about priority access.

- **Service territories and market-level pricing:** Service territories are zip-code-based geographic boundaries that enable per-market target costs and granular reporting. Currently project-specific (must be re-created per project), but the team is working on reusable territories and default FN-provided ones. Shyft already has service territories in Salesforce and sees direct mapping potential. Benjamin noted the value for challenging travel-cost counter-offers ("10 miles is 10 miles, bro"). Linda specifically brought up service territories because of alignment with market-level pricing discussions from the on-site.

- **Eligibility criteria flexibility:** Floors can be scoped per segment — e.g., require background checks for marketplace providers but not talent pool members you already trust. Priority ordering of criteria (quality vs. distance vs. experience) informs ranking trade-offs. Bree recognized this lets them keep a fair-PSS provider in a talent pool while enforcing Good+ for marketplace.

- **Reporting targets and priority-based dispatch overrides:** Enhanced projects support SLA targets (Assigned By, Arrive By, Complete By) tied to priority levels. A P1 can override the global dispatch duration — e.g., global is 4 hours but P1 triggers a 1-hour dispatch with immediate marketplace expansion. Linda suggested that priority could eventually be auto-set based on proximity to scheduled start, which would address Tim's current manual workaround of shortening tier durations for short-notice project work.

- **Shyft wants to prioritize break-fix over a new project pilot:** Bree pushed to focus on break-fix configuration first, despite the team's initial plan to experiment with a fresh project. The volume and operational issues on the maintenance side are consuming most of their bandwidth ("if my percentage of calls every day is any indication, break-fix is going to take priority"). Linda confirmed the team is ready to support this pivot.

- **Copy-to-project and change history:** Smart Dispatch settings can be copied across projects (reduces setup friction for new projects) and include a change history log showing who changed what and when — directly addresses a pain point Bree raised about tracking down who modified dispatch rules.

---

## Action Items

- **Laura Quist:** Schedule a service territory meeting with John Shaw and Ryan Schulte. Identify a project to experiment with service territory reporting.
- **Laura Quist:** Schedule a follow-up mapping session in May or June to map current auto-dispatch rules to the new Smart Dispatch model for specific projects.
- **Bree O'Hara:** Send feedback, ideas, and questions to Linda/Brent as they percolate after reviewing the session.
- **Linda Lo:** Send a follow-up Loom video detailing the steps for replacing/configuring specific parts of the Smart Dispatch setup.
- **Linda Lo (implied):** Coordinate with Shyft to prioritize break-fix Smart Dispatch configuration, adjusting the original plan to pilot with a new project.
