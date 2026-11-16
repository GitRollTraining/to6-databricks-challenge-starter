# Prepare the next team's case-selection dataset

The modelling team starts next week. A delivery of synthetic return records has arrived, but
the data owner warns that the first batch contains mistakes. Deliver a checked feature dataset:
reject the faulty batch, assess its replacement, and prepare separate training, validation and
test years. Your notebook and data contract will support later modelling and capstone work;
this task does not train a model or supply its target labels.

## Work in Databricks

Create, run and check your work in Databricks; do not develop a solution locally and import it.
Use the browser walkthrough in [START_HERE.md](START_HERE.md). Genie Code is optional.
The training team supplies compute, read-only inputs, Catalog descriptions, a lineage view and
an individual writable output schema. Missing access is an environment issue for the trainer.
The ungraded practice checks your connection; it is not part of the task's assessment.

## Supplied data

`returns_initial` and `returns_corrected` are already parsed synthetic tables covering 2022–2024.
One row represents one return. `return_id` identifies the return; `taxpayer_id` identifies its
filer, who may have returns in several years. Required fields are `return_id`, `tax_year`,
`income` and `deduction`. Each return must occur exactly once. Amounts are nonnegative;
zero income is valid. You do not need to build an XML parser.

## Task and judgment requirements

Work toward these four connected outcomes. Choose the implementation yourself.

1. **Understand the input.** Use Catalog descriptions, at least three sample rows and the
   supplied lineage view to establish the row meaning, relevant fields, uniqueness identifier
   and upstream source. Keep a concise source reference and explanation in the notebook.
2. **Decide which batch is usable.** Completeness (required-value) and uniqueness checks must reject the
   initial batch before feature output is written. Preserve the affected records and the reason
   for rejection. Assess `returns_corrected` with the same rules and use it for the final run.
   Removing checks, replacing unknown amounts with zero or silently discarding records is not
   an acceptable correction. Explain the failure and your next action using the actual records
   and rule in your submitted notebook or evidence files.
3. **Deliver a checked feature table.** Save `return_features` with exactly `return_id`,
   `tax_year` and `deduction_to_income`, where the ratio is deduction divided by income.
   A zero-income return stays in the table with a null ratio. Keep every accepted return once;
   exclude the filer identifier and raw amounts under the course's need-to-know rule. Verify a nonzero result against an expected
   value you derive from the source, and check the saved table by reading it back, including a
   zero-income case.
4. **Make the handoff usable.** From the saved features, save `features_train` for 2022,
   `features_validation` for 2023 and `features_test` for 2024. Every accepted return must appear in exactly one of these
   tables, in its assigned year. Two complete successful runs must leave the same return IDs
   and values, without appended copies. Provide a short handoff naming inputs, outputs,
   the ratio definition, checks performed and anything you could not verify.

The checks above establish this task's data contract; they do not prove that every possible
source of model leakage has been eliminated. Later modelling work must add suitable labels
and assess which information is available at prediction time.

## What to submit and how it is checked

Your repository's `main` branch must contain:

- `notebook.py`: readable notebook source with your implementation, source references,
  checks, initial rejection and saved-table readback evidence.
- `SUBMISSION.md`: inputs and outputs by full table name, ratio definition, checks and results,
  run references, the list of final exports and anything you could not verify.
- `outputs/`: small synthetic exports of the saved feature and split tables, affected initial
  records with rejection reasons, and evidence comparing the two successful runs.

These artifacts must demonstrate all four outcomes above. For each successful run, keep its
notebook URL and execution timestamp (or a Jobs run URL/ID if you used Jobs). Retain each run's
actual return IDs and values, or a reproducible comparison of snapshots, and show they match.
Row counts alone cannot show that IDs and values stayed the same. Keep the initial rejection
reference and affected records too; a successful replacement run does not replace that evidence.
Interactive notebook runs do not require Jobs run IDs.

Author the submission and exports inside Databricks. For the browser route, commit and push
through the Git folder; for the trainer-selected [optional route](OPTIONAL_TOOLS.md), use its
submission procedure. Verify the exact GitHub commit contains the notebook, submission and exports. Screenshots
may supplement the files but cannot replace code or saved outputs. Leave this repository's
navigation README in place.

In Classroom, submit the repository base URL (`https://github.com/{owner}/{repo}`), `main` and
the final submitted commit SHA. The assignment gives the deadline and reviewer instructions.
Do not add a future containing commit SHA to `SUBMISSION.md`; report it in Classroom instead.
