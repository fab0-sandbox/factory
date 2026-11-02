# T6.26 a: error response shape (#97)

## Goal

Every API error response has one JSON body shape, with named fields and stated types, so a client reads any error the same way whatever endpoint or error produced it. This is the first piece of the Feature (#96, "Give every API error one JSON shape, then list the error codes it can carry"); the list of error codes is the second piece, #98, and is not specified here.

## Scope

Done when:

- every API error response has a JSON body with the same top-level shape;
- the shape names, for each error, a machine-readable code, a human-readable message, and the field the error concerns when there is one;
- each of those fields has a stated type, and the type is the same on every endpoint;
- the body is the same shape for `4xx` and `5xx` responses.

Out of scope:

- the list of error codes and what each one means (#98);
- the HTTP status code chosen for each error;
- success responses and their bodies;
- localisation of the message text.

## Acceptance criteria

1. Every `4xx` and `5xx` response from an API endpoint has a `Content-Type` of `application/json` and a body that parses as a JSON object.
2. Every error body has a top-level `error` object, and no other top-level key.
3. The `error` object has a `code` field whose value is a non-empty string, and a `message` field whose value is a non-empty string.
4. When an error concerns a specific request field, the `error` object has a `field` field whose value is a string naming that field; when it concerns no field, `field` is `null` on every endpoint.
5. The `error` object carries no keys other than `code`, `message` and `field`.
6. An error from a route that fails validation, from a route that does not exist under the API's path prefix, and from an unhandled server failure all have the same shape, checked over at least one case of each.
7. The shape is written down in the API's documentation with the name and type of every field, so a reviewer can compare a response against it.

## Open questions

- The Story does not name the fields of the error body. This spec assumes the top-level `error` object with `code`, `message` and `field`, as above; the names are the implementer's to change before the code is written, but criteria 2 to 5 must then be read with the new names.
- The Story does not say whether `field` may be absent or must be `null` when there is no field. This spec requires `null`, the same on every endpoint.
- The Story does not say whether an error may concern more than one field. This spec assumes one field at most; a request with several invalid fields reports the first one found.
- The Story does not say whether a route outside the API's path prefix returns the shape. This spec assumes it does not, since only API endpoints are covered.
- The Story does not say what a `code` looks like (its format or case). This spec assumes a non-empty string; the format is fixed in #98.
