# Pitfalls

The ways evals lie to you, roughly in order of how often
they bite.

## Benchmark contamination

**The problem:** popular test sets end up in training data.
A model that "scores 90% on HumanEval" may have *memorized*
HumanEval. Scores inflate; the benchmark stops measuring
generalization and starts measuring recall.

**The defense:** prefer fresh, adversarial, or private
tests for decisions that matter — and treat public
leaderboard numbers as upper bounds, not truths. Your own
duel scenarios, written by you, are contamination-proof by
construction.

## Gaming the metric (Goodhart's law)

**The problem:** "when a measure becomes a target, it ceases
to be a good measure." Optimize purely for win rate and
you'll select for whatever the judge rewards — verbosity,
flattery, safe non-answers — not quality.

**The defense:** never let one metric rule alone. Pair win
rate with human spot-checks and task-specific checks (did
the code *run*?). If the metric and your eyes disagree,
trust your eyes and fix the metric.

## Small-sample theater

**The problem:** "undefeated in 5 duels!" Five duels is
barely evidence. Humans are wired to generalize from tiny
samples; evals let you put numbers on that wiring.

**The defense:** the rematch habit from
[Metrics That Matter](metrics-that-matter.md). Report N
alongside every win rate. "7–3 over 10" is information;
"won" is a story.

## Judge-model incest

**The problem:** judging Qwen-vs-Llama with a Qwen judge,
or judging with the same model that generated the outputs.
Family bias is real and measurable.

**The defense:** judge with a larger, different-family model
than the contestants — and spot-audit (see
[LLM as Judge](llm-as-judge.md)).

## Scenario overfitting

**The problem:** you tune prompts, roles, and turn counts
until your favorite model wins, then declare it the best.
You've built a test your model passes, not a test that
measures.

**The defense:** write scenarios *before* running them,
freeze them, and be suspicious of any scenario that was
"adjusted" after seeing results. Version your scenario
files.

## Hardware as a hidden variable

**The problem:** tok/s, timeouts, and even *which* model
wins can depend on the machine. A model that needs 8GB of
KV cache behaves differently on a 4GB box (swapping,
timeouts) than on a 16GB one.

**The defense:** run comparisons on the same hardware, and
note the hardware in the log. "A beats B *on this box*" is
honest; "A beats B" is a claim about the universe.

## The single-number fallacy

**The problem:** collapsing everything into one score
("Model A: 8.7") hides *what it's good at*. Averages lie by
omission.

**The defense:** break results down by scenario tag. "A
wins code 8–2 but loses debates 3–7" is ten times more
useful than "A wins 11–9 overall" — and it tells you which
model to actually deploy for *your* task.
