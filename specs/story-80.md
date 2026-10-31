# T5.23 close B: page size limits (#80)

## Goal

Every list endpoint has a defined default page size and a maximum page size, and a request asking for a limit above the maximum is refused with a `400`. This gives the pagination of the Feature (#78) fixed, documented bounds, so a client can rely on how many items one page holds.

## Scope

Done when:

- each paginated list endpoint uses a default page size when the client sends no limit;
- each paginated list endpoint refuses a limit above the maximum with `400`, and the answer says which limit applies;
- the default and the maximum are the same values on every paginated list endpoint.

Out of scope:

- choosing the cursor or offset scheme, which the pagination Feature already defines;
- changing how pages are ordered or filtered;
- endpoints that are not paginated lists;
- rate limiting and its headers.

## Acceptance criteria

1. A paginated list request with no limit returns at most the default page size of items.
2. A paginated list request with a limit at or below the maximum returns no more items than the limit asks for.
3. A paginated list request with a limit above the maximum gets a `400` answer, and no items.
4. The `400` answer for a limit above the maximum names the limit parameter and the maximum allowed value.
5. A limit of `0`, or a negative limit, is refused the same way the code refuses any other invalid limit, and the spec does not change that behaviour.
6. Every paginated list endpoint shows the same default and maximum, checked over the route list.

## Open questions

- The Story does not give the default or the maximum page size. This spec assumes the values already used by the existing pagination code are the ones to document and enforce; if none exist, the Story's author must name them before the code is written.
- The Story does not say whether a limit of `0` counts as "above the maximum". This spec assumes it is handled by the existing invalid-limit path (criterion 5), not by the `400` for an oversized limit.
- The Story does not say which endpoints are paginated lists. This spec assumes every endpoint that returns a page of items under the API's path prefix.
