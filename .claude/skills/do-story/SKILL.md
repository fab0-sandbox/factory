---
name: do-story
description: Does a Story in the fab0 sandbox - does the work it asks for and records it as one comment on the Story. Use when a fab0 step envelope names it.
---

# do-story

You do a Story for the `record-flow` pipeline and record what you did. The step makes nothing (`makes: none`). Your
one output is a comment on the Story. When the step is complete, fab0 labels the Story `Done`, and when every Story
of the Feature is `Done`, the Feature is finished.

## Inputs

The envelope's `inputs.items` names the Story, as `<owner>/<repo>#<number>`. Read it, its comments and its Feature
(the parent issue), whose plan comment says what this Story leaves done:

```
gh api repos/<owner>/<repo>/issues/<number>
gh api repos/<owner>/<repo>/issues/<number>/comments
gh api repos/<owner>/<repo>/issues/<number>/parent
gh api repos/<owner>/<repo>/issues/<parent>/comments
```

## What to write

Do the work the Story asks for. Then post one comment on the Story, in Markdown, with these sections and nothing
else:

1. **Done**: what you did, in a few lines.
2. **Checked**: how you know it is done: a command and its output, or a link to what changed.

The comment's last line is the envelope's `markers.comment`. Post it with:

```
gh api -X POST repos/<owner>/<repo>/issues/<number>/comments -f body=<body>
```

## Rules

- If the Story needs a change to a repository, which needs a pull request, fail the step: this step makes nothing.
- Don't create, edit or close issues, and don't change any labels. fab0 sets the Story's labels.
- If a comment ending with this step's `markers.comment` is already on the Story, edit that comment
  (`gh api -X PATCH repos/<owner>/<repo>/issues/comments/<id> -f body=<body>`). Don't post a second one.
- The step returns no outputs. Report it with `complete_step` and empty `outputs`.
