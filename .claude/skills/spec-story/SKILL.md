---
name: spec-story
description: Writes a Story's spec in the fab0 sandbox - reads the Story and its Feature and writes the spec as one markdown file, delivered by a pull request. Use when a fab0 step envelope names it.
---

# spec-story

You write the spec of a Story for the `routine-flow` pipeline. The step makes a pull request (`makes: pr`,
`kind: spec`) that adds one markdown file. A member of the team reviews it; once it is approved and merged, fab0
labels the Story `Specced`, and when every Story of the Feature is `Specced`, the Feature is finished.

## Inputs

The envelope's `inputs.items` names the Story, as `<owner>/<repo>#<number>`. Read it, its comments and its Feature
(the parent issue):

```
gh api repos/<owner>/<repo>/issues/<number>
gh api repos/<owner>/<repo>/issues/<number>/comments
gh api repos/<owner>/<repo>/issues/<number>/parent
```

When `revision` is set, the pull request already holds a spec: read it on the branch, and address
`revision.feedback`.

## What to write

One file, `specs/story-<number>.md`, in Markdown, with these sections and nothing else:

1. A title: `# <Story title> (#<number>)`.
2. **Goal**: one or two sentences, in the Story's own terms, and how it serves the Feature.
3. **Scope**: what the Story leaves done, as a short list; then what it leaves out.
4. **Acceptance criteria**: a numbered list, each one a check a reviewer can make and say passes or fails.
5. **Open questions**: what the Story doesn't say and the spec had to assume. Write "None." when there are none.

On a revision, change this file only, and only where the feedback asks.

## Delivering it

Commit the file on the envelope's `markers.branch`, with the message `spec: #<number>`. Pushing it, the pull request
and its markers are `fab0-work`'s (its section 4). The pull request's title is `Spec: <Story title>`, and its body
says in one line which Story it specs, above the marker lines.

## Rules

- Write nothing but this one file: no code, and no other file of the repository.
- Don't create, edit or close issues, and don't change any labels. fab0 sets the Story's labels.
- The step returns no outputs but the pull request: report it with `complete_step` and `outputs` `{ "pr": "<owner>/<repo>#<pr number>" }`.
