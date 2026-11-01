---
name: plan-feature
description: Plans a Feature in the fab0 sandbox - posts the plan as one comment on the Feature (record-flow), or writes its Stories as sub-issues (full-flow). Use when a fab0 step envelope names it.
---

# plan-feature

Two pipelines name this skill. When the envelope's `step.makes` is `none` (`record-flow`), follow this page down to
"Writing the Stories". When it is `issues` (`full-flow`), go to "Writing the Stories".

You plan a Feature for the `record-flow` pipeline: which Stories deliver it, in what order, and what each one
leaves done. The step makes nothing (`makes: none`). Your one output is a comment on the Feature. When the step is
complete, fab0 labels the Feature `InProgress`.

## Inputs

The envelope's `inputs.items` names the Feature, as `<owner>/<repo>#<number>`. Read it, its Stories (sub-issues)
and its comments:

```
gh api repos/<owner>/<repo>/issues/<number>
gh api repos/<owner>/<repo>/issues/<number>/sub_issues
gh api repos/<owner>/<repo>/issues/<number>/comments
```

## What to write

One comment on the Feature, in Markdown, with these sections and nothing else:

1. **Goal**: one or two sentences, in the Feature's own terms.
2. **Stories**: a numbered list, in the order they should be done. Each item gives the Story's `#number`, its
   title and one line on what it leaves done. Say which Story waits on another one.
3. **Gaps**: work the Feature asks for that no Story covers, and Stories that fall outside the Feature. Write
   "None" if there are none.

The comment's last line is the envelope's `markers.comment`. Post it with:

```
gh api -X POST repos/<owner>/<repo>/issues/<number>/comments -f body=<body>
```

## Rules

- Plan only. Don't create, edit or close issues, and don't change any labels. Stories become `Ready` when a person
  says so, and fab0 sets the Feature's labels.
- If the Feature has no Stories, list none and name the missing work under Gaps. The step is still done.
- If a comment ending with this step's `markers.comment` is already on the Feature, edit that comment
  (`gh api -X PATCH repos/<owner>/<repo>/issues/comments/<id> -f body=<body>`). Don't post a second one.
- Keep the plan short. Most Features need a few Stories.
- The step returns no outputs. Report it with `complete_step` and empty `outputs`.

## Writing the Stories (`full-flow`)

You plan a Feature for the `full-flow` pipeline by writing its Stories (`makes: issues`). The product owner approves
the plan with `/approve` on the Feature; then fab0 specs each Story whose blockers are closed, and holds the Stories
past the planner's budget until a person releases them.

Read the Feature and its comments as above. Then, for each piece of work the Feature asks for, in the order it
should be done, write one Story. `fab0-work` (its section 4) says how to create and link them; for each one:

1. Create it, with the type `Story`. Its title says what it leaves done; its body says it in a few lines, and names
   the Feature as `Part of #<feature>.`
2. Link it as a sub-issue of the Feature.
3. Assign it to the Feature's first assignee:
   `gh api -X POST repos/<owner>/<repo>/issues/<story>/assignees -f "assignees[]=<login>"`.
4. When it needs an earlier Story done first, add that Story as its blocker:
   `gh api -X POST repos/<owner>/<repo>/issues/<story>/dependencies/blocked_by -F issue_id=<the earlier Story's id>`.
5. Label it `Go`.

Report the step with `complete_step` and `outputs` `{ "issues": ["<owner>/<repo>#<story>", ...] }`, in the order
you wrote them.

- If the Feature asks for no work, write no Story and report `issues` empty.
- Don't edit the Feature or change its labels, and don't post a plan comment: the Stories are the plan.
