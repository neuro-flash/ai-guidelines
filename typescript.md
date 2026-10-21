# TypeScript

- Frontends never hold secrets (API keys, DB or AI credentials). Anything needing them goes through a backend authenticated with the zitadel/gateway token.
- Show errors with a `translationKey` to the user. Errors without one are server bugs: show a generic error with the `code` for support.
