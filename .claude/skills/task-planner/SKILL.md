---
name: task-planner
description: Splits a Story's approved specs into Tasks in the fab0 sandbox, one pull request each, in the repo where its work happens. Use when a fab0 attempt brief names it.
---

# task-planner

You turn the run's approved specs into Tasks. Each Task becomes one pull request in one repo, made by the
`implement` skill and checked by `code-review`, so each must stand on its own.

## Inputs

The brief gives the Story and each approved spec as `<owner>/<repo>:docs/specs/<run>.md`, merged into the repo's
`main`. Read each spec with:

```
gh api repos/<owner>/<repo>/contents/docs/specs/<run>.md -H "Accept: application/vnd.github.raw"
```

## What to write

The YAML list the brief asks for. Each item has:

- `title`: an imperative line, under 70 characters.
- `repo`: `backend` or `frontend`, where the change is made.
- `body`: Markdown with three parts:
  - **Change**: what to build or change, in a few sentences, naming the spec's section.
  - **Done when**: a checklist of observable checks, each one a test the implementer writes. Take them from the
    spec's acceptance checks, and keep the spec's numbering (R1, R2, …) so the reviewer can trace them.
  - **Not in this task**: what a neighbouring Task does instead, if it could be confused.

## Rules

- One Task per pull request a reviewer can read in ten minutes. Most runs need two to five Tasks.
- Tasks must not depend on each other's unmerged work: they run in parallel, each branched from `main`. Where the
  frontend needs the backend's API, the frontend Task works against the API as its spec defines it.
- Every acceptance check of every spec is covered by exactly one Task.
- On a revision, change what the feedback asks for and keep the rest.
- Write only the file the brief names. Don't edit the repo, and don't write to GitHub: fab0 creates the Tasks.
