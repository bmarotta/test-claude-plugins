---
description: Implement a GitHub issue end-to-end
argument-hint: <issue-number-or-url>
---

Implement GitHub issue $ARGUMENTS. Follow the repo's `AGENTS.md` / `CLAUDE.md` throughout (they override the defaults below). Work without stopping unless the issue is ambiguous or a human decision is needed.

1. Read the issue with `gh issue view $ARGUMENTS`. If requirements are unclear, ask before coding.
2. Create a branch `issue-<number>-<slug>` from the up-to-date default branch (`git symbolic-ref refs/remotes/origin/HEAD`). Install dependencies only if missing or stale.
3. Search the code for classes and functions to reuse, then plan the implementation.
4. Implement.
5. Run the repo's checks, using only what exists. Detect them from `AGENTS.md`/`CLAUDE.md` first, then `package.json` scripts, `composer.json` scripts, `Makefile`:
   - build/compile (e.g. `npm run build:dev`, `npm run build`)
   - tests (e.g. `npm test`, `composer test`, `vendor/bin/phpunit`, `php artisan test`)
   - format and lint, with the fixing variant (e.g. `npm run formatlint`, `npm run lint`, `vendor/bin/pint`, `vendor/bin/php-cs-fixer fix`)
   - static analysis (e.g. `vendor/bin/phpstan`, `tsc --noEmit`)
   Skip any step the repo doesn't define. Never invent a command. Fix all failures.
6. If the repo documents e2e or UI checks and the UI changed, run only the affected specs as documented.
7. Commit, push, and open a PR with `Closes #<number>` in the body. Use the repo's PR template if present.
8. Run `/github-workflow:review-loop <pr-number>`.
9. Stop and report the PR for approval. Keep fixing merge conflicts and failing checks per `AGENTS.md`.

After approval: address further comments on request. Merge only when asked.

If a `gh` command fails because GraphQL is blocked (`gh issue view`, `gh pr create`, `gh pr view`), use the REST equivalent: `gh api repos/{owner}/{repo}/issues/<number>`, `gh api repos/{owner}/{repo}/pulls -f title=... -f head=... -f base=<default-branch> -f body=...`.
