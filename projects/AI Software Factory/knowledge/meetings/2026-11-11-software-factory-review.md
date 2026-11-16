# Software Factory Review, 2026-11-11

**Date & Time:** Wednesday, November 11, 2026, 9:30 CST (recording ~46 minutes; transcript ends at 00:46:12)
**Organizer:** Chuck Haines (recurring meeting, created 2026-11-03; described as a touchpoint to review factory progress, share learnings, and get feedback)
**Original notes:** [Software Factory Review - 2026/11/11 Notes by Gemini](https://docs.google.com/document/d/1khKUYI2UAS7AeUKhTW2kgivXnf58JULuZy6nu3zXk0Y/edit)

**Invited (per calendar):** Andrew Kandels, Atiqul Haque, Ben Moormeier, Brian Chapeau, Chuck Haines, Cory Muir, Ehteshamul Tamvir, Ian Loverink, Jasim Alam, Redwan Shahzada, Spencer Moulton, Travis Emslander, Joe Hoppesch. Mike Mueller declined (conflict).
**Spoke in transcript:** Chuck Haines, Redwan Shahzada, Ehteshamul Tamvir, Brian Chapeau, Joe Hoppesch, Andrew Kandels, Travis Emslander, Jasim Alam, Spencer Moulton, Ian Loverink.

---

## Overview

Chuck opened by saying the factory has produced code that went to production, and he presented Warp as the likely go-forward platform for the factory and for local agentic development. He walked through the factory's triage, spec, implement, and review stages, the cost and telemetry view, and the Warp configuration model. Most of the discussion after the demo was about data access and SOC 2 for agent runners, how agent evaluation should work, and the planned Warp blueprint feature.

The Gemini summary says Warp was "selected." The transcript says Chuck called it "likely" the go-forward solution, so this note treats it as his working direction, not a recorded leadership decision.

## Key Points

- **Production result (Chuck's claim):** The factory has shipped code to production. Two FN Core PRs produced by the factory were under Chuck's review at the time. No independent verification in this note.
- **Platform:** Warp is the likely go-forward factory and local agent platform. Chuck said its configuration-driven model (prompts and YAML in GitHub) is similar to GitHub Actions. He also said the team's own two-week proof of concept was outpaced by Warp setup in under a day.
- **Workflow:** A triage agent asks clarifying questions until the task is clear, decides whether a spec is needed (for larger work, the spec is written in Linear for approval), then implements and opens a draft PR. The factory waits for unit tests, Sonar, and Gitar, addresses their feedback, and only then marks the PR ready for human review. Human review comments also trigger the factory.
- **SDLC enforcement:** The factory will not run work without a ticket. It can create one itself if none exists.
- **Cost (Chuck's preliminary numbers, from his own runs):** $10 to $12 per factory PR, against roughly $30 per PR for the UI modernization team. He said it was about a third of the cost of the lowest-cost person. Telemetry is planned to push into DX.
- **Model routing:** Warp's AI gateway supports a custom router that picks a model by estimated task difficulty. Chuck said he has not decided between Warp's gateway and Marcado's. Switching is a configuration change.
- **Models during POC:** The factory uses Warp-provided models on Warp infrastructure with unlimited tokens for the proof of concept. Bedrock use is possible later.
- **Blueprints:** Warp releases "blueprints" next week, a deterministic orchestration layer where agents do the work inside a fixed workflow. Chuck plans to be an early customer.
- **Per-team factories (direction, not decision):** Each team would get a factory scoped to its repositories, inheriting from a base factory with team overrides. This follows the entity topology work. Chuck said Warp's blueprints should enable this.
- **Budgets:** Budget and run caps are supported by Warp but not yet configured in the factory.
- **Evaluation:** Warp has built-in scorers and benchmarks, not yet configured. Chuck said Harness Evals stays in use for ai-toolbox skills. He does not plan to plug custom evals into the factory and expects better results from Warp's built-in tooling. Spencer Moulton argued that each agent config and skill should be evaluated in isolation before it is added to the factory. Chuck distinguished Sonar (deterministic code quality) from scorers (agent output quality).
- **Data access and compliance:** Spencer Moulton asked whether Warp is registered as a subprocessor and whether scrubbed data can run through its runners with SOC 2 coverage. Chuck said Warp is approved through the Tropic process and has SOC 2 reports, but he had no answer yet on sensitive data. Options discussed: self-hosted runners in a dedicated AWS account, or scrubbing data before it leaves. Spencer described a pseudo-anonymization method from the Pearl team that still needs infosec sign-off. Ian Loverink said a similar sanitize-and-ship approach has been discussed for eval loops.
- **Access and rollout:** Team members can request access (DM Chuck). Each person gets a scoped repository list in the factory config. Warp terminal and agent use locally also come with unlimited tokens during the POC. The POC has about two weeks left. SSO via Okta through Conductor One is planned; Chuck said he may set it up before the POC ends.
- **Known gaps:** Starting work from the web does not reliably send Slack notifications, though it does from Slack. Some telemetry and scoring data are noisy because Chuck was still building the factory during earlier runs.

## Action Items

- **Chuck Haines:** Debug why web-started tasks do not send Slack alerts.
- **Chuck Haines:** Discuss Warp subprocessor status, runner configuration, and data access with Ben Moormeier.
- **Chuck Haines:** Find out what inputs and granularity the default factory scoring runs evaluate.
- **Chuck Haines:** Send a Loom on blueprints before next week's release.
- **Chuck Haines:** Schedule a security settings review with Ian Loverink and Ben Moormeier.
- **Chuck Haines:** Connect Spencer Moulton with the Gitar-side contact (transcribed as "Gutham"; likely Gautam Korlam, unverified) and a Warp contact to discuss infrastructure and agent setup.
- **Chuck Haines:** Add interested people to the Warp factory on request.
- **Spencer Moulton:** Push for infosec approval of the data scrubbing method.

## Open Questions (from the meeting)

- Which AI gateway to use long term (Warp's or Marcado's), and whether to move model spend to Bedrock.
- Whether runners need a dedicated AWS account, and what access they would need there.
- Whether Warp counts as a registered subprocessor for sensitive customer data.
- What the scorers actually evaluate, and how much of it can be inspected.
- Whether team factories can pin a base-factory version or must take every update.

## Notes on the Source

The Gemini notes flagged several transcription errors: "Octa" (Okta), "Guitar" (Gitar), "Whiz" (Wiz), and "Skim" (unclear). Names and the vendor "Marcado" are as transcribed. The Gemini summary says "Guitar" and "Octa" as well. This note uses the corrected Okta and Gitar spellings where the context is clear.
