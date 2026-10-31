# T5.23 close A: request ids in logs (#77)

## Goal

Make every log line written while serving a request carry that request's id, so a single request can be followed through the service's logs. It serves the Feature "T5.23 close A: request ids" (#75) by making the request id visible in the logs, the property the Feature's close depends on.

## Scope

Done when:

- Every log line written while a request is being handled includes that request's id, in the log field `request_id`.
- The id on the log lines of a request is the same id that the request is identified by.
- Log lines written outside any request (start-up, shutdown) are unchanged.

Out of scope:

- Generating or propagating request ids to other services, and any change to the id format.
- Changes to what the service logs, beyond adding the request id.
- Any change to responses, routes, or the log destination.

## Acceptance criteria

1. Make a request to any route; every log line written while handling it contains that request's id in the field `request_id`.
2. Two requests handled concurrently each log only their own id on their own lines.
3. The id in the log lines matches the id the request carries, as the service already identifies it.
4. A log line written at start-up or shutdown, outside any request, carries no request id and is otherwise unchanged.
5. No route's response changes in status, headers, or body.

## Open questions

- Where the request id comes from: this spec assumes the service already identifies each request with an id. If it does not, the implementer must add one before logs can carry it.
- The Story does not say whether a log line written from a background task spawned by a request must carry the id: this spec assumes it does not.
