# Break/Fix Eng Leads Sync — 2026-07-14

**Attendees:** Chuck Haines, Amran Hossain, Fayshal Uddin, Bilas Chandra (Sarker), Mobarok Hossain, Ridi Haque

## Summary

Two threads: (1) resolving the zip-code-vs-H3 service territory debate for now, and (2) Chuck laying out a concrete, immediate change to his role and availability on Break/Fix.

## Decisions

- **Service territory definition stays zip-code-based; H3 migration deferred until an explicit business need emerges.** Mobarok explored a hybrid approach (keep both zip and H3, backfill H3 cells to resolve blank-space/PO-box-zip gaps) but flagged the backfill script would be computationally heavy. Chuck's recommendation (explicitly borrowing "defer decisions to the last responsible moment" from *Clean Architecture*): don't build H3 now. Service territories can stay zip-code-defined while dispatch logic still uses isoline/drive-time queries — the two are separable. Only revisit H3 if PO-box coverage gaps or whitespace actually become a business blocker (Product currently says they don't care about those edge cases). Mobarok to take this back to Lynsay Faucett for final sign-off.
- **Chuck's role transition to AI DLC is now immediate, not "likely" or pending.** (Updates the "likely stepping down... not yet finalized" language from the Jul 9/10 sync entries — this is no longer tentative.) The initiative previously called "AI Software Factory" is now referred to as **AI DLC**; Chuck is both architecture lead and PM for it (Red Wine/Redwan stepping back from PM duties there). Effective immediately: Break/Fix moves to a **pull model** — Chuck reviews technical designs and answers architecture questions when asked, but will not actively attend most B/F meetings going forward. Exceptions: he'll continue attending **Eng Leads Sync** and likely the **PEX Lead Sync**. Ridi to notify Mobarok (who missed the meeting) of the change.
- Confirmed: Chuck is out on vacation the next two weeks (Jul 19–30, previously flagged Jul 7).
- Possible backfill: Chuck reiterated the BD Principal Engineer candidate he interviewed "last week" (~Jul 8) was strong and may come over to help support Break/Fix architecture long-term — not yet confirmed/hired.

## Other

- Meeting Thursday (Jul 16) with Chuck, "Schaffen" (Shafi), Spencer, and Brian to clarify whether Pearl team's standardization work overlaps with the NetDev tool — outcome not yet known.
- Chuck to review 4 pending technical designs and the counter-offer/PDR documents before vacation.

## Source

Gemini notes + transcript: [Break/Fix Eng Leads Sync — Jul 14, 2026](https://docs.google.com/document/d/1VGJL_PeQVxyJbeXbZCUz_5qVm4Jrx2_9g-2zaxiK3TA/edit)
