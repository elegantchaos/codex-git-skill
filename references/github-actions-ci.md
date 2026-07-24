# GitHub Actions CI Triage

Use this workflow only for failing GitHub Actions checks on a pull request.

1. Confirm `gh` authentication with `gh auth status`; request user authentication if needed.
2. Resolve the PR from an explicit reference or the current branch.
3. Inspect checks with `gh pr checks` and obtain failing Actions run details and logs with `gh run view`.
4. If a check belongs to another provider, identify it as external and report it without attempting diagnosis.
5. Summarize the observed failure, relevant log evidence, and likely root cause. Do not overstate certainty when logs are missing.
6. Propose a focused fix plan and obtain approval before changing code.
7. After an approved change, run relevant local validation and recheck the PR status. State residual risk and any external failures.
