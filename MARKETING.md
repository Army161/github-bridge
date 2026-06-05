# MARKETING.md — go-to-market (post-launch)

> Do not market until the plugin is tested end-to-end (BUILD.md §3) and the safety
> review passes. This file is preparation only.

## Positioning
"Give Claude a verified, safe, live link to your GitHub — repos, PRs, files, and
history — without leaving the chat." Differentiator vs. raw MCP: built-in safety
gates, least-privilege defaults, and skills that bridge local projects with GitHub.

## Audience
Indie devs, small teams, and Claude Cowork/Code power users who want hands-off
GitHub operations.

## Channels
- GitHub repo (README is the landing page) + a short demo GIF/video.
- A Claude plugin marketplace listing (`marketplace.json`).
- Dev communities (relevant subreddits, X/Twitter, Hacker News "Show").
- A short write-up: "How I let Claude run my GitHub safely."

## Proof points to capture during testing
- A real PR opened + merged from chat.
- A repo created and pushed from a local project.
- The confirmation gate stopping an accidental merge/delete.

## Pricing
- v1.0: free / open-source (MIT). Reassess if a hosted convenience layer is added later.

## Pre-launch checklist
- [ ] End-to-end smoke test passes. [ ] Safety gates verified. [ ] No secrets in repo.
- [ ] README + demo ready. [ ] `marketplace.json` owner/URLs filled in. [ ] LICENSE present.
