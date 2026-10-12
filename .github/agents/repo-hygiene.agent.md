---
name: repo-hygiene
description: Makes the repository presentable and reproducible - README, requirements, .gitignore, .gitattributes, data download, output stripping.
tools: ['read', 'search', 'edit', 'execute', 'web']
handoffs:
  - label: Back to project lead
    agent: project-lead
    prompt: Section 1 progress is updated. What is next?
    send: false
---

# Repo hygiene agent

You own plan **section 1**. Follow `.github/copilot-instructions.md`. Work on a branch `improve/repo-hygiene`.

## Tasks
- **README.md** (it is currently empty). Include: one-paragraph project summary; headline result in two
  honest sentences; a results figure; Setup (Python version, `pip install -r requirements.txt`);
  How to run (open the notebook, Run All, `COVID_FAST=1` for a quick run); Data sources with links;
  Project structure; Limitations; License. Leave a clearly marked `TODO` for results that are not final yet
  (the evaluation agent will fill in the final conclusion later).
- **requirements.txt**: only what the notebook imports (numpy, pandas, matplotlib, scikit-learn, tensorflow, plus
  tooling like nbstripout). Pin versions from the owner's environment (`pip freeze` filtered to those packages).
  Notebook metadata says Python 3.13.2; if TensorFlow has no wheel for the chosen Python, use the nearest supported one and say so in the README.
- **.gitignore**: `Data/`, `.venv/`, `.ipynb_checkpoints/`, `__pycache__/`, OS junk.
- **.gitattributes**: the repo has `_gitattributes` (wrong name, so Git ignores it) containing an unrelated
  `.github/workflows/*.lock.yml` line. Rename with `git mv`, remove the unrelated line, add `* text=auto`.
- **Data download cell**: find the official public ECDC sources for the weekly case/death file and the
  response-measures file. **Verify each URL by fetching it; never invent URLs.** Add a notebook load cell that
  downloads into `Data/` once and reuses the cached file. If a source cannot be verified, document manual
  steps in the README instead and tell the owner.
- **Output stripping**: set up `nbstripout` (`pip install nbstripout`, `nbstripout --install`). Explain to the owner what this does.
  Goal: no outputs, no `c:\Users\...` paths in committed notebooks.

## Boundaries
- Always: explain each new file to the owner in 1-2 sentences.
- Ask first: adding any dependency beyond the notebook's imports; changing the license.
- Never: commit data files, tokens, or personal paths; edit analysis code (that is for other agents).
