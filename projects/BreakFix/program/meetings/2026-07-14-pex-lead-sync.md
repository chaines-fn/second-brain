# B/F PEX Lead Sync — 2026-07-14

**Attendees:** Ryan Schulte, John Shaw, Mobarok Hossain, Alyssa Eby, Bilas Chandra (Sarker), Ridi Haque, Lynsay Faucett, Chuck Haines, Amran Hossain

## Summary

Product/eng sync covering reporting access, the standardized service territory data model, map performance at scale, and the availability experiment.

## Decisions

- **Service territory data model: zip code intake + H3 cells for internal visualization and dispatch.** Restated/confirmed the split from the Jul 9 H3 POC review (hex cells = visualization/dispatch layer, zip codes = buyer-facing intake) as a locked decision. **Note the apparent tension with this same morning's Eng Leads Sync** (see `2026-07-14-eng-leads-sync.md`), where Chuck recommended *not* building any H3 infrastructure yet and staying zip-code-only until a concrete business blocker emerges. Chuck attended both meetings — worth him reconciling directly whether this PEX-sync line is just restating the already-standing Jul 9 direction (no new build committed) or actually commits to building H3 visualization now.
- **In-platform map layer customization is disallowed.** Users must either select a default territory or import a fully custom one off-platform — no in-platform redraw/reshape tool. This formally locks in, at the engineering-decision level, the "Standardize, Don't Customize" direction from Lynsay's Jul 13 product brief update (see `program/overview.md` and `teams/SLATE/knowledge/knowledge.md`) — worth checking whether this reflects an outcome from yesterday's on-site pitch to Jeff, since that outcome wasn't otherwise confirmed.
- **Availability feature scoped as a single, non-configurable experiment**: prevents provider assignment only when work order start times conflict (hard-start double-booking) — no buyer-facing UI/configuration in this first phase. Targets Clear Captions' double-booking concern specifically. Bilas expects it ready to enable early next sprint. Metrics (backouts, timeliness) will lag due to client onboarding timelines; future iterations (drive-time padding, confidence scoring) depend on more data and Mia's involvement.
- **Sprint scope**: Slate team will pivot to service territory configuration work once Chuck's technical design review clears — replacing previously planned filtering/pagination tasks.
- **Staff access bug**: staff users get an error viewing reporting targets that customer users can see; Bilas will remove the validation restriction causing it (minor fix).
- **Custom territory sets are static once created** — if a buyer builds a custom territory list from the default list, it won't auto-update when the master list changes. Confirmed acceptable (favors buyer configuration stability).

## New technical risk flagged

**Map view performance degrades badly at scale.** John Shaw and Lynsay Faucett flagged the map view becomes effectively unusable for large territory-count clients like DoorDash when loading up to 1,200 service territories. A ticket exists for boundary-rendering issues, but broader performance work isn't yet scoped — Lynsay to add a task/acceptance criteria to the service data enhancement ticket; Mobarok to plan and implement.

## Still open

- **Territory grouping/hierarchy strategy undefined.** Ryan and Lynsay discussed grouping the 1,200 default territories by region (state) and type (metro/suburban/rural) to make them manageable, but no design is finalized. This is the same underlying problem as the "too many small territories" reporting-grouping idea from the Jul 13 product brief.
- Mia's/Alyssa's involvement in service-territory/talent-pool technical design reviews — Mobarok to loop them in going forward so coverage-score models aren't broken by table changes.

## Source

Gemini notes: [B/F PEX Lead Sync — Jul 14, 2026](https://docs.google.com/document/d/1T0sr7ZOs7Lxg5ns4a0Q1UII8JF6ux9iLAxF9F_fJhPQ/edit)
