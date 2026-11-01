---
description: Run independent review rounds on a PR until every comment is fixed or addressed and CI is green
argument-hint: <pr-number>
---

Run a review loop on PR #$ARGUMENTS, for at most 5 rounds. Follow the repo's `AGENTS.md` / `CLAUDE.md` (they override the defaults below).

1. Spawn a NEW `pr-reviewer` subagent (`github-workflow:pr-reviewer`) with only this prompt: "Review PR #$ARGUMENTS." Don't pass any of your own context or reasoning.
2. Check CI and mergeability over REST: `gh api repos/{owner}/{repo}/pulls/$ARGUMENTS` returns `head.sha` and `mergeable`; `gh api repos/{owner}/{repo}/commits/HEAD_SHA/check-runs` returns the checks. Wait for running checks to finish. Fix the cause of each failed check; if `mergeable` is false, merge the base branch and resolve the conflicts.
3. If the reviewer returned `VERDICT: CLEAN`, every check passed and there is no conflict, stop and report the result, including any threads the reviewer marked "ready to resolve".
4. Unless the verdict was clean, fetch the review threads with `gh api repos/{owner}/{repo}/pulls/$ARGUMENTS/ccr/review_threads` (GitHub GraphQL is blocked in Claude Code sessions, so don't use `gh pr view` or `gh api graphql`) and handle each one the reviewer says still needs work (a reviewer reply or new comment); skip threads it marked "ready to resolve":
   - Valid → fix the code.
   - Disagree → reply in the thread with your reasoning (`gh api repos/{owner}/{repo}/pulls/$ARGUMENTS/comments/COMMENT_ID/replies -f body='...'`); don't change the code.
   - Never resolve threads yourself. A person resolves them; the reviewer lists the answered ones as "ready to resolve".
5. Commit, push, and reply to each fixed thread with a short note and the commit SHA.
6. Start the next round.

If you reach 5 rounds, stop and list the threads, failed checks and conflicts that are still open so I can decide.
