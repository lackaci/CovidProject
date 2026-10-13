# Learning log

Agents add an entry here after each **important** step (changes results, teaches a new practice, or fixes an instructive bug).
Routine edits are not logged. Read `docs/WORKFLOW_GUIDE.md` for the big picture.

## Entry template
```
### <date> - <short title>  (agent: <name>, branch: <branch>)
**What changed:** one or two sentences.
**Why:** the problem it fixes, and what was wrong before.
**Practice used:** standard name (e.g. "baseline comparison", "data leakage prevention", "continuous integration").
**Effect on results:** numbers before -> after, or "none".
**Physics analogy:** (optional, only if it genuinely helps)
**Check yourself:** (you write this: explain the idea in your own words, one or two sentences)
```

## Entries
<!-- Agents append below this line -->

### 2026-10-12 - Strip notebook outputs  (agent: repo-hygiene, branch: fixing-inconsistency)
**What changed:** Removed saved outputs, execution counts, and the local user path from the notebook. Installed `nbstripout` as a Git clean filter so future notebook commits are stripped automatically.
**Why:** Stored outputs made the notebook noisy and exposed a machine-specific path, which hurts reproducibility and privacy.
**Practice used:** Output stripping with `nbstripout`.
**Effect on results:** none.
**Check yourself:** Explain why notebook outputs should be regenerated rather than treated as source code.

### 2026-10-12 - Benchmark neural forecasts against null models  (agent: model-evaluator, branch: fixing-inconsistency)
**What changed:** Added persistence, rolling-mean, and lagged-log-rate ridge baselines, chronological MLP validation, common cases-per-100,000 metrics, and seed-averaged LSTM forecasts. The full five-seed run found ridge MAE 145.664, persistence 174.249, MLP 184.831, two-headed LSTM 245.494, and raw LSTM 557.223.
**Why:** A complex model is only useful if it beats a simple null hypothesis in the same units and on the same chronological test period.
**Practice used:** Baseline comparison and chronological validation.
**Effect on results:** The conclusion is now explicit: ridge beat persistence (MAE ratio 0.836); neither neural model did.
**Physics analogy:** Persistence is the null hypothesis: extra model structure must explain enough signal to reduce the residuals.
**Check yourself:** Explain why a model with a lower raw-count error is not automatically better when countries have different populations.

### 2026-10-13 - Explicit missingness for deaths  (agent: data-auditor, branch: improve/data-correctness)
**What changed:** Added a `deaths_missing` flag and printed the count before any numeric fill, instead of silently converting missing deaths to zero in the case dataset. The model-ready frame now retains the missingness signal and documents that the fill happens only in a visible downstream feature step.
**Why:** Missing data and real zero deaths are not the same thing. Treating the unknown as zero can fake stable or low-death weeks and distort the modelling target and historical features.
**Practice used:** Missing-data handling and explicit feature engineering.
**Effect on results:** none in the current sample; the fix makes the data integrity visible instead of silently changing the numbers.
**Physics analogy:** A missing measurement is like an unobserved meter reading, not a zero reading. You cannot infer the system is at rest unless you actually measured it.
**Check yourself:** Explain the difference between “No deaths were reported” and “Deaths were not reported.”
