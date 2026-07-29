---
name: pr-drafter
description: "Draft GitHub pull request titles and descriptions based on commits in the current branch. Use this skill whenever the user asks to create a PR, draft a pull request, open a PR, says 'PR for my changes', 'pull request for this branch', 'create a pull request', 'draft PR', 'PR description', 'summarize branch for PR', or mentions wanting to push and open a PR. Also trigger when the user says 'ready to merge', 'submit my changes', or 'send for review'. Works especially well for Microsoft Learn lab content repos that use Conventional Commits scoped to lab identifiers, but handles any repo."
---
 
# PR Drafter
 
Read all commits on the current branch (compared to the base branch), and
draft a pull request title and description ready to paste into GitHub.
 
## How to use this skill
 
### Step 1 — Identify the branches and upstream target
 
```bash
# Current branch name
git branch --show-current
 
# Detect the base branch (main or master) from origin
git remote show origin 2>/dev/null | sed -n 's/.*HEAD branch: //p'
 
# Detect the upstream MicrosoftLearning remote
# Check if 'upstream' remote exists pointing to MicrosoftLearning
git remote -v 2>/dev/null | grep -i 'MicrosoftLearning'
```
 
If the base branch cannot be detected, assume `master`. If the current branch
**is** the base branch, tell the user there is nothing to open a PR for and
ask which feature branch they meant.
 
**Upstream setup:**
 
The PR must always target the **MicrosoftLearning** upstream repository, not
the user's personal fork. If no `upstream` remote exists pointing to
MicrosoftLearning, add it:
 
```bash
# Get the repo name from origin (e.g., PL-900T00A-Microsoft-Power-Platform-Fundamentals)
REPO_NAME=$(gh repo view --json name --jq '.name' 2>/dev/null)
 
# If that fails, extract from origin URL
if [ -z "$REPO_NAME" ]; then
  REPO_NAME=$(git remote get-url origin | sed 's/.*\///' | sed 's/\.git$//')
fi
 
# Add upstream if it doesn't exist
if ! git remote get-url upstream &>/dev/null; then
  git remote add upstream "https://github.com/MicrosoftLearning/$REPO_NAME.git"
fi
 
git fetch upstream --quiet 2>/dev/null
```
 
Store the values:
- `CURRENT` = current branch name
- `BASE` = detected base branch (typically `master`)
- `UPSTREAM` = `MicrosoftLearning/$REPO_NAME` (the target for the PR)
 
### Step 2 — Fetch the commit log (current branch only)
 
The goal is to capture **only the commits made on this feature branch** —
not every commit that differs between the fork and upstream (which may
include old PRs, syncs, or rebased commits with different hashes).
 
Use `git merge-base` to find the exact point where the current branch
diverged from the base branch, then list only the commits after that point.
 
```bash
# Make sure we have the latest references
git fetch upstream "$BASE" --quiet 2>/dev/null
git fetch origin "$BASE" --quiet 2>/dev/null
 
# Find where this branch split off from the base
# Try upstream first, fall back to origin
MERGE_BASE=$(git merge-base HEAD upstream/"$BASE" 2>/dev/null)
if [ -z "$MERGE_BASE" ]; then
  MERGE_BASE=$(git merge-base HEAD origin/"$BASE" 2>/dev/null)
fi
 
# List ONLY commits on this branch since it diverged
git log "$MERGE_BASE"..HEAD --pretty=format:"%h %s" --reverse
```
 
If the log is empty, the current branch has no new commits. Tell the user
there is nothing to open a PR for.
 
> **Why merge-base?** A simple `upstream/master..HEAD` comparison includes
> every commit that differs between the two refs — including old commits
> from the fork that were already merged upstream with different hashes.
> `merge-base` finds the common ancestor, so only commits added after the
> branch was created are included.
 
### Step 3 — Gather the full diff summary (current branch only)
 
Use the same `MERGE_BASE` reference from Step 2 so the diff matches
exactly the commits that will be in the PR.
 
```bash
# File-level change summary — only changes from this branch
git diff "$MERGE_BASE"..HEAD --stat
 
# Detailed diff for analysis (limit to 8000 lines to stay within context)
git diff "$MERGE_BASE"..HEAD | head -8000
```
 
### Step 4 — Analyze the changes
 
For each commit, extract:
 
| Attribute       | Source                                                 |
|-----------------|--------------------------------------------------------|
| **Type**        | Conventional Commit prefix (`fix`, `style`, `docs`...) |
| **Scope**       | Lab or exercise ID from the commit scope               |
| **Summary**     | The commit subject line                                |
 
Then group commits by:
 
1. **Course code** (e.g., `PL-900`, `MS-700`, `MB-280`)
2. **Lab ID** within each course
3. **Change type** within each lab
 
Also note:
- Total number of files changed
- Which labs/exercises were touched
- Whether the changes are all one type or mixed
 
### Step 5 — Draft the PR title
 
**Format:**
 
```
<type>(<scope>): <concise summary of all changes>
```
 
**Rules:**
 
- If all commits share the **same type and scope**, mirror that directly.
  Example: `style(lab01): apply Microsoft Learn style guidelines`
- If commits share the **same type but different scopes**, use the course
  code as scope.
  Example: `fix(ms-700): update labs 01-03 for UI drift`
- If commits have **mixed types**, use the dominant type or `chore` and
  describe broadly.
  Example: `chore(pl-900): update labs for style compliance and UI drift`
