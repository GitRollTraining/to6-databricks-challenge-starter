# Start your first Databricks Challenge

**In short.** This guide connects your Cloud Workstation, Databricks and your own GitHub
repository, then runs an ungraded practice test that proves you can read, write and submit. It
doesn't contain the Challenge's answer. Finish it before Challenge day if you can. Start the actual
task ([CHALLENGE.md](CHALLENGE.md)) only once your trainer opens the Classroom assignment.

You'll go through seven phases:

1. Make your private repository
2. Install the tools
3. Sign in to Databricks and set your paths
4. Connect GitHub to Databricks, and check
5. Turn on conversation saving
6. Run the practice test and submit it
7. Save the practice conversation, then start the task

A reference section at the end explains the submission rules in detail.

## Before you start

The training team provides a Cloud Workstation with Python 3.11+, Git, uv, Codex and the Databricks
CLI, plus your Databricks access and serverless compute. Sign in with your training Google account,
as the course's "Set up your machine and the training environment" post describes.

**Never put passwords, tokens or sign-in codes in a Codex prompt or in this repository.**

## 1. Make your private repository

1. Open [the course starter](https://github.com/GitRollTraining/to6-databricks-challenge-starter).
2. Select **Use this template → Create a new repository**. Choose your personal GitHub account as
   the owner (not the course organization), name the repository, choose **Private**, and create it.
   Keep the default `main` branch.
3. In your Workstation terminal, clone **your new repository** using its HTTPS URL. Replace the two
   uppercase values with your GitHub username and repository name, here and in every later command:

   ```sh
   git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
   cd YOUR-REPOSITORY
   ```

4. Open this folder in the Workstation editor.
5. In your private repository on GitHub, open **Settings → Collaborators → Add people** and invite
   `TakalaWang`, the course delivery operator, so your submission and conversation record can be
   reviewed. Keep the repository private.

Your notebook lives in Databricks. This local folder holds only the tools and your saved Codex
conversation, never notebook solution files.

## 2. Install the tools

These commands install the connection tooling, not a solution. Run them from the repository's top
folder:

```sh
mkdir -p .tools
git clone https://github.com/databricks-solutions/ai-dev-kit.git .tools/ai-dev-kit
git -C .tools/ai-dev-kit checkout --detach 07f8e1ccf3dfc09a794eaf86fd15a5848f9d63d4
uv venv --python 3.11 .venv
uv pip install --python .venv/bin/python -r requirements.txt ./.tools/ai-dev-kit/databricks-tools-core ./.tools/ai-dev-kit/databricks-mcp-server
```

If `.tools/ai-dev-kit` already exists, check that it's at that revision instead of cloning over it:
the script requires this exact revision, with its tracked files unchanged. Git ignores the tools,
the environment and the local receipts.

Run the local checks. They should all pass:

```sh
.venv/bin/python -m unittest discover -s tests -v
```

## 3. Sign in to Databricks and set your paths

Sign in to the course workspace with a profile named `TO6`. The CLI opens your browser:

```sh
databricks auth login --host 'https://dbc-48a16403-f66f.cloud.databricks.com?w=7474648218609329&account_id=d0c409e4-5865-49d6-86f5-241979d24355' --profile TO6
```

Use your own learner account, not an operator account. Don't set token or host environment
variables to override this profile.

Then save your personal settings, with your own repository URL:

```sh
.venv/bin/python tools/databricks_platform.py init --repository https://github.com/YOUR-USERNAME/YOUR-REPOSITORY
```

**Already signed in to this workspace under another profile,** for example on Thursday? Run
`databricks auth profiles` to see its name, and add `--profile THAT-NAME` to `init` instead of
signing in again. Copying a profile's host into a new profile doesn't copy its login, and you
should never copy tokens. Later commands reuse the saved profile automatically.

`init` reads your Databricks username and writes `.platform/config.json`. Read the settings it
prints before going on:

| Setting | Value |
|---|---|
| Profile | `TO6` by default, or the signed-in profile you chose |
| Notebook | `/Users/<your Databricks username>/to6-challenge3/notebook` |
| Input schema | `training.to6_challenge3_inputs` |
| Input tables | `returns_initial` and `returns_corrected` in that schema |
| Supplied lineage view | `training.to6_challenge3_inputs.source_lineage` |
| Output schema | `training.to6_c3_` followed by the first 12 hex characters of the SHA-256 of your exact Databricks username; `init` works it out |
| Submission | your repository URL, branch `main` |

The administrator must already have created your output schema and given you read access to the
inputs. Don't change your schema or path to another learner's values.

**The script is not a sandbox.** Your SQL and notebook code run with your real Databricks
permissions; Unity Catalog grants and workspace permissions are what keep learners apart. Always use
full three-part table names.

## 4. Connect GitHub to Databricks, and check

Databricks reaches GitHub through a managed connection named `system.ai.github`. Its endpoint in
this workspace is:

`https://dbc-48a16403-f66f.cloud.databricks.com/ai-gateway/mcp-services/system.ai.github`

1. While signed in to your training Databricks account, open
   [Authorize GitHub for Databricks](https://dbc-48a16403-f66f.cloud.databricks.com/mcp-service-login?name=system.ai.github).
2. Authorize with **your own GitHub account**, the owner of the repository you just created, and
   allow it to access that private repository. This connection is yours: an operator's account
   can't submit your work.
3. Back in the Workstation, never paste authorization tokens into a prompt. Run:

```sh
.venv/bin/python tools/databricks_platform.py check
```

`check` confirms that the connected GitHub user owns your repository. It then reads your Databricks
identity, both input tables' descriptions and columns, your output schema, and the README on your
`main` branch. It writes nothing. Passing `check` proves you can read; the practice test in phase 6
proves you can run code, write and submit.

## 5. Turn on conversation saving

Complete [SESSION_RECORD.md](SESSION_RECORD.md), steps 1–2, before you start Codex. Your genuine
Codex conversation, including its tool activity, is part of what you hand in. Screenshots and
retyped chats don't count.

## 6. Run the practice test and submit it

Start Codex in this repository folder. Give it these working rules:

> Use `.venv/bin/python tools/databricks_platform.py execute` with JSON on stdin containing inline `code`.
> Author and update the notebook through that native operation, never a local solution file or
> `file_path`. Run only on Databricks. Read `.platform/config.json` for my exact paths/schema.
> Use `sql` for independent readback and `submit` for native exports. Ask before writes whose
> target I have not authorized. Never edit a downloaded/exported notebook locally.

Have Codex send this Python through `execute`, with `YOUR_OUTPUT_SCHEMA` replaced by your output
schema from phase 3. No local `.py` file is created:

```python
import json
schema = "YOUR_OUTPUT_SCHEMA"
practice = schema + ".connection_smoke"
spark.createDataFrame([(1, "connected")], ["check_id", "message"]).write.mode("overwrite").saveAsTable(practice)
rows = [r.asDict() for r in spark.table(practice).collect()]
assert rows == [{"check_id": 1, "message": "connected"}]
dbutils.notebook.exit(json.dumps({"files": {
    "README.md": "Ungraded connection smoke. Native output: " + practice + "\n",
    "outputs/connection-smoke.json": json.dumps(rows)
}}))
```

`execute` takes exactly `{"code":"...inline notebook source..."}` on stdin; Codex can build that
JSON and pass it directly. Open the run URL it returns, and your notebook, in Databricks in the
browser. Confirm the run succeeded.

Read the saved practice table yourself with `sql`, passing this JSON on stdin (replace
`YOUR_OUTPUT_SCHEMA` first):

```json
{"query":"SELECT * FROM YOUR_OUTPUT_SCHEMA.connection_smoke"}
```

Also look at the inputs:
- Open the two input tables in Catalog Explorer.
- Open the supplied lineage view at **Catalog → training → to6_challenge3_inputs → source_lineage**,
  or query `SELECT * FROM training.to6_challenge3_inputs.source_lineage` with `sql`. This view is a
  source map the course supplies; it's separate from the Lineage tab in Catalog Explorer.

If a permission or description is missing, the administrator fixes it.

Now submit the practice result and read it back:

```sh
.venv/bin/python tools/databricks_platform.py submit
.venv/bin/python tools/databricks_platform.py read notebook.py
.venv/bin/python tools/databricks_platform.py read README.md
```

`submit` takes the notebook's source and the output of its last successful run, commits them to
your `main` branch, then reads every file back from that commit to confirm it arrived intact. Open
the commit in GitHub. If the receipt says `verified: false`, a commit may already exist but its
check failed: look at it before trying again.

## 7. Save the practice conversation, then start the task

Complete [SESSION_RECORD.md](SESSION_RECORD.md), steps 3–5, and confirm the conversation reached
GitHub. Commits that `submit` makes on GitHub don't trigger your Workstation's Git hooks, which is
why these steps are needed after every submission.

The practice table and `outputs/connection-smoke.json` can stay as ungraded evidence. Then start
[CHALLENGE.md](CHALLENGE.md).

## Reference: how submission works

**Use the same notebook for the task.** The initial batch is meant to fail: keep that failed run,
its affected records and the reason. The script keeps run IDs in ignored local receipts, so put
the evidence a reviewer needs in your handoff and output files.

**A successful run must end with `dbutils.notebook.exit`**, returning JSON of this shape, built in
Databricks:

```json
{"files":{"README.md":"Your handoff text","outputs/your-output.json":"Your small synthetic output export"}}
```

- Only `README.md` and paths under `outputs/` are allowed.
- The script adds `notebook.py` (the notebook's source) and `outputs/native-run.json` (the run
  reference) itself. Don't supply either.
- Large or truncated notebook output is rejected, so keep the exports small.
- No verified receipt is produced if the latest run failed, the notebook changed after the run,
  the run's status isn't successful, or the GitHub readback doesn't match.

**`sql` returns the real query result.** If it returns a pending `statement_id`, check on it with
`.venv/bin/python tools/databricks_platform.py sql --statement-id ID`. Repeat while it's pending,
without resending the original query. `sql` doesn't restrict access: use it to inspect your inputs
and saved outputs, and never to change the shared inputs. You may write only to your own output
schema, which platform grants enforce.

**After the final `submit`,** put the commit SHA in your Classroom submission alongside your
repository's base URL. Don't edit the handoff to add its own commit SHA. Your Codex conversation is
handed in separately ([SESSION_RECORD.md](SESSION_RECORD.md)). This starter doesn't promise that
automated grading is ready.

**Each submit adds or updates only the files its run names**; it doesn't delete older exports. List
the files that belong to your final task delivery in your handoff, so a reviewer can tell them
apart from practice or earlier outputs.
