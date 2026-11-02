# README: name the factory's pipelines file (#104)

## Goal

The README gains one line that names the factory's pipelines file, so a reader of the README can find where the factory's pipelines are defined. This is one of the two independent pieces of the Feature (#102, "T6.30a: merge acts under the review rule"); the other Story, #103, adds its own line and is not specified here.

## Scope

Done when:

- `README.md` has one new line that names the factory's pipelines file by its path in the repository;
- the line is written in the same style as the README's other lines.

Out of scope:

- any other change to the README, or to any other file of the repository;
- the content of the pipelines file itself;
- the line added by #103.

## Acceptance criteria

1. `README.md` contains exactly one line that did not exist before this change.
2. That line names the factory's pipelines file by its path in the repository, and the path resolves to a file that exists on the default branch.
3. The line is a single line of text: no heading, no list marker added beyond the README's own convention, and no code block.
4. No file other than `README.md` is changed, and `specs/story-104.md` is the only file added.
5. The change is one commit on the branch for this spec, and the commit message is `spec: #104`.

## Open questions

- The Story does not say which file is "the factory's pipelines file", nor its path. This spec assumes it is the file the repository holds for the pipelines, located at the path the implementer finds when the README line is written; the path must resolve to an existing file, as criterion 2 requires.
- The Story does not say where in the README the line goes. This spec assumes the implementer places it where it reads naturally next to the README's other lines, without reordering them.
