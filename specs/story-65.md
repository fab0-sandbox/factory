# T5.20 routine check: GET /ready (#65)

**Goal.** Give the service a `GET /ready` endpoint that tells a caller whether the service can take traffic: it answers 200 once its dependencies are reachable, and 503 until then. This is the readiness check of the Feature's health endpoints (#63), which the routine check uses to confirm the service is up.

**Scope.**

Done when:

- `GET /ready` exists and is routed alongside the service's other endpoints.
- It answers 200 when every dependency it checks is reachable.
- It answers 503 while any of those dependencies is unreachable.

Left out:

- Liveness (`GET /health`), which belongs to another Story of #63.
- Retries, timeouts tuning, or caching of the dependency checks.
- Any change to the dependencies themselves.

**Acceptance criteria.**

1. A `GET /ready` request to the running service returns HTTP status 200 when all of its dependencies are reachable.
2. A `GET /ready` request returns HTTP status 503 when at least one dependency is unreachable, with a JSON body that lists the names of the dependencies that are down.
3. A dependency check that doesn't answer within 2 seconds counts as unreachable, so `GET /ready` returns 503 within 3 seconds of the request, and the service keeps answering after the dependency comes back: the next `GET /ready` returns 200.
4. The endpoint is covered by an automated test that exercises both the 200 and the 503 case.
5. The endpoint's path, methods and responses are documented alongside the service's other endpoints.

**Open questions.**

- Which dependencies count as "the service's dependencies"? The Story doesn't name them, so the spec assumes every external dependency the service already connects to at startup.
- The Story doesn't say how long a dependency check may take. The spec assumes 2 seconds per check, so a slow dependency is reported as down rather than holding the request open.
- The response body on 200 isn't specified; the spec assumes only the status code is part of the contract there. The 503 body is the JSON list of down dependencies named in criterion 2.
