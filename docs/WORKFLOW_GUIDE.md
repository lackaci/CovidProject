# Workflow guide: why the agents work the way they do

This explains the process and the engineering practices behind it. It is meant to be read once, then used as a reference.
It was written by an AI assistant, so treat it as a starting point and check claims that matter against the sources named in each section.

## The loop

```
/next-task  ->  project-lead picks a task and a specialist
      |
      v
specialist works on its own git branch  ->  you run the notebook (fast mode)  ->  read the diff
      |
      v
commit -> push -> pull request on GitHub -> you merge
      |
      v
important step? -> entry in docs/LEARNING_LOG.md     (then back to /next-task)
```

## Why this order of agents
| Order | Agent | Why here |
|---|---|---|
| 1 | repo-hygiene | Anyone (including you in six months) must be able to run the project. Reproducibility comes before any science. |
| 2 | data-auditor | Garbage in, garbage out. Fix data errors before measuring models, otherwise every number is recomputed later. |
| 3 | model-evaluator | Once the data is right, test whether the models are actually good. |
| 4 | notebook-polisher | Restructure only after the content is settled, otherwise cells are rearranged twice. |
| 5 | quality-gate | Lock in correctness with automatic checks so future edits cannot silently break it. |
| 6 | reviewer | An independent read-only check, like peer review. |

## Practices in use, and the reasoning

**Reproducibility (README, pinned requirements, data download cell, seeds).**
A result nobody else can rerun is an anecdote. Pinning versions is like recording the apparatus and calibration.

**Git branches, small commits, pull requests.**
Each task lives on its own branch so `main` always works. Small commits make it possible to find which change broke something
(like changing one parameter at a time). A pull request is where you review before accepting.

**Baselines / null model.**
A forecasting model is only meaningful if it beats a trivial predictor such as "next week equals this week" (persistence).
This is the null hypothesis. Without it, an error number has no scale.

**Chronological splits and data leakage prevention.**
Randomly mixing weeks lets the model peek at the future. Scalers are fitted on training data only for the same reason:
anything computed from test data is information the model would not have had in real use.
This is the most common way ML results look better than they are.

**Common units and honest reporting.**
Comparing models in different units (raw counts vs per 100k) is a category error. Reporting mean and spread over several seeds
shows how much of a result is luck. Stating negative results plainly is part of scientific integrity.

**Silent failures vs loud failures.**
`fillna(0)` on missing deaths turns "unknown" into "zero" without any warning. A good pipeline fails loudly or reports what it did.
This is why the data-auditor measures, fixes, verifies and reports each problem.

**Assertions as tests.**
A test is an experiment with a known answer. Example: a synthetic series where each value equals its index lets you assert that the window
for target `t` only contains indices below `t`. If someone later breaks the code, the check fails immediately.

**Continuous integration (GitHub Actions).**
A server reruns the checks on every push in a clean environment. It catches "works on my machine" problems.

**Pre-commit hooks and nbstripout.**
Notebooks store outputs and local paths inside the file, which pollutes Git history and leaks information (your Windows username was visible).
nbstripout removes outputs on commit. Pre-commit runs such cleanups automatically.

**Separation of concerns (one agent per role).**
Each agent has a narrow job and explicit boundaries (Always / Ask first / Never). That keeps changes reviewable and reduces the chance an agent
"fixes" something it was not asked to touch. The reviewer is read-only on purpose: the one who judges should not also edit.

**Definition of done.**
A task is finished only when the notebook runs top to bottom, checks pass, the plan is ticked and the change is explained.
Agreeing on this up front prevents "almost done" work.

## Glossary with physics analogies
- **Baseline**: null hypothesis the model must beat.
- **Data leakage**: using information that would not exist at measurement time, a systematic error that fakes a signal.
- **Overfitting**: fitting noise; excellent on training data, poor on new data (train R2 0.93 vs test R2 0.22 in this project).
- **Regime shift**: the system enters a state not covered by calibration data (Omicron vs earlier waves).
- **Seed**: initial condition of the random number generator; results across seeds show run-to-run variance.
- **CI**: an automated lab technician rerunning your control experiments after every change.

## Using this to actually learn
- Read each `LEARNING_LOG.md` entry, then write the "Check yourself" line in your own words before merging. If you cannot, ask the agent to explain differently.
- After each merged branch, try to predict what the next fix will be before running `/next-task`.
- Verify one claim per week against a primary source (scikit-learn docs, Git docs, GitHub Actions docs). The agents can be wrong, and checking is itself a skill.
