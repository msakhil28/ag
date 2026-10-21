---
name: dev
description: Fast build loop - plan, UX guide, implement (chunks), review, UX check, docs
argument-hint: <feature or fix description>
agent: dev-lead
---
The task is the text the user typed after `/dev`. If it is empty, ask for it and stop.

Run this pipeline using subagents:
1. `planner` -> plan with chunks and exclusive files. Show me the plan briefly.
2. `ux-reviewer` in GUIDE mode on the plan, only if it has UI chunks. Pass each chunk's rules to its `implementer`.
3. `implementer` per chunk (with its UX rules). Chunks that share no files may run in parallel if supported; otherwise run them one after another, in the planner's order.
4. `reviewer` on the combined diff. Fix high/med findings via `implementer`, then re-review once.
5. `ux-reviewer` in CHECK mode on the diff, only if it touches UI. Fix high/med findings via `implementer`.
6. `docs-keeper` to update docs and release notes.

Full checks (lint, typecheck, all tests, build) are NOT run here; they run in /qa.
Do not commit, push or deploy.

Finish with a short table: what changed (file, change), open items.
