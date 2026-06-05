# ARCHITECTURE.md — github-bridge

## Overview
`github-bridge` is a thin, declarative plugin. It does **not** ship a custom server.
It references GitHub's official remote MCP server and adds skills that guide Claude
to use it well. This keeps the surface small, secure, and maintenance-light.

```
+-------------------+        +------------------------+        +------------------+
|  Claude (Cowork / |  MCP   |  GitHub MCP server     |  REST  |   GitHub API     |
|  Claude Code)     | <----> |  api.githubcopilot.com | <----> |  repos/PRs/...   |
|                   |        |  /mcp/                 |        |                  |
|  + github-bridge  |        +------------------------+        +------------------+
|    skills         |               ^
|  + local project  |               | OAuth 2.1 (PKCE) or Bearer PAT
|    files (FS)      |               |
+-------------------+        [ user authorizes once ]
```

## Components
1. **`.mcp.json`** — declares one remote MCP server, `github`, of `type: http` at
   `https://api.githubcopilot.com/mcp/`, with `oauth.scopes` pinned to
   `repo read:org read:user workflow`.
2. **`skills/github-workflows`** — the "how to operate GitHub safely" knowledge:
   choosing the right tool, branch hygiene, PR etiquette, and confirmation rules.
3. **`skills/repo-setup`** — creating a repo and linking a local project folder.

## Auth model
Two independent layers (see PLAN.md §4):
- **User → Claude:** existing Google/email login to the Claude app. Not in scope.
- **Claude → GitHub:** one of three paths, in priority order:
  1. **OAuth 2.1 + PKCE (default).** `.mcp.json` omits any static `Authorization`
     header; on first use the server returns 401 and Claude runs the browser OAuth
     flow, pinning the scopes declared in `oauth.scopes`.
  2. **Fine-grained PAT (fallback).** Add `headers.Authorization: "Bearer ${GITHUB_PAT}"`
     to the `github` server in `.mcp.json`; set `GITHUB_PAT` in the environment.
  3. **Local Docker (offline / Desktop-restricted).** Run
     `ghcr.io/github/github-mcp-server` via stdio with `GITHUB_PERSONAL_ACCESS_TOKEN`.
  Full snippets: `skills/github-workflows/references/auth-setup.md`.

## Toolset (provided by the GitHub MCP server)
Repos/files: `create_repository`, `get_file_contents`, `create_or_update_file`,
`push_files`, `delete_file`, `fork_repository`, `create_branch`, `list_commits`,
`get_commit`. PRs: `create_pull_request`, `update_pull_request`, `merge_pull_request`,
`update_pull_request_branch`, `request_copilot_review`. Search: `search_code`.
Plus issues, labels, projects, actions, code_security, dependabot, orgs, users.
Read-only mode is available server-side (`--read-only`).

## Data flow (example: open a PR)
1. User: "open a PR from feature/x into main in owner/repo".
2. Skill guidance → Claude calls `create_branch` (if needed), `create_or_update_file`
   / `push_files`, then `create_pull_request`.
3. Before any `merge_pull_request`, the skill requires explicit user confirmation.

## Safety (the #1 recommended feature)
- **Confirmation gate** on destructive actions: merge, delete file/branch/repo,
  change repo visibility, force operations. The skill instructs Claude to summarize
  the action and wait for a "yes" before executing.
- **Least privilege:** default scopes are the minimum for the documented workflows.
  Read-only installs can drop `repo` write by using the server's read-only mode.
- **No secrets in files:** `.gitignore` excludes `.env`/`*.token`; tokens via env only.

## Why no custom MCP server
GitHub maintains the official server (auth, rate limits, tool coverage, security
updates). Re-implementing it would add risk and maintenance for zero benefit.
