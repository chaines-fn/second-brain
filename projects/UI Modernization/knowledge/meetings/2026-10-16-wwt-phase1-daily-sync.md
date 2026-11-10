# WWT Phase 1 Daily Sync - 2026-10-16

**Date & Time:** Friday, October 16, 2026, 8:45 CDT (calendar: "Obsidian Daily Sync + Stakeholders")

**Organizer:** Maria Saxton (OOO 10-15 to 10-19; slot kept on the calendar for team use)

**Attendees per Gemini notes:** Yurii Dmytriiev (invited), Andriy Kurenda, Ivan Sakhman, Todd Folie, Andrew Kandels, Chuck Haines, Karol Tluscik. Maria Saxton and Laura Quist declined.

**Notes Source:** [Gemini notes, Google Doc](https://docs.google.com/document/d/1-fiCqcvxCE7ktnsB2UcFseXUSMn571DR0qt_rGKzi8A/edit?usp=drivesdk&ouid=104527139387599317887)

**Context:** The 7:45 CDT UI Modernization Sync was not held today because of a technical design review. Andrew posted in #proj-ui-modernization that the team could connect at 8:45 instead.

---

## Summary

Project handoff status, version tagging, Sonar configuration, analytics migration, and a mobile modal issue. Andrew ran the meeting.

## Key notes

- **Project handoff:** Todd tested 3 PRs successfully and Ivan closed them. Andrew said about 15 PRs are still pending review. The project handoff also waits on a routes PR. Gemini records the author as "Mera," which is unverified.
- **Testability target:** Andrew aims to make the project testable by next Monday (2026-10-19), contingent on PR approvals.
- **MUI version tagging:** Andriy flagged that the MUI 9.1 version tag release triggered updates across multiple repos. Andrew asked Andriy to update the UI repo to 9.1 and to notify Travis Lang, who manages the long-term engagements repo and is waiting on 9.1.
- **Sonar:** Yurii asked how to scan the whole repo rather than only modified files. Andrew said access comes through Conductor One and that the Sonar CLI can scan the core repo locally. Yurii will run a full-repo scan.
- **Analytics (OBS-594):** Karol asked about schema and Amplitude event differences when moving Provider Match analytics to core-ui. Provider Match sends events to Appcues and core-ui does not. Decision: keep one-for-one event naming with the previous setup so existing reports keep working.
- **Mobile edit schedule modal (OBS-634):** Ivan could not reproduce it on emulators or several browsers. Andrew suspects the production Salesforce chat icon in the bottom right covers the mobile Save button. Andrew will ask Sarah (likely Sarah Welter, CS, unconfirmed) to have the customer drag the chat icon to confirm.

## Action items

- **Andrew Kandels:** Make the project testable by next Monday, contingent on PR approvals.
- **Andriy Kurenda:** Update the UI repo to MUI 9.1. Notify Travis Lang about the 9.1 release.
- **Yurii Dmytriiev:** Run a full-repo Sonar scan with the CLI instead of modified-files-only.
- **Andrew Kandels:** Ask Sarah to confirm the chat-icon overlap with the customer on mobile.

## Linear status noted during the workspace sync (2026-10-16)

- OBS-572 (buyer redirects and /flightboard URL, Andrew) Done 10-15.
- OBS-629 (remove virtualization render on main FB, Ivan, Urgent) Done 10-15.
- OBS-585 (Today filter when saved, Ivan, Urgent), OBS-587 (Condition "contains" operator, Ivan, Urgent), and OBS-553 (auto-dispatch filter on project FB, Ivan, Urgent) Done 10-16.
- OBS-631 (new project conversion not loading on QA pages, Miroslav) In Review.
- OBS-632 (favicon cache persisting old logo, Miroslav, Urgent) In Progress, under the Q4 '26 Field Nation Brand Refresh project.
- OBS-637 (elasticlist warnings: project FB query returned exactly 10,000 results for company 53552, project 77808, possible OpenSearch truncation, Andrew, Low) created 10-16.
- OBS-638 (PSS on flightboard efficiency: project FB needs IN() cross-queries against OpenSearch for 10k rows per view, Andrew, High) created 10-16.

## Open items

- Whether the exactly-10,000-row result on project 77808 is silent truncation or a warning-only artifact. Andrew will need to confirm. Unknown as of this sync.
</content>
