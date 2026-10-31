# T5.22 two members: rate limit headers (#72)

## Goal

Every answer of the API carries the three rate limit headers (`RateLimit-Limit`, `RateLimit-Remaining`, `RateLimit-Reset`), so a client can see its quota before it is refused. This is the rate limit part of the Feature "T5.22 two members: rate limits" (#70).

## Scope

Done when:

- every API answer, successful or refused, carries `RateLimit-Limit`, `RateLimit-Remaining` and `RateLimit-Reset`;
- the three values describe the same quota window as the request that received them.

Out of scope:

- choosing the limit values or the window length;
- the refusal itself (the status code and body of a limited request);
- headers on anything that is not an API answer.

## Acceptance criteria

1. A successful API answer carries all three headers.
2. A refused (rate limited) API answer carries all three headers.
3. `RateLimit-Limit` is a positive integer: the quota of the window.
4. `RateLimit-Remaining` is a non-negative integer, never greater than `RateLimit-Limit`, and decreases by one for each request in the window.
5. `RateLimit-Reset` is a non-negative integer: the seconds until the window resets.
6. The three values in one answer are taken from the same window, as of the moment of that answer.
7. No API answer lacks a header because of an error path (an error answer is an API answer too).

## Open questions

- The Story does not say the window length or the quota. The spec assumes both are already set by the rate limiter and are only exposed here.
- The Story does not say whether `RateLimit-Reset` is in seconds or a timestamp. The spec assumes seconds, as in the IETF draft for these headers.
