# T6.26 b: error codes (#98)

## Goal

Every error an API endpoint can return carries one of a named, documented set of error codes, each with a stated HTTP status and meaning, so a client can branch on `error.code` and know what the failure was. This is the second piece of the Feature (#96, "Give every API error one JSON shape, then list the error codes it can carry"); it builds on the error response shape in #97, which fixes the `code` field this list fills.

## Scope

Done when:

- there is one written list of error codes that the API can return;
- each code in the list is a non-empty string, unique within the list;
- each code states the HTTP status it is returned with and its meaning, in one or two sentences;
- every error response the API returns uses a code from this list, and no other value;
- the list is kept with the API's documentation, next to the error shape from #97.

Out of scope:

- the shape of the error body and its fields (#97);
- success responses and their bodies;
- the message text of each error;
- localisation of the message text;
- codes for errors that no endpoint can return today.

## Acceptance criteria

1. The API documentation has one list of error codes, and it is the only list.
2. Every entry in the list has a `code`, an HTTP `status` (a three-digit `4xx` or `5xx` number), and a `meaning` of one or two sentences.
3. No two entries in the list have the same `code`.
4. Every `code` is a non-empty string, and every `code` matches the format fixed in #98's documentation (one format for the whole list).
5. Every `4xx` and `5xx` response returned by an API endpoint has an `error.code` that appears in the list.
6. For each entry, at least one endpoint or test case returns that code with the stated status, so no code in the list is unreachable.
7. The status a code states is the status the response carries: a check of each entry against one response returns the same status number.
8. A reviewer can read the list and find, for any `error.code` in an API response, its status and meaning, without reading the source code.

## Open questions

- The Story does not say what the codes are. This spec assumes they are derived from the errors the API already returns today; the implementer lists them from the current code, and a code the API does not return today is not added.
- The Story does not say what format a `code` takes (case, separators). This spec assumes one format is chosen and written down in the list's documentation, and criterion 4 is checked against that choice.
- The Story does not say whether the list lives in code or in documentation. This spec assumes the documentation, since criterion 8 needs a reviewer to read it without the source.
- The Story does not say whether a status may map to more than one code. This spec allows it: the same status can carry several codes, each with its own meaning.
- The Story depends on the error response shape in #97. This spec assumes #97 is merged first, so the `error.code` field exists when this list is checked.
