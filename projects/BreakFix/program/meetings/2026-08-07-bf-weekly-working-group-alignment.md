# B/F Weekly Working Group Alignment — Aug 7, 2026

**Attendees:** Ann-Bettina Colace, Ryan Schulte, John Shaw, Alyssa Eby, Lynsay Faucett, Chuck Haines, Melanie Payeur, Amran Hossain, Kayle Schiele, Sadat Haque, Tauhedul Shagor, Rick Jundt

Source: Gemini notes, [B/F Deep Dive weekly sync doc](https://docs.google.com/document/d/1AqZdq1Wqi3Tlgm6rKNjjrIvHbilsf1y3E7TauRZTQog/edit?tab=t.gvmlmpw3ya0j)

## Summary

Workshop planning, recruitment tool status, and reporting updates. John Shaw noted this meeting hit its one-year anniversary as a standing sync.

## Q4/H1 Workshop Planning

Three off-site workshops planned for Q4/H1: coverage assessments, reporting for future network development automation, and defining the next iteration of Smart Dispatch requirements. Ties into the "Field Week" working sessions already tracked (next-gen Smart Dispatch, network health automation).

## Network Development and Recruitment

- Seven active recruitments running in the NetDev tool; platform integration ongoing for background checks, drug tests, territory/pay-rate/volume data.
- Staging work — described as the last major unlock before full automation — targeted to begin in the coming weeks.
- Recruitment tracking has a visibility gap: some team members aren't using the tool, and an existing tracking doc hasn't been shared. John Shaw asked Ryan Schulte to follow up on whether email caps or other constraints are causing recruitments to bypass the tool.
- **Decision:** recruitment tool will be updated to automatically block suspended/blocked providers from appearing on recruitment lists (Alyssa Eby flagged a case where 12 providers for an Interface recruitment included blocked providers, wasting review time). Ryan Schulte to submit the dev request.

## Service Territories

- Standardized service territory TD nearing completion (Lynsay Faucett); last outstanding item is migrating data reporting from the data warehouse to the data platform, coordinating with the MIA team, targeted for early next week.
- Network expansion TD underway; conflict prevention and service territory configuration development targeted for release "next week" (i.e., week of Aug 10).

## Reporting and Data Analysis

- Coverage scores now integrated into the reporting schema with daily backend feeds; front-end still needs to consume the daily feeds (Google-site refresh is the current blocker).
- A Parquet-blob POC is in testing — could enable daily live reporting within 2-3 weeks.
- John Shaw noted the latest reporting metrics are already showing positive impact for Unicus, DoorDash, Interface, and Zones, driving specific program recommendations.
- **Decision:** return trip rate reporting stays scoped to break-fix programs, available on request — team decided not to go broad, since the underlying model/UI may still change.
- NotebookLM identified as a resource for reporting/data analysis; ownership assigned to Alyssa Eby.

## Operational Execution (MARBLE)

- Parts return fit-and-finish in progress (Sadat Haque); Smart Audit scoping is done and development starts once fit-and-finish wraps.
- Parts pickup location visibility (a source of provider confusion/compensation requests) prioritized for end-of-month completion.
- Shagor working on provider onboarding/permanent-placement messaging, including in-app messaging.

## Sales and CS Enablement

- Pipeline movement: Bailey Wick, PlayOn, Luxer One; a new large opportunity referenced as "NCR — Pizza Ranch contract, 2027 start" was added (transcript is ambiguous on naming — may relate to the already-tracked NCR/"NCR Voyex" ~$1.5M/2027 pipeline item; worth Chuck confirming whether this is the same deal or a distinct one).
- **Decision / role change:** Kayle Schiele is stepping back from customer success/sales enablement to focus solely on professional services and execution (solution engineers, onboarding specialists, program management). Melanie Payeur takes over primary ownership of sales and CS enablement.
- PM capacity constraint expected in September (an upcoming leave — likely Luchian's already-tracked 12-week leave); plan is to lean on onboarding specialists and Hunari for coverage.
- Execution playbook considered complete for the current phase; team can iterate on it independently going forward.
- Exploring folding service territory / break-fix features into LMS certification courses, framed around buyers becoming "Field Nation champions" who pick the right tools for their use case rather than just learning platform mechanics.

## Next Steps

- Ryan Schulte: submit dev request to block suspended/blocked providers from recruitment lists.
- John Shaw: update the recurring meeting link so it redirects to the current session.
- Lynsay Faucett: transfer NotebookLM project ownership to Alyssa Eby.
