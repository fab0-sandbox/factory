# T5.38 amendment 7: request id header (#87)

## Goal

Every API response carries an `X-Request-Id` header that a client can quote when it reports a problem, so the request can be found in the server's logs. The header's format, and the rule for when the server generates the id and when it echoes the client's id, are fixed here. This serves the Feature (#86, "Give every API response a request id the client can quote") by making the id present and predictable on every response.

## Scope

Done when:

- every API response, success or error, carries an `X-Request-Id` header;
- when a request carries a valid `X-Request-Id`, the response repeats that same value;
- when a request carries none, or an invalid one, the server generates a new id and returns it;
- the format of the generated id is the same on every endpoint.

Out of scope:

- writing the id into the server's logs, or the log format;
- any request id in the response body;
- tracing across services, or trace-context headers such as `traceparent`;
- endpoints that are not part of the API (static files, health checks left as they are today, if they are outside the API's path prefix).

## Acceptance criteria

1. A request to any API endpoint, with no `X-Request-Id` sent, gets a response that has an `X-Request-Id` header with a non-empty value.
2. A request that sends a valid `X-Request-Id` gets a response whose `X-Request-Id` header is byte-for-byte the value the client sent.
3. A request that sends an `X-Request-Id` that is not valid (see the assumptions in Open questions) gets a response with a newly generated `X-Request-Id`, and the invalid value is not returned.
4. Two requests with no client id get two different `X-Request-Id` values.
5. Error responses (`4xx` and `5xx`) carry the `X-Request-Id` header the same way success responses do.
6. Every generated id has the same format: the same length and character set, checked over a sample of at least 100 responses.
7. The header name is spelled `X-Request-Id` in responses, and a client may send it in any letter case.

## Open questions

- The Story does not define the format of the generated id. This spec assumes an opaque, URL-safe string with no spaces, the same length on every response; the length and alphabet are for the implementer to choose and document, and criterion 6 checks whichever they choose.
- The Story does not say what makes a client's `X-Request-Id` valid. This spec assumes a value is valid when it is 1 to 128 printable ASCII characters with no spaces, and that an invalid value is replaced, not refused. If the team wants an invalid value refused with a `400`, the Story's author must say so before the code is written.
- The Story does not say whether a request that sends more than one `X-Request-Id` header is valid. This spec assumes it is treated as invalid under criterion 3.
- The Story does not say which paths are "API endpoints". This spec assumes every path under the API's path prefix, as the existing routes define it.
