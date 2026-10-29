# TypeScript

- Frontends never hold secrets (API keys, DB or AI credentials). Anything needing them goes through a backend authenticated with the zitadel/gateway token.
- Show errors with a `translationKey` to the user. Errors without one are server bugs: show a generic error with the `code` for support.
- Use yarn, never npm, for installing dependencies and running scripts. Commit `yarn.lock`, never `package-lock.json`.
