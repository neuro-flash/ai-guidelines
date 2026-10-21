# Go

- Propagate `context.Context` through all calls, carrying `userId` and `requestId` for logging.
- Return errors wrapped with context (`fmt.Errorf("...: %w", err)`); map them to the global error response format only at the HTTP layer.
- No package-level mutable state, so instances stay stateless.
