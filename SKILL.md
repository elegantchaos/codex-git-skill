---
name: codex-git
description: Use when working with git or GitHub. It covers safe git command and escalation practices, GitHub CLI workflows, pull requests, draft releases, review feedback, and GitHub Actions CI triage.
---

# Codex Git and GitHub

Use this skill for local Git work and GitHub workflows across Codex, command-line, and other agent environments. The Git rules apply to every Git repository. The GitHub workflows apply only when the repository is hosted on GitHub or the task otherwise targets GitHub. Keep GitHub connector use optional: prefer it for structured repository, issue, pull-request, and comment data when available; use `gh` for the gaps it cannot cover.

## Git Rules

- Prefer the git@ or ssh:// protocol for remotes, to avoid password prompts and other issues with https:// remotes.
- Avoid `git -C <path> ...`, and prefer setting the command `workdir` to the directory path, while running plain `git ...` commands.

## Sandboxing and Escalation

Some git commands are read-only, while others are write-side and touch files in the `.git` directory.

Some agent environments have sandbox restrictions that require escalation for writes to `.git/`, even when in a trusted directory where escalation would normally not be required. Codex.app on macOS is one such environment.

When these agents attempt to run a write-side git command without escalation, they may encounter an `index.lock` failure. This is because the sandbox prevents them from updating `.git/index.lock` without escalation.

These agents sometimes misinterpret this failure as a sign of lock-contention, and then try the operation again or report the contention, which can lead to wasted time and confusion.

The sandboxing of `.git/` will happen even if a local rule matches the command and would normally allow it to run without escalation.

This can also confuse agents, and they will sometimes misinterpret the situation and try to target the git command on a specific directory using `-C <path-to-repo>`. This can cause further confusion because the addition of the `-C` option early in the list of command line options can then defeat the rule matching, changing the behavior of the command.

## Rule Matching and Command Prefix

Some agents rule matching works better when the git command itself is the command prefix.

Writing git commands as `git -C <path-to-repo> <command> <arguments>...` can prevent the rules from matching correctly.

For this reason:

- Avoid `git -C <path> ...` unless it is explicitly required
- If you need to target a specific directory, prefer setting the command `workdir` to the directory path,  the running plain `git ...` commands.

## Read-Only Commands

These are typically read-only and can usually run without escalation:

- `git blame`
- `git branch --show-current`
- `git diff`
- `git log`
- `git ls-files`
- `git remote -v`
- `git rev-parse`
- `git show`
- `git status`

These commands are likely to also be marked as "allow" in `default.rules` or `git.rules`.

If you hit a sandbox restriction with a read-only command, check whether the command is actually write-side or if another sandbox restriction is causing the failure.

## Escalation Workarounds

Git commands that write repository state will update one or more files in `.git/`. 

If you are an agent that knows it is running in an environment where escalation is required
for writes to `.git/`, then you should treat all git commands that write to `.git/` as escalation-required, even if they would normally be allowed by local rules.

If you hit repeated `index.lock` failures, it suggest that you are such an agent, and that you should follow the advice above.

Do not parallelize write-side git commands against the same repository. Run them serially. This will help to avoid `index.lock` failures and other issues that are genuine contentions, but which may then
be misinterpreted as the result of this advice.

If unsure, bias toward escalation got a `git` command, rather than probing with a failing write attempt.

## Git Only

This skill only applies for `git` commands. 

Never use it as a general guide for escalation decisions about other commands.

## GitHub Workflows

- For repository, PR, or issue orientation, read [references/github-triage.md](references/github-triage.md).
- For PR creation or editing, read [references/pull-requests.md](references/pull-requests.md).
- For draft releases, read [references/draft-releases.md](references/draft-releases.md).
- For unresolved review feedback, read [references/review-comments.md](references/review-comments.md).
- For failing GitHub Actions checks, read [references/github-actions-ci.md](references/github-actions-ci.md).

Keep PRs and release notes factual and scoped to the actual diff. Run `actionlint` when changing GitHub Actions workflows if it is installed; otherwise record that it was unavailable without installing it solely for validation.
