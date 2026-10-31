# T5.22 two members: rate limit headers (#72)

## Goal

Every API answer carries the three rate-limit headers (`RateLimit-Limit`, `RateLimit-Remaining`, `RateLimit-Reset`), so a client can see its budget on each response. This is the rate-limit part of the Feature's two-member check (#70): the other member reviews this spec before any code is written.

## Scope

Done when:

- every API answer includes `RateLimit-Limit`, `RateLimit-Remaining` and `RateLimit-Reset`;
- the headers are set on successful answers and on answers that refuse a request for exceeding the limit;
- the values describe the same limit window the server enforces.

Out of scope:

- choosing or changing the limit values themselves;
- the rate-limiting algorithm or storage;
- headers on anything that is not an API answer (static pages, health checks unless they are API answers);
- documenting the headers for external clients beyond what the Story asks.

## Acceptance criteria

1. A successful API answer contains all three headers, each with a non-empty value.
2. `RateLimit-Limit` is the number of requests allowed in the current window.
3. `RateLimit-Remaining` is the number of requests still allowed in the current window after the answered request, never negative.
4. `RateLimit-Reset` is the number of seconds until the current window resets.
5. An answer that refuses a request because the limit is exhausted contains all three headers, with `RateLimit-Remaining` at `0`.
6. For a sequence of requests inside one window, `RateLimit-Remaining` decreases by one per request and `RateLimit-Limit` stays the same.
7. The headers appear on every API answer type, not only on some routes: a check over the route list finds none without them.

## Open questions

- The Story does not say whether `RateLimit-Reset` counts seconds or a timestamp. This spec assumes seconds until reset, the form the draft IETF `RateLimit` headers use.
- The Story does not say which answers count as "API answers". This spec assumes every response under the API's path prefix, and excludes non-API routes.
- The Story does not name the limit window or the limit values. This spec assumes the existing limiter defines both, and the headers only report them.