- Keep the title under **72 characters**.
 
### Step 6 — Draft the PR description
 
Use this template:
 
````markdown
## Summary
 
<!-- One or two sentences: what this PR does and why -->
 
## Changes
 
<!-- List each meaningful change as a bullet point, grouped by lab when
     multiple labs are involved. Rewrite commit messages into readable
     descriptions with specific detail. -->
 
- <change description>
- <change description>
 
## Files changed
 
<!-- List every file touched, one per line, with a brief note on what
     changed in that file. Use inline code for file paths. -->
 
- `path/to/file.md` — <what changed>
- `path/to/file.md` — <what changed>
````
 
**Rules for filling in the template:**
 
- **Summary**: Write 1–2 sentences in your own words summarizing the full
  set of changes and the motivation. Do not just list commit messages.
- **Changes**: Rewrite commit subjects into readable bullet points. Include
  specific detail (e.g., "updated wizard from 5 to 3 pages" not just
  "updated wizard"). If multiple labs are involved, group with sub-headers
  (`### Lab 01`) or prefix each bullet with the lab ID.
- **Files changed**: List every file from `git diff --stat`. Use the full
  relative path in backticks followed by a short description of what changed
  in that file. This gives reviewers a quick at-a-glance map of the PR.
 
### Step 7 — Create the pull request
 
After drafting the title and description, **create the PR directly** using
the GitHub CLI. Do not just show the command — run it.
 
**Pre-flight checks:**
 
```bash
# 1. Verify gh is installed and authenticated
gh auth status
 
# 2. Make sure all changes are pushed to the user's fork
git push origin "$(git branch --show-current)" 2>&1
```
 
If `gh` is not installed or not authenticated, tell the user and provide
instructions to set it up (`gh auth login`). Do not fall back to just
showing the command.
 
**Create the PR targeting the upstream MicrosoftLearning repo:**
 
Always use `--repo` to target the MicrosoftLearning upstream, and `--head`
to specify the user's fork branch as the source.
 
```bash
# Get the user's GitHub username (fork owner)
FORK_OWNER=$(gh api user --jq '.login' 2>/dev/null)
 
# Get the upstream repo name
REPO_NAME=$(git remote get-url upstream | sed 's/.*MicrosoftLearning\///' | sed 's/\.git$//')
 
# Create the PR against MicrosoftLearning
gh pr create \
  --repo "MicrosoftLearning/$REPO_NAME" \
  --head "$FORK_OWNER:$(git branch --show-current)" \
  --base "$BASE" \
  --title "<drafted title>" \
  --body "<drafted description>"
```
 
After creation, `gh` returns the PR URL. Present it to the user:
 
---
 
**PR created successfully:**
 
`https://github.com/MicrosoftLearning/<repo>/pull/42`
 
**Target:** `MicrosoftLearning/<repo>:master` ← `<user>:<branch>`
 
**Title:** `style(ms-700): apply Microsoft Learn style guidelines to labs 01-03`
 
**Description:**
 
<!-- show the description that was submitted -->
 
---
 
**If the PR already exists** for this branch, `gh` will return an error.
In that case, tell the user a PR already exists and offer to update it:
 
```bash
# Get the existing PR number on the upstream repo
gh pr list \
  --repo "MicrosoftLearning/$REPO_NAME" \
  --head "$FORK_OWNER:$(git branch --show-current)" \
  --json number --jq '.[0].number'
 
# Update the existing PR title and body
gh pr edit <number> \
  --repo "MicrosoftLearning/$REPO_NAME" \
  --title "<drafted title>" \
  --body "<drafted description>"
```
 
### Handling edge cases
 
**Single commit branch:**
- PR title = commit message as-is (if it follows Conventional Commits).
- PR description = expanded version of the commit with file-level detail.
 
**Very large branch (20+ commits):**
- Summarize by lab/type rather than listing every commit.
- Add a collapsible section with the full commit log:
 
  ````markdown
  <details>
  <summary>Full commit log (24 commits)</summary>
 
  | Hash | Message |
  |------|---------|
  | `a1b2c3d` | fix(lab01): update step 3 ... |
  | ... | ... |
 
  </details>
  ````
 
**No Conventional Commits format:**
- If commits don't follow the convention, still draft a proper PR title
  using the convention based on what the diff shows.
- Mention in the description that commit messages could be improved.
 
**Non-lab repositories:**
- Drop the lab-specific table and testing checklist.
- Use a simpler template: Summary, Changes (grouped by module/folder),
  and Type of changes.
 
### Quick reference — PR title patterns
 
| Scenario                                          | PR title                                                      |
|---------------------------------------------------|---------------------------------------------------------------|
| Single lab, style fixes only                      | `style(lab01): apply Microsoft Learn style guidelines`        |
| Single lab, UI drift fix                          | `fix(lab02): update portal URLs and wizard steps`             |
| Multiple labs, same course, mixed changes         | `chore(ms-700): update labs 01-03 for drift and style`        |
| New exercise added                                | `feat(lab04): add exercise 3 for dataverse table creation`    |
| PowerShell cmdlet migration across labs           | `fix(pl-900): migrate AzureAD cmdlets to Microsoft Graph`     |
| Screenshot replacements only                      | `fix(mb-280): replace outdated screenshots in labs 01-02`     |
| Repo-wide tooling change                          | `chore(repo): update CI pipeline for markdown linting`        |