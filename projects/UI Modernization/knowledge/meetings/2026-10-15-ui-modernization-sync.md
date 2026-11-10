# UI Modernization Sync - 2026-10-15

**Date & Time:** Thursday, October 15, 2026, 7:45 CDT (~13 minutes; transcript ended at 00:12:51)

**Attendees:**
- Andrew Kandels - Engineering lead, day-to-day technical lead, meeting organizer
- Chuck Haines - Sr. Principal Software Engineer (cross-cutting architecture)
- Ivan Sakhman - Contractor, Frontend Engineer
- Yurii Dmytriiev - Contractor, Frontend Engineer
- Miroslav Isikiyski - Backend Engineer
- Karol Tluscik - Joining full-time this quarter (role and title still to be confirmed)

**Invited, not present:** Andriy Kurenda (out sick today per Slack), Fayshal Uddin, Laura Quist (optional), Maria Saxton (OOO 10-15 to 10-19)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1-UDgppQ0aFuvlkfAJzC-HpkiimdifSHD6kpNuVopY2I/edit?usp=drivesdk)

---

## Overview

The team reviewed bug-closure policy, the main Flightboard virtualization removal, and the Sonar and dependency-security work that is now gating merges. Most of the discussion settled how fixes move through review. Andrew said customer-visible UI fixes should get a reviewer, while backend or invisible bug fixes can be closed by the author because no one else reviews them. Ivan will apply that rule as he works through his current tickets.

The main new decision is that virtualization comes out of the main Flightboard for both columns and rows. Andrew confirmed the change applies to the main Flightboard only and not the project Flightboard. Sonar and security were the other large topic. The team agreed that Sonar's magic-number rule flags Material UI color constants and cannot be scoped by rule, so the team will compile a list of false positives rather than disable the rule project-wide. On dependencies, the team agreed to fix high and critical findings and ignore low and medium ones, with Karol making a best-effort attempt on the current ticket and spinning up a backlog ticket for any larger package replacement.

---

## Key Notes

- **Bug closure policy:** Customer-visible UI fixes get a peer review. Backend or invisible bug fixes can be closed directly by the author. Andrew said he is the only one who would review those. Ivan can ask in the ticket if he is unsure which category a fix falls into. _(Decided, ~00:00:00 in transcript)_

- **Virtualization removal on the main Flightboard:** Virtualization is removed for both columns and rows on the main Flightboard. The project Flightboard is excluded. Ivan will continue with this removal. _(Decided, ~00:01:12)_

- **Sonar magic-number false positives:** Sonar flags MUI style constants such as `gray 100` and `gray 200` as magic numbers. Sonar's rules are all-or-nothing, so disabling the rule removes its value across the project. Chuck said disabling magic numbers project-wide may not be worth the hassle. Andrew noted that the agent usually catches magic numbers anyway. Yurii will compile the list of instances and send it to Chuck, and the team will identify the others that are similar before deciding how to handle them. _(Open, ~00:02:17 to 00:04:03)_

- **Dependency vulnerabilities:** Karol reported about 20 high-severity findings that remain even though the packages are current. The findings trace to the semantic-release toolchain and http-proxy-middleware, which Andrew said is what he uses locally to reach QA1. Policy: fix high and critical findings and treat low and medium as non-blocking. Replacing a package is acceptable if needed. Karol makes a best-effort attempt on the current ticket and spawns a backlog ticket for larger replacements. _(Decided, ~00:04:44 to 00:06:32)_

- **Whiz gate status:** Chuck and Andrew reviewed the Whiz scan on Karol's latest PR and found no findings. The team concluded the change is clean. Whiz has not blocked any merges from these findings, which suggests the findings sit in dev dependencies or have exceptions. The team will treat the current state as acceptable unless Whiz starts flagging them. _(Concluded, ~00:06:32 to 00:11:50)_

- **Sonar Vortex and local setup:** Sonar Vortex pre-checks code against quality gates and reference architecture before push. Chuck said the team has it enabled. Sonar CLI and the MCP server should be set up locally so the agent catches issues before push. Ivan needs Sonar access, which is requested through Conductor One, and Chuck will approve it. Chuck will also post the setup link in Slack. _(Action, ~00:09:32 to 00:10:14)_

- **Conversion page not loading in QA (OBS-631):** Miroslav picked up this urgent bug and plans to submit a PR soon. Andrew asked him to confirm that all pages, including manage projects, deliverables, and work orders, are registered in the chart-script templates mapping. Missing mappings block QA access. He also asked that code links and routes point to the new destinations. Andrew expects review tomorrow because Andriy's templates PR is still in the queue. _(Action, ~00:07:18 to 00:08:49)_

- **OBS-593 done:** Miroslav added the provider-matching flags to the experimentation repository for core-ui tickets. _(Completed; Linear shows Done 2026-10-15)_

- **Virtualization ticket status:** Ivan's virtualization ticket (OBS-629) is in review, and Linear marks OBS-584 (disappearing column headers) Done on 2026-10-15. See the WWT Phase 1 daily notes for the testing status of OBS-584.

---

## Action Items

- **Yurii Dmytriiev:** Review comments on and merge the enabled layers quality-gate PR.
- **Yurii Dmytriiev:** Fix the axios speed-dependency issue (OBS-577).
- **Yurii Dmytriiev:** Compile the list of Sonar magic-number false positives and send it to Chuck.
- **Ivan Sakhman:** Request Sonar access via Conductor One.
- **Ivan Sakhman:** Install the Sonar CLI and set up the MCP locally.
- **Ivan Sakhman:** Continue virtualization removal on the main Flightboard (columns and rows, excluding project Flightboard).
- **Ivan Sakhman:** Close backend bug fixes directly. Request review for customer-visible UI fixes. Ask in the ticket if unsure.
- **Chuck Haines:** Approve Ivan's Sonar access request when submitted.
- **Chuck Haines:** Post the Sonar Vortex and CLI setup link in Slack. Also locate and post the Slack thread about Sonar Vortex that Andrew asked for.
- **Chuck Haines:** Review Yurii's magic-number list when it arrives.
- **Karol Tluscik:** Discuss the security dependency findings with Yurii and settle remediation strategy.
- **Karol Tluscik:** Best-effort fix on the current ticket. Spawn a backlog ticket for larger replacements such as http-proxy-middleware.
- **Miroslav Isikiyski:** Submit the OBS-631 PR as soon as possible.
- **Miroslav Isikiyski:** Verify manage projects, deliverables, and work orders are registered in the chart-script templates mapping. Update code links and routes to the new destinations. (Implied owner for the routes update, confirmed by Andrew's instruction.)
- **Andrew Kandels (implied):** Review the OBS-631 PR tomorrow if it lands.
