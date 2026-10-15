# Pairwise Duels

A duel: two models, one task, alternating turns, several
rounds. The simplest serious evaluation you can run, and the
one this guide recommends most.

## The format

1. **Pick a task** ("scenario"): a topic plus rules. *Debate
   AI consciousness. Write a Python function from a spec.
   One defends an unpopular opinion; the other attacks it.*
2. **Assign roles**: Model A gets a persona/position, Model B
   gets the opposing one. Roles matter — "you are a skeptic"
   produces very different text than no role at all.
3. **Alternate turns** for a fixed number of rounds (6–10
   turns total is the sweet spot). Each model sees the full
   transcript so far.
4. **Pick a winner**: a judge model reads the transcript and
   declares a winner, or a human does, or a rule decides
   (e.g. "whose code passed the tests").

**Check:** a real duel summary looks like this:

```
=== Model performance ===
Model         Turns  Gen tok/s  Prompt tok/s  Tokens  Truncated
qwen3:1.7b        4        2.2           4.3     614          0
smollm2:1.7b      4        2.5           4.2     503          0
--- session ended (8 replies) ---
```

Turns each, speed, tokens, whether anything got cut off —
the raw material every other metric is built from.

## Why turns beat single shots

A single prompt measures *first impressions*. Turns measure
*character*:

- **Consistency:** does it contradict itself by turn 6?
- **Steerability:** does it actually follow the role, or
  drift into generic assistant-speak?
- **Depth:** can it build on the opponent's argument, or
  does it repeat its opening forever?
- **Robustness:** code that "works" in one shot vs code that
  survives an adversary trying to break it (builder vs
  breaker).

Six turns of interaction reveal more than sixty single-shot
prompts.

## Designing good scenarios

A scenario is a test; design it like one:

- **One thing under test.** "Debate consciousness" tests
  argumentation. "Write parse_duration" tests code. Don't
  test both at once.
- **Adversarial where it helps.** Builder-vs-breaker (one
  writes code, the other tries to break it) finds bugs that
  friendly tests miss.
- **Fixed length.** Same turn count every run, or results
  aren't comparable.
- **Tags.** Label scenarios (`code`, `debate`, `small`) so
  you can run subsets: `--tag code` runs just the code
  duels. Ten tagged scenarios beat a hundred untracked ones.

## The dedup guard

Small models sometimes get stuck and repeat a previous reply
verbatim. A serious harness detects this: if a reply matches
the model's last reply, re-roll once at slightly higher
temperature with an explicit "don't repeat yourself" nudge.
If it repeats *again*, keep it — that's data too (the model
is genuinely stuck, and the log should show it).

## What duels don't tell you

- **Absolute quality.** A beats B, B beats C — but is A any
  good? Duels rank; they don't grade.
- **Your exact task.** Unless the scenario *is* your task
  (write it that way when it matters).
- **Statistical certainty** from one run. Models are
  stochastic; rematch close calls (see
  [Metrics That Matter](metrics-that-matter.md)).
