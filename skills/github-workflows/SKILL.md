---
name: github-workflows
description: >
  This skill should be used whenever the user wants to work with GitHub through
  the connected GitHub MCP server — "list my repos", "read/edit a file in a repo",
  "create a branch", "commit changes", "open a pull request", "review/update/merge a PR",
  "show commit history", "search code", "triage issues", or "check Actions/CI". It also
  applies when bridging a local Claude Code project with its GitHub repository. It encodes
  safe-operation rules (confirmation gates for destructive actions) and which GitHub MCP
  tools to use for each task.
metadata:
  version: "0.1.0"
---

# GitHub Workflows

Drive GitHub through the connected `github` MCP server safely and predictably.
This skill covers tool selection, branch/PR hygiene, and mandatory confirmation
gates. Detailed patterns live in `references/`.

## Before acting
1. Confirm the `github` MCP server is connected. If a tool call returns 401/403,
   the user needs to authorize GitHub — point them to `references/auth-setup.md`.
2. Identify the target `owner/repo` and branch. If ambiguous, ask once; otherwise
   infer from context (e.g., the repo linked to the current project).
3. Call `get_me` when you need the authenticated login (e.g., for default owner).

## Choosing tools (summary; full detail in references)
- **Read repo/files:** `get_file_contents`, `list_commits`, `get_commit`, `search_code`.
- **Write files:** `create_or_update_file` (single file), `push_files` (multiple files
  in one commit). Always target a branch — never commit straight to a protected branch
  without saying so.
- **Branches:** `create_branch`.
- **Repos:** `create_repository`, `fork_repository`.
- **Pull requests:** `create_pull_request`, `update_pull_request`,
  `update_pull_request_branch`, `request_copilot_review`, `merge_pull_request`.
- **Issues / labels / projects / actions:** use the corresponding toolset tools.

See `references/repo-and-files.md` and `references/pull-requests.md` for exact
parameters and end-to-end recipes.

## Safe-write rules (REQUIRED)
Summarize the action and get an explicit "yes" from the user before ANY of:
- `merge_pull_request`
- deleting a file (`delete_file`), branch, or repository
- changing repository visibility (public ↔ private)
- force-push or any history rewrite
For ordinary commits to a feature branch, state what you're about to do, then proceed.
Never expose or echo tokens. Never commit secrets — if a write would include a token
or credential, stop and warn the user.

## Standard PR workflow
1. Ensure a feature branch exists (`create_branch` from the base).
2. Apply changes with `create_or_update_file` or `push_files`.
3. Open the PR with `create_pull_request` (clear title + description; link issues).
4. Optionally `request_copilot_review`.
5. Merge only after explicit confirmation (`merge_pull_request`), then report the result.

## Bridging local projects
When the user's local Claude Code project maps to a GitHub repo, you can read local
files with the filesystem tools and write them to GitHub with `push_files`, or pull
remote content with `get_file_contents` to update local files. Keep the two in sync
deliberately and tell the user which direction you're moving changes.

## On errors
- 401/403 → auth/scope problem → `references/auth-setup.md`.
- "tool not found" → the GitHub MCP server may have renamed a tool → check the
  reference docs and adapt to the closest current tool.
- Merge conflicts → report them; offer to update the PR branch (`update_pull_request_branch`).
