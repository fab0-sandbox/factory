# T5.23 close A: request id header (#76)

## Goal

Every API answer carries an `X-Request-Id` header. The value echoes the client's `X-Request-Id` when the client sends one, and is a new UUID otherwise, so a request can be traced across the client and the server. This is the request-id part of the Feature's close (#75): each Story of the close gets its own short spec before any code is written.

## Scope

Done when:

- every API answer includes an `X-Request-Id` header with a non-empty value;
- when the request carries an `X-Request-Id` header, the answer's header has the same value;
- when the request carries none, the answer's header is a new UUID, different for each request.

Out of scope:

- generating or storing request ids anywhere beyond this one answer (no logging, tracing or database changes);
- validating or rejecting the format of a client's `X-Request-Id`;
- headers on anything that is not an API answer (static pages, health checks unless they are API answers).

## Acceptance criteria

1. A successful API answer contains an `X-Request-Id` header with a non-empty value.
2. A request sent with `X-Request-Id: abc-123` gets an answer whose `X-Request-Id` is exactly `abc-123`.
3. A request sent without `X-Request-Id` gets an answer whose `X-Request-Id` is a UUID in the canonical 8-4-4-4-12 hexadecimal form.
4. Two requests sent without `X-Request-Id` get two different `X-Request-Id` values.
5. An answer that refuses a request (for example a 4xx or 5xx from the API) still contains the `X-Request-Id` header.
6. The header appears on every API answer type, not only on some routes: a check over the route list finds none without it.

## Open questions

- The Story does not say whether a client's `X-Request-Id` must be validated or length-limited. This spec assumes it is echoed as sent, with no validation, as Out of scope above says.
- The Story does not say which answers count as "API answers". This spec assumes every response under the API's path prefix, and excludes non-API routes.
- The Story does not say whether the header must also appear on answers produced before routing, such as a generic 404 for an unknown path. This spec assumes it must, because such an answer is still an API answer when it is under the API's path prefix.
