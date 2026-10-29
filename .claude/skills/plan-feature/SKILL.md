---
name: plan-feature
description: Plans a Feature in the fab0 sandbox - reads it and its Stories and posts the plan as one comment on the Feature. Use when a fab0 step envelope names it.
---

# plan-feature

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
