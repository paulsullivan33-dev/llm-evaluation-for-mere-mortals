# LLM Evaluation for Mere Mortals

**LLM evaluation** is how you answer "which model is actually
better?" with evidence instead of vibes. This guide covers the
main approaches — **benchmarks**, **human eval**,
**LLM-as-judge**, and **pairwise duels** — the metrics that
matter, the traps that mislead, and how to run evals yourself
on your own hardware.

No CS degree required. Ideas first, then real methodology.

## What this guide covers

- [**What evaluation is**](what-is-eval.md) — why "just try
  it" doesn't scale, and what a good eval measures
- [**Ways to evaluate**](ways-to-evaluate.md) — benchmarks,
  human eval, LLM-as-judge, pairwise duels: the landscape
- [**Pairwise duels**](pairwise-duels.md) — two models, one
  task, head to head: the format in depth
- [**LLM as judge**](llm-as-judge.md) — using a model to score
  models: how it works and where it lies
- [**Metrics that matter**](metrics-that-matter.md) — win
  rates, tokens/sec, cost, and statistical honesty
- [**Running evals yourself**](running-evals.md) — harnesses,
  scenarios, batch runs, and logging like you mean it
- [**Pitfalls**](pitfalls.md) — contamination, gaming the
  metric, small-sample noise
- [**Troubleshooting**](troubleshooting.md) — symptom → fix,
  seven ways

## What it doesn't cover

Training or fine-tuning models, building new benchmark
datasets from scratch, and the mathematics of Elo ratings
beyond the working intuition. This guide is about *evaluating
models you didn't train* — the situation nearly everyone is
in.

## How to use this guide

Methodology claims were checked against published benchmark
documentation and real duel transcripts before shipping.
**Check:** blocks show verified output or real examples.
Numbers from public leaderboards are dated — they move.
Anything labeled illustrative says so.

## Chapters

1. [What Is Evaluation?](what-is-eval.md)
2. [Ways to Evaluate](ways-to-evaluate.md)
3. [Pairwise Duels](pairwise-duels.md)
4. [LLM as Judge](llm-as-judge.md)
5. [Metrics That Matter](metrics-that-matter.md)
6. [Running Evals Yourself](running-evals.md)
7. [Pitfalls](pitfalls.md)
8. [Troubleshooting](troubleshooting.md)
9. [Glossary](glossary.md)
