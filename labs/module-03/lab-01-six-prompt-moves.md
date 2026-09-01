# Lab 01 — The Six Prompt-Engineering Moves

**Time:** ~25 min · **Surface:** Copilot Chat (Ask mode) ·
**Prereq:** Module 03 [set-up](README.md#set-up--do-this-once-before-lab-01) done

## Objective

Take one real Task Board request and evolve a single prompt through the **six
moves** — frame, role/instruction, constraints, examples, decompose, iterate —
watching the output get sharper at each step. You will keep a journal so the
before/after is undeniable.

## Why it matters for the enterprise

Prompt quality is the cheapest lever there is: no new tools, no config, just
words. But "just wing it" produces results that vary engineer to engineer and
day to day. The six moves are a **repeatable sequence** anyone on the team can
run, so a well-formed prompt stops being a personal talent and becomes a
standard. This is also the raw material Module 02's prompt files were built
from — you are learning to author them.

## Background — the six moves

| Move | What it adds | Example fragment |
|------|--------------|------------------|
| 1 · Frame the task | One sentence: what exists when it's done | "Add a read-only stats endpoint that returns task counts by status." |
| 2 · Set role & instruction | Perspective + approach | "As the backend maintainer, propose the change; don't write code yet." |
| 3 · Add constraints | The limits that must hold | "Counts query in the repository; keys exactly `todo`/`in-progress`/`done`; no schema change." |
| 4 · Give examples | Anchor format & quality | A sample JSON response; a link to an existing endpoint to mirror. |
| 5 · Decompose | Independently checkable steps | "1) repository method 2) service method 3) router 4) test." |
| 6 · Iterate & verify | Refine the prompt, not just the answer | "The service is doing SQL — move it to the repository and show the diff." |

---

## Step 1 — Create the journal

```md
<!-- worksheets/prompts-journal.md -->
# Prompts Journal — the six moves

Task: add `GET /api/tasks/stats` (see labs/module-03/README.md)
Backend: __________   Model: __________

## v1 — Frame only
Prompt:
> 

Output summary / problems:

## v2 — + Role & instruction
Prompt:
> 

What changed:

## v3 — + Constraints
Prompt:
> 

What changed:

## v4 — + Examples
Prompt:
> 

What changed:

## v5 — + Decompose
Prompt:
> 

What changed:

## v6 — Iterate & verify
Follow-up prompt(s):
> 

Final verdict (is the plan correct and complete? Y/N):

## Reflection
- Which single move improved the output the most?
- Which move mattered least for this task?
- Paste the final v6 prompt — this is a candidate for a `.github/prompts/` file.
```

## Step 2 — Run the six versions

Use **Ask** mode (not Agent) and a **fresh chat for each version** so context
doesn't leak between them. Ask only for a *plan*, not code — it keeps the loop
fast and the comparison about the prompt, not the implementation.

**v1 — Frame only**

```
Add a stats endpoint to the task board.
```

Record what's vague: which backend? what shape? where does the query go?

**v2 — add Role & instruction**

```
You are the maintainer of this backend. Propose how to add a read-only
GET /api/tasks/stats endpoint that returns task counts by status. Describe the
change; do not write code yet.
```

**v3 — add Constraints**

```
You are the maintainer of this backend. Propose how to add GET /api/tasks/stats
returning task counts by status.

Constraints:
- The aggregation query goes in the repository layer only.
- Response keys are exactly "todo", "in-progress", "done", plus "total".
- No change to database/schema.sql; no migration.
- Consistent with the existing 404/422 error contract.

Describe the change across layers; no code yet.
```

**v4 — add Examples**

Append to the v3 prompt:

```
Target response:
{ "todo": 3, "in-progress": 2, "done": 1, "total": 6 }

Mirror the structure of the existing GET /api/tasks handler in this backend.
```

**v5 — add Decompose**

Append:

```
Break the plan into independently verifiable steps, in this order:
1) repository method  2) service method  3) router handler  4) test file + cases
For each step: the file, the function, and how I verify it in isolation.
```

**v6 — Iterate & verify**

Read v5's plan. Push back on the weakest part, e.g.:

```
Step 2 has the service doing the GROUP BY. Move all aggregation into the
repository per our constraints and re-list steps 1–2 only.
```

Repeat until the plan is correct and complete.

## Step 3 — Fill in the reflection

Answer the three reflection questions in the journal. The honest answer to
"which move mattered least" is usually **examples** *for a task this small* — and
**constraints** is usually the biggest jump, because that is where your
repository knowledge enters a prompt that otherwise has none. Hold that
thought — Labs 03–05 are about supplying that knowledge as *context* instead of
retyping it every time.

## Step 4 — Commit

```bash
git add worksheets/prompts-journal.md && git commit -m "Module 03 Lab 01: six-moves prompts journal"
```

## Verify

- [ ] `worksheets/prompts-journal.md` has all six prompt versions filled in with
      "what changed" notes.
- [ ] v6's plan puts the aggregation in the repository and uses the exact status
      keys.
- [ ] The reflection names the highest- and lowest-impact move for this task.
- [ ] The final prompt is captured verbatim (Lab 04 reuses it).

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Every version gives a near-identical answer | Your repo's `.github/copilot-instructions.md` is already supplying constraints. That's the point of Module 02 — for this lab, note it and compare v1/v2 wording quality instead. |
| Ask mode keeps writing code | Add "Describe the change only. Do not produce code." explicitly; it's move 2. |
| Answers drift as you iterate | Start a new chat for each numbered version; only v6 is a running conversation. |

## Recap & carry-forward

You have a repeatable prompt sequence and evidence of which moves pay off.
**Lab 02** runs the same task end-to-end two ways — a bare prompt vs a
context-engineered one — and measures the gap in tokens, iterations, and time.
