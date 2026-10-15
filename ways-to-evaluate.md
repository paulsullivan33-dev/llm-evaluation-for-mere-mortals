# Ways to Evaluate

Four approaches cover nearly all practical LLM evaluation.
Each answers a different question.

## 1. Benchmarks (the standardized tests)

Fixed question sets with known answers, scored
automatically. The famous ones:

| Benchmark | Tests | Format |
|---|---|---|
| MMLU | broad knowledge (57 subjects) | multiple choice |
| HumanEval | Python coding | write functions, run tests |
| GSM8K | grade-school math | word problems, exact answer |
| MT-Bench | open-ended quality | LLM-judged, 1–10 |
| Arena-Hard | hard prompts | LLM-judged vs baseline |

**Good for:** first-pass filtering ("is this model even in
the right league?"), tracking progress across versions.

**Weak at:** your specific task. A model can top MMLU and
still write terrible SQL for *your* schema. Benchmarks also
**leak** — popular test sets end up in training data (see
[Pitfalls](pitfalls.md)).

## 2. Human eval (the gold standard, priced accordingly)

Humans read outputs and rate or rank them. Nothing measures
taste, helpfulness, and "would I actually use this" better.

**Good for:** final decisions, style, anything subjective.

**Weak at:** cost and speed. Paying humans to rate thousands
of outputs is expensive; doing it yourself doesn't scale past
a handful of comparisons.

## 3. LLM-as-judge (the scalable stand-in)

A capable model reads two outputs (or one output + rubric)
and scores them. MT-Bench and Arena-Hard work this way.
Cheap, fast, repeatable — and biased in known ways (see
[LLM as Judge](llm-as-judge.md)).

**Good for:** ranking many candidates quickly, regression
testing ("did the new version get worse?").

**Weak at:** the judge's blind spots become your blind
spots. A judge that loves verbose answers will crown verbose
models.

## 4. Pairwise duels (the head-to-head)

Two models, same task, alternating turns, several rounds —
then a winner is picked (by a judge, a human, or a rule).
This is the format this guide goes deepest on, because it's
the best effort-to-insight ratio for a home operator:

- **Cheap:** two small models on your own hardware.
- **Revealing:** multi-turn interaction exposes
  inconsistency, sycophancy, and reasoning failures that
  single-shot benchmarks miss.
- **Repeatable:** same scenario file, same result
  (within noise).

**Good for:** "which of these two should I actually use,"
run as often as you like.

**Weak at:** absolute scoring — duels tell you *A beats B*,
not *how good A is*.

## How they combine

The practical stack: **benchmarks** to shortlist,
**duels** to compare finalists on your kind of task,
**human eval** (your own eyes) for the final call, and
**LLM-as-judge** to scale the duels when you have dozens of
matchups. No single method does it all.
