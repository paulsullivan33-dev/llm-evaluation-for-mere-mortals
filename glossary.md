# Glossary

**benchmark** — a fixed set of test questions with known
answers and automatic scoring (MMLU, HumanEval, GSM8K).
Fast and standard; distant from your actual task.

**contamination** — benchmark test data leaking into a
model's training data, inflating its score. The reason
public leaderboard numbers are upper bounds.

**dedup guard** — detecting when a model repeats its
previous reply verbatim and re-rolling once before
accepting the stuck output.

**Elo rating** — a scoring system (from chess) where beating
a strong opponent gains more points than beating a weak
one. Used by LMArena-style leaderboards; the intuition
matters more than the math.

**eval** — any structured attempt to measure model quality:
a task plus a scoring rule plus repeatability.

**harness** — the software that runs evals: loads
scenarios, calls models, logs transcripts, computes
metrics. Boring is good.

**human eval** — humans rating or ranking model outputs.
The gold standard for taste; expensive and slow.

**LLM-as-judge** — using a (usually larger) model to score
model outputs. Scalable; carries position, verbosity, and
self-preference biases.

**pairwise duel** — two models, one task, alternating
turns; the winner is picked by judge, human, or rule.

**position bias** — judges favoring the first (or second)
presented option regardless of quality. Mitigated by
swapping order across runs.

**scenario** — one duel's test definition: topic, roles,
turn count, and settings, kept as data (not code).

**truncation** — a reply cut off at the token limit. Track
it: a duel decided by `max_tokens` wasn't decided by the
models.

**verbosity bias** — judges scoring longer answers higher
regardless of quality. Mitigated by explicit rubrics or
length caps.

**win rate** — fraction of matchups a model won. Always
reported with N; "7–3 over 10" is information, "won" is a
story.
