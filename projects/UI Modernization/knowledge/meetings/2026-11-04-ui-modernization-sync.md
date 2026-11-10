# UI Modernization Sync, 2026-11-04

**Date & Time:** Wednesday, November 4, 2026, 7:44 CST (~17 minutes)

**Attendees:**
- Andrew Kandels, Engineering lead, day-to-day technical lead (meeting organizer)
- Chuck Haines, Sr. Principal Software Engineer
- Yurii Dmytriiev, Contractor, Frontend Engineer
- Ivan Sakhman, Contractor, Frontend Engineer
- Andriy Kurenda, Contractor, Frontend Engineer
- Isikiyski Miroslav, Backend Engineer
- Karol Tluscik, Joining full-time this quarter (role and title still to confirm)
- Invited, did not speak: Laura Quist (Sr. Customer Success Executive, optional), Maria Saxton (Linear project lead, tentative), Fayshal Uddin (reviews/releases core-ui)

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1wwcAvBlFNwEH9UQ3HWGBmAytOWFTYxAcx04pRAzee4M/edit?usp=drivesdk&ouid=104527139387599317887) (Gemini notes and transcript; transcript is computer generated and may contain errors)

---

## Overview

The sync was mostly status, with one meaningful announcement at the end. Karol has a draft PR for the provider card (PSS column, popovers, and related components) and expects to submit it for review today or tomorrow. This is one of the first features moving from Provider Match into core-ui, and Karol reported the AI agent mapped most of the move well with some clarifications needed. Andriy is working on the providers page, with the controls section already in review, and has a large PR up that he asked the group to review. Ivan is still fixing comments and pipeline failures on his in-progress work. Miroslav tested and deployed the data key change, and he plans to finish and submit the elastic list project-pass PR for review today. Andrew kicked off the navigation change early in the morning, and it should be released shortly.

Test health was the second theme. Yurii is nearly done fixing intent (test) failures by tagging failing tests and batching fixes, and he needs one more full coverage run because some tests fail intermittently from server capacity. He also plans to cut test shards from about 15 to 10, since each shard takes about two minutes. Andrew reported that E2E tests on the Flightboard now need about 50 percent fewer retries than on the old UI, which he attributes to better loading-state handling in core-ui. Yurii flagged that around three tests may need quarantine because filters on the main Flightboard appear to have new bugs that he is verifying manually. The group also discussed the tension between running E2E suites before every core-ui release and the time that costs. The E2E run caught a contract change that had been broken for about a week, so there is a reason to keep it, but the full and critical suites are slow.

Chuck closed the sync by asking the group to start running their work through Warp Factories, the software factory tooling he has been building for about a week and a half. The team agreed to start using it to stress-test the tooling and find where it fails. Chuck explained that a Linear ticket assigned to Warp starts a run that gathers requirements (asking for more detail in Slack DM if needed), addresses Sonar and other scanner findings, and sends a PR for human review with a video of the PR tested against a mock backend. The stated long-term goal is for all FieldNation software engineering to use the factory, and the team plans to build a UI agent that logs into QA environments and records test videos.

---

## Key Notes

- **Adoption of Warp Factories (decision):** The team aligned on running development work through Warp Factories to stress-test and evaluate it. Chuck is sending invites and offered a demo. Usage is currently on unlimited tokens during a proof-of-concept period. Chuck wants feedback on where runs fail, what works, and where humans need to step in. He also noted that asking the factory why a run failed produces a self-improvement PR in the factory repo for his review. Every change is supposed to have a Linear ticket, and the factory creates one if none exists.

- **Factory capabilities:** Automations sync AI toolbox skills and FN engineering standards into the factory. Custom agents are supported, and Chuck has one that audits repos for agent-friendliness. In a stress test, the factory correctly moved a core-ui ticket to the right repo (FN core) without being told, which Chuck cited as evidence it can infer target repositories. A planned UI agent would log into QA with test accounts, click through flows, and record videos. Chuck noted that a deploy-to-QA step is not yet wired up, and the factory asked him which environment to deploy to during one run.

- **Provider card migration:** Karol has a draft PR for the provider card (PSS popovers and components, tracked as OBS-601). Testing is still in progress because of the amount of code. Andrew asked whether the agent could map locations correctly when moving features from Match, and Karol said it worked well with some clarifications needed.

- **Providers page and large PR:** Andriy reported the providers page controls are in review. He is building the page itself and pulling in teammates' PRs locally, with Ivan helping in parallel. The large PR he posted needs review. Ivan's parallel work (horizon) is still in progress, and the group agreed Andriy can continue on the providers page for now.

- **Data key and elastic list:** Miroslav tested and merged the data key that Andrew provided, then deployed it. He is finishing the elastic list project-pass PR, which took longer than expected because of local test coverage. He plans to submit it for review today.

- **Navigation change:** Andrew kicked off the navigation change early today and expects it to release shortly. The redirect fix (OBS-644) was closed as Done at 13:28 UTC today, after the 11-03 daily planned it for that afternoon.

- **Test suite optimization:** Yurii is almost done fixing intent failures. He needs one more full coverage run to confirm, because server capacity can make some tests fail intermittently. He is also preparing a PR to reduce test shards from about 15 to 10, since each shard takes about two minutes and the full set is more than needed. Further work is planned on TypeScript optimization and speeding up Ox lint. Yurii prepared a list of core-ui tickets that move to the Ox format.

- **E2E flakiness and loading states:** Andrew observed that E2E tests on the Flightboard need about 50 percent fewer retries than on the old UI. His explanation is that old pages load data and then fetch more without retrying, while core-ui has explicit loading states that prevent the race. He expects flakiness to shift as more pages move over.

- **Main Flightboard filter bugs:** Yurii found that filters on the main Flightboard do not work correctly, which surfaced as failing E2E tests. He thinks these are new bugs rather than stale tests, and he may quarantine around three tests while he verifies them manually and gives feedback. This is consistent with the 11-03 daily note about filter issues, though the 11-03 notes say a filter fix was approved and merged.

- **E2E gate versus release timing (open):** Andrew and Yurii discussed how much E2E coverage to run before each core-ui release. The current UI guidance is to run E2E before deploying. The core-ui suites take too long to run at every release, but the last run caught a contract change that had been broken for about a week. Andrew said the group needs to find a balance, but no decision was made.

---

## Action Items

- **Karol Tluscik:** Submit the provider card PR (PSS popovers and components) for code review. Expected today or tomorrow morning.
- **Andriy Kurenda:** Keep building the providers page, and request review for the large providers page PR.
- **The group:** Review the large providers page PR posted by Andriy.
- **Ivan Sakhman:** Fix comments and pipeline failures on the active PR (horizon).
- **Isikiyski Miroslav:** Finalize and submit the elastic list project-pass PR for code review (stated as today).
- **Yurii Dmytriiev:** Rerun the full test coverage suite to confirm intent fixes.
- **Yurii Dmytriiev:** Prepare a PR to reduce test shards from about 15 to 10.
- **Yurii Dmytriiev:** Manually verify the detected main Flightboard filter bugs and give feedback. Decide whether about three tests go into quarantine.
- **Chuck Haines:** Send Warp Factories onboarding invites to the group. Offered a live demo if wanted.
- **Chuck Haines (implied):** Resolve conflicts in the open PR. Andrew asked for this during the sync.
- **The group:** Start running work through Warp Factories as time allows, and report what works and what fails.
