# Pull Request Review Feedback

## Workflow

1. Resolve the PR from an explicit reference or the current branch.
2. Read thread-aware review data when unresolved state, inline context, or file anchors matter. A GitHub connector's flat comment list is insufficient for that purpose; use `gh api graphql` or an equivalent thread-aware query.
3. Group feedback by file or behavior and separate actionable requests from approvals, resolved or outdated threads, duplicates, and informational comments.
4. Confirm scope before editing unless the user explicitly asks to address every unresolved actionable thread.
5. Keep each change traceable to the selected feedback and run focused validation.

## Write Safety

- Never reply to GitHub, resolve threads, or submit a review without explicit user permission.
- Surface conflicting or ambiguous feedback before making a change.
- If a comment calls for explanation rather than code, draft a response instead of forcing a code change.
