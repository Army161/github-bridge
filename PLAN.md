# PLAN.md — github-bridge

## 1. Problem & goal
Claude Cowork and Claude Code can work on local project files, but have no
first-class, *live* link to GitHub. The goal is a single installable plugin that
gives Claude verified, OAuth-backed, full read/write access to a user's GitHub
account — repos, files, branches, pull requests, issues, Actions, and history —
and that bridges those operations with the user's local Claude Code projects.

## 2. Who it's for
- Solo developers and small teams using Claude Cowork / Claude Code.
- People who want Claude to open and merge PRs, edit files across repos, and keep
  a local project and its GitHub repo in sync — without leaving the chat.

## 3. Scope
### In scope
- Reference the **official GitHub MCP server** (no custom server to maintain).
- OAuth-first auth with PAT and local-Docker fallbacks documented.
- Workflow skills that make Claude's GitHub actions safe and predictable.
- A skill to create a repo and link a local project folder to it.
- Distribution via a marketplace descriptor.

### Out of scope (for v0.1)
- A custom-built MCP server (unnecessary — GitHub ships one).
- Building auth for Google / Claude.ai / Cowork accounts. Those are how the *user*
  signs into Claude; they are not part of GitHub access. See ARCHITECTURE.md §Auth.
- GitHub Enterprise Server self-hosting specifics (documented as future work).

## 4. Auth reality (important correction to the original idea)
The original ask described "OAuth verified through Google / GitHub / Claude.ai /
Claude Desktop / Cowork accounts." In practice these are two separate layers:
- **Your identity to Claude** (Google or email login to Cowork/Claude.ai) — already
  handled by the Claude app. The plugin does not build or touch this.
- **GitHub authorization** — handled by GitHub's own OAuth (or a PAT / GitHub App).
  This is the only auth the plugin is concerned with, and it is what grants repo access.

So: one real auth integration (GitHub), riding on top of your existing Claude login.

## 5. Success criteria
1. Plugin installs cleanly; `plugin.json` validates.
2. After authorizing GitHub, Claude can: list repos, read a file, create a branch,
   commit a change, open a PR, and merge it — all from chat.
3. A new repo for *this* project is created and the plugin's files are pushed to it.
4. Docs are complete enough that a new user can set it up unattended.

## 6. Phased plan
- **Phase 0 — Validate** ✅ Confirm GitHub MCP toolset, OAuth model, plugin packaging.
- **Phase 1 — Scaffold** Manifest, `.mcp.json`, skills, marketplace, docs.
- **Phase 2 — Connect** Authorize GitHub (OAuth) in the Claude app.
- **Phase 3 — Prove it live** Create this project's repo and push files.
- **Phase 4 — Verify** Validate structure; run a real PR round-trip as a smoke test.
- **Phase 5 — Package** Build the `.plugin` artifact for install/sharing.
- **Phase 6 — Market (later)** Only after testing. See MARKETING.md.

## 7. Risks & mitigations
| Risk | Mitigation |
|------|------------|
| Browser-OAuth connector flow unsupported in some Desktop builds | Ship PAT + Docker fallback, documented in auth-setup.md. |
| Over-broad token permissions | Pin OAuth scopes; document fine-grained PAT; default to least privilege. |
| Destructive writes (force-push, bad merge) | Safe-write rules in the skill: confirm before merge/delete/force operations. |
| GitHub MCP tool names change | Skills describe *intent*; reference doc lists current tool names and is easy to update. |

## 8. Top recommendation (requested)
**Add a safety/confirmation layer + least-privilege scoping as a first-class feature**
— see ARCHITECTURE.md §Safety. Giving Claude full write access to an entire GitHub
account is powerful and risky; the single highest-value addition is making every
destructive action (merge, delete, force-push, repo visibility change) require an
explicit confirmation, and defaulting auth to the narrowest scopes that still work.
This protects the user, builds trust, and is exactly what a production/marketplace
reviewer will look for.
