# Setup

AI clients don't fetch URLs in their instruction files (e.g. `@https://...` in `CLAUDE.md` is silently ignored). So the guidelines must be downloaded to `~/.ai-guidelines/` at the start of every session:

```bash
mkdir -p ~/.ai-guidelines && for f in GLOBAL go typescript; do curl -sf --max-time 10 https://raw.githubusercontent.com/neuro-flash/ai-guidelines/main/$f.md -o ~/.ai-guidelines/$f.md.tmp && mv ~/.ai-guidelines/$f.md.tmp ~/.ai-guidelines/$f.md; rm -f ~/.ai-guidelines/$f.md.tmp; done
```

If the download fails (e.g. offline), the last downloaded version stays in place.

## Claude Code

Add a `SessionStart` hook to `~/.claude/settings.json`. It downloads the files and adds them to the context of every session:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "mkdir -p ~/.ai-guidelines && for f in GLOBAL go typescript; do curl -sf --max-time 10 https://raw.githubusercontent.com/neuro-flash/ai-guidelines/main/$f.md -o ~/.ai-guidelines/$f.md.tmp && mv ~/.ai-guidelines/$f.md.tmp ~/.ai-guidelines/$f.md; rm -f ~/.ai-guidelines/$f.md.tmp; cat ~/.ai-guidelines/$f.md 2>/dev/null; echo; done"
          }
        ]
      }
    ]
  }
}
```

Merge it into existing `hooks` if there are any. Don't additionally import the files in `CLAUDE.md`, or they're loaded twice.

## Other AI clients

1. If the client has session start hooks, run the download command there.
2. Otherwise add it to `~/.bashrc` / `~/.zshrc` (with `&` at the end so it runs in the background), so it runs whenever a terminal is opened.
3. Add to the client's global instructions file (e.g. `~/.codex/AGENTS.md`, `~/.gemini/GEMINI.md`):

   ```
   Always follow ~/.ai-guidelines/GLOBAL.md. At the start of every session, read it.
   If you can run shell commands, first run the download command from
   https://github.com/neuro-flash/ai-guidelines/blob/main/SETUP.md to get the current version.
   ```

   If the client supports file imports (e.g. Gemini CLI: `@~/.ai-guidelines/GLOBAL.md`), use an import instead.

When a new file is added to this repo, add it to the commands above.
