# Repos & files — patterns

## Identify context
- `get_me` → authenticated login (use as default owner when the user says "my repos").
- Repos are addressed as `owner/repo`.

## Read
- `get_file_contents` — read a file (params: owner, repo, path, optional ref/branch).
- `list_commits` — history for a repo/branch (params: owner, repo, sha/branch, path, perPage).
- `get_commit` — a single commit's details and diff.
- `search_code` — search code across repos (GitHub code-search query syntax).

## Create a repo
- `create_repository` — params commonly include: name, description, private (bool),
  autoInit (bool). After creation, the default branch is usually `main`.

## Write files
- `create_or_update_file` — one file. Params: owner, repo, path, content, message
  (commit message), branch. For updates you typically must pass the file's current `sha`
  (read it first with `get_file_contents`).
- `push_files` — multiple files in a single commit. Params: owner, repo, branch,
  message, and a list of files (path + content). Prefer this for pushing a project.

## Branches
- `create_branch` — params: owner, repo, branch (new name), from_branch/sha (base).

## Recipe: push a local project to a new repo
1. `create_repository` (private unless told otherwise; autoInit true to get `main`).
2. Gather local files (filesystem tools).
3. `push_files` to `main` (or a branch) with all files in one commit.
4. Report the repo URL.

## Recipe: edit one file via PR
1. `create_branch` from `main`.
2. `get_file_contents` to get current content + sha.
3. `create_or_update_file` on the new branch.
4. `create_pull_request` into `main`.
