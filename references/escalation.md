# Codex Git Escalation Rules

## Core Rules

- Prefer setting the command `workdir` to the repository path and running plain `git ...` commands.
- Avoid `git -C <path> ...` unless it is explicitly required, because Codex rule matching works better when the git command itself is the command prefix.
- Treat git commands that write repository state as escalation-required.
- Do not probe non-escalated first for write-side git commands.
- Do not parallelize write-side git commands against the same repository. Run them serially.
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

## Notes

- This rule is about Codex.app's handling of git metadata writes, not about repository location.
- The rule still applies inside writable roots.
- An `index.lock` failure in Codex.app means a write-side git operation was attempted the wrong way. The cause may be missing escalation, conflicting concurrent writes, or another interrupted write. Do not assume the cause without checking how the command was run.
- If a git write command fails in a surprising way, first check whether it should have been escalated and whether another write-side git command was running against the same repository.
