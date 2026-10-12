---
description: 'Conventions for editing the Jupyter notebook in this project'
applyTo: '**/*.ipynb'
---

# Notebook conventions

## Structure
- Order: title + results-at-a-glance, config, load data, descriptive plots, build dataset,
  baselines, models, evaluation, conclusion, limitations.
- Every section starts with a markdown cell: what it does, what it assumes, what it produces.
- Number sections once, consistently (1, 2, 3 ...). No duplicate numbers. Keep the outline cell in sync.
- Definitions (functions) live in cells separate from experiments (fit/score/plot cells).
- No cell should both fit models, compute metrics and draw plots. Split them.

## Config
- One config cell near the top holds paths, seeds, cutoff, lookback, epochs.
- `FAST = os.environ.get("COVID_FAST") == "1"` switches to 1 seed and ~2 epochs for quick checks and CI.
- Paths are relative to the repo root. No absolute or user-specific paths anywhere.

## Code style
- Small single-purpose functions with docstrings. Add type hints to new/edited functions.
- Replace dictionaries-of-arrays that get passed around (like `prep["X_train"]`) with a small `@dataclass`.
- No `except Exception: pass`. Catch specific errors and say what happened.
- No dead code, no stale comments, no duplicated computation (compute once, reuse).
- Use `assert` with a helpful message for invariants, in clearly marked "Sanity checks" cells.
- Warnings that appear in outputs must be fixed or explicitly explained in a markdown cell.

## Reproducibility
- Set seeds explicitly (`tf.keras.utils.set_random_seed`, numpy, sklearn `random_state`).
- Print library versions in one cell.
- Do not rely on files not in the repo. Data is downloaded or cached by the load cell and listed in `.gitignore`.

## Commit hygiene
- Outputs stripped on commit (`nbstripout`). Figures that matter go to `docs/img/` and are referenced from the README.
