# MEMORY.md — durable project context

Append-only log of decisions and context that outlive a single session.

## Identity
- Project: `github-bridge` — a Claude plugin for live GitHub access + local-project bridging.
- Owner: Army (armygep5@gmail.com).

## Key decisions
- **2026-05-31 — Use the official GitHub MCP server, not a custom one.** GitHub maintains
  `https://api.githubcopilot.com/mcp/`; reusing it removes auth/maintenance risk.
- **2026-05-31 — OAuth-first, PAT + Docker as fallbacks.** Cowork/Desktop browser-OAuth can
  be inconsistent across builds, so all three paths are documented.
- **2026-05-31 — Skills over a custom server.** Value-add is *how* Claude operates GitHub
  safely (confirmation gates, branch hygiene), delivered as skills.
- **2026-05-31 — Safety/least-privilege is a first-class feature** (the #1 recommendation).
- **2026-05-31 — Auth scope correction.** Google/Claude logins are the user's identity to
  Claude and are out of scope; only GitHub authorization is built.

## Conventions
- Plugin name: `github-bridge` (kebab-case). Version starts `0.1.0`.
- Pinned OAuth scopes: `repo read:org read:user workflow`.
- Tokens only via env (`${GITHUB_PAT}`); never committed.

## Open questions / to fill in
- GitHub owner/login for `repository`/`homepage` URLs in `plugin.json` and `marketplace.json`
  (currently `REPLACE_OWNER`). Update after the repo is created.
- Whether to add a read-only install variant (server `--read-only`) as a separate profile.

## Live status
- [x] Validated. [x] Scaffolded + docs. [ ] GitHub connected. [ ] Repo created/pushed.
- [ ] PR round-trip smoke test. [ ] Packaged `.plugin`. [ ] Marketed.
