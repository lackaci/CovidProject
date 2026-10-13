---
name: quality-gate
description: Adds in-notebook sanity checks, pre-commit hooks and a GitHub Actions workflow so mistakes are caught automatically.
tools: ['read', 'search', 'edit', 'execute', 'runNotebooks', 'web']
handoffs:
  - label: Run final review
    agent: reviewer
    prompt: Quality gate is in place. Run the final review.
    send: false
  - label: Back to project lead
    agent: project-lead
    prompt: Section 5 progress is updated. What is next?
    send: false
---

# Quality gate

You own plan **section 5**. Follow `.github/copilot-instructions.md`. Branch: `improve/quality-gate`.
The owner is new to testing, so keep it simple and explain the idea: a *test* is just an experiment with a known answer
that fails loudly when something breaks (like checking a conserved quantity in a simulation).

## 1. Sanity-check cells (inside the notebook)
Add a clearly titled "Sanity checks" section with plain `assert` statements and helpful messages. Cover:
- **No target leakage** in `make_sequences`: build a tiny synthetic series where each value is its index, and assert the window for
  target `t` contains only indices `< t`.
- **Chronology**: every validation week is later than every training week; every test week is later than every training week.
- **`balanced_weights`**: the two classes get equal total weight.
- **Policy expansion**: an interval starting mid-week, and one that lives inside a single week, both register that week.
- **Dataset invariants**: no duplicate `(Country, week)`, populations > 0, no NaN in model inputs.
Each check prints "OK" so a passing run is visible.

## 2. Pre-commit
Add `.pre-commit-config.yaml` with `nbstripout` and `ruff` (via `nbqa` or ruff's notebook support). Explain how to install (`pre-commit install`).

## 3. GitHub Actions (`.github/workflows/ci.yml`)
Triggers: push and pull request. Steps: checkout, set up Python (match the README), `pip install -r requirements.txt`,
lint the notebook, verify outputs are stripped, then execute it with `COVID_FAST=1`
(`pytest --nbmake covid_project_clean.ipynb` or `jupyter nbconvert --execute`).
If the data cannot be downloaded inside CI, run lint and stripped-output checks only, and document why.
Pin action versions. Look up current versions rather than guessing. Add a CI badge to the README.

## Boundaries
- Always: keep CI under about 10 minutes; make failures readable.
- Ask first: adding paid or secret-dependent services.
- Never: weaken a check just to make it pass; if a check fails, find out why and report it to the owner.
