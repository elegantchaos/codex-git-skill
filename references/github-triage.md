# GitHub Triage

## Context

1. Resolve the repository, PR, issue, or local branch from the user request.
2. For a current branch or PR, use local Git context and `gh` only as needed to discover its PR.
3. Ask for a repository identifier if scope remains ambiguous.

## Routing

- Triage repository, PR, issue, patch, labels, reactions, or top-level comments directly.
- Read `pull-requests.md` for creating or editing a PR.
- Read `review-comments.md` for unresolved review threads or requested changes.
- Read `github-actions-ci.md` for failing checks or Actions logs.
- Read `draft-releases.md` for a draft release.

Use a GitHub connector for structured repository and PR data when it is available. Do not imply that it provides GitHub Actions logs; use `gh` for those.
