# Running Evals Yourself

The methodology is worthless without a harness that runs it
while you sleep. Here's what a serious home setup looks
like — generic patterns, adaptable to any machine running
Ollama.

## Scenario files

Keep scenarios as data, not code. One JSON file per duel:

```json
{
  "topic": "Debate: is AI consciousness possible in principle?",
  "roles": ["Materialist", "Skeptic"],
  "turns": 8,
  "tags": ["debate"],
  "max_tokens": 1024,
  "timeout": 3600
}
```

Why this shape:

- **Topic + roles** — the test itself, readable at a glance.
- **Turns** — fixed length, so runs are comparable.
- **Tags** — run subsets (`--tag code`) without maintaining
  separate lists.
- **max_tokens** — caps reply length; on slow hardware this
  is also a *time* cap (tokens ÷ tok/s = worst-case seconds).
- **timeout** — per-turn ceiling. Set it from the math:
  `max_tokens ÷ your tok/s`, plus headroom for prompt
  processing. A timeout shorter than a legitimate turn is a
  bug in your config, not in the model.

## Batch runs

Run the whole directory, not one file at a time. The
harness should:

- Run scenarios sequentially (one duel at a time — two
  duels fighting over one GPU/CPU helps nobody).
- **Skip on failure, never abort the batch.** One wedged
  scenario shouldn't kill the other 74.
- Log each duel to its own file, plus a one-line summary
  per scenario in a batch log.

## Logging like you mean it

Every duel produces a transcript file with:

- The exact scenario (topic, roles, turn count, settings)
- Every reply, in order, with per-turn token counts and
  tok/s
- Code-run results, if the scenario executes code
- The judge verdict (if any) and the session-end marker

**Check:** the end of a healthy log:

```
=== Model performance ===
Model         Turns  Gen tok/s  Prompt tok/s  Tokens  Truncated
qwen3:1.7b        4        2.2           4.3     614          0
smollm2:1.7b      4        2.5           4.2     503          0
--- session ended 2026-10-14 16:15:22 (8 replies) ---
```

If a duel "stopped early," the log says *why* — a batch you
can't debug from its logs is a batch you can't trust.

## Operating a long batch

- **Timeouts beat babysitting.** Every layer gets one: per
  turn, per code-run, per scenario.
- **Notifications on completion.** A push notification when
  each duel finishes (ntfy, or any webhook) means you check
  results, not processes.
- **Thermal awareness.** Sustained inference heats small
  machines; pause between turns above a temperature
  threshold rather than cooking the box all week.
- **One batch at a time.** Two concurrent batches on one
  machine contend for the same model slot and produce
  garbage results. It's always slower than you hope and
  never as parallel as you think.

## Keep the harness boring

The harness is infrastructure, not the experiment. Stable
scenario files, stable settings, append-only logs. When a
result surprises you, you want the *only* explanation to be
"the models differ" — not "did I change the temperature
last Tuesday?"
