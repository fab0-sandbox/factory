# T5.22 two members: per-client rate limit (#71)

## Goal

API clients are held to 60 requests a minute each, so that one client cannot starve the others of the API. This is the rate-limit part of the Feature T5.22 rate limits (#70): a client over its allowance is refused until its window resets.

## Scope

Done when:

- every API request is counted against the client that sent it;
- a client that has made 60 requests within the current minute gets the next request answered `429 Too Many Requests` with a `Retry-After` header giving the whole seconds until it may try again;
- clients under their allowance are unaffected, and their requests are served as before.

Out of scope:

- limits other than the per-client limit of 60 requests a minute (per-route, per-IP, per-account, or global limits);
- changing the allowance per client or per plan;
- storing or reporting rate-limit history;
- changes to the response bodies of requests that are served.

## Acceptance criteria

1. A client sending 60 requests within one minute has all 60 served with their normal status.
2. The 61st request from that client within the same minute is answered `429` and carries a `Retry-After` header with a whole number of seconds, at least 1.
3. Requests from a different client in the same minute are not refused by the first client's count.
4. After the window for a refused client has passed, its next request is served, not refused.
5. Each refusal's `Retry-After` value does not exceed the seconds left in the window it refers to.
6. Requests served under the limit are returned with the same status and body they would have without the limit.

## Open questions

- Window: this spec assumes a fixed one-minute window, starting with the client's first request in it and resetting one minute later. A sliding window would also meet the goal; the choice is open.
- Client identity: this spec assumes each API client is identified by the credential it presents (for example its API key or token). If a request carries no credential, the spec assumes it is counted against its source address. Neither is stated in the Story.
- Storage: the count is assumed to be kept per running instance. Whether it must be shared across instances is not stated.
