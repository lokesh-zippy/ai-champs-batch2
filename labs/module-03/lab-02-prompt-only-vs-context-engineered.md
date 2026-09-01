# Lab 02 — Prompt-Only vs Context-Engineered, Measured

**Time:** ~35 min · **Surface:** Copilot Chat (Agent mode) + terminal ·
**Prereq:** Lab 01 complete; database + your backend running

## Objective

Implement `GET /api/tasks/stats` **for real, twice** — once from a bare prompt
with no repository context, once with deliberately engineered context — and
record the difference on the [token worksheet](token-worksheet.md): quality,
tokens, iterations, time.

## Why it matters for the enterprise

This is the measurement that justifies everything else in the module. "Context
engineering is worth the effort" is an opinion until you have numbers: a
prompt-only pass that needed 4 iterations and still shipped the wrong keys,
versus a context-engineered pass that was right the first time for a fraction
more up-front tokens. Those numbers are your baseline for Module 11's ROI work.

## Set-up — isolate the two passes

Branch off `module-03-context` (your Module 03 working branch) for each pass:

```bash
git checkout module-03-context
git checkout -b m03-lab02-passA        # Pass A works here
```

Copy the worksheet:

**macOS / Linux**

```bash
cp ../module-03/token-worksheet.md worksheets/token-worksheet-$USER.md
```

**Windows (PowerShell)**

```powershell
Copy-Item ..\module-03\token-worksheet.md worksheets\token-worksheet-$env:USERNAME.md
```

---

## Pass A — prompt-only

**Rules for this pass:** a **new empty chat**, Agent mode, **no `#` context, no
open files that matter, no reference to `copilot-instructions.md`**. To make the
comparison fair, temporarily disable the repo instruction file so Pass A really
is context-free:

**macOS / Linux**

```bash
mv .github/copilot-instructions.md .github/copilot-instructions.md.off
```

**Windows (PowerShell)**

```powershell
Rename-Item .github\copilot-instructions.md .github\copilot-instructions.md.off
```

### Step A1 — one prompt

```
Add a GET /api/tasks/stats endpoint to this project that returns how many tasks
are in each status and a total. Implement it and add a test.
```

### Step A2 — iterate until you'd accept it

