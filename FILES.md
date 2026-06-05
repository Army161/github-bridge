# FILES.md — repository map

Every file in `github-bridge`, what it is, and why it exists.

## Plugin (functional)
| Path | Purpose |
|------|---------|
| `.claude-plugin/plugin.json` | Plugin manifest (name, version, metadata). Required. |
| `.claude-plugin/marketplace.json` | Marketplace descriptor so the plugin can be shared/installed. |
| `.mcp.json` | Declares the official GitHub remote MCP server with OAuth scope pinning. |
| `skills/github-workflows/SKILL.md` | Core workflow skill: safe GitHub operations. |
| `skills/github-workflows/references/repo-and-files.md` | Repo + file read/write/commit patterns. |
| `skills/github-workflows/references/pull-requests.md` | PR create/update/review/merge patterns. |
| `skills/github-workflows/references/auth-setup.md` | OAuth, PAT, and Docker auth options + scopes. |
| `skills/repo-setup/SKILL.md` | Connect/create a repo and link a local project to GitHub. |

## Documentation (planning & governance)
| Path | Purpose |
|------|---------|
| `README.md` | Overview, setup, usage, install. |
| `PLAN.md` | Goals, scope, success criteria, phased plan. |
| `BUILD.md` | Build & test runbook. |
| `ARCHITECTURE.md` | System design, auth model, data flow, decisions. |
| `MEMORY.md` | Durable context, decisions, and open questions. |
| `SECURITY.md` | Permission model, least privilege, safe-write rules. |
| `ROADMAP.md` | Future milestones. |
| `MARKETING.md` | Go-to-market notes for production/distribution. |
| `CONTRIBUTING.md` | How to contribute and dev conventions. |
| `CHANGELOG.md` | Version history. |
| `FILES.md` | This file. |
| `LICENSE` | MIT license. |
| `.gitignore` | Excludes secrets, build artifacts, OS cruft. |
