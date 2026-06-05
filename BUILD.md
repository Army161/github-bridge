# BUILD.md — runbook

Step-by-step to build, connect, test, and package `github-bridge`.

## Prerequisites
- Claude Cowork (or Claude Code) with plugin support.
- A GitHub account.
- (Fallback only) Docker, if using the local GitHub MCP server.

## 1. Scaffold (done)
Files created: `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`,
`.mcp.json`, `skills/*`, and the docs set. Verify:
```
test -f .claude-plugin/plugin.json && echo manifest ok
test -f .mcp.json && echo mcp ok
ls skills/*/SKILL.md
```

## 2. Connect GitHub
**OAuth (default):**
1. Enable the plugin so its `.mcp.json` loads.
2. Run `/mcp` (Claude Code) or Settings → Connectors (Cowork); authorize **github**.
3. Approve scopes `repo read:org read:user workflow`.

**PAT fallback:** create a fine-grained PAT (Contents: RW, Pull requests: RW,
Administration: RW for repo creation), export `GITHUB_PAT`, and switch `.mcp.json`
to the header form in `references/auth-setup.md`.

## 3. Smoke test (prove it live)
Ask Claude, in order:
1. "Who am I on GitHub?" → expects `get_me`.
2. "List my repos." → expects repo list.
3. "Create a private repo `github-bridge`." → `create_repository`.
4. "Push the files in this project folder to it." → `push_files`.
5. "Create a branch `test/ci`, change README, open a PR into main." → branch + PR.
6. "Merge it." → Claude should ask for confirmation first, then `merge_pull_request`.

## 4. Validate structure
Manual validation (CLI validator may be unavailable in Cowork):
- `.claude-plugin/plugin.json` is valid JSON with a kebab-case `name`.
- `.mcp.json` is valid JSON.
- Each `skills/*/` has a `SKILL.md` with `name` + `description` frontmatter.

## 5. Package
```
cd <plugin-dir>
zip -r /tmp/github-bridge.plugin . -x "*.DS_Store" "*.tmp" "*.plugin"
cp /tmp/github-bridge.plugin <outputs>/github-bridge.plugin
```
The `.plugin` file appears in chat with an install button.

## 6. Troubleshooting
- **401/403 loops:** scopes too narrow, or token expired — re-auth / re-issue PAT.
- **OAuth won't open in Desktop:** use the PAT or Docker fallback.
- **Tool not found:** GitHub may have renamed a tool — update the reference doc.
