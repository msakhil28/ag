# /dev and /qa for GitHub Copilot (VS Code)

## Install

1. Copy the `.github/` folder into the root of your repo. If the repo already has `.github/`, merge them; the existing workflows stay untouched.
2. Open `.github/copilot-instructions.md` and fill in the **Project commands** table.
3. In VS Code: open Copilot Chat, select the **Local** session and **Agent** mode.
4. Type `/dev add a logout button` or `/qa logout button`.

Note: `.github/` is a hidden folder. In Finder, press `Cmd+Shift+.` to show it.

## Files

| File | Role |
|---|---|
| `prompts/dev.prompt.md` | The `/dev` command. Runs in `dev-lead`. |
| `prompts/qa.prompt.md` | The `/qa` command. Runs in `qa-lead`. |
| `agents/dev-lead.agent.md` | Coordinator for /dev. Delegates and never codes. |
| `agents/qa-lead.agent.md` | Coordinator for /qa. Delegates, then commits and pushes. |
| `agents/planner.agent.md` | Read-only. Splits the task into chunks with exclusive files. |
| `agents/implementer.agent.md` | Writes the code and unit tests for one chunk. |
| `agents/reviewer.agent.md` | Read-only bug and security review of the diff. |
| `agents/ux-reviewer.agent.md` | Read-only UX rules (GUIDE) and UX check (CHECK). |
| `agents/test-writer.agent.md` | Writes E2E and edge-case tests. Edits test files only. |
| `agents/docs-keeper.agent.md` | Updates README, docs and the changelog. |
| `agents/verifier.agent.md` | Runs lint, typecheck, tests and build, and reports the results. |
| `copilot-instructions.md` | Project rules every agent follows. |

Worker agents have `user-invocable: false`, so they don't show up in the agent dropdown. Only the leads use them.

## Pipelines

| Command | Steps | Commits? |
|---|---|---|
| `/dev <task>` | planner → ux-reviewer (GUIDE, UI only) → implementer per chunk → reviewer → ux-reviewer (CHECK, UI only) → docs-keeper | No |
| `/qa <feature>` | test-writer → reviewer → ux-reviewer (UI only) → docs-keeper → verifier (FULL, up to 3 fix rounds) → commit + push | Only if every check passes, and never on main |

## If something doesn't work

| Symptom | Fix |
|---|---|
| `/dev` isn't listed | Make sure the session is **Local** and you're on a recent VS Code. Check that the setting `chat.promptFiles` is on. Your org may need to allow prompt files and custom agents. |
| The lead does the work itself | Check that `chat.customAgentInSubagent.enabled` (or a similarly named subagent setting) is on, and that the `agent` tool is enabled under Configure Tools. |
| A tool isn't available | Tool names vary by VS Code version. Each agent lists both `execute` and `runCommands`; names that don't exist are ignored. Open Chat → Configure Tools to see what your version calls them. |
| You want a specific model | Add `model: 'GPT-4.1'` (or any model in your picker) to an agent's frontmatter. |

Differences from Claude Code: subagents may run one at a time instead of in parallel, and there's no hook that blocks `rm` or `.env` access. Those rules live in `copilot-instructions.md`, so they depend on the model following them.
