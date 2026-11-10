# Team Moissanite: Daily Standup — 2026-09-15

**Date & Time:** Tuesday, September 15, 2026 (~54 minutes)

**Attendees:**
- Chuck Haines — Sr. Principal Software Engineer, Architecture Lead (meeting organizer)
- Asad Ali — Contractor, demoed the ADR playbook/skills
- Vitalii Belinskyi — Contractor, ai-toolbox infrastructure
- Mykhailo Sakhnik — Contractor, repo-readiness toolchain
- Joe Hoppesch — Principal Engineering Consultant (invited; no lines attributed to him in the transcript, so he likely didn't join)
- Redwan Shahzada — struck through on the invite list, consistent with having rolled off AI-DLC as of 2026-07-14

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/1LMoOEBfE_G_DRyW8TA-ENvzoeNCPblNSzY_fydLPL-I/edit)

---

## Overview

This standup covered three threads: the repo scorecard and code-review rollout, CI/CD status checks, and a new ADR (architecture decision record) tooling demo from Asad. Mykhailo reported the repo scorecard template is now at roughly 100% with all Copilot-specific checks stripped out, since Copilot is no longer part of the code-review evaluation; Chuck confirmed the scorecard is in good shape, and the next phase is working through the 100 target repositories he'd identified, tracked in a spreadsheet he was setting up that day. Vitalii merged the harness PR Joe had requested and clarified that the "red validate run" check correctly signals a broken pipeline when conditions aren't met; Chuck asked him to clear the outdated comments on the (already-approved) Kafka PR so it can merge once checks go green.

The rest of the meeting was Asad's demo of a proposed ADR playbook and three new Claude skills — create, enforce, and review — meant to generate, check, and maintain architecture decision records inside repos. Chuck decided not to fold ADR generation into the general repo scorecard yet, since not every repo will have a reason to produce one, and asked Asad to open a draft PR so the team could look at the skills more closely before deciding how many are actually needed. After Chuck dropped off around the 24-minute mark, Mykhailo and Asad spent the rest of the call debating whether enforce and review are separable from create, and whether a newer ADR should outrank an older one in a conflict — Asad's position is that the whole mechanism is an audit log, not an automated priority system, and any precedence call is a human one. Both questions were left open for Slack discussion. Separately, Chuck confirmed Copilot code reviews are now off for new PRs, with Gitar (referred to as "Guitar" throughout the transcript) as the sole review tool going forward, which resolved friction Asad had been having with Copilot re-flagging issues one at a time instead of all at once.

---

## Key Notes

- **Repo scorecard near-complete, Copilot references removed:** Mykhailo updated the scorecard template, removed Copilot-specific checks now that Copilot isn't part of the review process, and plans to run it across multiple repos next, followed by a harness-vs-baseline comparison.

- **100-repo rollout is the next big push:** Chuck identified the 100 highest-priority repos and was setting up a tracking spreadsheet the same day; the plan is to work through the list as fast as possible.

- **Harness PR merged, red validate-run check fixed:** Vitalii merged the harness PR Joe requested and finished the repo-SDK bootstrap work (initializes OpenSpec in a repo, generates ticket specs). He confirmed the "red validate run" check correctly flags a broken pipeline, and the earlier flakiness there is resolved.

- **Kafka PR pending cleanup:** Chuck has already approved it; Vitalii still needs to resolve outdated review comments before it merges once checks are green.

- **Copilot code reviews off, Gitar is now the sole tool:** Chuck turned off Copilot code review on new PRs org-wide, which directly resolved Asad's complaint that Copilot kept surfacing new issues iteratively rather than all at once.

- **ADR playbook and three-skill design demoed:** Asad walked through a playbook (in the repo-scorecard repo) with example ADR templates and a numbering/versioning scheme, plus three Claude skills — create, enforce, review — installed the same way as the existing fn-api-design skills. Create generates an ADR from ticket/prompt context and decisions made during implementation; enforce checks a new decision against existing ADRs for conflicts before generation; review validates changes against relevant ADRs and manages expiration/re-review dates so records don't accumulate indefinitely.

- **ADRs are meant to be immutable; conflicts resolve via suppression, not editing:** when a new decision conflicts with an accepted one, the older ADR gets marked "suppressed" by the new one, with rationale logged in both records — an audit trail, not an automatic override. Asad and Chuck agreed the immutability enforcement still needs verification (a revert Asad attempted appeared to take effect when it shouldn't have).

- **ADR generation stays out of the repo scorecard for now:** Chuck's call — not every repo has a reason to generate an ADR yet, so he doesn't want an empty ADR folder read as a deficiency. He wants to understand how the three skills interact before deciding whether one, two, or three are actually needed.

- **Skill count (1 vs. 2 vs. 3) is unresolved:** Mykhailo argued create and enforce feel too similar and that review might fold into one of the other two; Asad's take is create and enforce could combine, but review (triggered later, on expiration/re-review and after human approval) should stay separate. No resolution — Asad planned to move the discussion to Slack.

- **Conflict precedence between old and new ADRs is an open question:** Mykhailo suggested the more recently created ADR should carry more weight in a conflict, since it reflects newer code; Asad maintained there's no built-in priority — the agent can flag a conflict, but a human decides which decision wins, and the system just logs who chose what and why.

- **Next step on ADR work:** Asad will open a draft PR (starting in repo-scorecard, moving to ai-toolbox later) so Vitalii and Mykhailo can review the skills together and give first-round feedback before it merges; Chuck will pull it locally and test it himself once it's pushed, as he does with any new skill.

---

## Action Items

- **Chuck Haines:** Review the "red validate run" status check behavior on the harness PR.
- **Vitalii Belinskyi:** Resolve outdated comments on the Kafka PR so it can merge once checks are green.
- **Asad Ali:** Open a draft PR for the ADR skills (repo-scorecard first, ai-toolbox later) for iterative feedback.
- **Asad Ali, Mykhailo Sakhnik, Vitalii Belinskyi:** Connect to review the ADR skills together and decide how many skills the workflow needs.
- **Chuck Haines:** Pull and test the ADR skills locally once Asad's PR is up.
- **Asad Ali:** Post the create/enforce/review consolidation question and the conflict-priority question in team chat for discussion.
