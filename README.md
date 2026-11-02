# claude-plugins

Shared Claude Code plugins.

## Install

Add to a repo's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "bmarotta": { "source": { "source": "github", "repo": "bmarotta/test-claude-plugins" } }
  },
  "enabledPlugins": { "gh@bmarotta": true }
}
```

Or locally: `/plugin marketplace add bmarotta/claude-plugins` then `/plugin install gh@bmarotta`.

## gh

- `/gh:implement-issue <n>`
- `/gh:review-loop <pr>`
- agent `pr-reviewer`

Repo-specific build, test, format and lint commands come from each repo's `AGENTS.md` / `CLAUDE.md`, then `package.json`, `composer.json`, `Makefile`. Missing tools are skipped.
