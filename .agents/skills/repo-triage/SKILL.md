---
name: repo-triage
description: Triage GitHub repository issues and drive selected issues toward fixes. Use when Codex needs to inspect open issues, classify bugs/features/docs/infra work, identify duplicates or stale items, choose high-value next work, investigate a specific issue, reproduce a problem, implement a focused fix, run verification, or prepare a PR linked to an issue.
---
 
# Repo Triage
 
## Overview
 
Use this skill to move from issue noise to clear maintainer action. Prefer GitHub connector data for issues and PR metadata, local repo inspection for code and tests, and `gh` only when connector coverage is insufficient, such as Actions logs or current-branch discovery.
 
Keep the workflow proportional: a broad triage pass should produce ranked next actions; an issue-to-fix pass should end with code, tests, verification, and PR-ready notes when feasible.
 
## Resolve Scope
 
First identify the operating context:
 
- Repository: from the user prompt, current local checkout, issue URL, PR URL, or explicit `owner/repo`.
- Mode: broad issue triage, specific issue investigation, issue-to-fix, maintainer sweep, or CI-related follow-up.
- Constraints: labels, milestone, priority class, time budget, affected package, release branch, or "quick wins only".
 
Ask a concise question only when the repository or target issue cannot be discovered safely. If a local checkout exists, inspect the branch and repo before asking.
 
## Broad Issue Triage
 
For open issue sweeps:
 
1. Gather issue title, number, labels, author, age, recent activity, assignee, milestone, linked PRs, and comment summary.
2. Group issues into `bug`, `feature`, `docs`, `infra`, `question`, `duplicate`, `stale`, and `unclear`.
3. Mark confidence: `high` when reproduction and affected area are clear, `medium` when the likely area is clear but proof is missing, `low` when the issue needs user input or deeper discovery.
4. Identify duplicates, blocked items, stale items with no current action, issues with linked PRs, and issues that should be closed only with maintainer confirmation.
5. Rank next work by user impact, regression risk, clarity, expected effort, and testability.
 
Output the shortest useful triage table or list. Include issue number, classification, priority, confidence, owner/area if known, and next action.
 
## Specific Issue Investigation
 
For one issue:
 
1. Read the full issue body and important comments; preserve exact error messages, versions, logs, screenshots, and reproduction steps.
2. Check linked PRs, related issues, recent commits, and relevant labels.
3. Search the codebase for named symbols, error strings, routes, flags, config keys, and tests.
4. Decide whether the issue is reproducible, under-specified, already fixed, duplicate, expected behavior, or a real change request.
5. State the likely affected files and the smallest useful verification path.
 
If reproduction is missing, infer a plausible reproduction only when clearly grounded in the code. Otherwise ask for the missing input and still provide the best next maintainer action.
 
## Issue To Fix
 
When asked to solve an issue end to end:
 
1. Reproduce or localize the failure before editing whenever practical.
2. Add or update a focused test for bug fixes and regressions. For trivial docs/config fixes, explain why a test is not useful.
3. Make the smallest change that addresses the issue without unrelated refactors.
4. Run focused verification first, then broader tests only when the changed surface warrants it.
5. Inspect the diff for accidental churn, unrelated formatting, generated files, or user changes that should not be touched.
6. Prepare a PR-ready summary with issue link, root cause, fix, tests run, and residual risk.
 
Do not close or label issues, push branches, or open PRs without making the target explicit. If the user asked for publishing, follow the repo's GitHub publish workflow.
 
## Maintainer Decisions
 
Use these labels for recommendations unless the repo has its own taxonomy:
 
- `P0`: production outage, data loss, security, or severe regression.
- `P1`: major user-visible breakage or release blocker.
- `P2`: normal bug with clear impact and bounded fix.
- `P3`: polish, minor docs, low-impact cleanup, or uncertain impact.
 
Recommend closing only when there is strong evidence of duplicate, obsolete, invalid, answered, or already fixed status. Prefer "needs reproduction" or "needs maintainer decision" over pretending uncertainty is certainty.
 
## Final Output
 
For triage, report:
 
- What was inspected.
- Top recommended issues to address next.
- Items needing more information.
- Any labels, assignments, or closure candidates to consider.
 
For fixes, report:
 
- Root cause.
- Files changed.
- Tests or checks run.
- PR-ready summary and remaining risk.
 
Keep output concise and action-oriented. Use issue and PR numbers wherever possible.
