# Engineering Interview Exercise v2: AI-Augmented Problem Solving

**Replaces:** the "maxEvents" interval-scheduling gist (chaines-fn/97145507b04f55d15234f420ea06ba2b)
**Format:** Live pairing session, 30-40 minutes total (Setup 1 / Part 1 ~10 / Part 2 ~10-12 / Part 3 ~8-10 / Wrap-up ~3-5)
**Philosophy:** AI coding tools are part of the job now. This exercise doesn't test whether a candidate can avoid using them — it tests whether they can direct one well, catch its mistakes, and actually understand the code it produces. A candidate who prompts well, questions ambiguity, and rigorously verifies output is a stronger hire than one who writes clean code from memory but can't tell when an AI-generated solution is subtly wrong.

---

## Setup (1 min)

Tell the candidate up front, quickly:

> "Use any AI tool you'd like — Claude, ChatGPT, Copilot, whatever you normally reach for. We're not testing whether you can solve this unassisted. We're watching how you work: what you ask for, how you verify what comes back, and how you handle changes and bugs along the way. Think out loud. We'll move fast — about half an hour total."

Share your screen or theirs, whichever they're comfortable coding on. Skip small talk here — the clock is tight.

---

## Part 1: The initial ask (~10 min)

Give the candidate this prompt **verbatim** — it is intentionally underspecified. Do not proactively clarify unless asked.

> **Problem:** We need a function that schedules field technicians to job bookings. Given a list of bookings, each with a `start` and `end` time (in minutes, same day), write a function that returns the maximum number of bookings a single technician can complete in a day without conflicts.
>
> ```ts
> type IBooking = { start: number; end: number };
> function maxBookings(bookings: IBooking[]): number
> ```

**What we're watching for:**

