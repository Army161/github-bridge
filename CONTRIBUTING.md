# CONTRIBUTING.md

## Dev conventions
- All file/dir names are kebab-case.
- Skills: third-person `description` with concrete trigger phrases; imperative body;
  keep `SKILL.md` lean and push detail into `references/`.
- Never hardcode paths — use `${CLAUDE_PLUGIN_ROOT}`.
- Never commit secrets. Tokens via env vars only.

## Making changes
1. Edit files; keep `FILES.md` and `CHANGELOG.md` in sync.
2. Re-validate: valid JSON in `plugin.json`/`.mcp.json`; each skill has `SKILL.md`.
3. Re-package the `.plugin` (see BUILD.md §5).

## Adding a skill
- Create `skills/<name>/SKILL.md` with `name` + `description` frontmatter.
- Put long reference material in `skills/<name>/references/`.
- Update `FILES.md` and `README.md`.

## PRs
- One logical change per PR. Describe intent and test steps. Follow the safe-write rules.
