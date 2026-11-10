# Testing skill trigger effectiveness for ai-toolbox

**Research question:** How do we test whether an ai-toolbox skill actually gets invoked when it should (and stays quiet when it shouldn't), as a complement to the DeepEval-based testing we already run on skill output quality?
**Date:** 2026-08-04
**Scope:** Skill invocation/triggering only (not MCP tool selection or subagent routing). Covers Claude Code first, Copilot as a stretch goal, Claude-only as an acceptable fallback.

---

## Summary

Trigger testing is a different axis from what DeepEval is doing today. DeepEval (per the fn-event-integration eval results already in `knowledge/tech-context.md`) answers "given that the skill fired, is the output good" — that's a content-quality question. What you're describing is "does the skill fire on the right prompts and stay silent on the wrong ones" — a classification problem (precision/recall), not an output-quality problem. It needs its own test harness, but it's a solved problem: Anthropic's skill-creator skill and OpenAI's Codex team both independently published near-identical approaches in the last few weeks, and the pattern is straightforward to adapt to ai-toolbox.

The approach both converged on: build a labeled set of realistic prompts (some that should trigger the skill, some that deliberately shouldn't — the "shouldn't" ones being near-misses, not obviously-irrelevant throwaways), run each prompt through the tool non-interactively, and check whether the skill fired. That gives you precision and recall you can track over time, the same way you'd track any other regression suite.

The good news for ai-toolbox specifically: since Agent Skills became an open standard in December 2025, the SKILL.md file is byte-identical across Claude Code, Copilot, Codex, and others — you're already seeing this in the `.claude/skills/` reuse Vitalii found for the Copilot pilot. The bad news: triggering is still a per-engine decision. Each vendor's own model reads the same description and makes its own call, so a description tuned for Claude Code isn't guaranteed to trigger the same way in Copilot. The fix is the same labeled prompt set run against both engines, not two separate test designs.

Claude Code side is essentially free — Anthropic's `skill-creator` skill already ships a full trigger-eval loop (60/40 train/test split, iterative description rewriting, held-out scoring) that you can point at any ai-toolbox skill today. Copilot side needs a small custom harness using Copilot CLI's non-interactive mode; the one open question is whether Copilot exposes a structured trace of "did skill X fire" the way Claude's transcript and Codex's `--json` output do — that's worth a short spike before committing further, and Claude-only is a perfectly reasonable fallback in the meantime, consistent with what you flagged going in.

---

## Findings

### The gap is real, and it's already named in your own workspace

`knowledge/tech-context.md` calls this out directly, from the OTel telemetry work: *"Trigger-miss is undetectable: a skill that should have fired but didn't leaves no invocation to trace. Handle via spot-checks."* That's exactly the problem you're asking about, and it's structural, not a tooling gap you can close with better Datadog queries. Telemetry (the `skill_activated` events, `api_request.skill.name`) can only tell you about skills that fired — it has no visibility into the cases where a skill should have fired and silently didn't. Production telemetry is a precision signal (is it over-triggering in the wild) but can never be a recall signal. Recall requires an offline, labeled test set where you already know the right answer.

### How triggering actually works (why description quality is the whole ballgame)

Per skill-creator's own documentation: skills appear in the agent's `available_skills` list as just name + description at startup (progressive disclosure tier 1, ~100 tokens). The agent decides whether to consult a skill purely by matching the request against that description — the body of SKILL.md doesn't get loaded until the description-match decision is already made. This is true across Claude Code, Copilot, and Codex, since they all implement the same open standard. Practically: your recent description-optimization pushes to ai-toolbox skills are the single highest-leverage lever you have over trigger rate, and testing them is really testing description quality, not the skill body.

One nuance worth knowing: the agent only consults a skill for tasks it can't trivially handle with basic tools. A simple one-shot request like "read this PDF" may skip a matching skill entirely regardless of description quality, because the model judges it doesn't need help. Your eval prompts need to be substantive enough that the skill would actually be useful — trivial prompts are false negatives in the test, not the skill.

### Two independent implementations of the same pattern

**Anthropic's `skill-creator` skill** (already installed in this environment) has a full description-optimization workflow:

1. Generate ~20 eval queries, split roughly 8-10 should-trigger / 8-10 should-not-trigger. The should-trigger set should vary phrasing (formal/casual), include cases where the user never names the skill or file type, and include cases where this skill competes with another skill but should win. The should-not-trigger set is the one that matters most — near-misses that share keywords or domain with the skill but genuinely need something else. An obviously-irrelevant negative ("write a fibonacci function" as a negative for a PDF skill) tests nothing.
2. Queries get reviewed by a human (an HTML review UI ships with the skill) before being locked in.
3. An automated loop (`scripts/run_loop.py`) splits the set 60% train / 40% held-out test, evaluates the current description by running each query 3x for reliability, has Claude propose an improved description based on what failed, and repeats up to 5 iterations — picking the final description by test-set score, not train-set score, specifically to avoid overfitting to the visible examples.
4. This requires the `claude` CLI (`claude -p`) and is explicitly Claude Code / Claude.ai only — the doc says to skip this step entirely outside Claude Code.

**OpenAI's Codex team published an almost identical guide** for Codex skills (developers.openai.com/blog/eval-skills, Jan 2026). Same structure, different mechanics:

- Categorizes prompts as explicit invocation (names the skill), implicit invocation (matches the skill's actual use case without naming it — the real test of description quality), contextual/noisy invocation (realistic surrounding detail), and negative control (should NOT trigger — again, the valuable ones are near-misses like "add Tailwind styling to my existing app" as a negative for a "scaffold a new Tailwind app" skill).
- Runs prompts through `codex exec --json`, which streams structured JSONL events including every command execution — giving you a deterministic, scriptable trace rather than eyeballing transcripts.
- Recommends starting with a small CSV (10-20 prompts) that grows as a living regression record every time a real trigger failure is found in the wild — same idea as a bug-driven regression test suite in traditional software.
- Layers in a second, rubric-based grading pass (via `--output-schema` constraining output to a JSON Schema) for the qualitative "did it do it the way we wanted" questions once the basic trigger/no-trigger question is settled.

The convergence between two independent teams on the same design (explicit / implicit / contextual / hard-negative categories, held-out scoring, structured JSON traces) is a good signal this is close to a settled best practice, not a one-off Anthropic idiosyncrasy.

### Extending this to Copilot

GitHub Copilot CLI supports a non-interactive mode (`copilot -p "<prompt>" --no-ask-user`) explicitly designed for scripting and CI, the same shape as `claude -p` and `codex exec`. Copilot also reads `.claude/skills/` directly using the identical SKILL.md format (already confirmed in your own tech-context notes on the marketplace-plugin pivot). That means the same labeled prompt CSV/JSON could in principle drive both a Claude Code runner and a Copilot CLI runner, giving you an apples-to-apples precision/recall comparison across engines for the same ai-toolbox skill.

The open question I couldn't confirm from public docs: whether Copilot CLI exposes a structured, scriptable signal of "skill X was invoked" the way Claude's transcript and Codex's `--json` JSONL stream do. If it only gives you free-text output, you'd need to detect triggering indirectly (e.g., did the expected SKILL.md-specified steps show up in the transcript, or instrument via the same `PostToolUse`-style hook pattern your OTel work already uses to stamp `skill.name` onto tool events). This is worth a short, cheap spike — probably a half-day for Vitalii or whoever's closest to the Copilot pilot — before deciding how much to invest in a full dual-engine harness versus shipping Claude-only first and adding Copilot once the marketplace-plugin rollout (from your Aug 4 pivot discussion) is further along anyway.

### How this could plug into the DeepEval pipeline you already have

DeepEval already ships a `ToolCorrectnessMetric` built for exactly this shape of question in the agentic-eval sense: it compares `tools_called` (what actually happened) against `expected_tools` (what should have happened) for a given test case, and scores deterministically (LLM-as-judge only enters if you also want it to judge whether the tool choice was *optimal* given `available_tools`, which isn't your question here). Framed as skill-trigger testing:

- Each labeled prompt becomes a DeepEval `LLMTestCase`.
- `expected_tools = [skill-name]` for should-trigger cases, `expected_tools = []` for should-not-trigger cases.
- `tools_called` gets populated from whatever your harness captures as having fired — parsed from a Claude transcript, a Codex JSONL trace, or (pending the Copilot spike) a Copilot equivalent.
- Aggregate pass rate across the labeled set gives you your precision/recall proxy, and it runs through the same `deepeval test run` / CI machinery you're already using for output-quality evals — same reporting, same historical trend view, one pipeline instead of two.

DeepEval's own guidance targets keeping false-positive and false-negative rates under 5% combined and treats this as a standard CI regression gate, which is a reasonable bar to adopt for ai-toolbox's own skill-trigger suite, and it gives you a concrete number to put in front of the team rather than "seems to trigger fine."

### Where to prioritize first

Given the description-optimization work you just pushed, the highest-value next step is probably running the existing skill-creator trigger-eval loop against 1-2 of the ai-toolbox skills you most recently touched (a natural candidate given the recent MOI-48 split: `fn-event-integration` and whatever boundary skill it sits next to, since authority-boundary skills are exactly where near-miss negatives are richest and most likely to matter). That validates the approach cheaply on Claude Code alone, using tooling you already have installed, before investing in a custom cross-engine harness.

---

## Key uncertainties

- Whether Copilot CLI/Copilot Chat exposes a structured, scriptable "did skill X fire" signal comparable to Claude's transcript or Codex's `--json` JSONL stream. Public docs confirm non-interactive scripting support but not this specific observability detail — needs a direct spike, not more searching.
- Whether `ToolCorrectnessMetric`'s per-test-case shape aggregates cleanly into a precision/recall report across a whole labeled set, or whether it's cleaner to write a thin custom aggregator on top of DeepEval's dataset/CI primitives. Both are plausible from the docs; worth prototyping both on a small skill before committing.
- This research didn't cover MCP tool selection or subagent routing testing, per the scope you set — flagging in case that becomes a related need later, since the same explicit/implicit/negative-control pattern would likely apply there too.

---

## Sources

- [Testing Agent Skills Systematically with Evals — OpenAI Developers](https://developers.openai.com/blog/eval-skills)
- [Tool Correctness — DeepEval docs](https://deepeval.com/docs/metrics-tool-correctness)
- [Adding agent skills for GitHub Copilot CLI — GitHub Docs](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills)
- [Running GitHub Copilot CLI in Scripts and CI/CD Pipelines (Headless Mode)](https://www.devleader.ca/2026/07/27/running-github-copilot-cli-in-scripts-and-cicd-pipelines-headless-mode)
- [GitHub - agentskills/agentskills: Specification and documentation for Agent Skills](https://github.com/agentskills/agentskills)
- Anthropic `skill-creator` skill (installed locally in this environment — `SKILL.md`, description-optimization section)
- Internal: `knowledge/tech-context.md` (OTel telemetry section — "Trigger-miss is undetectable" finding; MOI-48 skill-splitting eval results)
