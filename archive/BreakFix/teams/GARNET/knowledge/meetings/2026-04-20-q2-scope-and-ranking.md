# Q2 Scope and Ranking — 2026-04-20

**Date & Time:** Monday, April 20, 2026, ~7:00 CDT (~24 minutes)

**Attendees:**
- **Bilas Sarker** — Engineering Lead, GARNET (meeting organizer) *(his email is bilas.chandra@fieldnation.com but he goes by Sarker — don't let the email fool you)*
- **Ridi Haque** — Engineering Lead, GARNET
- **Chuck Haines** — Sr. Principal Engineer

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1XbpBxq-TqXgDmUHD-3tQnwmlU4gfdmNKjpu-F3Wuxds/edit?usp=meet_tnfm_email)

---

## Overview

A short working session between Bilas, Ridi, and Chuck to align on the Q2 engineering strategy for Smart Dispatch V3 — specifically around hard filters, default ranking behavior, and whether to layer CSR-based ranking in as an interim step. Bilas walked through his technical design, proposing the team expand beyond the service-tier dependency by enabling smart dispatch and criteria support as hard filters, then use default Match 2.0 ranking, with priority ranking and change-log display deferred.

The core debate was whether to implement CSR-based ranking (where buyer-configured criteria dynamically generate a formula at runtime) as an interim solution for Q2. Bilas made the distinction clear: CSR-based ranking is a separate, category-only ranking path that would sit alongside provider match, while default ranking uses predefined criteria and weights already baked into the ranker. Both Bilas and Chuck agreed the CSR-based approach is throwaway work that conflicts with the long-term priority-ranking direction, doesn't speed up Q2 delivery, and would only add confusion. Chuck was firm that engineering quality cannot be traded away for PM delivery pressure — if Q2 needs to ship sooner, scope needs to be cut elsewhere (talent pools were mentioned as a candidate).

The group aligned on a phased Q2 delivery: hard filters first (based on kit settings), then default Match 2.0 ranking out of the box, with change-log UI deferred to Q3 (data capture via sedd already exists; only the display surface is on hold). Chuck committed to reviewing Bilas's two Confluence technical designs the following morning. Cross-team alignment with DMX and Linda/Brent on Provider Match UI transparency for priority ranking was flagged as needed before implementation.

---

## Key Notes

- **Q2 delivery is incremental: hard filters → default ranking; change-log deferred to Q3:** Hard filters (kit-setting-based) ship first, then default Match 2.0 ranking. The change-log display is deferred to Q3 but data capture is already in place via sedd, so only UI surfacing is being held. Bilas's design proposes a view table that queries multiple system-type tables to generate the change-log view when it's eventually brought up.

- **CSR-based ranking explicitly rejected for Q2:** Bilas and Chuck converged that CSR-based ranking is throwaway work — it's a separate ranking path that only ranks by category, doesn't move toward the long-term priority-ranking goal, and won't accelerate Q2 delivery. This aligns with and reinforces the Apr 14 scope decision, but walks back the "temporary behind-the-scenes CSR approach" that was floated there as an interim for Q2. Chuck's stance: if PM wants priority ordering sooner, cut scope elsewhere (talent pools was Chuck's example).

- **Default vs. CSR-based ranking — the technical distinction:** Default ranking uses predefined criteria, weights, and conditions baked into the ranker service's core algorithm. CSR-based ranking takes buyer-configured criteria (PSS, equipment, distance, etc.) and dynamically builds a formula at runtime in provider-MS, which is sent to the ranker. The ranker algorithm itself isn't changing in either case — the change is in what data/formula gets passed in. Bilas noted that *if* priority-based ranking is eventually built, the pattern is similar to CSR ranking (dynamic formula assembly in provider-MS) but with implementation differences — reinforcing his point that CSR work isn't reusable.

- **Boolean vs. formula-based criteria — alignment needed with Linda:** Certificates, equipment, and licenses are boolean (meets-criteria = full marks, else zero). Distance is formula-based (e.g., 0 miles = 100 points, 1 mile = 99, linear decay). Bilas proposed treating background check and drug test as boolean because recency-based scoring doesn't reflect a quality delta ("within 1 month vs. 6 months" doesn't mean a provider is meaningfully better). This needs explicit alignment with Linda before locking in.

- **Provider Match UI transparency is an unresolved UX + engineering problem:** If priority ranking is eventually implemented, showing multiple ranking systems on the Provider Match page is confusing, especially alongside existing CSR ranking and default ranking. Bilas flagged this needs DMX + Linda/Brent alignment before implementation. CSR ranking already surfaces a ranking bar with provider points; a priority-ranking system would need a similar transparency story.

- **Framing for Linda:** Chuck was explicit that messaging to Linda needs to land cleanly — the team isn't doing extra work to "support default ranking" (that's out of the box); what they're declining to do is layer CSRs on top, which *is* extra work with no long-term payoff. Bilas should lead with this framing when communicating the Q2 plan back to Linda.

- **Throwaway-work stance as a general principle:** Ridi raised Linda's preference for priority ordering as a Q2 release deliverable and asked about an interim solution. Chuck's response was general and worth capturing: the team won't cut engineering corners to meet PM-driven dates; scope is PM's lever, not quality.

---

## Action Items

- **Chuck Haines:** Review Bilas's two technical design documents in Confluence (one for hard-filter / default-ranking implementation, one for priority-based ranking approach) on the morning of April 21 and provide feedback.
- **Bilas Sarker:** Share the technical designs with the broader ("border") engineering team after Chuck's review lands.
- **Bilas Sarker (implied):** Schedule an alignment meeting with Linda to confirm treating background check and drug test as boolean criteria (vs. recency-scored).
- **Bilas Sarker + Chuck Haines (implied):** Sync with DMX and Linda/Brent on Provider Match UI transparency for priority ranking before implementation begins.
- **Bilas Sarker (implied):** Communicate the Q2 incremental delivery plan to Linda using Chuck's framing (hard filters first → default ranking; CSR layering explicitly not in scope; scope-cut elsewhere if priority ordering is needed sooner).
