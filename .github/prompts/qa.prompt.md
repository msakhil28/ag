---
name: qa
description: Full QA - write tests, review, run every check, update docs, then commit and push
argument-hint: <feature to test>
agent: qa-lead
---
The feature under test is the text the user typed after `/qa`. If it is empty, use the current uncommitted changes and say so.

1. `test-writer` adds E2E and edge-case unit tests for the feature.
2. `reviewer` reviews the full diff (code and tests). Production fixes go to `implementer`; test fixes go to `test-writer`.
3. `ux-reviewer` in CHECK mode, only if the diff touches UI. Fix high/med findings via `implementer`.
4. `docs-keeper` updates README, docs/ and release notes to match.
5. `verifier` in FULL mode: lint, typecheck, all unit tests, build, and the full E2E suite.
   If anything fails, fix it (via `implementer` or `test-writer`) and re-run `verifier`. Try at most 3 rounds.
   If it still fails, STOP: do not commit or push; report the failures.
6. Only when everything passes: commit and push to the current branch.
   - If the current branch is `main` or `master`, STOP and ask me for a branch name first.
   - Stage only files from this work (`git add <files>`, never `git add -A` blindly). Never stage `.env` files or secrets.
   - Commit message: short summary line, then a few bullets.
   - Plain `git push` (use `git push -u origin <branch>` if it has no upstream). Never force-push, never push to main directly, never deploy.

Finish with a short table: what changed, each check's result, commit hash, branch pushed.
