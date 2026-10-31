# T5.20 routine check: GET /health (#64)

## Goal

Give the service a `GET /health` endpoint that reports it is up, with its version and how long it has been running, so the routine check of the Feature can confirm the service answers. It serves the Feature "T5.20 routine check: health endpoints" (#63) by being the endpoint each health check reads.

## Scope

Done when:

- `GET /health` answers `200 OK` with a JSON body holding the service's `version` and `uptime`.
- `version` is the service's version as the code declares it.
- `uptime` is the number of seconds since the service process started.

Out of scope:

- Authentication or authorization on the endpoint.
- Health checks of dependencies (databases, queues, other services).
- Any other route, and any change to existing routes.

## Acceptance criteria

1. `GET /health` returns HTTP status `200`.
2. The response `Content-Type` is `application/json`.
3. The body is a JSON object with a `version` field whose value is a non-empty string.
4. The body is a JSON object with an `uptime` field whose value is a non-negative number of seconds.
5. `uptime` grows between two calls made a few seconds apart on the same running process.
6. `version` is the same on every call while the process runs.
7. The endpoint needs no credentials: a request with no `Authorization` header gets the same `200` response.
8. No other route's response changes.

## Open questions

- Where the version comes from: this spec assumes the version the service already declares (for example in its package manifest). If the service declares none, the implementer must add one before the endpoint can answer.
- The exact name of the uptime unit is fixed here as seconds, a number; the Story does not say which unit it wants.
- The Story does not say whether the endpoint should answer while the service is starting or shutting down. This spec assumes it answers `200` whenever the process is serving requests.
