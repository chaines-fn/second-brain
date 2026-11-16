# UI Modernization Sync: 2026-11-12

**Date & Time:** Thursday, November 12, 2026, 7:45 CST (~9 minutes)

**Attendees:**
- Andrew Kandels: Engineering lead, organizer (invited, no speaking turns recorded)
- Chuck Haines: Sr. Principal Software Engineer
- Yurii Dmytriiev: Contractor, Frontend Engineer
- Ivan Sakhman: Contractor, Frontend Engineer
- Andriy Kurenda: Contractor, Frontend Engineer
- Miroslav Isikiyski: Backend Engineer
- Karol Tluscik: Frontend Engineer, joining full-time this quarter
- Tashfiqul Islam: New to the team this morning; will coordinate and execute legacy (classic page) migration tasks
- Invited, no speaking turns recorded: Laura Quist, Maria Saxton, Fayshal Uddin

**Original Transcript:** [Notes by Gemini](https://docs.google.com/document/d/105uur4erYQA5ljd70vOLv1Zq_HIXS3E0wq8jh4K-n94/edit?usp=drivesdk&ouid=104527139387599317887)

---

## Overview

The team reported on status and hit a blocker: a login problem on the local proxy server, with a new two-step login page that drops a session cookie the local proxy does not have. Chuck believes the cause is server secrets rotated recently without rotating the matching secrets in the Core UI layer, so the proxy cannot pass its own session. He committed to identifying which secrets need rotation and sending them to the team after the meeting, and to recording them in the tracker sheet that Tashfiqul suggested. Karol is working around it by manually switching cookies.

The meeting also marked a strategic pivot. Chuck introduced Tashfiqul Islam, who is joining to help accelerate migration of classic pages (old Angular and PHP pages) into Core UI. Status items included the global bookmarks work for filters (Andriy, OBS-703, DPR created), the BSS flightboard efficiency deploy (Miroslav, deployed and smoke-tested), and a missing relative date option on project fraud filtering (Karol, to be reworked).

---

## Key Notes

- **Login failures from unrotated secrets:** Chuck's working theory is that secret rotations on the servers were not carried over to the UI layer in Core UI. Yurii saw an extra login step and a missing session cookie locally. Chuck said he is not aware of a reason for the double login beyond this, and that he is "pretty confident" it is caused by the rotation. Action on Chuck.
- **Karol's workaround:** Karol was blocked in the morning until Yurii suggested manually switching cookies. Karol will continue that way for now. Separately, Karol posted in channel that the QA1 proxy returns 403 on the roles endpoint for buyer.admin even though it works locally.
- **Classic page migration pivot:** Chuck said the team will shift focus to accelerating migration of classic pages (Angular and PHP) to Core UI. Tashfiqul will coordinate and do some of the work himself, and will join standups. This follows Andrew's two-track plan discussed on 2026-11-11.
- **BSS flightboard efficiency:** Miroslav reported the work is complete, deployed, smoke-tested, and synced with the frontend, with results "better than expected." (Gemini transcript calls it "BSS"; on 11-11 it was called "slideboard efficiency.")
- **Problem filter (provider-only, ticket 706):** Miroslav is working on the problem filter and expects to submit a PR today. He is blocked on an IAM user for OpenSearch and will sync with Andriy, who discussed it with him in Slack.
- **Global bookmarks for filters:** Andriy finished OBS-703, created a DPR, and has questions for Laura.
- **Relative date options (project fraud filtering):** Maria commented that relative date options are still missing in one location. Karol will rework that section.
- **E2E and CI:** Yurii needs to finish the end-to-end testing PR so it can merge, prepare a smaller PR with three test fixes, and review old CI improvement tickets.
- **PSS column:** Ivan reviewed a PR for the PSS column from the mirror and will finish it.

---

## Action Items

- **Chuck Haines:** Track down the secrets that need rotation to fix the proxy login issue and send them to the team as soon as the meeting ends.
- **Chuck Haines:** Add the rotated secrets to the tracker sheet, as suggested by Tashfiqul Islam.
- **Yurii Dmytriiev:** Finish the end-to-end testing PR so it is ready to merge.
- **Yurii Dmytriiev:** Prepare a smaller PR containing three test fixes.
- **Yurii Dmytriiev:** Review the old CI improvement tickets to check past action items.
- **Ivan Sakhman:** Finish the PR for the BSS column from the mirror.
- **Miroslav Isikiyski:** Submit the PR for the problem filter today, after syncing with Andriy on the OpenSearch IAM user.
- **Karol Tluscik:** Rework project fraud filtering to add relative date options where Maria said they are still missing.
- **Andriy Kurenda (implied):** Follow up with Laura on the questions raised for the OBS-703 work. Also has pending PRs waiting for Chuck's review.
