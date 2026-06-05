# SECURITY.md — github-bridge

## Permission model
- GitHub access is granted by **your** GitHub authorization (OAuth scopes or PAT
  permissions). The plugin cannot do more than the token allows.
- Default scopes: `repo read:org read:user workflow`. Drop `workflow` if you don't
  need to trigger Actions; drop write entirely by running the server `--read-only`.

## Least privilege
- Prefer a **fine-grained PAT** scoped to specific repos over a classic PAT.
- For org use, request `read:org` (read) rather than `admin:org`.
- Consider a read-only profile for browsing/analysis sessions.

## Safe-write rules (enforced by the skill)
Claude must summarize and get an explicit "yes" before any:
- `merge_pull_request`
- `delete_file`, branch deletion, or repo deletion
- repo visibility changes (public ↔ private)
- force operations or history rewrites
Non-destructive reads and ordinary commits to a feature branch do not require a gate,
but Claude should still state what it's about to do.

## Secrets handling
- Never write tokens into tracked files. Use environment variables.
- `.gitignore` excludes `.env`, `*.token`, `*.pat`, `secrets/`.
- If a token is ever committed by mistake: revoke it immediately on GitHub, then rotate.

## Transport
- Remote MCP is HTTPS only. OAuth uses PKCE. Tokens are stored by the client, not in repo.

## Reporting
- For issues in this plugin, open a GitHub issue. For GitHub MCP server issues, see
  github/github-mcp-server. Do not include tokens or private data in reports.
