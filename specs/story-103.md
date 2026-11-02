# README: name the factory's specs folder (#103)

## Goal

The factory's README names the `specs/` folder and says what it holds, so a reader of the repository's front page can find the Story specs without opening the tree. This is one of two independent Stories of the Feature (#102, "T6.30a: merge acts under the review rule"); the README line describes one part of the factory and does not depend on the other Story.

## Scope

Done when:

- the README contains one line that names the `specs/` folder and says it holds the work items' specs;
- the line sits in the README's existing text, and the README's other lines are unchanged.

Out of scope:

- any other README section, heading or rewording;
- any file other than `README.md`;
- the contents of the `specs/` folder, and the naming of its files;
- a description of the other folders or files of the repository.

## Acceptance criteria

1. `README.md` contains a line that includes the text `specs/`.
2. That line says the folder holds the specs of the work items (Stories, Features or Tasks, in the words the line uses), in one sentence.
3. Every line of `README.md` other than that one is unchanged from the README on the default branch.
4. No file other than `README.md` is changed by this pull request, apart from `specs/story-103.md`.
5. The README still renders as Markdown with no broken structure: the heading `# factory` is still its first line.

## Open questions

- The Story does not say where in the README the line goes. This spec assumes it goes after the existing description, as one new line, since the Story asks only for one line.
- The Story does not give the wording. This spec leaves the wording to the implementer, within criterion 2.
