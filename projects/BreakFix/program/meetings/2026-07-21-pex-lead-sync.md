# B/F PEX Lead Sync - Jul 21, 2026

**Organizer:** Lynsay Faucett · 8:30 AM CDT
**Attendees:** Ryan Schulte, John Shaw, Mobarok Hossain, Alyssa Eby, Bilas Sarker, Ridi Haque, Lynsay Faucett, Amran Hossain, Sadat Haque
**Source:** [Notes by Gemini](https://docs.google.com/document/d/1rGQopGy3t3xQPTsIGXtn3uycdiMRJg_HSk9C7n_QGC0/edit)

> Chuck invited, on vacation.

## Key Decisions

| Decision | Detail |
|----------|--------|
| Service territory config committed to next sprint at **38 points total** | Split GARNET/SLATE. Both Mobarok and Bilas committed to full scope in one sprint despite PTO. Confirms territory config did **not** ship Jul 27 - only SD cancellation did. |
| **Smart Dispatch cancellation** code complete | Final testing only. Target Thu Jul 23, contingency Mon Jul 27. Includes Amplitude events. |
| Service territory tagging approved as a **lightweight single comma-separated DB field** | Surfaced as tags in-platform, available to buyer and internal users. Mobarok confirmed feasible, no TD deemed necessary. **Reversed one week later - see Jul 28.** |
| **Reporting Targets Phase 2 paused** | Bilas revealed the original Phase 2 technical design was previously deleted/removed. Team must decide whether a new TD is needed or whether the existing priority-level foundation can support multiple targets per project via templates. Driven by Black Box and ETS. |
| Amran restructured **NetDev OKRs** | P1/P2 separated; now two milestones for tool development and two for architectural review. |

## Risks & Blockers

- **NetDev email ceiling (new, hard):** Google Workspace caps sends at **2,000/day**. Ryan's stated safe operating need is **10,000/day**. The current workaround of spinning up multiple Google accounts was called "unsustainable."
- **Black Box parts integration on hold** - Black Box will not use parts until advanced data capture and the Shift return experience ship. Project timeline being re-evaluated; open question whether Black Box proceeds on current runbooks or waits.
- **Auto-accept counter offers will slip** (Ridi). Schedule-conflict and SD-cancellation milestones still hold.

## Notes

- Alyssa Eby is effectively the reporting/analytics PM voice in this program now - driving reporting requirements and the territory-tagging one-pager.
