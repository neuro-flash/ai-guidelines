# Setup

AI clients don't fetch URLs in their instruction files (e.g. `@https://...` in `CLAUDE.md` is silently ignored). So the guidelines must be checked out to `~/.ai-guidelines/` with git and updated at the start of every session. This requires SSH access to the neuro-flash GitHub organization:

```bash
(export GIT_SSH_COMMAND="ssh -o ConnectTimeout=10 -o BatchMode=yes"; [ -d ~/.ai-guidelines/.git ] || { t=$(mktemp -d) && git clone -q git@github.com:neuro-flash/ai-guidelines.git "$t" && rm -rf ~/.ai-guidelines && mv "$t" ~/.ai-guidelines; rm -rf "$t"; }; git -C ~/.ai-guidelines pull -q --ff-only)
```

It clones the repo if `~/.ai-guidelines` isn't a checkout yet (replacing files from an older setup), then fetches and pulls updates. If that fails (e.g. offline), the last pulled version stays in place.

## Claude Code

Add a `SessionStart` hook to `~/.claude/settings.json`. It updates the checkout and adds the files to the context of every session:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "(export GIT_SSH_COMMAND=\"ssh -o ConnectTimeout=10 -o BatchMode=yes\"; [ -d ~/.ai-guidelines/.git ] || { t=$(mktemp -d) && git clone -q git@github.com:neuro-flash/ai-guidelines.git \"$t\" && rm -rf ~/.ai-guidelines && mv \"$t\" ~/.ai-guidelines; rm -rf \"$t\"; }; git -C ~/.ai-guidelines pull -q --ff-only); for f in GLOBAL go typescript; do cat ~/.ai-guidelines/$f.md 2>/dev/null; echo; done"
          }
        ]
      }
    ]
  }
}
```

Merge it into existing `hooks` if there are any, replacing the hook of an older setup. Don't additionally import the files in `CLAUDE.md`, or they're loaded twice.

## Other AI clients

1. If the client has session start hooks, run the update command there.
2. Otherwise run it once now, then add it to `~/.bashrc` / `~/.zshrc` (with `&` at the end so it runs in the background), so it refreshes whenever a terminal is opened.
3. Add to the client's global instructions file (e.g. `~/.codex/AGENTS.md`, `~/.gemini/GEMINI.md`):

   ```
   Always follow ~/.ai-guidelines/GLOBAL.md. At the start of every session, read it.
   If you can run shell commands, first run the update command from
   ~/.ai-guidelines/SETUP.md to get the current version.
   ```

   If the client supports file imports (e.g. Gemini CLI: `@~/.ai-guidelines/GLOBAL.md`), use an import instead. It's more reliable than asking the AI to read the file, but a refresh only applies from the next session on.

When a new file is added to this repo that must be loaded in every session, add it to the Claude Code hook above.