- Do they ask what counts as a "conflict"? (Back-to-back bookings at the same site vs. different sites behave differently — this isn't specified yet, on purpose.)
- Do they ask about scale/constraints before or after prompting the AI?
- What do they actually type into the AI tool? Vague ("solve this") vs. specific (includes constraints, edge cases, complexity target)?
- Do they read the generated code before running it, or paste-and-run immediately?
- Do they write their own test cases, or trust the AI's tests (if any) at face value?

A reasonable AI-generated answer here is the classic greedy interval-scheduling solution (sort by end time, pick greedily). That's fine and expected — this part is a warm-up to get them oriented and talking. The real signal starts in Part 2.

**Time-box:** the moment they have a solution passing the reference tests below, move on — don't let them polish or over-explain. If they're stuck past ~8 minutes, nudge toward "conflict = overlapping time ranges" and move to Part 2.

**Reference test cases** (same as v1, for continuity):

```ts
const testCases = [
  { bookings: [{start:1,end:3},{start:2,end:9},{start:3,end:5},{start:2,end:4},{start:4,end:6}], expected: 2 },
  { bookings: [{start:1,end:2},{start:2,end:3},{start:3,end:4},{start:4,end:5}], expected: 4 },
  { bookings: [{start:1,end:10},{start:2,end:3},{start:4,end:5},{start:6,end:7},{start:8,end:9}], expected: 4 },
];
```

---

## Part 2: The curveball (~10-12 min)

Once they have a working solution, introduce a new requirement:

> "One more thing — technicians need at least 30 minutes of travel buffer between two bookings at **different** job sites. Back-to-back bookings at the **same** site are fine with zero gap. Bookings now include a `siteId`."
>
> ```ts
> type IBooking = { start: number; end: number; siteId: string };
> ```

This breaks the naive "does end <= next start" check and forces a real change to the comparison logic, not just a tweak. It also can't be solved by re-running the same prompt — they have to either re-prompt with the new constraint (and verify the AI actually applied it correctly) or hand-edit the logic themselves.

**What we're watching for:**

- Do they immediately understand *why* their existing solution breaks, or just re-prompt and hope?
- If they re-prompt: is the new prompt precise about the buffer-only-applies-across-sites rule? Do they re-verify against test cases, especially the same-site zero-gap case, which is an easy place for an AI to over-apply the buffer?
- Do they write a new test case for the same-site-adjacent scenario themselves, or wait for it to fail?
- Debugging behavior when something's wrong: do they read the diff, add print/log statements, reason step by step — or just keep re-prompting with "it's still not working"?

**Time-box:** if they haven't converged on a correct fix by ~10 minutes, tell them directly which test case is failing and move to Part 3 regardless — the bug-review section matters more than a fully working Part 2.

**Curveball test cases:**

```ts
const bufferTestCases = [
  // Same site, back-to-back, no buffer needed — should be allowed
  { bookings: [{start:1,end:3,siteId:"A"},{start:3,end:5,siteId:"A"}], expected: 2 },
  // Different sites, back-to-back, needs 30 min buffer — should conflict
  { bookings: [{start:1,end:3,siteId:"A"},{start:3,end:5,siteId:"B"}], expected: 1 },
  // Different sites, exactly 30 min gap — should be allowed
  { bookings: [{start:1,end:3,siteId:"A"},{start:33,end:35,siteId:"B"}], expected: 2 },
];
```

---

## Part 3: Code review — spot the AI's mistake (~8-10 min)

Hand the candidate the snippet below and say it came from a teammate (or "an AI") and needs review before merging. Don't say there's a bug — just ask them to review it as they would a PR.

```ts
function maxBookings(bookings: IBooking[]): number {
  const sorted = [...bookings].sort((a, b) => a.start - b.start);
  let count = 0;
  let lastEnd = -Infinity;
  let lastSite = "";

  for (const b of sorted) {
    const buffer = b.siteId === lastSite ? 0 : 30;
    if (b.start >= lastEnd + buffer) {
      count++;
      lastEnd = b.end;
      lastSite = b.siteId;
    }
  }
  return count;
}
```

**The bug:** it sorts by `start` instead of `end`. This is the single most common mistake in interval scheduling — sorting by start time instead of end time — and it silently produces a *valid but non-optimal* count rather than crashing, which is exactly the kind of AI mistake that's dangerous in practice: it looks plausible, runs without error, and passes some but not all test cases.

Run it against `[{start:1,end:10,siteId:"A"},{start:2,end:3,siteId:"A"},{start:4,end:5,siteId:"A"}]` — sorting by start picks the long booking first (count 1) when the optimal is 2.

**What we're watching for:**

- Do they catch the sort-key bug by reading the code, or only by running the failing test case?
- Can they articulate *why* sorting by end time is the correct greedy choice (exchange argument / earliest-finish-time reasoning), or do they just know "swap to `.end`" without understanding why?
- Do they suggest a test case that would have caught this, if the failing test wasn't already provided?

**Time-box:** if they haven't spotted it within ~5 minutes, point them at the failing test case from the note above and ask them to explain the fix — the explanation is the signal, not the discovery speed.

---

## Wrap-up discussion (~3-5 min)

Pick two, don't try all three if time's short:

- "What didn't you trust about the AI's output, and how did you check it?"
- "If this were going into production, what would you want in place before you'd merge AI-generated code like this?"
- "Where did the AI help you move faster, and where did it slow you down or mislead you?"

These answers are as much signal as the code itself.

---

## Evaluation rubric

| Signal | Weak | Strong |
|---|---|---|
| Requirements handling | Prompts immediately on ambiguous spec, accepts first output | Asks clarifying questions before or instead of prompting |
| Prompt quality | Vague, one-shot, no constraints stated | Specific, iterative, states edge cases and complexity needs |
| Verification | Trusts output, runs happy path only | Writes/extends test cases, checks edge cases unprompted |
| Debugging | Re-prompts repeatedly without diagnosing | Reads code, reasons about root cause, forms a hypothesis before changing anything |
| Bug review (Part 3) | Misses the sort-key bug, or fixes it without explaining why | Catches it by reading, explains the earliest-finish-time reasoning |
| Judgment | Ships whatever the AI gives without scrutiny | Treats AI output as a first draft, owns correctness personally |

---

## Notes for interviewers

- If a candidate solves everything almost instantly with no visible verification, that's not automatically a green flag — probe Part 3 harder and ask them to explain the algorithm without looking at the code.
- If a candidate refuses to use AI tools at all, that's fine, but flag it — unassisted candidates will likely run over the 40-minute cap on Part 1 alone. Let Part 1 run a bit long rather than cutting it short, and drop Part 3 if needed instead.
- Hard cap: 40 minutes. Part 3 (the bug review) is the highest-signal section — protect it. If a choice has to be made, trim Part 2 before Part 3, and Part 1 before Part 2.
