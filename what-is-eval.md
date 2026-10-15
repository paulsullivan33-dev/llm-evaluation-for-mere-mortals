# What Is Evaluation?

You try two models. One feels smarter. Which do you pick?

"Feels smarter" is where everyone starts, and it's fine for
one decision. It collapses the moment you need to choose
*repeatedly* — across model versions, sizes, quantizations,
or providers. Human impression is slow, inconsistent, and
impossible to re-run. Evaluation is the discipline of turning
"feels smarter" into something you can **measure, repeat, and
trust**.

## What a good eval measures

A good eval has three properties:

1. **A task.** Not "be smart" — a concrete job: answer these
   questions, write this code, hold this debate. If you can't
   say what the model was asked to do, you can't say whether
   it did it well.
2. **A scoring rule.** Decided *before* you run it: exact
   match, a judge's verdict, a human rating. The rule is the
   contract; changing it after seeing results is cheating.
3. **Repeatability.** Same task + same rule tomorrow gives
   the same answer (within noise). If rerunning shuffles the
   winner, you measured luck, not quality.

## What eval can't do

- **Prove general intelligence.** A model can ace every
  benchmark you own and still fumble your actual task. Eval
  measures *the tasks you tested*, nothing more.
- **Replace trying it yourself.** For the final call on
  *your* workload, nothing beats running your workload.
- **Settle taste.** Style, tone, personality — measurable
  only by the humans who'll read the output.

## The honest hierarchy

For most people, most of the time:

1. **Run your actual task** on each candidate. Nothing beats
   it.
2. **Pairwise duels** on task-like scenarios. Cheap,
   revealing, repeatable.
3. **Published benchmarks** as a first filter. Fast, but
   distant from your use case.
4. **Vibes.** Fine for a first impression; never the final
   answer.

This guide spends the most time on #2 and #3, because #1 is
obvious and #4 is where everyone already lives.
