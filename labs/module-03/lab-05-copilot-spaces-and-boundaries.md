# Lab 05 — Copilot Spaces & Context Boundaries

**Time:** ~25 min · **Surface:** GitHub.com (Copilot Spaces) + VS Code ·
**Prereq:** Lab 04 complete; the repo pushed to GitHub (Module 02 Lab 06)

## Objective

Bundle the Task Board's stable context — repo, `usecase.md`, the coding rules, a
couple of issues — into a shared **Copilot Space** the whole team reuses, then
practise the **boundary habits** that keep a working session from inflating:
context reset, reference-don't-resend, and model/window selection for cost.
Deliverable: the Space plus `CONTEXT.md` in the repo.

## Why it matters for the enterprise

Labs 01–04 made *one engineer* good at context for *one task*. This lab makes it
a team asset. A Space is curated once and reused by everyone — no re-attaching
the same five files, no drift in what "the contract" means. And the boundary
habits are what stop the token bill from creeping: most inflation isn't one big
prompt, it's the same context re-sent on turn after turn of a long session.

## Background

- **Copilot Spaces** (github.com/copilot/spaces) bundle repositories, specific
  files, free-text instructions, and issues/PRs into a named, shareable context.
  They are the successor to Enterprise knowledge bases. You chat against a Space
  on GitHub.com and (increasingly) from the IDE.
- **Context boundary** = the deliberate call about what reaches the agent vs.
  what stays available but unsent. The anti-patterns table in the Module 03
  guide is the checklist.

---

## Part A — build the shared Space

### Step 1 — create it

On GitHub.com: **Copilot → Spaces → New space** (or
`https://github.com/copilot/spaces`). Name it **`TaskBoard Context`**.

### Step 2 — add sources (curate, don't dump)

| Add | Which | Why |
|-----|-------|-----|
| Repository | your Task Board repo | code + tests, searchable |
| Files | `usecase.md`, `database/schema.sql`, `.github/copilot-instructions.md` | the contract & rules, pinned |
| Issues | 1–2 real ones (e.g. the Module 02 coding-agent issue, a "stats endpoint" issue) | decision history |
| Instructions (free text) | see Step 3 | how to use this Space |

Do **not** add: build output, `node_modules`, the three backends' compiled
artifacts, unrelated repos.

### Step 3 — write the Space instructions

Paste into the Space's instructions field:

```
This Space is the shared context for the Engineering Task Board.

When answering:
- Treat .github/copilot-instructions.md as binding: Controller/Router → Service
  → Repository layering, 404/422 error contract, schema owned only by
  database/schema.sql.
- The API contract in usecase.md is authoritative for routes, status codes, and
  JSON shape. status is exactly one of: todo, in-progress, done.
- Keep the three backends behaviourally identical.
- Prefer citing a file/line over guessing. If the contract doesn't cover
  something, say so.
```

### Step 4 — use it

In the Space, ask:

```
What would GET /api/tasks/stats need to look like to fit this project's
contract and layering? Cite the files.
```

Confirm the answer is grounded in *your* files (mentions the repository layer,
the exact status strings) without you attaching anything. Share the Space with a
teammate (or your instructor) via **Share**.

## Part B — context boundaries in a session

### Step 5 — audit a bloated session

Open VS Code Copilot Chat. Simulate a real half-day: in **one** long chat, ask
about the stats endpoint, then pagination, then a frontend bug, attaching files
each time and never clearing. After ~6 turns, ask:

```
List every file and snippet currently in your context for this conversation.
```

Note how much is now irrelevant to whatever you'd ask next. That carried weight
is billed on every subsequent turn.

### Step 6 — apply the five habits

Redo the same three tasks with the anti-pattern fixes from the concept guide:

| Habit | Do |
|-------|-----|
| Scope per task | **New chat** for pagination; don't carry the stats context. |
| Reference, don't resend | "As in `task_repository.py` (already discussed)" instead of re-attaching it. |
| Summarise specs | Attach `worksheets/context-brief-*.md`, not `usecase.md`. |
| Select, don't dump | `#selection` of one function, not the whole file. |
| Compress over big-window | Use a smaller/cheaper model with a tight brief before reaching for a max-context model. |

### Step 7 — write `CONTEXT.md`

Capture the repo's context conventions so the next person doesn't re-derive them:

```md
<!-- CONTEXT.md -->
# Working with AI context in this repo

## Start here
- Shared Copilot Space: **TaskBoard Context** (ask a maintainer for access).
- Per-task: run `/context-brief` (`.github/prompts/context-brief.prompt.md`) and
  attach only its output.

## What's usually enough context
1. `.github/copilot-instructions.md` (auto-included)
2. The one repository/service/router file that is the pattern for the change
3. The 6 contract lines from `usecase.md` that apply
4. The one test file you'll touch

## What to leave out unless the task needs it
- The frontend, for backend-only tasks (and vice versa)
- The other two backends — parity is checked with the `port-endpoint` skill, not
  carried as context
- Whole files when a `#selection` will do
- `database/schema.sql` unless the task changes the schema

## Session hygiene
- New chat when the task changes.
- Don't re-attach a file already discussed — reference it.
- Attach compressed briefs (`worksheets/context-brief-*.md`), not raw specs.
- Reach for a large-context model only after a tight brief has failed.
```

### Step 8 — commit

```bash
git add CONTEXT.md && git commit -m "Module 03 Lab 05: repo context conventions"
```

Optionally open a PR merging `module-03-context` into `main` so `CONTEXT.md`,
`context-brief.prompt.md`, and the token helper are on the default branch for
Module 04.

## Verify

- [ ] A **TaskBoard Context** Space exists, curated (not dumped), with
      instructions, and shared with at least one other person.
- [ ] A Space chat answers a stats-endpoint question grounded in your files.
- [ ] You audited a long session and can point to the carried-but-irrelevant
      context.
- [ ] `CONTEXT.md` is committed.

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| No Spaces option | Needs Copilot Enterprise/Business and org enablement — ask your admin. Fall back to a shared `CONTEXT.md` + the `/context-brief` prompt. |
| Space answers are generic | Add the key files explicitly (don't rely only on "repository"); tighten the Space instructions. |
| Session context list is unavailable | Not every build exposes it — instead start counting: every `#` you add and never remove is still there. |
| PR to main blocked | Branch protection from Module 02 — get the review, that's the intended gate. |

## Recap — Module 03 complete

You added to the Task Board repo:

```
labs/module-01/
├── .github/prompts/context-brief.prompt.md   # Lab 04 — /context-brief
├── CONTEXT.md                                 # Lab 05 — repo context conventions
├── tools/estimate-tokens.{sh,ps1}             # set-up — rough token counter
└── worksheets/                                # your journals, manifest, briefs, worksheet
```

…plus a shared **TaskBoard Context** Copilot Space, and a measured baseline
(the token worksheet) for every later module's cost discussion.

**Module 04** builds specifications on exactly this discipline: a spec is a
deliberately layered, compressed, scoped context artifact — and the
`/context-brief` habit is the warm-up for writing one.
