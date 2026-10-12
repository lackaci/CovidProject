# Project context (read this first, every time)

**Project:** COVID-19 weekly case forecasting (ECDC data, 2020-2022) with government-measure features.
Models: MLP, one-headed LSTM, two-headed (direction + magnitude) LSTM.

**Single source of truth:** `covid_project_clean.ipynb`. Keep the project **notebook-first**.
Do NOT create `.py` modules or a package unless the owner explicitly asks.

## Who you are working with
The owner is a physicist fresh out of university. They are comfortable with notebooks, maths and
numerical thinking, but new to software-engineering practice (git workflow, CI, testing, packaging).
- Explain *why* in plain language, not just *what*. One or two sentences per change is enough.
- Prefer physics-style analogies where they genuinely help (e.g. "a persistence baseline is the
  'null hypothesis' every model must beat").
- Never assume knowledge of tooling. Give exact commands and say what they do.

## How to work
1. Read `docs/IMPROVEMENT_PLAN.md`. Work only on unchecked items that belong to your role.
2. Work on a branch named `improve/<short-topic>`. Never commit to `main`, never force-push.
3. Make small, focused changes. One topic per commit, message like `data: stop zero-filling missing deaths`.
4. When finished, tick the item in `docs/IMPROVEMENT_PLAN.md` and add a one-line note on what changed.
5. Ask before: deleting cells with results, changing the train/test cutoff, adding a dependency,
   or anything that changes reported numbers. Say clearly when numbers changed and why.

## Notebook-editing rules
- Edit through the notebook cell-editing tools. **Never hand-edit raw `.ipynb` JSON.**
- Keep "Run All" working from top to bottom on a fresh kernel. Run it (in fast mode) before finishing.
- Outputs are stripped before commit (nbstripout). Never commit absolute local paths or usernames.
- Details: `.github/instructions/notebook.instructions.md`.

## Scientific rules (non-negotiable)
- Splits are **chronological only**. Validation weeks come after training weeks; test after validation.
- Fit every scaler/imputer on training rows only.
- No information from the target week may appear in the inputs.
- Every model is compared against **naive baselines** (at least "same as last week") in the **same units** (cases per 100k).
- Report bias and variance across seeds, not a single run.
- Reported cases are not causal evidence about policy. Never imply causation.
- Report negative results honestly. "No model beat persistence" is a valid, publishable conclusion.

## Definition of done
Notebook runs top-to-bottom in fast mode; sanity-check cells pass; plan item ticked; changes explained
in plain language; branch ready for the owner to review as a pull request.

## Learning log (this project is also a learning project)
The owner wants to understand the *workflow and the practices*, not just receive finished changes.
After completing a plan item, decide if it is an **important step**. It is important if it:
- changes reported numbers or the scientific conclusion,
- introduces a new practice or tool the owner has not used (branching/PRs, CI, pre-commit, dataclasses, asserts as tests, baselines),
- fixes a subtle bug whose cause is instructive (leakage, silent imputation, off-by-one in time alignment).

Do NOT log routine edits (renames, formatting, typo fixes, tick-box updates).

For each important step, append one entry to `docs/LEARNING_LOG.md` using the template at the top of that file.
Keep entries short (under about 150 words). Name the practice using its standard term so the owner can search for it.
Never invent facts: if you are unsure why something is best practice, say so. The entry ends with a
"Check yourself" line the owner fills in with their own words.