Let the agent work. Each follow-up you send ("no, the keys should be…", "move
that query", "the test is failing") is **one iteration — tally it**. Stop when
the result is something you would open a PR with.

### Step A3 — score Pass A

- Run the backend tests:

  **macOS / Linux**

  ```bash
  cd backend-python && pytest -q ; cd ..
  ```

  **Windows (PowerShell)**

  ```powershell
  cd backend-python ; pytest -q ; cd ..
  ```

- Fill the **Pass A** column of the rubric and metrics tables. Estimate context
  sent: for a bare prompt it's roughly your prompt text plus whatever files the
  agent opened itself — note the IDE's context gauge value if shown, else
  `~<prompt chars>/4` plus a note.
- Note every rubric item Pass A got wrong (status keys as `inProgress`, query in
  the router, missing `total`, no test, etc.).

### Step A4 — reset

```bash
git checkout -- . ; git clean -fd backend-python frontend    # discard Pass A code
git checkout module-03-context ; git checkout -b m03-lab02-passB
```

Restore the instruction file:

**macOS / Linux**

```bash
mv .github/copilot-instructions.md.off .github/copilot-instructions.md
```

**Windows (PowerShell)**

```powershell
Rename-Item .github\copilot-instructions.md.off .github\copilot-instructions.md
```

---

## Pass B — context-engineered

**Rules for this pass:** a new chat, Agent mode, and context you **choose on
purpose**. Not "add everything" — the minimum that makes the task unambiguous.

### Step B1 — assemble the context

In the chat input, use **Add Context** (or type `#`) to attach exactly these:

| Context | Why it's in | How to add |
|---------|-------------|-----------|
| `.github/copilot-instructions.md` | Layer rules, error contract, schema ownership | auto-included; or `#copilot-instructions.md` |
| The repository file for your backend (e.g. `repositories/task_repository.py`) | The pattern the count query must follow | `#task_repository.py` |
| The existing list handler (`routers/tasks.py`) | Error-mapping + response style to mirror | `#tasks.py` |
| The matching test file | The conventions the new test must match | `#test_tasks_api.py` |
| `usecase.md` — **the API-contract section only** | The status enum and JSON shape | paste the 6 relevant lines, don't attach the whole file |

Do **not** attach: the frontend, the other backends, the whole `usecase.md`, the
Module 02 skills. They're available; they're not relevant to this task.

### Step B2 — one prompt, with the six moves from Lab 01

```
You are the maintainer of this backend. Implement GET /api/tasks/stats.

Returns:
{ "todo": 3, "in-progress": 2, "done": 1, "total": 6 }

Constraints (see the attached copilot-instructions.md):
- The GROUP BY / aggregation query goes in the repository layer only.
- Keys are exactly "todo", "in-progress", "done", plus "total".
- Mirror the error-mapping and response style of the attached list handler.
- No change to database/schema.sql.

Steps: 1) repository method  2) service method  3) router handler
4) tests in the attached test file (happy path + total-equals-sum check).
Run the tests and show me the diff.
```

### Step B3 — score Pass B

- Run the tests (same commands as A3).
- Fill the **Pass B** columns. For "context sent", run:

  **macOS / Linux**

  ```bash
  tools/estimate-tokens.sh .github/copilot-instructions.md backend-python/repositories/task_repository.py backend-python/routers/tasks.py backend-python/tests/test_tasks_api.py
  ```

  **Windows (PowerShell)**

  ```powershell
  .\tools\estimate-tokens.ps1 .github\copilot-instructions.md backend-python\repositories\task_repository.py backend-python\routers\tasks.py backend-python\tests\test_tasks_api.py
  ```

  Add your prompt (~150 tokens) and the pasted contract lines (~80). That sum is
  your **context budget** for this task.

### Step B4 — keep Pass B

```bash
git add -A && git commit -m "Add GET /api/tasks/stats (context-engineered)"
git checkout module-03-context && git merge --no-ff m03-lab02-passB
```

---

## Step 4 — Compare and reflect

Fill the **Notes** section of the worksheet:

- Pass A total rubric score vs Pass B (Pass B should be 12–14; Pass A often 6–9).
- Tokens: Pass B's *up-front* context is bigger, but add up Pass A's **iteration
  prompts + re-reads** — prompt-only usually costs *more* total tokens by the
  time it's correct, and more wall-clock time.
- The last worksheet line: what is the **smallest** context that still gets Pass
  B's result? (Often: `copilot-instructions.md` + the repository file + the
  contract lines. The test file and list handler may be trimmable.) You test
  that hypothesis in Lab 04.

## Verify

- [ ] `worksheets/token-worksheet-<you>.md` is filled in for both passes.
- [ ] Pass B is committed; Pass A code was discarded.
- [ ] `.github/copilot-instructions.md` is restored (no `.off` file left behind).
- [ ] You can state the token and iteration difference in one sentence.

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Pass A came out fine | Your instruction file wasn't actually disabled, or an important file was open in the editor (the agent reads it). Re-check, redo. |
| Can't tell how many tokens were sent | Use the IDE chat context gauge if present; otherwise the `estimate-tokens` sum of attached files is a fair proxy. Precision doesn't matter — the *ratio* does. |
| Left a `.off` file | `git status` will show it. Rename it back and delete the `.off`. |
| Pass B needed iterations too | Note which context was missing and add it — that *is* the lab working. |

## Recap & carry-forward

You have numbers. **Lab 03** zooms in on Step B1 — the *select* stage — and
turns "attach exactly these" into a repeatable context manifest.
