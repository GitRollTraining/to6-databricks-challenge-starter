# Prepare the next team's case-selection dataset

**In short.** Reject `returns_initial` with data checks and keep the evidence. Build
`return_features` from `returns_corrected`, split it into one table per year, and write a short
handoff README for the modelling team. You don't train a model, and there are no target labels.

The modelling team starts next week. A delivery of synthetic return records has arrived, but
the data owner warns that the first batch contains mistakes. Deliver a checked feature dataset:
reject the faulty batch, assess its replacement, and prepare separate training, validation and
test years. Your notebook and data rules will support later modelling and capstone work.

**You've practised each part this week.** Understanding the input is Monday's catalog and lineage
work. Rejecting the batch is Tuesday's exercise 1 (tests that fail the dirty batch). The feature
table is Wednesday's exercise 2 (a ratio, null when the bottom number is 0), and the split is
Wednesday's exercise 4. Driving Databricks through Codex builds on Thursday. Watch the
differences: the table names, column names and years here are not Wednesday's.

## Work in Databricks

Use Codex to direct and review the work, through the connection set up in
[START_HERE.md](START_HERE.md). Write one notebook and its handoff files in Databricks, run them on
Databricks compute, and submit the files the script exports to GitHub. Don't write or run a
solution on your Workstation and then import it.

Your trainer provides the workspace, compute, read-only inputs, your own writable output schema,
this starter with its connection, read and write examples, and the way to save your Codex
conversation. Create your own private GitHub repository from this starter and use its `main`
branch. Finish the ungraded practice test before starting. If access is missing, ask the trainer;
setting up accounts, compute or MCP is not part of this task.

## Supplied data

`returns_initial` (the faulty first batch) and `returns_corrected` (its replacement) are already
parsed synthetic tables covering tax years 2022–2024, in the read-only schema
`training.to6_challenge3_inputs`. One row is one return. `return_id` identifies the return;
`taxpayer_id` identifies its filer, who may have returns in several years. The required fields are
`return_id`, `tax_year`, `income` and `deduction`. Each return must occur exactly once. Amounts
are never negative; zero income is valid. You don't need to build an XML parser.

## The four outcomes

Work toward these four connected outcomes. How you implement them is up to you.

1. **Understand the input.** Use the Catalog descriptions, at least three sample rows and the
   supplied lineage view (`source_lineage`) to establish what one row is, which fields matter,
   which column identifies a row, and where the data came from. Keep a short source reference and
   explanation in the notebook.
2. **Decide which batch is usable.** Checks for required values and for uniqueness must reject
   the initial batch before any feature output is written. Keep the affected records and the
   reason for rejection. Assess `returns_corrected` with the same rules and use it for the final
   run. Removing checks, replacing unknown amounts with zero, or quietly dropping records is not
   an acceptable correction. Explain the failure and your next step using the actual records and
   rule; related observations may be in your Codex conversation.
3. **Deliver a checked feature table.** Save `return_features` with exactly `return_id`,
   `tax_year` and `deduction_to_income`, where the ratio is deduction divided by income. A
   zero-income return stays in the table with a null ratio. Keep every accepted return exactly
   once. Leave out the filer identifier and the raw amounts: the model next week doesn't need
   them, and data a use doesn't need stays out (Monday's need-to-know rule). Check one nonzero
   ratio against a value you work out from the source, and read the saved table back, including a
   zero-income case.
4. **Make the handoff usable.** From the saved features, save `features_train` for 2022,
   `features_validation` for 2023 and `features_test` for 2024. Every accepted return must appear
   in exactly one of these tables, in its own year. Running the notebook twice, both times
   successfully, must leave the same return IDs and values, with no appended copies. Write a short
   handoff naming the inputs, the outputs, the ratio's definition, the checks you ran and anything
   you couldn't verify.

These checks set this task's data rules. They don't prove that every possible source of model
leakage is gone. Later modelling work must add suitable labels and check which information is
available on the day a prediction is made.

## Before you submit, check

- [ ] The initial batch's failed run, its affected records and the rule they broke are in your
      handoff.
- [ ] `return_features` has exactly `return_id`, `tax_year` and `deduction_to_income`.
- [ ] A zero-income return is present, with a null ratio.
- [ ] One nonzero ratio is checked against a value you worked out from the source.
- [ ] Every accepted return is in exactly one of `features_train`, `features_validation` and
      `features_test`, in its own year.
- [ ] Running the notebook a second time leaves the same rows, with no duplicates.
- [ ] The handoff names the inputs, outputs, ratio, checks, run references and what you couldn't
      verify, and lists which `outputs/` files belong to the final delivery.

## Submit to GitHub

From Databricks, export the notebook as readable source, and produce `README.md` plus small
synthetic output files. Include the source and output table names and the actual run references,
and keep the initial rejection evidence. Use the script (`submit`) to commit these files to your
personal repository's `main` branch, without editing them on your Workstation. Check that the
exact submitted commit contains the notebook, the handoff and the exports.

Save your genuine Codex session as [SESSION_RECORD.md](SESSION_RECORD.md) describes. The review
looks at your own decisions and behaviour there (the Ownership and Trait assessment); Codex's
replies and tool activity give context, not a substitute for your own judgment. Don't rebuild a
conversation afterwards or include credentials. Genie logs are not required, and screenshots
can't replace the code, the saved outputs or the conversation record.

## What you hand in

- [ ] In Classroom: your repository's base URL (`https://github.com/{owner}/{repo}`), the branch
      `main`, and the final commit SHA (in a private comment). Then select **Turn in**.
- [ ] In your repository, committed by `submit`: `notebook.py`, `README.md` (your handoff) and the
      files under `outputs/`, including `outputs/native-run.json`.
- [ ] Your saved Codex conversation on the branch `entire/checkpoints/v1`
      ([SESSION_RECORD.md](SESSION_RECORD.md), steps 3–5).
- [ ] The reviewer `TakalaWang` invited to your private repository, and the invitation accepted.

Don't add the commit SHA to your handoff: a file can't name the commit that contains it. Put the
SHA in Classroom instead.
