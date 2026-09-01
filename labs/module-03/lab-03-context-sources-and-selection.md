# Lab 03 — The Context Sources Map & the *Select* Stage

**Time:** ~25 min · **Surface:** VS Code context controls (`#`, `@`, Add Context) ·
**Prereq:** Lab 02 complete

## Objective

Map every context source the Task Board repo offers, then practise **selecting**
— choosing the few that a given task actually needs and rejecting the rest.
You will produce a reusable **context manifest** for the shared task and prove
that "less, but the right pieces" beats "attach the whole file".

## Why it matters for the enterprise

The model has a finite attention budget. Every irrelevant file you attach spends
that budget and dilutes the signal — and costs tokens on every turn. Selection
is the highest-leverage stage of the pipeline: get it right and layering,
compression, and scoping have far less to do.

## Background — the five context sources

From the Module 03 concept guide, mapped onto this repo:

| Source | In the Task Board repo | Answers |
|--------|------------------------|---------|
| Repository files & dependencies | `backend-*/`, `frontend/src/`, `requirements.txt`, `package.json` | What exists now |
| Architecture & design notes | `usecase.md`, `README.md`, `.github/copilot-instructions.md` | How it was built, and why |
| APIs & standards | `usecase.md` §API contract, `database/schema.sql`, error contract | What must be respected |
| Issue history | GitHub issues / PRs / discussions (Module 02 created some) | Why past choices were made |
| Specifications & tests | `backend-*/tests/`, `frontend/src/**/__tests__/` | What must be validated |

### How to add each in VS Code

| Control | Adds |
|---------|------|
| `#<filename>` | A specific file |
| `#selection` | The currently selected lines |
| `#codebase` (or `@workspace` in older builds) | Lets Copilot search the repo for relevant snippets itself |
| `#changes` | The current git diff |
| `#<symbol>` | A specific function/class by name |
| **Add Context** button → *Files / Symbols / Problems* | Same, via picker |
| Paste / drag a file or image | Ad-hoc context |
| `@github` | Pulls in issues, PRs, and repo metadata from GitHub.com |

---

## Step 1 — Inventory the sources for the shared task

Create the manifest:

```md
<!-- worksheets/context-manifest.md -->
# Context manifest — GET /api/tasks/stats

## Candidate context (everything available)
| # | Source | Path / handle | Relevant to THIS task? | Why / why not |
|---|--------|---------------|:---------------------:|---------------|
| 1 | Repo rules | `.github/copilot-instructions.md` | | |
| 2 | Repository layer | `backend-<x>/repositories/…` | | |
| 3 | Service layer | `backend-<x>/services/…` | | |
| 4 | Router/controller | `backend-<x>/routers/…` (list handler) | | |
| 5 | Backend tests | `backend-<x>/tests/…` | | |
| 6 | API contract | `usecase.md` §API contract (status enum, shape) | | |
| 7 | DB schema | `database/schema.sql` | | |
| 8 | Frontend service | `frontend/src/services/taskService.js` | | |
| 9 | Frontend components | `frontend/src/components/…` | | |
| 10 | Other backends | `backend-<y>/`, `backend-<z>/` | | |
| 11 | Issue history | `@github` — issues/PRs mentioning "stats"/"tasks" | | |

## Selected context (the manifest)
List ONLY the rows marked "yes", in the order you'd attach them:
1. 
2. 
3. 

## Rejected, and why
- 
```

## Step 2 — Decide, with a rule

For each candidate, apply this test: **"If I remove this, could Copilot get the
task wrong in a way the other context wouldn't catch?"** If no → reject it.

Expected outcome for `GET /api/tasks/stats`:

- **Select:** repo rules (1), repository layer (2), the list handler (4) for
  style, the contract lines (6). Maybe the test file (5).
- **Reject:** service layer if it's a thin pass-through (2 already shows the
  pattern), schema (7 — no schema change, and the enum is in the contract),
  frontend (8, 9 — this task has no UI), other backends (10 — parity is a
  *check*, done later with the Module 02 skill, not context here), issue history
  (11 — nothing relevant exists).

Fill in the "why" column — the reasoning is the transferable skill.

## Step 3 — Prove "whole file" is wasteful

Compare attaching the whole router vs just the relevant handler:

**macOS / Linux**

```bash
tools/estimate-tokens.sh backend-python/routers/tasks.py
# then copy just the list_tasks handler into a scratch file and measure that
```

**Windows (PowerShell)**

```powershell
.\tools\estimate-tokens.ps1 backend-python\routers\tasks.py
```

In a fresh chat, try the Lab 02 Pass B prompt twice:

1. Attach the **whole** `routers/tasks.py` + whole `usecase.md`.
2. Attach **`#selection`** of just the `list_tasks` function + the 6 contract
   lines pasted.

Both should produce a correct plan. Note the context-size difference (often
5–15×) for the *same* output quality. Record it in the manifest.

## Step 4 — Use `#codebase` as a selection *assistant*

When you don't know which files matter, let Copilot find them, then lock the
selection:

```
#codebase Which files would I need to change to add GET /api/tasks/stats to the
Python backend, and which existing file is the best pattern to copy? List paths
only.
```

Take its answer, verify each path is actually relevant, and copy the good ones
into your manifest as explicit `#file` attachments. `#codebase` is great for
*discovery*, worse for *precision* — it can pull broad snippets every turn.

## Step 5 — Commit

```bash
git add worksheets/context-manifest.md && git commit -m "Module 03 Lab 03: context manifest for stats endpoint"
```

## Verify

- [ ] `worksheets/context-manifest.md` has every candidate rated with a reason.
- [ ] The selected list is 3–5 items, not 11.
- [ ] You measured whole-file vs selection token cost for the same result.
- [ ] You used `#codebase` for discovery and converted its answer to explicit
      selections.

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `#codebase` / `@workspace` not recognised | Newer VS Code uses `#codebase`; older uses `@workspace`. Agent mode searches the codebase implicitly — you can also just describe what you need. |
| Manifest feels arbitrary | Re-apply the Step 2 test literally, one row at a time. If you can't name a failure mode the file prevents, reject it. |
| Copilot ignores an attached file | Very large files get truncated. Attach a `#selection` or a summary instead — that's Lab 04. |

## Recap & carry-forward

You can now select context deliberately. **Lab 04** takes the selected pile and
runs it through *layer → compress → scope*, then bottles the whole routine as a
reusable prompt file.
