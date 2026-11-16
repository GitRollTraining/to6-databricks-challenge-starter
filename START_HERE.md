# Start in Databricks

Connect your own repository, try one ungraded operation, then complete [the task](CHALLENGE.md).
No local installation is needed. Use the workspace link, compute and reviewer instructions in
the Classroom environment card; ask the trainer about missing access. Never save credentials
in a notebook, repository or prompt.

## 1. Create and connect your repository

1. Open [the starter](https://github.com/GitRollTraining/to6-databricks-challenge-starter).
   Choose **Use this template → Create a new repository**, your personal account and **Private**.
   Keep the `main` branch. Invite the reviewer named in Classroom.
2. In Databricks, open your user menu → **Settings → Linked accounts → Add Git credential**.
   Select GitHub and link your own account, allowing access to your new private repository.
3. Open **Workspace**, go to your own user folder, then **Create → Git folder**. Paste your
   own repository's HTTPS URL, select GitHub and create the folder. Check its branch is `main`.
4. Open `notebook.py` in that folder and select the compute named in Classroom. Its setup cell
   reads your authenticated identity to calculate your assigned output schema. The trainer
   must have provisioned it; do not change it to another person's schema.

## 2. Try the connection (ungraded)

Add a SQL cell to the notebook, run `SELECT 1 AS connection_check`, and confirm the result is 1.
This tests compute, not the Challenge requirements. Save a small practice file from a Python cell:

```python
from pathlib import Path
import json

# Copy this Git folder's absolute path from its Workspace menu; include /Workspace/.
repo_root = Path("/Workspace/Users/YOUR-USER/YOUR-GIT-FOLDER")
assert (repo_root / "README.md").is_file(), "Check the Git folder path"
practice_result = spark.sql("SELECT 1 AS connection_check").first().asDict()
assert practice_result == {"connection_check": 1}
(repo_root / "outputs").mkdir(exist_ok=True)
(repo_root / "outputs/connection-practice.json").write_text(
    json.dumps(practice_result) + "\n", encoding="utf-8"
)
```

The filename is relative to your Git folder root. An explicit root is needed because serverless
compute does not guarantee the notebook's working directory. Refresh the Workspace folder and
open the file. This example only demonstrates saving a file; it is not a task solution.

Open the Git dialog by clicking the branch name. Review the notebook and practice-file changes,
enter a commit message, then **Commit & Push**. Open your own repository on GitHub and confirm
both files changed on `main`. Leave `README.md` in place. If a write or push fails, resolve it
with the trainer before starting the task. Do not turn in this practice result.

## 3. Do the task and save your evidence

Use the scaffold's headings for the four outcomes in [CHALLENGE.md](CHALLENGE.md). The setup
cell defines `INPUT_SCHEMA`, `INITIAL_TABLE`, `CORRECTED_TABLE`, `LINEAGE_VIEW` and `OUTPUT_SCHEMA`.
Inspect the input descriptions and samples in Catalog Explorer. The supplied lineage view is
`training.to6_challenge3_inputs.source_lineage`, separate from the Catalog UI's Lineage tab.
Only write tables in your assigned output schema; do not change shared inputs.

Develop and run your code here. Genie Code may help if enabled; review and verify its suggestions.
Preserve the rejected batch's records and reasons before the successful replacement run. Run
all task stages successfully twice and retain both execution references and an IDs-and-values
comparison as [the task requires](CHALLENGE.md).

Edit `SUBMISSION.md` in the Databricks file editor, or write it from a notebook Python cell
using `(repo_root / "SUBMISSION.md").write_text(your_text, encoding="utf-8")`.
Use the practice file-writing pattern to save your own small exports under `outputs/`; author
that export code in Databricks too. The browser route needs no notebook exit command.
The Git folder saves your notebook as source when committed; no local download or edit is needed.

## 4. Submit the completed task

Review the deliverables in [CHALLENGE.md](CHALLENGE.md), then **Commit & Push** from the Git dialog.
In GitHub, open the resulting commit and verify `notebook.py`, `SUBMISSION.md` and every final
export. Record that commit's full SHA. Confirm the Classroom reviewer can access your private repo.

In Classroom, use **Add or create → Link** for your repository base URL. Add branch `main` and
the final SHA in a private comment, then **Turn in** by the assignment's deadline.

References: [Git connection setup](https://docs.databricks.com/aws/en/repos/repos-setup),
[Git folder operations](https://docs.databricks.com/aws/en/repos/git-operations-with-repos),
[writing workspace files](https://docs.databricks.com/aws/en/files/workspace-interact),
[Genie Code](https://docs.databricks.com/aws/en/notebooks/code-assistant).
