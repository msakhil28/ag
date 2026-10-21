# Project conventions

Every Copilot agent reads this file. Edit it to match the project.

- Keep changes small and focused; match the style of the surrounding code.
- Write or update tests for what you change, and run them before reporting done.
- Run lint, typecheck and build when the project has them.
- Never read or edit `.env` files or other secrets.
- Never delete files (no `rm`). Move them into a `.trash/` folder in the workspace instead.
- Commit, push or deploy only when asked (the `/qa` command is an explicit request to commit and push).
- Update the README or docs when behaviour changes.
- Keep replies short. Prefer tables.

## Project commands

<!-- Fill these in so the agents use the right ones. -->
| Check | Command |
|---|---|
| Install | `npm install` |
| Lint | `npm run lint` |
| Typecheck | `npm run typecheck` |
| Unit tests | `npm test` |
| E2E tests | `npm run test:e2e` |
| Build | `npm run build` |
