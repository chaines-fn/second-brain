# AI Guild Weekly — 2026-07-02

**Date & Time:** Thursday, July 2, 2026, ~8:00 AM CDT (~52 minutes, per transcript 00:01–00:53)

**Attendees (spoke on transcript):**
- Chuck Haines — Sr. Principal Software Engineer, Field Nation (Architecture Lead, AI Software Factory/Moissanite)
- Safin Ahmed — ran the meeting/agenda this session (role not on file — confirm if he's the current Guild chairperson)
- Sony Sadi — raised the membership/chairperson rotation question (role not on file)
- Mahim Safa — presented Ponytail plugin findings (role not on file)
- Mahmudur Rahman — presented Headroom plugin findings; had connectivity issues throughout (role not on file)
- Almansur Siddiqui — presented Caveman plugin findings; works in the "net recruitment tools" repo (role not on file)
- Andrew Kandels — weighed in on documentation/readability tradeoffs (role not on file)
- Ian Loverink — brief participation, mostly logistics (role not on file)
- Azizul Hakim — brief participation, technical difficulties comment (role not on file)
- Spencer Moulton — Principal Software Engineer, Field Nation; joined partway through (author of the SDDF/"STD" spec-driven development framework)

**Note on attendance:** This is a recurring Guild meeting with a large standing invite list (~46 people, mostly Bangladesh engineering plus a handful of US folks — Redwan Shahzada, Mike Mueller, Joe Hoppesch, and others were invited). Safin noted several US-side people were out for the July 4th holiday weekend, and Mike Mueller/Spencer Moulton had declined the invite (Spencer joined anyway partway through). The list above reflects only people who spoke on the transcript, not the full invite list.

**Original Transcript:** [Google Doc](https://docs.google.com/document/d/12yXtAqb2xnRBL8vATKTppSZgIaec1SMDPGq9uUTWrXM/edit?usp=meet_tnfm_email)

---

## Overview

This is Field Nation's **AI Guild** — a weekly, cross-org practitioner forum (mostly Bangladesh engineering plus some US folks) that's conceptually distinct from the **AI Software Factory / Moissanite** initiative Chuck co-leads, though the two overlap in people and sometimes in work. The Guild is where day-to-day agentic-development problems get raised and workshopped by whoever's using the tools; the Software Factory is the specific engineering initiative building the production PEV pipeline. This meeting is a good example of how they connect: the Guild surfaced a question (which spec-driven development framework should Field Nation standardize on?) and Chuck routed the actual evaluation work to his AI Software Factory contractors, with results to be reported back to the Guild before anything goes to the "standards guild" for a final call.

The meeting covered three main threads. First, a batch of token-saving plugin evaluations (Ponytail, Caveman, Headroom) that three members had volunteered to investigate — all three showed marginal-at-best token savings with real costs to code/comment readability, and the group tabled the whole token-optimization thread in favor of "just write better prompts and use skills well." Second, a meatier discussion on spec-driven development framework candidates (Google's GSD, GitHub Spec Kit, Spencer Moulton's SDDF/STD, and a new one called "Ship Shape") and where documentation should live (repo-local vs. centralized) — Chuck committed to having the Software Factory contractors branch FN Core to test candidate frameworks and report back. Third, a shorter thread on evaluation tooling: the AI Toolbox now has a forked, de-Claude-ified "DeepEvals" utility for evaluating skills, which Chuck offered as an answer to a couple of open questions about verifying tool/skill effectiveness.

The meeting ran short on time and didn't get to a planned "loop engineering" agenda item, which Chuck explicitly deprioritized in favor of the spec-driven development conversation, consistent with his running theme this session: keep the Guild focused on solving real Field Nation problems rather than open-ended tooling exploration.

---

## Key Notes

- **Guild membership and chairperson continuity:** Sony Sadi asked whether there's a formal rotation policy for members or the chairperson. Chuck said this has never been formally addressed in any FN guild. His recommendation: no prescribed process for members (handle workload/availability changes case-by-case as they come up), but the chairperson role should hold for at least 6–12 months for continuity. Safin also noted the member list is closed for now — no further signups this round.

- **Token-saving plugins evaluated and shelved:** Three members volunteered to test plugins aimed at reducing LLM token usage. Mahim Safa found Ponytail improves code quality on small tasks (via its "ultra" mode) but doesn't meaningfully cut tokens and risks deleting existing logic/side effects in larger codebases. Almansur Siddiqui's A/B testing on Caveman (which strips filler words from output) found only ~0.4–2% real-world cost impact — far below the vendor's advertised 12.8% — and it visibly hurt comment readability. Mahmudur Rahman found Headroom (a proxy-server-based compression tool, provider-agnostic) saved roughly 7,000 tokens out of 2 billion in his test — negligible — and he's not confident it preserves output quality.

- **Decision: table token optimization, prioritize readability.** Chuck framed the deciding argument: readability drives result quality, so degrading it to save a small percentage of tokens is a bad trade. Andrew Kandels agreed, adding that obscuring English-language docs/comments (in the name of compressing them for the LLM) creates real maintenance burden for humans. The group's consensus, which Chuck proposed as a close: table token-usage optimization as a Guild topic unless it becomes an actual persistent operational problem — better levers are context hygiene, model/agent selection, and skill-based workflows.

- **Spec-driven development framework evaluation, routed to the Software Factory:** The Guild discussed candidates: Google's GSD (Chuck's preference — lightweight), GitHub Spec Kit (Chuck's experience: too heavyweight), and Spencer Moulton's open-source SDDF. Spencer, joining partway through, said he's actually moved away from SDDF toward a newer, "poorly named" framework of his own called STD — a simpler, phase-based approach better suited to greenfield work, and offered to share it. A framework called "Ship Shape" also came up (introduced by Chuck secondhand); Chuck is skeptical because it's Cucumber-spec-based (his words: "immediately made me almost lose my breakfast") but thinks it's worth evaluating for thoroughness. **Decision:** rather than have Guild members individually spend time on this, Chuck will have the AI Software Factory/Moissanite contractor team branch FN Core, integrate the candidate frameworks (GSD + Spencer's newer framework, likely also Ship Shape), and evaluate real output quality/results. Findings come back to the Guild before anything is proposed to the "standards guild" as an org-wide standard.

- **Documentation placement — repo-local vs. centralized, and human- vs. LLM-readability:** Chuck's stated (self-described "somewhat controversial") position: documentation should live closest to the source — in-repo if it's repo-specific, in a centralized system (Confluence, or Guru/Unblocked once decided) if it's cross-cutting. Andrew Kandels raised a related concern: some of these frameworks push toward documentation that's optimized for LLM consumption at the expense of human readability, and he prefers docs that serve both audiences equally — Chuck agreed this is worth a fuller conversation since people outside this meeting likely disagree. A related open question from Safin: as a codebase and its specs both grow, what's the strategy for keeping spec volume manageable — how much detail should live in-repo vs. link out to Linear tickets? No answer was settled; Chuck volunteered to have the contractor team fold this into their upcoming framework evaluation work.

- **Skill evaluation via DeepEvals:** Mahim Safa asked for a session on how to evaluate the skills/tools people are using day-to-day, since usage is high but nobody's systematically comparing before/after results. Chuck pointed to a new addition in the AI Toolbox: a forked version of Anthropic's skill-creator skill with Claude-specific language and evals swapped out for "DeepEvals" — it works with Claude, Copilot, or Bedrock, can be used on new or existing skills, and lets you actually run the evaluation. Separately, Safin asked whether this could help verify the confidence/quality of documentation generated by the Marble team's hackathon project — Chuck said DeepEvals is possible there but probably not the most appropriate tool for that specific case (didn't elaborate further).

- **"Loop engineering" agenda item dropped for time.** Safin raised it as the next topic, but Chuck asked the group to prioritize the spec-driven development conversation instead, given limited remaining time and the Guild's stated charter (solve real FN problems). The topic didn't come back up before the meeting closed.

---

## Action Items

- **Chuck Haines:** Have the AI Software Factory/Moissanite contractor team create a branch in FN Core, integrate the spec-driven development framework candidates (Google GSD + Spencer Moulton's newer "STD" framework, likely Ship Shape), and evaluate real results/output quality. Report findings back to the Guild before taking any proposed standard to the standards guild.
- **Chuck Haines:** Have the contractor team also investigate how much spec detail should live in-repo vs. linked out externally (e.g., Linear tickets) as codebases and specs scale, and bring a recommended strategy back to the Guild.
- **Mahmudur Rahman:** Give a ~10-minute demo at the next meeting of his current workflow/tooling practices for efficient agent execution (model + agent selection) — carried over from this session due to connectivity issues.
- **Spencer Moulton (implied):** Share his newer "STD" framework (and the Ship Shape link) with the Guild for review.
- **Safin Ahmed (implied):** Guild member list is now closed for this round; no further action needed unless someone raises a change in workload/availability.
