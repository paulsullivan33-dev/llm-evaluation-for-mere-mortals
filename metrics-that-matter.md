# Metrics That Matter

Numbers from an eval are only useful if you know what they
mean and how much to trust them.

## Win rate

The headline metric of duels: out of N matchups, how often
did A beat B?

**The honest version:** a win rate from 3 duels means
nothing. Rough guide:

| Matchups | What a 70% win rate means |
|---|---|
| 3 | noise — could easily be luck |
| 10 | suggestive — worth a closer look |
| 30+ | a real signal |

You don't need the statistics textbook; you need the habit:
**close calls get rematches.** If A beats B 6–4, run ten
more before declaring anything.

## Tokens per second

Two numbers, both useful:

- **Generation tok/s** — how fast the model writes. This is
  the "does it feel fast" number.
- **Prompt tok/s** — how fast it *reads* context. Matters
  when transcripts get long.

**Check:** real numbers from small models on modest
hardware:

```
qwen3:1.7b   4 turns   2.2 gen tok/s   4.3 prompt tok/s
smollm2:1.7b 4 turns   2.5 gen tok/s   4.2 prompt tok/s
```

Measure on *your* hardware — tok/s numbers from someone
else's GPU tell you nothing about your box. And measure with
the same context length you'll actually use; a model at
512 tokens of context and at 4096 are different machines.

## Truncation

Did any reply hit the token limit and get cut off? A
truncated reply isn't a full answer — track the count. If
one model truncates constantly and the other doesn't, your
`max_tokens` setting is deciding the duel, not the models.

## Cost per eval

For API models: tokens in × price + tokens out × price, per
duel. For local models: wall-clock time × your patience.
Either way, know it — it decides how many rematches you can
afford, which decides how certain your conclusions are.

## The one statistical idea worth having

**Variance.** Model outputs are random samples; every score
is a sample, not the truth. The practical consequences:

- Run the *same* matchup more than once before trusting it.
- Distrust big claims from small N ("undefeated in 4
  duels!" is a coin flip away from 2–2).
- When comparing across days, keep everything else fixed:
  same scenario files, same temperatures, same judge.

You don't need p-values for home evals. You need the
discipline to say "not enough data" — which is itself the
most underused metric in the field.
