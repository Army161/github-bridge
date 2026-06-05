# Auth setup — OAuth, PAT, Docker

The plugin references the official GitHub remote MCP server:
`https://api.githubcopilot.com/mcp/`.

## Option A — OAuth (default)
`.mcp.json` ships with no static auth header, so the client runs OAuth on first use:
```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "oauth": { "scopes": "repo read:org read:user workflow" }
    }
  }
}
```
Authorize via `/mcp` (Claude Code) or Settings → Connectors (Cowork), approve scopes.

## Option B — Personal Access Token (most reliable on Claude Desktop)
Create a **fine-grained PAT** (repo permissions: Contents RW, Pull requests RW,
Administration RW if creating repos; Metadata read). Then:
```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": { "Authorization": "Bearer ${GITHUB_PAT}" }
    }
  }
}
```
Export the token in your environment as `GITHUB_PAT` — never write it into the file.

## Option C — Local Docker (offline / restricted environments)
```
docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN \
  ghcr.io/github/github-mcp-server
```
Then point `.mcp.json` at a stdio server running that image. Use `GITHUB_READ_ONLY=1`
(or the `--read-only` flag) for analysis-only sessions.

## Scopes / permissions cheat-sheet
- Write to repos / create files / branches / PRs / read history / code search → `repo`.
- Org/team reads → `read:org`. Trigger Actions → `workflow`. Code scanning → `security_events`.
- Read-only enforcement is server-side: `--read-only` / `GITHUB_READ_ONLY=1`.

## Rotating a leaked token
Revoke it on GitHub → Settings → Developer settings immediately, then issue a new one.
