# T5.23 close B: cursor pagination (#79)

## Goal

List endpoints return a bounded page of results, with an opaque cursor and a `next` link, so a client can walk a collection of any size without the server loading all of it at once. This is the pagination part of the Feature's close (#78): each list endpoint gets the same paging contract, so clients need one way to page.

## Scope

Done when:

- every list endpoint accepts a `limit` query parameter and returns at most that many items;
- every list endpoint accepts an opaque `cursor` query parameter and returns the cursor for the next page;
- each list answer carries a `next` link when more items follow, and no `next` link on the last page;
- a request with a `cursor` returns the page after the one the cursor came from, in the same order as the first page.

Out of scope:

- changing the default or maximum page size values beyond what the Story requires;
- sorting or filtering options on list endpoints;
- pagination of anything that is not a list endpoint;
- the storage format or internal contents of the cursor.

## Acceptance criteria

1. A list request with `limit=N` returns at most `N` items, for any `N` the server accepts.
2. A list request without `limit` returns a default number of items, and the default is stated in the endpoint's documentation.
3. A `limit` above the server's maximum is refused, or capped at the maximum, and the answer says which.
4. When more items follow, the answer contains a `next` link that, followed unchanged, returns the next page.
5. On the last page, the answer contains no `next` link.
6. Following `next` links from the first page until none remains returns every item exactly once, with no gaps and no repeats, for a collection that does not change during the walk.
7. The `cursor` is opaque: a client that alters or decodes it gets a refusal for a malformed cursor, not a page of other items.
8. Every list endpoint behaves the same way: a check over the route list finds none that lacks `limit`, `cursor` or `next` handling.

## Open questions

- The Story does not give the default or maximum `limit`. This spec assumes the existing list endpoints' page sizes are kept as the default, and that a maximum is set at the server's current largest list size.
- The Story does not say whether a `limit` above the maximum is refused or capped. This spec allows either, as criterion 3 says, and leaves the choice to the implementer.
- The Story does not say what happens when the collection changes between two page requests. This spec assumes criterion 6 applies only to a collection that does not change during the walk.
