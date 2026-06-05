# ROADMAP.md — github-bridge

## v0.1 (now)
- Official GitHub MCP via `.mcp.json` (OAuth + PAT + Docker documented).
- `github-workflows` and `repo-setup` skills.
- Safety confirmation gates + least-privilege defaults.
- Full docs set + marketplace descriptor.

## v0.2
- Read-only install profile (server `--read-only`) for analysis sessions.
- A `pr-review` skill: structured, checklist-based PR reviews using `get_diff`/`search_code`.
- A `release` skill: changelog generation + tagged releases.

## v0.3
- Optional hook: warn before any push to a protected branch (`main`/`release/*`).
- "Sync project" skill: reconcile a local Claude Code folder with its remote repo.
- GitHub Enterprise Server / data-residency endpoint guidance.

## v1.0
- Hardened safety review; marketplace listing; usage docs + short demo.
- Telemetry-free quality checklist passed (no secrets, least privilege, confirmations).
