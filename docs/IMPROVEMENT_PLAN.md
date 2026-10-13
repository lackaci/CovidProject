# Improvement plan

Shared checklist for the agent team. Agents tick items and add a one-line note. Work top to bottom:
**Hygiene -> Data -> Evaluation -> Notebook polish -> Quality gate -> Final review.**

Context: an external review scored the repo about 4/10. The notebook's analysis is careful (chronological
split, train-only scaling, multi-seed), but the repo is not reproducible, results are not benchmarked
against baselines, and there are no checks or CI.

Legend: `[ ]` todo, `[x]` done. Owner role in brackets.

## 1. Repo hygiene  [repo-hygiene]
- [x] Write `README.md`: what it does, headline result (2 sentences), setup, how to run, data sources, limitations, license — Added project scope, provisional result status, setup/run instructions, verified ECDC sources, limitations, license, and result-figure TODO.
- [x] Add `requirements.txt` with pinned versions (note Python version; TensorFlow wheel availability decides 3.12 vs 3.13) — Pinned notebook imports and `nbstripout` from the active environment; documented Python 3.11 verification and the notebook's Python 3.13.2 metadata.
- [x] Add `.gitignore` (`Data/`, `.venv/`, `.ipynb_checkpoints/`, `__pycache__/`) — Added local data, virtual-environment, notebook-cache, Python-cache, and OS-junk rules.
- [x] Rename `_gitattributes` -> `.gitattributes`; remove the unrelated `.github/workflows/*.lock.yml` line — Kept the correctly named `.gitattributes` and replaced the stale workflow rule with `* text=auto`.
- [x] Data: add a load cell that downloads/caches both ECDC files into `Data/` (verify real URLs; never guess). If not possible, document manual steps in README — Added cache-aware downloads using verified official ECDC CSV URLs and documented the sources in README.
- [x] Strip notebook outputs (nbstripout) so the leaked local path `c:\Users\...` disappears from history going forward — Cleared all outputs and execution counts, removed personal paths, and installed the nbstripout Git clean filter.
- [ ] Add a results figure under `docs/img/` and reference it from the README

## 2. Data correctness  [data-auditor]
- [x] Stop `fillna(0)` on missing deaths; count them, decide (drop / indicator column), document — Kept a `deaths_missing` flag and printed the missing count before any model-ready numeric fill; the unknown death count is no longer silently changed into zero.
- [ ] Fix policy-to-week alignment: a measure active any day in a Mon-Sun week counts for that week (current `W-MON` expansion misses weeks where an interval starts after Monday or lives inside one week)
- [ ] Handle week gaps: split or reindex so sequence windows never span a gap (currently only a warning)
- [ ] Count and report negative ECDC corrections before `clip(lower=0)`
- [ ] Define direction when the rate is unchanged (ties currently count as "down"); report tie share
- [ ] Fix the meaningless "Deaths available: True" check
- [ ] Add data sanity-check cells (duplicates, populations, ranges, country name matches)

## 3. Evaluation  [model-evaluator]
- [x] Add baselines: persistence (same as last week), mean of last 2 and last 4 weeks, simple ridge/AR on lagged log-rates — Added persistence, rolling means, and fixed-alpha ridge baselines on lagged log-rates.
- [x] Put ALL models in one table, same units (cases per 100k), include bias and MAE relative to persistence — Added a common cases-per-100k table with bias, median error, direction metrics, persistence-relative MAE, and seed spread.
- [x] MLP: replace the random early-stopping validation split with a chronological one; resolve the non-convergence warning; stop printing raw-count MAE next to per-100k MAE — Added chronological validation and training-target standardization; the reported comparison uses per-capita units.
- [x] Investigate two-headed LSTM direction AUC = 0.463 (below chance) and write down the explanation — Added an evaluation note covering class ties, the 0.5 threshold, regime shift, and the need for full-seed confirmation.
- [ ] Study the regime shift: split test period before/after the Omicron wave, report both
- [ ] Review `magnitude_cap` (99th percentile of train changes), which cannot represent a surge bigger than anything seen in training
- [ ] Let test windows use the last LOOKBACK training weeks as context instead of discarding the first 12 test weeks per country
- [ ] Rolling-origin backtest with 3+ cutoffs (fast mode: fewer)
- [ ] Forecast plots must use the seed-averaged prediction (with spread), not the last seed
- [x] Write a plain-language **Conclusion** section stating what the evidence supports, including negative results — Added a conclusion that labels FAST metrics as provisional and does not claim neural models beat persistence.

## 4. Notebook polish  [notebook-polisher]
- [ ] Fix section numbering (two "4a"), update the outline cell, add a "Results at a glance" cell at the top
- [ ] Add config cell with `FAST` mode (1 seed, ~2 epochs) driven by `COVID_FAST=1`
- [ ] Remove dead/stale code: unused `prev2`, stale "original regular LSTM" comment, no-op rename in `plot_lstm_predictions`, the `main = {...}` dict, duplicate `SEED`/`SEEDS`
- [ ] Compute `prepare_lstm_data` once and reuse for both LSTMs
- [ ] Merge `score_forecast` and `score_direct_forecast` into one function
- [ ] Split the big comparison cell into fit / score / plot cells
- [ ] Replace `except Exception: pass` in the plotting helper; slim the 18-argument plot function
- [ ] Replace the `prep[...]` dictionary with a `@dataclass`; add type hints to functions
- [ ] Add a library-versions cell

## 5. Quality gate  [quality-gate]
- [ ] "Sanity checks" cells with asserts: no target leakage in `make_sequences` (synthetic series), validation weeks strictly after training weeks, `balanced_weights` gives equal class weight, policy expansion handles mid-week and same-week intervals, no train/test week overlap
- [ ] Pre-commit with nbstripout (and ruff via nbqa)
- [ ] GitHub Actions workflow: install, lint notebook, run notebook in FAST mode (or lint + stripped-output check only if data cannot be fetched in CI)
- [ ] CI badge in README

## 6. Final review  [reviewer]
- [ ] Re-run the  review (team lead) and compare scores with the original (about 4/10)
- [ ] List anything still open and decide what is "good enough"
