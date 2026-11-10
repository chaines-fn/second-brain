# 2026-04-07 — Work Thru Unisys Requirements

**Attendees:** Linda Lo, Brent Grill, Chuck Haines, Chris Broveleit (Scooter Aspen / Stephanie Aspen)
**Context:** Internal working session to synthesize what the Field Nation team heard from Matt (Unisys) and align on a path forward for WO capacity logic and dispatch integration patterns.
**Sources:** [Gemini transcript notes](https://docs.google.com/document/d/10kOwNGpvh8X4uFiTKJ0_BdxohxIlfeFXD4eisHsAVkY) | [Manual notes & background](https://docs.google.com/document/d/16jO13sI4hMXDk2I1d7cpZvmvxVcZybQwlvCcvrJDbAA/edit?tab=t.fn73e6n9u2z7#heading=h.7hpceju2rmmh)

---

## What We Learned About the "Cap"

The 5 WO/day cap is a default guideline, not a hard rule. The real objective is SLA protection. Unisys's historical average across 25 years is 4–4.5 tickets closed per day per in-house provider — 5 is a reasonable ceiling for typical B/F, not a sacred number. Matt confirmed: the main objective is protecting against SLA impact.

Key nuances:
- **Seasonality matters.** Jan–early March is peak; summer slows down; back-to-school picks up; Nov–Dec declines. A static cap could over-constrain during peak and under-utilize in slow periods. Open Q: Is flex-on-volume a NetDev/reporting problem or a Smart Dispatch problem?
- **Unisys wants control, not a constraint.** Matt: "We want to have some control over when we can turn it on, when we can turn it off." Project work (e.g., Gurney, IL) regularly pushes providers above the threshold intentionally. Whatever gets built should be opt-in, configurable, and reversible.
- **Manager discretion is central.** Area managers know their techs individually. A manager might cap a specific provider at 3 and bump another to 10 for project work on a given day. The cap is an input to a human decision, not a system enforcement.

---

## What Should Influence Dispatching

**Same-site or co-located WOs count differently.** Matt: a tech "will not be able to do more than five unless they're at the same location or has a bulk of them at the same location." Concrete example: 20 NIC card replacements at one site weekly, two CA techs handling 30 calls/week for project work. Chris Broveleit pushed for a "systemically distinguishable" differentiator — one location, one address, one pickup/drop-off.

FN platform notes on co-location bundling:
- Client API can bundle WOs at create time, but requires multiple calls — Unisys has explicitly said they don't want multiple calls (confirmed during the payload work discussions)
- Mass upload is an option but not supported OOTB in Smart Dispatch flow
- Scooter suspects Unisys may have a column in their internal tables that AMs use to track WO site-to-site relationships — worth investigating

**The three factors Unisys weighs (in priority order):**
1. Travel distance / travel impact on SLA (highest weight)
2. Individual skill level and trust
3. Product line

**Travel is a hard scheduling problem for us.** Chuck called this out explicitly: we don't know the actual work time for range or business-hour schedules until a provider checks in. Without knowing when WO 1 ends, you can't accurately calculate travel time to WO 2. This is a fundamental platform constraint that limits how sophisticated the travel logic can get right now.

**Tier framing:** Tier 1 = "go-tos" with proven history — higher default cap is appropriate. Tier 2/3 = ramping up. Important: tiering should inform smart defaults, NOT be used as a carrot/stick incentive system. Matt pushed back hard on that framing — it causes "major consternation" when the coverage pool is small (e.g., a two-person support arm).

**Individual-level caps are desired.** Managers want per-provider tuning: set one tech to 3, another to 10 for a specific week/project. This points toward a manager-facing control in the product rather than a blanket system setting.

---

## Where Humans Fit In

**Platform handles routine dispatching; managers only get pulled in for exceptions.** Matt agreed with this pattern: once all local providers are at capacity and more work is coming in, that's when managers should be notified to intervene. Their offshore team currently monitors aged tickets and new assignments manually — that's the overhead the platform should absorb.

**Email escalation is acceptable for now.** When auto-dispatch fails or all providers hit cap, an email notification to the team is sent — managers then come to FieldNation.com to make manual assignments or adjust caps. This is the current fallback and the group agreed it's good enough for the first lean phase. (Longer-term desire: integrated ServiceNow alerts, but not blocking.)

**Manager quality and approach varies significantly.** Some managers minimize travel and optimize ticket closure; others "throw techs into the portal and let the chips fall." This variation is a risk if the solution depends on manager judgment. Data-driven recommendations from the platform could help normalize dispatch quality regardless of manager style.

---

## Decision: Lean, Preventative Approach (Agreed)

**Don't try to solve travel bundling or co-location logic in the first phase.** The consensus was to implement a simple **max daily assignment cap per provider** as a dispatch criterion, and use email escalation to pull managers in when all providers have hit cap or not responded.

Rationale for deferring location-based bundling:
- Travel bundling requires solving the provider-buy-in problem (if a provider requests one WO, automating assignment of co-located WOs is complex when other ranking/criteria factors apply)
- No-travel-cost context (Unisys doesn't pay travel) makes this less immediately impactful than it would be for other buyers
- Real-time cap counts are technically necessary to prevent race conditions (provider grabs multiple WOs simultaneously once the cap is set) — need to solve this regardless

**Phase 1 scope:**
- Max assignments/day per provider as a dispatch filter criterion
- Email escalation when all providers are at cap or unresponsive
- Manager-controlled overrides (adjust cap per provider, toggle on/off)

**Deferred:**
- Site-to-site location bundling logic
- Seasonality-based cap flex
- Proactive batching

---

## Integration Pattern: Still Open, Needs Documentation

The group revisited the single-call vs. two-call API integration debate. Current state: two-call pattern (separate API call to trigger smart dispatch, returns `dispatch_run_id`). Some buyers are pushing back on making multiple calls.

**The core problem with single-call is error handling.** If smart dispatch fails (technical error, no providers found, bad dispatch settings), the WO is already created. What happens? Does it sit in draft? Is there a rollback? Chuck: we need to define all edge cases explicitly before committing to a direction — buyers will assume the WO is being dispatched when it may not be.

**Chuck also raised the orchestration layer angle:** this may not be an "either/or" — an orchestration layer could manage both calls. He's heard the integrations team is already solving something similar at the broker layer for integrations, but unclear if that applies to the Client API path. Linda will follow up.

**Action: write a proper pros/cons doc** covering single call vs. two separate calls, with explicit treatment of all error edge cases. This needs to be "beefed up" enough to make the decision deliberately. Brent builds the prototype; Chuck reviews and validates; Linda + Chuck document the API approach the following week.

---

## Side Note: Project Name ≠ Unique Identifier

A separate discussion surfaced: one integration (via Belos) is apparently passing project by name instead of ID. Linda noted that multiple projects can share the same name across different IDs — she has a concrete example from Fujitsu/color retail imaging projects. Chuck: if project names aren't unique per buyer, using name as an identifier is a "non-starter." Chuck to loop Linda into the Belos conversation.

---

## Dependencies Flagged

These are platform gaps that would make the capacity logic more accurate once resolved — they're in-flight but not blocking this effort:

- **WO relationship mapping** (parent/task, reschedule vs. revisit): a provider with 3 WOs where 2 are reschedules from yesterday looks different from 3 net-new WOs. Platform can't distinguish these today.
- **Reschedule tracking:** not clearly tracked in FN today; "revisits" and "reschedules" are conflated.
- **Multiple check-in validation:** can't distinguish valid vs. invalid check-ins.
- **Provider schedule + WO schedule type intersection:** unresolved for travel calculation purposes.

---

## Action Items

| Owner | Action |
|-------|--------|
| **Brent Grill** | Lead prototype for working session with Unisys |
| **Brent Grill** | Start analysis on solution directions; investigate specific approaches |
| **Brent Grill** | Pull group into review meeting for solution feedback once prototype is designed |
| **Chuck Haines** | Beef up documentation on integration call trade-offs (single vs. two-call, error edge cases) |
| **Chuck Haines** | Validate Brent's prototype for technical constraints |
| **Chuck Haines** | Document pros/cons of single-call vs. two-call API integration approach |
| **Chuck Haines** | Loop Linda into conversation with Belos re: project names |
| **Linda Lo** | Send Chuck an example of projects using same name but different IDs |
| **Linda Lo** | Follow up with integrations team: are they solving the orchestration layer for Client API? |
| **Group** | Show Scooter prototype before the full-group review to catch issues early |
