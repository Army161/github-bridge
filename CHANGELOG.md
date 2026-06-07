# Changelog

All notable changes to `github-bridge`. Format: Keep a Changelog; SemVer.

## [0.1.0] - 2026-05-31
### Added
- Initial plugin scaffold: `plugin.json`, `.mcp.json` (official GitHub remote MCP, OAuth scope pinning).
- Skills: `github-workflows` (+ references: repo-and-files, pull-requests, auth-setup), `repo-setup`.
- Safety confirmation gates and least-privilege defaults.
- Full documentation set (PLAN, BUILD, ARCHITECTURE, MEMORY, SECURITY, ROADMAP, CONTRIBUTING, MARKETING, FILES).
- Marketplace descriptor for distribution.

## [0.1.1] - 2026-06-07
### Changed
- `.mcp.json` switched to environment-variable auth (`Authorization: Bearer ${GITHUB_PAT}`) for reliable, automatic connection on Claude Desktop/Cowork. OAuth remains documented in `skills/github-workflows/references/auth-setup.md`.
