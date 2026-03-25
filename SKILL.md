---
name: codex-git
description: Use when working with git. It gives instructions which help to match general git rules, and to avoid escalation mistakes.
---

# Git Usage

## General Rules

- Prefer the git@ or ssh:// protocol for remotes, to avoid password prompts and other issues with https:// remotes.
- Avoid `git -C <path> ...`, and prefer setting the command `workdir` to the directory path, while running plain `git ...` commands.

## Use This Skill When

- a task needs git commands
- the work may update repository state
- you need to decide whether a git command requires escalation

## Sandboxing and Escalation

Some git commands are read-only, while others are write-side and touch files in the `.git` directory.

Some agents have sandbox restrictions that require escalation for writes to `.git/`, even when in a trusted directory where escalation would normally not be required. One example is the macOS `Codex.app` application.

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

