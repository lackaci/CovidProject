# Using the agent team in VS Code

## One-time setup
1. Copy the `.github/` folder and the `docs/` folder into the root of your repo (next to `covid_project_clean.ipynb`).
2. Open the repo in VS Code with GitHub Copilot Chat installed and signed in. Keep VS Code and the extension up to date.
3. Open Copilot Chat. The agent picker (dropdown at the top of the chat) should list: `project-lead`, `repo-hygiene`,
   `data-auditor`, `model-evaluator`, `notebook-polisher`, `quality-gate`, `reviewer`.
4. Create a branch for each round of work (the agents will name it for you), for example `git switch -c improve/repo-hygiene`.

## Daily flow
1. Type `/next-task` in chat. The project lead tells you what to do next and shows a button to hand over to a specialist.
2. Let the specialist work. Read what it changes; use `/explain-change` if anything is unclear.
3. Run the notebook yourself (`COVID_FAST=1` for a quick run) and check it still works.
4. Commit, push the branch, open a pull request on GitHub, and merge it when happy.
5. If the step was important (changed results, new practice, instructive bug), run `/log-learning` and write the "Check yourself" line yourself.
6. Back to step 1. When the plan is mostly ticked, run `/review-repo`.

Read `docs/WORKFLOW_GUIDE.md` once first: it explains why the agents work this way.

## Good habits
- Only one agent at a time. Finish and merge before starting the next.
- If an agent changes your numbers, it must tell you why. If it does not, ask.
- You stay in charge. If you do not understand a change, do not merge it. Ask the agent to explain it.

## If something does not show up
- Agents must be in `.github/agents/` and end in `.agent.md`.
- If a `tools:` entry is flagged in the file, tool names vary between VS Code versions. Open the agent file and use the
  tools button in chat to re-pick the equivalents (read, edit, search, run commands, notebooks, web).
