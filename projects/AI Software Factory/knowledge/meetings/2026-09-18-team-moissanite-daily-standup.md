# Team Moissanite: Daily Standup — 2026-09-18

**Date & Time:** Thursday, September 18, 2026 (morning DM + call)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead
- Mykhailo Sakhnik — Contractor, repo-readiness toolchain + packaging
- Joe Hoppesch — Engineering lead (Harness support)

**Primary Channel:** DM thread (Chuck + Mykhailo), #fieldnation-sonar (Sonar integration Q&A)

---

## Overview

Chuck and Mykhailo discussed progress on MOI-118/119 (ai-repo-readiness CLI packaging), with a focus on Apple Developer ID signing requirements for macOS release automation. Chuck confirmed Field Nation has an existing Apple developer account (used for the mobile app) and offered to source the necessary credentials in real-time. Joe Hoppesch completed setup of a Gitar service account in Harness, unblocking CI integration. Chuck also conducted a live demo call with Mykhailo on Gitar code-review capabilities, validating the tool's agent-friendly output format.

---

## Key Notes

### Packaging & macOS Release Work (MOI-118, MOI-119, MOI-124)

- **MOI-118 (Package ai-repo-readiness standalone CLI)** — implementation complete locally; PR #14 in `ai-repo-score` repo needs review/merge to enable UI-based runs.

- **MOI-119 (Add macOS packaging CI)** — now In Review; first part (local signing validation) complete in PR comments per Mykhailo.

- **MOI-124 (Add macOS Developer ID signing and notarization)** — new ticket, critical path for release automation. Chuck immediately offered to pull Field Nation's existing Apple Developer credentials (used for the mobile app team) and set up as GitHub Actions secrets. Mykhailo clarified he doesn't need the raw credentials directly — ideally whoever owns FN's Apple signing infrastructure can add the Developer ID certificate/private key + notarization credentials as GitHub Actions secrets, and Mykhailo will update the workflow to consume them. Mykhailo will send the exact secret names and format needed.

- **Workflow next step:** Merge PR #14 (UI runner) first, then Mykhailo will set up Workflow Dispatch for manual releases once the Apple secrets are in place.

### Gitar Code-Review Validation Call

- Chuck started a Google Meet (`meet.google.com/syz-gwzd-bfy`) to show Mykhailo live Gitar capabilities. Mykhailo's reaction: *"gitar really provides review like human!"* and *"it gives the prompt for an agent!"* — genuine enthusiasm for the review quality and output format's suitability for downstream agents.

- This is the first real-world validation from a team member (not just Chuck's solo assessment) that Gitar's output is substantive and agent-ready, a major signal for the open O1 code-review decision.

### Harness Service Account & CI Integration

- Chuck asked Joe whether he has Harness admin access to create a service account for Gitar. Joe confirmed he can access Harness and set it up.

- Joe created a `Gitar` service account in Harness and stored the credentials in 1Password (`start.1password.com/open/i?a=7SQ3GK6VWFBTBBUGJWMWCEDG54&v=jahzrdpx2jmvfwljc3icyyxdsa&i=fx6dabicvnfq43kz3k5wi4fjei&h=fieldnation-team.1password.com`). Chuck confirmed the credentials validated.

- This unblocks Gitar's integration into Field Nation's CI pipeline (previously scoped as a trial against real PRs; the service account now enables broader testing).

### Sonar Integration & Enhancements

- Chuck asked Gautam Korlam (Sonar) about a risk-assessment dashboard showing risk per PR, so the team can review and adjust risk thresholds as needed. Gautam committed to adding it and surfacing it in multiple places.

- Chuck also asked whether the Atlassian connector (if connected to Confluence) could feed Sonar's findings into code reviews, using TDR/design context as guidance during reviews. Gautam confirmed Atlassian connector can do this; it's currently in Sonar's internal dogfood and should be available to FN next week.

### Personnel & Capacity Notes

- **Vitalii's availability:** Continues working compressed hours (evenings/nights) for family care (grandmother's Parkinson's flare-up). No timeline impact reported; team remains supportive.

- **Joe's capacity:** Joe may pull Vitalii next week for a separate project, but indicated willingness to split time on AI-DLC items if needed. Chuck agreed Vitalii can split his attention.

---

## Action Items

- **Mykhailo Sakhnik:** 
  - Prepare exact GitHub Actions secret names/format needed for Apple Developer ID + notarization credentials.
  - Merge PR #14 (`ai-repo-score`, UI runner) once approved.
  - Set up Workflow Dispatch for manual releases once Apple secrets are available.

- **Chuck Haines:**
  - Coordinate with Field Nation's Apple Developer account owner to source Developer ID certificate/private key + notarization credentials and set them up as GitHub Actions secrets in `ai-repo-score` repo.
  - Review and approve PR #14.

- **Joe Hoppesch:**
  - Service account created and in 1Password — Gitar CI integration can proceed.

- **Sonar (Gautam Korlam):**
  - Add risk-assessment dashboard per PR (in progress).
  - Atlassian Confluence connector support (available next week).

---

## Open Questions / Blockers

- **Apple credentials setup:** Timing depends on whoever owns FN's Apple signing infrastructure getting the credentials into GitHub Actions. Chuck is driving this directly.

- **PR #14 review/merge:** Currently awaiting sign-off. Once merged, Mykhailo can proceed with workflow setup.

---

## Signal Summary

**High-confidence signals for workspace update:**
1. **Gitar validation:** First non-Chuck independent assessment confirms agent-friendly review output; material evidence for O1 code-review decision.
2. **Harness integration:** Service account ready; CI integration pathway cleared.
3. **Sonar enhancements:** Risk dashboard + Confluence integration both in motion, unblocking code-review guidance from design docs.
4. **Packaging progress:** Apple signing requirement identified and sourcing plan in place; no timeline slip expected.

---

*This standup captures the key developments from morning DMs and the live Gitar validation call. Format mirrors the daily standup structure.*
