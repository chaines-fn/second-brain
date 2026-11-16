# Talking Points — Talent Pool Membership for Service Territories

> **Pre-meeting prep (Chuck's talking points), not meeting notes.** This was the argument Chuck prepared *going into* the June 1 session. For what was actually discussed and decided, see the outcome notes: `program/meetings/2026-06-01-talent-pool-service-territory-mapping.md`. The meeting largely validated this framing (pool = source of truth, territory = live view, "one rule, two views") and added: work-history as a dispatch/coverage signal, non-static AWS-isoline distance bands, the Unicus ~179-pool migration challenge, and a decision to **defer the build** and use DoorDash's 2nd B/F project as the friction test.

**June 1, 2026 · Product + UX audience**

The plan: agree with Ryan, then get specific about how we actually build it so we don't end up with two lists that disagree again.

---

## The point I'm making

Ryan's right — pool should be the single source of truth. The cleanest way to get there is to make a territory a *live view* of the pools you link to it, instead of a separate list someone maintains by hand. Most of the plumbing already exists. The one thing we need to decide today is where the matching rule lives.

---

## Ryan's case (lead here, I agree with it)

Requiring pool membership makes the pool the source of truth. That gets buyers:

- No more double work (add Jane once, not to five territories)
- Things stay in sync (remove from pool = actually removed)
- Auditing is one lookup, not reconciling a pile of lists
- One mental model that matches Smart Dispatch
- And it's the precondition for "manage above the project" — you can't deploy once everywhere unless the pool is the master

If someone asks "why *require* it vs. default to it": you can't promise a single source of truth if a provider can sit in a territory outside every pool. The guarantee needs the requirement.

---

## The actual problem to solve

Here's the part that makes this easier: a service territory is *already* a set of talent pools under the hood. Creating a territory quietly spins up hidden provider lists. So adding someone to a territory already adds them to a pool — just an invisible, throwaway one nobody can reuse.

So the double work isn't two systems. It's that we built the same system twice and didn't connect the copies. The fix isn't to bolt a gate between two things — it's to stop maintaining the second copy at all.

---

## The fix — 3 changes

1. **Stop auto-creating the hidden lists.** Buyers link real pools they own and reuse.
2. **Set the matching rule when they link** — who from this pool applies here (ZIPs, distance, work history). That rule is what's been missing, and it's basically what "auto-mapping" was trying to be. Not a separate feature — part of linking.
3. **Treat those pools as real** — reusable, editable, shared. Not throwaway.

Overrides still exist so buyers keep control: force someone in (commutes from an hour away), ban someone out. Default does the busywork.

**How this gives Ryan exactly what he wants:**

| What Ryan wants | How this delivers it |
| --- | --- |
| Pool = single source of truth | Territory *is* the pools — no second copy to drift. Structural, not a rule we hope holds. |
| Kill double work | Link once, rule once. Roster fills itself. |
| Stays in sync | Edit the pool → every linked territory updates. It was never a copy. |
| Easy auditing | "Who's in the territory" = linked-pool members who meet the rule. Always current. |
| Matches Smart Dispatch | SD already runs on pools — this makes territories the *same* object, not just similar. |
| Manage above the project | Pools are already account-level, so this is the on-ramp. |

---

## The one decision we need today

When a buyer sets the matching rule, what does it control — **what they see** (who's on the map / in the territory) or **who gets the job** (who gets the offer)?

My answer: **same rule, two views.** Write it once, it drives both. We do *not* build two separate filters. The second "who's in the territory" rule drifts from the "who gets dispatched" rule, and we're right back to two lists that disagree — the exact thing Ryan's killing, just one level down.

Technical reason (plain version): dispatch already has one engine that decides who's eligible, and it already re-checks right before sending an offer (catches expired creds, someone who moved). We use that one engine. We don't stand up a second one for territory membership.

Net: "membership in a territory" stops being a maintained list. It's a linked-pool member who currently meets the rule. That's the whole shift.

---

## Why this is safe, not a moonshot

- Smart Dispatch V3 (in review now) already makes talent pools first-class dispatch targets — the same "link real pools" model.
- It *explicitly rejected* reusing today's territory structure (their words: "a band-aid on a model that wasn't built for this") because the old model only holds one pool per slot and doesn't scale.
- The hard plumbing mostly exists: the fast membership copy for dispatch, the live coverage counter, the freeze-on-dispatch behavior.

So Ryan's "match Smart Dispatch" point is right — and the way to honor it is to put territories on the *same model* SD already moved to. Otherwise it's one source of truth on paper, two builds underneath.

---

## Quick answers to the 5 open questions

1. **Require pool membership?** Yes. They're already pool members under the hood — we're just making it honest.
2. **Removal cascades immediately?** Yes, with a warning + impact preview ("removes Jane from 12 territories"). Cheap once it's a live view — the work is the warning UX, not the plumbing.
3. **In two pools, removed from one — stay?** Yes, automatically. Membership is the math, not a rule we write.
4. **Old projects or new rules first?** New model for new projects, migrate existing opt-in. It's a relabel, not a data move — they're already pool members.
5. **Auto-mapping on by default?** Yes — auto-mapping basically *is* the link rule now. Don't build a constant-recompute loop; the re-check-at-dispatch already handles freshness.

---

## Likely pushback

- **"Buyers lose control of their territory?"** No — overrides stay. Default does the busywork, control's still there.
- **"Massive rebuild?"** Most plumbing exists. Real work is the linking/override UX plus reconciling two back-end models that already split. More unify-and-surface than build-from-scratch.
- **"Does this break Smart Dispatch?"** It matches where SD already went. One thing to reconcile: priority tiers — SD moved to flexible bands (several pools per level), old territory model allows one pool per slot. That's the real eng conversation.
- **"Didn't April blow up?"** Yeah, a feedback loop that needed a reboot. The newer dispatch design engineers that out and adds tracing. We ride that — and we don't build a constant-recompute loop on membership.
- **"Why is architecture in a product meeting?"** Because this product rule quietly picks a data model, and two of our designs already picked different ones. Decide the principle here, engineering converges.

---

## The ask

Three principles — not UI, not field names:

1. **Pools are the master.** Territory = live view of linked pools, not a hand-maintained list.
2. **One rule, two views.** The matching rule drives both the map and dispatch. We don't build two filters.
3. **We converge.** Territories move onto the same model Smart Dispatch already took.

Agree on those three and the five questions mostly resolve. I'll take the tier reconciliation back to engineering with a clear direction. (Still need to confirm how locked the V3 table is — haven't checked with Bilas yet.)

---

## If someone goes deep (term cheats)

- *Live membership* → live view of the pools, not a saved copy
- *Snapshot at dispatch* → when a job goes out, we freeze who was eligible so the record doesn't shift
- *Re-validate at publish* → right before the offer, we double-check they still qualify
- *Single eligibility engine (Provider Match)* → one brain decides who's eligible; we don't want two
- *Coverage Summary* → the live "how many providers cover this area" counter

Sources: Product brief (Ryan); Network Development TDR (Mobarok); Dispatch Automation TDR (me); Smart Dispatch Expansion V3 (Bilas).
