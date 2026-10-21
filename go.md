# Go

- Services must be stateless: never keep request-spanning state in memory or on local disk. Use DB, buckets or redis instead.
- On 500 return only `{"code": "<unique errorCode>"}` and log the details with that code. 4xx errors include `code`, `error` and a camelCase `translationKey`.
- Log `context.userId` and `context.requestId` (from/propagated via `X-REQUEST-ID` header), no personal data like email or name. Return signed bucket URLs instead of proxying files.
