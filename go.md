# Go

- Prefer early returns over nested `if`/`else`: handle errors and edge cases first, keep the happy path unindented.
- Return HTTP errors via nf-go `humaErr`, never build error responses manually (gin `nferr.ServErr`/`CliErr` are deprecated):
  - `humaErr.ServerError(ctx, err, "could not load refund")` for unexpected errors (500, logs with a unique error code).
  - `humaErr.ClientError(http.StatusNotFound, "refundNotFound", "refund %s not found", id)` for client errors (4xx, camelCase translationKey).
- Propagate `context.Context` through all calls. No package-level mutable state, so instances stay stateless.
