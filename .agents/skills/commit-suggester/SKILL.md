---
name: commit-suggester
description: "Suggest git commit messages based on staged or unstaged changes in a repository. Use this skill whenever the user asks for a commit message, wants help writing a commit, says 'suggest a commit', 'what should my commit message be', 'commit message for my changes', 'summarize my changes for a commit', or pastes a git diff. Also trigger when the user mentions 'git diff', 'staged changes', or asks to review changes before committing. Works especially well for Microsoft Learn lab content repos where Conventional Commits scoped to lab identifiers (e.g., fix(lab01): ...) are the standard."
---
 
# Commit Suggester
 
Analyze git changes (staged, unstaged, or a pasted diff) and suggest clear,
well-scoped commit messages following Conventional Commits format.
 
## How to use this skill
 
### Step 1 — Gather the diff
 
Run the commands below **in order** and stop at the first one that returns
output. This ensures you capture what the user actually intends to commit.
 
```bash
# 1. Staged changes (user already ran `git add`)
git diff --cached
 
# 2. If nothing is staged, show unstaged changes
git diff
 
# 3. If both are empty, show untracked files
git ls-files --others --exclude-standard
```
 
If the user pastes a diff directly in the chat, skip the commands and use that.
 
If there is **no diff and no untracked files**, tell the user there are no
changes to commit.
 
### Step 2 — Classify every changed file
 
For each file in the diff, identify all four levels of the content hierarchy:
 
| Attribute       | What to look for                                                |
|-----------------|-----------------------------------------------------------------|
| **Course code** | Parent folder or repo name (`PL-900`, `MB-280`, `MS-700`, etc.) |
| **Lab ID**      | Folder name or filename pattern (`Lab01`, `lab02`, etc.)        |
| **Exercise**    | Heading or section marker (`Exercise 1`, `Ex02`, `## Exercise`) |
| **Task**        | Sub-section within an exercise (`Task 1`, `### Task 3`)         |
| **Change type** | See the type table below                                        |
 
**How to detect exercise and task numbers:**
 
- Look at the **diff context lines** (lines starting with `@@`) — they
  often include the nearest heading, revealing the exercise or task.
- Search the changed lines and surrounding context for patterns like:
  `## Exercise 1`, `### Task 2`, `Exercise 1:`, `Task 3 –`, `Ex01`, `T02`.
- If the file is structured with one exercise per file, use the filename
  (e.g., `Exercise02.md` → Exercise 2).
- If a change spans multiple tasks within the same exercise, note the range
  (e.g., Tasks 2–4).
- If task-level granularity cannot be determined, stop at exercise level.
  If exercise-level cannot be determined, stop at lab level.
 
### Step 3 — Pick the commit type
 
| Type       | When to use                                                        |
|------------|--------------------------------------------------------------------|
| `fix`      | Corrects a broken step, wrong URL, outdated UI path, typo          |
| `style`    | Microsoft Learn style-only changes (select vs click, bold, tense)  |
| `docs`     | Rewrites or clarifies instructions without fixing a bug            |
| `refactor` | Restructures steps or sections without changing meaning            |
| `chore`    | Tooling, CI, config, or non-content changes                        |
| `feat`     | Adds a new lab, exercise, or major section                         |
| `remove`   | Deletes deprecated content                                         |
 
### Step 4 — Build the commit message
 
Follow **Conventional Commits** format:
 
```
<type>(<scope>): <short summary>
```
 
**Rules:**
 
- **Scope** = the most specific identifier you can determine, using the
  format `labXX/exYY/tZZ` and dropping trailing levels when not applicable.
  Examples:
  - `lab01/ex02/t03` — change in Lab 01, Exercise 2, Task 3
  - `lab01/ex02` — change spans multiple tasks in Exercise 2
  - `lab01` — change spans multiple exercises in Lab 01
  - `pl-900` — change spans multiple labs in the course
- **Summary** = imperative mood, lowercase start, no period, ≤ 72 chars.
- If changes span **multiple labs**, use the course code as scope
  (e.g., `fix(mb-280): ...`) or suggest separate commits.
- If changes are repo-wide (CI, tooling), omit the scope or use `repo`.
 
### Step 5 — Present the suggestion
 
Return the suggestion in this format:
 
---
 
**Suggested commit message:**
 
```
fix(lab01/ex02/t03): update step 3 to reflect new sensitivity label wizard
```
 
**What changed:**
- `lab01/instructions.md` — Exercise 2, Task 3: replaced 5-page wizard
  steps with 3-page flow, updated screenshot reference
 
**Alternative (if the changes could be split):**
 
```
fix(lab01/ex02/t03): update sensitivity label wizard steps
style(lab01/ex02): apply microsoft learn style to tasks 1-4
```
 
---
 
### Handling complex diffs
 
When a diff touches many files or mixes concern types:
 
1. **Group by scope first** (lab/exercise), then by type.
2. **Suggest splitting** into multiple commits if the diff mixes unrelated
   changes (e.g., a bug fix in lab01 + style cleanup in lab03).
3. For each suggested commit, list which files belong to it.
4. If the user wants a single commit anyway, combine into the broadest
   accurate type and scope and mention the mix in the body:
 
   ```
   fix(pl-900): update labs 01 and 03 for UI drift and style compliance
 
   - lab01/ex02/t03: update sensitivity label wizard from 5 to 3 pages
   - lab03/ex01: replace "click" with "select", add backticks to typed values
   ```
 
### Non-lab repositories
 
If the repo is **not** a Microsoft Learn lab repo (no lab identifiers, no
course codes), fall back to standard Conventional Commits:
 
- Use the module, component, or folder name as scope.
- Use `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore` as types.
- Same formatting rules apply.
 
### Quick reference — common lab change patterns
 
| Change pattern                               | Suggested message                                                    |
|----------------------------------------------|----------------------------------------------------------------------|
| URL updated in Ex1 Task 2                    | `fix(lab02/ex01/t02): update admin center URL to current endpoint`   |
| "Click" → "Select" across Exercise 1         | `style(lab01/ex01): replace click with select per style guide`       |
| Wizard steps reduced in a single task        | `fix(lab03/ex02/t01): update wizard flow to match current 3-page UI` |
| Deprecated term replaced across a lab        | `style(lab01): replace Azure AD with Microsoft Entra ID`             |
| New exercise added                           | `feat(lab04): add exercise 3 for dataverse table creation`           |
| Screenshot replaced in specific task         | `fix(lab02/ex01/t05): replace outdated screenshot`                   |
| Callout block fixed in one task              | `style(lab01/ex03/t02): fix callout block alignment to column 0`     |
| PowerShell cmdlet migration in one exercise  | `fix(lab03/ex01): migrate from AzureAD to Microsoft Graph cmdlets`   |
| Multiple labs touched for same reason         | `style(ms-700): apply microsoft learn style across all labs`         |