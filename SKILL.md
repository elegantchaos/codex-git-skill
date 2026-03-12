---
name: codex-git
description: Use when working with git in Codex.app. It explains when git commands that write repository state require escalation, and how to avoid failed non-escalated write attempts.
---

# Codex Git

Use this skill when running git commands from Codex.app.

## Purpose

Codex.app requires escalation for git commands that write repository state, even when the repository lives inside an otherwise writable root. Use this skill to choose the correct escalation behavior up front and avoid failed write attempts against `.git`.

## Rules

- Treat git commands that write repository state as escalation-required.
- Do not probe non-escalated first for write-side git commands.
- Read-only git commands can usually run without escalation.
- If a git command will update `.git/index`, refs, commits, stashes, branches, tags, or other repository metadata, request escalation before running it.

## Escalate Before Running

Escalate these command categories in Codex.app:

- `git add`
- `git commit`
- `git push`
- `git merge`
- `git rebase`
- `git cherry-pick`
- `git revert`
- `git stash push`, `git stash pop`, `git stash apply`, `git stash drop`
- `git branch` when creating, deleting, or renaming branches
- `git switch -c` and `git checkout -b`
- `git tag` when creating, deleting, or moving tags
- commands that edit remotes, config, hooks, or submodules when they write repository state

## Usually Safe Without Escalation

These are typically read-only and can usually run without escalation:

- `git status`
- `git diff`
- `git log`
- `git show`
- `git blame`
- `git rev-parse`
- `git remote -v`
- `git branch --show-current`
- `git ls-files`

## Workflow

1. Decide whether the planned git command is read-only or write-side.
2. If it writes repository state, request escalation immediately.
3. If it is read-only, run it normally unless some other sandbox restriction blocks it.
4. If unsure, bias toward escalation rather than retrying after a failed write attempt.

## Notes

- This rule is about Codex.app's handling of git metadata writes, not about repository location.
- The rule still applies inside writable roots.
- If a git write command fails in a surprising way, check whether escalation was missing before assuming the repository is broken.
