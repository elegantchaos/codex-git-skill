---
name: codex-git
description: Use when working with git in Codex.app. It explains when git commands that write repository state require escalation, and how to avoid failed non-escalated write attempts.
---

# Codex Git

Use this skill when running git commands from Codex.app.

Read `references/escalation.md` before running write-side git commands.

## Use This Skill When

- a task needs git commands in Codex.app
- the work may update repository state
- you need to decide whether a git command requires escalation

## Workflow

1. Decide whether the planned git command is read-only or write-side.
2. For write-side commands, follow `references/escalation.md`.
3. For read-only commands, run them normally unless another sandbox restriction blocks them.
4. If unsure, bias toward escalation rather than probing with a failing write attempt.

## References

- `references/escalation.md`: command-shape rules, write-side escalation policy, safe read-only commands, and failure handling
