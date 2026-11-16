# First Databricks Challenge: prepare the next team's case-selection dataset

**In short.** You receive two batches of synthetic returns. Reject the faulty batch with data
checks, build one ratio feature from the corrected batch, split the result by year into training,
validation and test tables, and write a short note for next week's modelling team. Codex does the
typing in Databricks; you make the decisions and check the results.

## Where to start

1. **Set up and run the practice test.** It's ungraded; finish it before Challenge day if you can.
   [START_HERE.md](START_HERE.md)
2. **Do the task**, once your trainer opens the Classroom assignment.
   [CHALLENGE.md](CHALLENGE.md)
3. **Save your Codex conversation and turn in.**
   [SESSION_RECORD.md](SESSION_RECORD.md)

## What you hand in

The full checklist is in [CHALLENGE.md](CHALLENGE.md#what-you-hand-in). In brief:

- your private repository's base URL, branch `main` and final commit SHA, in Classroom;
- in that repository, written by the script and never edited by hand: `notebook.py`, `README.md`
  and small files under `outputs/`;
- your saved Codex conversation, on the branch `entire/checkpoints/v1`;
- the reviewer `TakalaWang` invited to the repository.

Automatic scoring is not turned on for this assignment. Your trainer explains how the work is
reviewed.

## Why the unusual rules

- **Write and run everything inside Databricks, and submit with the script.** That way the work
  that's reviewed is exactly the notebook that ran, with its real results. A notebook written on
  your laptop and pasted in can't show that.
- **Save your genuine Codex conversation.** It shows the decisions you made, which is part of what
  is reviewed. Codex's replies give context; they don't replace your own judgment.

## Words used in this repository

| Word | Meaning here |
|---|---|
| native | made and run inside Databricks, not on your Workstation |
| the script | `tools/databricks_platform.py`, which runs your notebook in Databricks and commits its exports to GitHub |
| handoff | the `README.md` your notebook writes for the team that uses your data next |
| receipt | the script's record of a run or a commit, kept in the ignored `.platform/` folder |
| practice test (the "connection smoke" or "round trip") | the ungraded walkthrough in START_HERE.md that checks you can read, write and submit |
| MCP | the connection Codex and the script use to reach Databricks and GitHub |
| Entire, checkpoint | the tool that saves your Codex conversation, and one saved copy of it |

When you first submit, the script replaces this README with your own handoff. `START_HERE.md`,
`CHALLENGE.md` and `SESSION_RECORD.md` stay in your repository.
