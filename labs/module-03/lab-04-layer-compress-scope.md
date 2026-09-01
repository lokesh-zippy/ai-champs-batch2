# Lab 04 — Layer → Compress → Scope

**Time:** ~25 min · **Surface:** Copilot Chat + a new prompt file ·
**Prereq:** Lab 03 complete (you have a context manifest)

## Objective

Take the selected context from Lab 03 and run it through the rest of the
pipeline — **layer** it by relevance, **compress** out redundancy, **scope** it
to the task — then capture the whole routine as
`.github/prompts/context-brief.prompt.md` so the team runs it as `/context-brief`.

## Why it matters for the enterprise

Selection decides *which* sources; layering, compression, and scoping decide how
they're *delivered*. A 6-line summary of the API contract beats the 90-line file.
Ordering the most decision-relevant context first survives truncation better.
Resetting context between tasks stops a session's tokens from growing all day.
Done well, a sharp 800-token context outperforms a vague 8,000-token one — at a
tenth of the cost on every turn.

## Background — the three stages

| Stage | Question | Techniques |
|-------|----------|-----------|
| **Layer** | What order? | Most decision-relevant first: rules → contract → pattern file → nice-to-have. Truncation eats the bottom. |
| **Compress** | What's redundant? | Summarise specs to the lines that bind; drop imports/boilerplate; reference instead of re-paste; one example not five. |
| **Scope** | What's this task, and only this task? | Exclude other features; start a fresh chat per task; state explicit non-goals. |

---

## Step 1 — Layer your manifest

Open `worksheets/context-manifest.md`. Reorder the *selected* list by
**decision relevance** — if the model could only read the first two items, which
two prevent the most mistakes? For `GET /api/tasks/stats`:

```
1. .github/copilot-instructions.md      (the rules — layer boundaries, contract)
2. API-contract lines (status enum + JSON shape)   (what "correct" means)
3. repositories/task_repository.py       (the pattern to copy)
4. tests/test_tasks_api.py               (test conventions — trim to 1 example)
```

Add a `## Layered order` section to the manifest with this list and a one-line
justification each.

## Step 2 — Compress each piece

Create a compressed brief — the actual text you'd paste:

```md
<!-- worksheets/context-brief-stats.md -->
# Context brief — GET /api/tasks/stats

## Rules (from .github/copilot-instructions.md — the parts that bind THIS task)
- Layers: Controller/Router → Service → Repository. ALL DB access + the
  GROUP BY in the repository. No SQL in the router/service.
- Error contract: 404 missing id, 422 missing title / unknown status. Don't
  invent codes. (This endpoint has no error cases beyond a malformed request.)
- Schema owned only by database/schema.sql — no migration, no create_all.

## Contract (from usecase.md §API contract)
- status ∈ { "todo", "in-progress", "done" }  — literal strings, hyphen included.
- New route: GET /api/tasks/stats → 200
  { "todo": <int>, "in-progress": <int>, "done": <int>, "total": <int> }
- total == todo + in-progress + done.

## Pattern to copy
repositories/task_repository.py → follow the style of `list_tasks`; add
`count_by_status()` returning a dict keyed by the status string.

## Test
tests/test_tasks_api.py → add one test: seed 3 tasks across 2 statuses, GET
/api/tasks/stats, assert the counts and that total == sum. Use the existing
client fixture; no real DB.

## Non-goals
No frontend change. No change to GET /api/tasks. No new columns.
```

Measure the compression:

**macOS / Linux**

```bash
tools/estimate-tokens.sh .github/copilot-instructions.md usecase.md backend-python/routers/tasks.py backend-python/tests/test_tasks_api.py
tools/estimate-tokens.sh worksheets/context-brief-stats.md
```

**Windows (PowerShell)**

```powershell
.\tools\estimate-tokens.ps1 .github\copilot-instructions.md usecase.md backend-python\routers\tasks.py backend-python\tests\test_tasks_api.py
.\tools\estimate-tokens.ps1 worksheets\context-brief-stats.md
```

Expect the brief to be **5–20×** smaller than the raw sources.

## Step 3 — Test that the compressed brief still works

Fresh chat, Agent mode. Attach **only** `worksheets/context-brief-stats.md` (no
other `#` context, instruction file still active) and prompt:

```
Implement GET /api/tasks/stats in the Python backend following the attached
context brief exactly. Run the tests and show the diff.
```

Compare the result to Lab 02 Pass B. If it matches on the rubric, the brief
captured what mattered. If it missed something, add the missing line to the
brief — that line was load-bearing.

## Step 4 — Bottle it as a prompt file

```md
<!-- .github/prompts/context-brief.prompt.md -->
---
description: Build a compressed, layered, task-scoped context brief before implementing a change
mode: ask
---
I am about to implement: ${input:task:one sentence — what should exist when done}

Produce a **context brief** for this task and nothing else. Use this structure:

## Rules
The 3–6 lines of `.github/copilot-instructions.md` that actually bind this task.
Quote them; don't summarise vaguely.

## Contract
The exact API shape, field names, types, and status codes involved — from
`usecase.md`. 6 lines max.

## Pattern to copy
The single best existing file/function to mirror, with the path and a one-line
note on what to take from it.

## Test
The one test file to touch and the 1–2 cases to add.

## Non-goals
What this task must NOT change (other endpoints, the frontend, the schema…).

Rules for the brief:
- Layer it in the order above (rules first — they survive truncation).
- Compress: reference files, don't paste them; one example, not many.
- Scope: if something isn't needed for THIS task, leave it out and say so under
  Non-goals.
- Total brief ≤ 400 tokens. If you can't fit it, the task is too big — tell me
  to split it.
```

## Step 5 — Use it, then commit

Run `/context-brief` with task
`add ?assignee= filtering to GET /api/tasks/stats` and sanity-check the brief it
produces — then discard (it's a demo). Commit the artifacts:

```bash
git add .github/prompts/context-brief.prompt.md worksheets/context-manifest.md worksheets/context-brief-stats.md
git commit -m "Module 03 Lab 04: context-brief prompt + compressed brief"
```

## Verify

- [ ] `worksheets/context-brief-stats.md` exists and is measurably smaller than
      the raw sources.
- [ ] The brief-only prompt reproduces Lab 02 Pass B's rubric score.
- [ ] `.github/prompts/context-brief.prompt.md` runs as `/context-brief` and
      enforces the ≤400-token / "split it" rule.
- [ ] The manifest has a `## Layered order` section with justifications.

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Brief-only result is worse than Pass B | A load-bearing line was compressed away — diff the two prompts, restore it, note what it was. |
| `/context-brief` output is still huge | Tighten the "≤ 400 tokens" instruction and the "reference, don't paste" rule; re-run. |
| Hard to know what's "decision-relevant" | Ask: which line, if wrong, produces a bug the others don't catch? Those go first. |

## Recap & carry-forward

You can now compress a task's context to its essentials and generate that brief
on demand. **Lab 05** goes team-scale: a shared Copilot Space for the whole Task
Board, plus the boundary habits that stop token inflation across a working
session.
