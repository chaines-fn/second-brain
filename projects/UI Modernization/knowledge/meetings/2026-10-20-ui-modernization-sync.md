# UI Modernization Sync - 2026-10-20

**Date & Time:** Tuesday, October 20, 2026, 7:45 CDT (~8 minutes of recorded transcript; calendar slot runs to 8:00)

**Attendees:**
- Andrew Kandels - Engineering lead, day-to-day technical lead (meeting organizer)
- Chuck Haines - Sr. Principal Software Engineer
- Yurii Dmytriiev - Contractor, Frontend Engineer
- Ivan Sakhman - Contractor, Frontend Engineer
- Miroslav Isikiyski - Backend Engineer (joined briefly; cut short by a home emergency, see Key Notes)
- Andriy Kurenda - Contractor, Frontend Engineer
- Karol Tluscik - Joining full-time this quarter; analytics and notifications work
- Maria Saxton - Invited, tentative (not in transcript)
- Fayshal Uddin - Invited (not in transcript)
- Laura Quist - Optional invitee, no response recorded (not in transcript)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1NrotxlNKCZvTMOgSqrl-JWvMJj93T3wmcPrxsHqpIYw/edit)

---

## Overview

This was a short status round ahead of the refinement session that followed. The main news was that the MUI 9.1 release completed without reported blockers, and that the favicon cache fix (which stops browsers showing the old logo) was submitted for review. Andriy confirmed two downstream repos, the UI repo and the long-term engagement repo, need manual updates to pick up 9.1; the others update automatically. Andriy is also continuing project Flightboard pagination.

Feature work is moving through review. Karol finished the analytics portion for the Provider Match port and submitted it with screenshots, and is finishing notifications before taking a new task. Yurii and Ivan are both working through Sonar cleanup. Yurii will use Storybook to verify components in the MUI "muya" library, and Ivan has a payment method component PR waiting on pipelines.

Two process decisions came out of the round. The UI guild persist-method review requirement was removed from the notification tickets, and those changes will be reviewed directly in the pull request. Andrew said AI tooling has been adding guild review requirements to tickets, and he strips them out. Miroslav raised an urgent bug where the Flightboard menu still redirects, plus a project listing ticket that links to the old project page. He planned to take the listing ticket after the urgent fix.

---

## Key Notes

- **MUI 9.1 release:** Andriy confirmed the release completed successfully. The UI repo and the long-term engagement repo need manual updates. Andrew had previously asked Andriy to tell the affected teams when to update. Andriy also needs to update the icons cache and open a separate PR for it, which corresponds to Miroslav's favicon change.

- **Favicon cache fix:** Miroslav fixed the favicon cache that was showing the old logo and submitted it for code review. This is tracked as OBS-632 in Linear.

- **Redirect and project listing bugs:** Miroslav reported an urgent ticket where the Flightboard still redirects (tracked as OBS-644 in Linear). He also found a project listing ticket that links to the old page (OBS-643 in Linear). He will fix the redirect first, then the listing links.

- **Miroslav left early:** Miroslav could not join the refinement session because of a home emergency and asked Andrew to pass along apologies. Andrew added Karol to the refinement calendar at Miroslav's request. Karol had not been on that calendar before.

- **Analytics for Provider Match:** Karol finalized the analytics portion, submitted it for review with screenshots, and is finishing notifications before picking up a new task. This tracks to OBS-594.

- **Guild review requirement removed from notification tickets:** Karol asked about a ticket line requiring agreement with the UI guild on a persist method for notifications. Chuck and Andrew both serve on the UI guild. Andrew said AI tooling kept adding guild review requirements to tickets, and he strips them out because they are representative of the guild, not of every ticket. The group agreed to remove the requirement and review the persist method directly in the PR.

- **Sonar work:** Yurii is continuing Sonar fixes. His next ticket is Sonar cleanup for the muya library, and he asked how to test it. Andrew said Storybook is the best option because the team has been consistent about adding components to it. Yurii will run small tests in Storybook to confirm components render correctly. Ivan is cleaning up Sonar issues in the payment method components. He raised a PR and will request review once pipelines pass.

- **Mobile button priority:** Ivan asked about a button ticket discussed the previous day. It is in progress. Andrew said he does not think it is high priority because the user can work around it with the Enter key. Andrew said almost everything on the customer's list is resolved except this item, and he will sync with Sarah to confirm whether any further action is needed. The ticket identity is not stated in the transcript.

- **Andriy's current work:** Andriy is working on project Flightboard pagination (tracked as OBS-558 in Linear per the overview file) and will do the icons cache PR after that.

- **Refinement follow-up:** Andrew said the refinement session would focus on Provider Match tickets and some larger feature-based tickets.

---

## Action Items

- **Andriy Kurenda:** Manually update the UI repo and the long-term engagement repo to MUI 9.1. Notify the affected teams. Open a separate PR for the icons cache update that matches Miroslav's favicon fix.
- **Andriy Kurenda:** Continue project Flightboard pagination.
- **Miroslav Isikiyski:** Fix the urgent Flightboard redirect bug, then update project listings so they link to the correct page. Favicon cache fix is awaiting code review.
- **Yurii Dmytriiev:** Fix Sonar issues in the muya library. Verify component rendering in Storybook.
- **Ivan Sakhman:** Submit the payment method component PR for review once pipelines finish. Pick up the next task.
- **Karol Tluscik:** Finish notifications work and submit it. Review of the analytics PR is pending. Pick up the next task after notifications.
- **Andrew Kandels:** Sync with Sarah on the mobile button ticket priority and whether further action is needed.
- **Andrew Kandels (implied):** Confirm the redirect and listing fixes are picked up in the rollout plan.
