# Pull requests — patterns

## Create
- `create_pull_request` — params: owner, repo, title, head (source branch), base
  (target branch), body (description), draft (bool, optional). Link issues in the body
  with "Closes #123" when relevant.

## Update
- `update_pull_request` — change title/body/base/state of an existing PR.
- `update_pull_request_branch` — update the PR's branch with the latest base (resolves
  "branch is out of date"; can surface conflicts to resolve).

## Review
- `request_copilot_review` — request an automated review on a PR.
- Read changes with `get_commit` / `search_code` as needed to summarize a diff.

## Merge — CONFIRMATION REQUIRED
- `merge_pull_request` — params: owner, repo, pullNumber, optional merge_method
  ("merge" | "squash" | "rebase"), commit_title/message.
- BEFORE calling: summarize the PR (what merges into what, # of files, any open
  reviews/checks) and get an explicit "yes". Never auto-merge.

## Recipe: end-to-end PR
1. `create_branch` (feature/x from main).
2. `push_files` or `create_or_update_file` on feature/x.
3. `create_pull_request` (head: feature/x, base: main, clear body).
4. (optional) `request_copilot_review`.
5. On user "yes" → `merge_pull_request` (prefer squash for tidy history).
6. Report merged SHA + PR URL.

## Conflicts / failures
- Out-of-date branch → `update_pull_request_branch`.
- Failing checks → report which checks failed (Actions toolset) before suggesting merge.
