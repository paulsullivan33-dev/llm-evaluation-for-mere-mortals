# LLM as Judge

When you have more matchups than you have evenings, you let
a capable model do the judging. It works — with known,
named biases you must design around.

## How it works

The judge gets the finished transcript (or the two outputs)
plus a rubric, and returns a verdict. The robust pattern:

1. Strip or anonymize model names (judge sees "A" and "B").
2. Ask for a **verdict in a parseable format** — e.g. the
   last line must be `WINNER: A`. Free-form praise is
   unparseable; the verdict line is the contract.
3. Log the judge's full reasoning *and* the verdict line.
   When the verdict surprises you, the reasoning is where
   you audit it.

**Check:** a judge prompt ends like this:

```
Read the debate above. Decide which speaker argued better:
clearer reasoning, stronger evidence, fewer fallacies.
Your final line must be exactly: WINNER: <name>
```

And the harness looks for that final line — best-effort
parsing, with the full text kept for audit.

## The known biases

These are documented in the literature and observable in
practice. Design around all four:

- **Position bias.** Judges favor the *first* (or sometimes
  the second) option. Mitigation: run each matchup twice,
  swapping A/B order, and require agreement — or average.
- **Verbosity bias.** Longer answers score higher,
  regardless of quality. Mitigation: instruct the judge to
  penalize fluff explicitly, or cap response lengths.
- **Self-preference.** A judge slightly favors outputs that
  sound like itself. Mitigation: use a *different* (ideally
  larger) model as judge than the contestants.
- **Sycophancy blindness.** Judges under-penalize answers
  that flatter the prompt. Mitigation: rubrics that name it
  ("deduct for agreeing with the user without evidence").

## Judge selection

- **Bigger than the contestants** is the rule of thumb. A
  1.7B model judging 1.7B contestants is a coin flip with
  extra steps; a 14B+ judge over small contestants is
  meaningfully discriminating.
- **Different family helps.** A Qwen judge over Qwen-vs-Llama
  still carries family bias; cross-family judges are cleaner.
- **Temperature low.** You want the judge's modal answer,
  not its creativity. Temperature ~0.2 or lower.

## When not to use a judge

- **The final call on your workload.** Your eyes beat any
  judge for "would I deploy this."
- **Code correctness.** Run the tests — a judge's opinion
  of code is worth less than `pytest`. (This is why code
  duels execute the code: the *run result* is the judge.)
- **Close calls you care about.** Judge verdicts on
  near-ties are noise. Rematch, or decide yourself.

## The audit habit

Every so often, read a judged transcript yourself and ask:
"would I have picked the same winner?" If you keep
disagreeing, the problem is your rubric, your judge, or your
scenario — and that's valuable information, not a failure.
