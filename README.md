# claude-plugins

Shared Claude Code plugins.

## Install

Add to a repo's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "bmarotta": { "source": { "source": "github", "repo": "bmarotta/claude-plugins" } }
  },
  "enabledPlugins": { "github-workflow@bmarotta": true }
}
```

Or locally: `/plugin marketplace add bmarotta/claude-plugins` then `/plugin install github-workflow@bmarotta`.

## github-workflow

- `/github-workflow:implement-issue <n>`
- `/github-workflow:review-loop <pr>`
- agent `pr-reviewer`

Repo-specific build, test, format and lint commands come from each repo's `AGENTS.md` / `CLAUDE.md`, then `package.json`, `composer.json`, `Makefile`. Missing tools are skipped.
