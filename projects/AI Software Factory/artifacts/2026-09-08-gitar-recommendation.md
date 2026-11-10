# Recommendation: Move Forward with Gitar for AI Code Review

We ran a three week head-to-head from Aug 19 to Sep 9, comparing Gitar against an _expanded_ Copilot setup, not stock Copilot. Copilot got balanced mode plus a committed code review skill as well as access to skills in our ai-toolbox so it was reviewing at its best.

**Head-to-head repos:** `core-ui`, `fn-core`, `engineering-dbt`, `geospatial`, `dispatch-automation`

**Gitar enabled with additional context:** `ui`, `integration-worker-salesforce`, `integration-api`, `integration-broker-fieldservice`, `webhook-v3`, `herman`, `long-term-engagements`, `work-order-execution-agents`, `workato-connectors`, `integration-embedded`

The point of the wide net was to see whether Gitar holds up across very different codebases: the PHP monolith, React UI, Go services, dbt, and the integration platform.

## What the team said

We got feedback from Mike Mueller, Andrew Kandels, Kimberly Zito, Brian Chapeau, Travis Lang, Nate Kramber, Jobaer Ahamed, and the Obsidian contractors. A few in their own words:

> "The interactivity is great. One of the things I hated about Copilot is once that first pass is done it would check out, and if you resolved things the summary would just be stale. If you wanted another pass you'd have to request it. I like that as you fix issues and comment, it's replying and re-checking fixes, and updating the existing summary to match current state. As far as the items it finds, seems on par with Copilot so far (3 flagged items were identical between the two), but that interactivity definitely puts it over the edge."
> **Andrew Kandels**

> "My initial feedback is that I REALLY like how it watches the PR, analyzes even things like CI failures, and resolves its own comments once addressed."
> **Kimberly Zito**

> "So far I've been mostly impressed by its feedback. Not in love with the mechanics of the 'apply fix', but the comments themselves have been pretty good. That same PR on its first cut this morning came back with a rubber stamp pretty quick from Copilot that Gitar found several reasonable issues with."
> **Brian Chapeau**

> "Gitar is looking good so far. Caught a few things on LTE PR #202 that GitHub and Claude did not."
> **Nate Kramber**

> "Gitar found many things that claude code review locally did not find, and many of them were legitimiately good findings"
> **Andriy Kurenda**

## Why Gitar over Copilot

**Cost is predictable.** Gitar is a fixed price per PR no matter how many times it reviews that PR. Copilot code review bills to whoever opened or requested the review, out of that person's budget, so we can't forecast it and we can't see it cleanly. A fixed per-PR rate turns an unpredictable variable cost into a line item we can plan against, and it doesn't quietly eat an engineer's AI budget every time they open a PR.

**It stays on the PR.** Copilot reviews once and checks out. Gitar watches the PR, re-checks your fixes, resolves its own comments, updates the summary to match current state, and diagnoses CI failures with a root cause. That's the single most consistent piece of feedback we got, from people on completely different stacks.

**You can argue with it.** You can ask Gitar why it did or didn't flag something, and tell it to remember that answer going forward. Mike put it well: "Gitar also told me I'm not always right, and I appreciated being able to ask why it didn't make a comment." That directly addresses my long-standing objection to agentic reviewers, which is that you can't tell what the agent actually looked at. Being able to interrogate the review is a real answer to that, even if it isn't the published-process summary I originally asked for.

**It gets better on our code.** Gitar learns from our corrections and now supports cross-repo analysis, which opens up things Copilot structurally can't do. Mike's example: when a team changes a schema, a cross-repo review can check `engineering-dbt` for breaking changes and flag that a corresponding PR is needed.

**The Sonar and Gitar team has been genuinely responsive.** When we found Gitar was missing things in FN-Core, they root-caused it to a feature flag gate deployed in the wrong place, fixed it, and told us what it was, inside a day. They have also been very forward about roadmap items, many of which I am excited about.

## What didn't go well

As with any pilot there were some things that didn't go well and want to cover a few of them.

- **FN-Core lagged early.** Mike found Copilot did better on several monolith PRs, one "by a lot." That was traced on Sep 3 to the thorough-mode feature flag bug above and Gitar improved after the fix, but we do not yet have enough post-fix monolith data to call it settled. Everywhere else (`core-ui`, `herman`, `lte`) Gitar was better than or equal to Copilot throughout.
- **The bot pushed a commit nobody asked for.** On a revert PR in FN-Core, a short reply to Gitar caused it to push a commit directly to the branch. We've reported it. We need to lock down its commit permissions and agree that pure reverts don't get reviewed at all before we roll wider.
- **"Apply fix" over-applies.** Brian asked it to fix one comment and it fixed all of them, including one he'd explicitly told it to leave alone. Workable for now (use Gitar for feedback, humans decide what to act on), but we should look at disabling apply-fix by default.
- **Merge-block governance isn't built yet.** Gitar can block a merge, but today anyone can comment `gitar unblock` to clear it. Role-based unblock is on their near-term roadmap and not shipped.
- **Copilot got better during the trial too.** Mike noticed it. Some of that is likely our own skills work rather than Copilot itself, but I'm not going to pretend the gap is static.

None of these are reasons not to buy. They're the punch list for the first thirty days.
