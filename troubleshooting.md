# Troubleshooting

Symptom → fix, in the order you'll hit them.

## Duel "stopped early" on timeout

**Symptom:** log ends with `--- stopped early ---` and a
timeout message.

**Cause:** the per-turn timeout is shorter than a legitimate
turn on your hardware. At 2 tok/s, a 1024-token reply needs
~9 minutes *before* prompt processing.

**Fix:** set the timeout from the math: `max_tokens ÷ your
measured tok/s`, plus headroom. If timeouts persist at sane
values, the model may be stuck in a repetition loop — check
the partial transcript.

## Model stuck "loading" between turns

**Symptom:** long pauses between turns, one model never
unloads.

**Cause:** on small machines, model switches briefly need
both models in RAM. If that exceeds physical memory, the
unload thrashes in swap.

**Fix:** limit the server to one loaded model at a time
(Ollama: `OLLAMA_MAX_LOADED_MODELS=1`) so unload finishes
before the next load starts. Don't run two batches
concurrently on one box.

## Judge verdict missing or unparseable

**Symptom:** no `WINNER:` line in the log, or the judge
wrote an essay instead.

**Cause:** the judge prompt didn't constrain the output
format hard enough, or the judge model is too small to
follow instructions reliably.

**Fix:** put the format requirement last in the prompt
(recency helps), and use a larger judge than the
contestants. Parse best-effort; always keep the full judge
text for audit.

## Same matchup, different winner

**Symptom:** rematch flips the result.

**Cause:** normal variance — or a scenario balanced on a
knife's edge.

**Fix:** this is data, not a bug. Run more matchups; the
win rate converges. If it never converges, the models are
evenly matched *on this scenario* — say so and move on.

## One model always goes first

**Symptom:** Model A wins suspiciously often.

**Cause:** position bias (in judged duels) or first-mover
advantage in the scenario design.

**Fix:** swap the speaking order across runs. If the win
rate follows the *position* rather than the *model*, you've
found bias, not quality.

## Transcripts full of repeated replies

**Symptom:** a model pastes the same reply twice in a row.

**Cause:** small models get stuck in loops, especially at
low temperature.

**Fix:** the dedup guard (see [Pairwise Duels](pairwise-duels.md)):
detect, re-roll once with higher temperature and a
no-repeat nudge. If it persists, the model is genuinely
stuck — log it as a loss for that turn, not a harness bug.

## Batch dies on the first failure

**Symptom:** one bad scenario kills the whole overnight run.

**Cause:** the batch aborts instead of skipping.

**Fix:** the harness must catch per-scenario errors, log
them, and continue. A batch of 75 where 3 fail is a
successful batch with 3 known issues; a batch that aborts
at #4 is a wasted night.
