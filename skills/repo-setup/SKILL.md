---
name: repo-setup
description: >
  This skill should be used when the user wants to create a new GitHub repository,
  connect an existing local Claude Code project folder to GitHub, or push a project's
  files to a repo for the first time. Triggers include "create a repo for this project",
  "put this project on GitHub", "initialize a GitHub repo", "link this folder to GitHub",
  or "push these files to a new repo". Requires the connected github MCP server.
metadata:
  version: "0.1.0"
---

# Repo Setup

Create a repository and bring a local project onto GitHub.

## Steps
1. **Confirm GitHub is connected.** If tools 401/403, point to the github-workflows
   skill's `references/auth-setup.md`.
2. **Get the owner.** Call `get_me` for the authenticated login unless the user names an org.
3. **Confirm details with the user** before creating: repo name, description,
   visibility (default **private** unless told otherwise), and license. Repo creation
   is a write action — state what you'll do, then proceed.
4. **Create the repo:** `create_repository` (set `autoInit: true` to get a `main`
   branch and an initial commit).
5. **Push project files:** gather the local files (filesystem tools) and commit them
   in one shot with `push_files` to `main`. Respect `.gitignore` — never push secrets,
   `.env`, tokens, or build artifacts.
6. **Report** the repo URL and what was pushed.

## After setup
- Offer to update placeholders (e.g., `REPLACE_OWNER` in `plugin.json` /
  `marketplace.json`) with the real owner/repo, then commit that change.
- Offer to set up a feature-branch + PR workflow for future changes (see github-workflows).

## Safety
- Default to private repos.
- Confirm before changing visibility or deleting anything.
- Verify `.gitignore` covers secrets BEFORE the first push.
