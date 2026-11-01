---
name: plan-story
description: Plans a specced Story in the fab0 sandbox - reads its merged spec and posts the plan as one comment on the Story. Use when a fab0 step envelope names it.
---

# plan-story

You plan a Story for the `full-flow` pipeline once its spec is merged and the product owner has written `/plan`:
the work its spec asks for, in the order to do it. The step makes nothing (`makes: none`). Your one output is a
comment on the Story. When the step is complete, fab0 labels the Story `Shipped`.

## Inputs

The envelope's `inputs.items` names the Story first, then its Feature, as `<owner>/<repo>#<number>`.
`inputs.prs` holds the Story's merged spec pull request with its approved `sha`. Read the Story, its comments, and
the spec at that commit:

```
gh api repos/<owner>/<repo>/issues/<number>
gh api repos/<owner>/<repo>/issues/<number>/comments
gh api "repos/<owner>/<repo>/contents/specs/story-<number>.md?ref=<sha>" --jq .content | base64 -d
```

If `inputs.prs` names no spec pull request for the Story, or the file isn't there, fail the step with that reason.

## What to write

One comment on the Story, in Markdown, with these sections and nothing else:

1. **Steps**: a numbered list, in the order to do them. Each gives what it changes and the spec's acceptance
   criteria it meets, by number.
2. **Not covered**: acceptance criteria no step meets, and why. Write "None" if there are none.

The comment's last line is the envelope's `markers.comment`. Post it with:

```
gh api -X POST repos/<owner>/<repo>/issues/<number>/comments -f body=<body>
```

## Rules

- Plan only. Don't create, edit or close issues, change files, or change any labels. fab0 sets the Story's labels.
- If a comment ending with this step's `markers.comment` is already on the Story, edit that comment
  (`gh api -X PATCH repos/<owner>/<repo>/issues/comments/<id> -f body=<body>`). Don't post a second one.
- The step returns no outputs. Report it with `complete_step` and empty `outputs`.
