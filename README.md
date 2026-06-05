# github-bridge

**Live, OAuth-connected GitHub access for Claude Cowork and Claude Code.**

`github-bridge` is a Claude plugin that wires the official GitHub MCP server into
Claude, then layers workflow skills on top so Claude can work across your GitHub
account *and* your local Claude Code projects in one place — reading and writing
repos, files, branches, pull requests, issues, Actions, and commit history.

---

## What it does

- **Full repo access** — list, create, read, and search across all your repos.
- **Read / write / edit files** — single-file edits or multi-file commits, on any branch.
- **Branches & history** — create branches, read commit history and diffs.
- **Pull requests** — create, update, review, and merge PRs end to end.
- **Issues, labels, projects, Actions** — triage, automate, and monitor CI.
- **Bridges local + remote** — connect a local Claude Code project folder to its
  GitHub repo so Claude can move between your filesystem and GitHub seamlessly.

## Components

| Component | What it is |
|-----------|------------|
| `.mcp.json` | Declares the official GitHub remote MCP server (`https://api.githubcopilot.com/mcp/`) with OAuth scope pinning. |
| `skills/github-workflows` | Core skill: how Claude should drive GitHub operations safely (PRs, files, branches, history). |
| `skills/repo-setup` | Skill for connecting/creating repos and linking a local project to GitHub. |

## Setup

### Option A — OAuth (default, recommended)
1. Install the plugin (see Install below).
2. Run `/mcp` (Claude Code) or open **Settings → Connectors** (Cowork) and authorize **GitHub** when prompted. A browser opens GitHub's consent screen.
3. Approve the requested scopes (`repo read:org read:user workflow`).

### Option B — Personal Access Token (fallback, most reliable on Claude Desktop)
If the browser-OAuth connector flow is unavailable in your build, use a fine-grained PAT.
See `skills/github-workflows/references/auth-setup.md` for the exact `.mcp.json` header form and a local-Docker alternative.

> **Security:** never paste a token into a file that gets committed. Tokens belong in an
> environment variable (`${GITHUB_PAT}`) — `.gitignore` already excludes `.env`/`*.token`.

## Install

**Via marketplace (recommended for sharing):**
```
/plugin marketplace add REPLACE_OWNER/github-bridge
/plugin install github-bridge@armys-plugins
```

**Local (this folder):** point your plugin/marketplace config at this directory, or
install the packaged `github-bridge.plugin` file from the Cowork chat.

## Usage

Once connected, just ask in natural language:
- "List my GitHub repos and show the 5 most recently updated."
- "Open a PR from `feature/x` into `main` in `owner/repo` with this description."
- "Create a new private repo `my-thing` and push the files in this project folder."
- "Show the last 10 commits on `main` and summarize what changed."

## Documentation

- `PLAN.md` — goals, scope, success criteria, phased plan.
- `ARCHITECTURE.md` — how the pieces fit; auth model; data flow.
- `BUILD.md` — step-by-step build & test runbook.
- `SECURITY.md` — permissions, least-privilege, safe-write rules.
- `ROADMAP.md` — what's next.
- `MEMORY.md` — durable project context/decisions.
- `MARKETING.md` — go-to-market notes (post-launch).
- `FILES.md` — map of every file in this repo.

## License

MIT — see `LICENSE`.
