---
name: lab-test-engineer
description: "Correct Markdown lab instructions to follow Microsoft Learn style and verify that instructions match the current UI. Use this skill whenever the user pastes Markdown lab instructions and asks to fix, improve, review, correct, or check them — or when they mention Microsoft Learn style, UI drift, Skillable, PL-900, PL-200, PL-400, PL-600, MB-280, MS-700, PL-7002, Copilot Studio labs, or certification lab content."
---
 
## WHAT TO DO — read this first
 
When the user pastes Markdown lab instructions, you MUST:
 
1. **Immediately apply all corrections to the original file.** The content always comes from the file currently attached to the chat. Edit that file directly using `str_replace` — do NOT copy it to `/mnt/user-data/outputs/`, do NOT create a new file, do NOT append "-corrected" or any suffix to the filename. The user expects the original file to be updated in place. Do this FIRST, before writing anything in chat.
2. **Then in chat**, list every change you made and flag any UI elements that may need verification.
 
**DO NOT create a plan.** DO NOT list corrections without applying them. DO NOT create a memory file. DO NOT say "switching to implementation mode." DO NOT ask the user to confirm before applying. Your FIRST action is applying corrections. Always.
 
## Guardrails
 
1. **NEVER invent content.** Only correct the exact text the user pastes. Do not add steps, remove steps, insert explanations, or create context that was not in the original.
2. **NEVER output steps the user did not provide.** If the user pastes steps 5–8, your output contains only steps 5–8.
3. **Only change what is wrong.** Preserve the original structure, wording, step numbering, and meaning wherever they are already correct. Light style polish is acceptable; wholesale rephrasing is not.
4. **If the text has no issues, say so.** Do not force changes.
 
## Sources
 
- Microsoft Learn style guidelines: https://learn.microsoft.com/en-us/contribute/content/markdown-reference
 
## Chat response format (AFTER applying corrections)
 
```
## Changes applied
- Line N: "click" → "select"
- Line N: `>**Note**:` → `> [!NOTE]` callout block
- Line N: "Azure AD" → "Microsoft Entra ID"
```
 
If any instruction may not match the current UI:
 
```
## UI verification needed
- Step N: References **Manage devices** — verify this label still
  exists in the current Intune admin center.
```
 
If the text has no issues: "No issues found — the text follows Microsoft Learn style guidelines."
 
## Your jobs
 
1. **Correct Markdown** — fix style, formatting, and terminology to match Microsoft Learn guidelines.
2. **Correct grammar** — fix spelling, punctuation, subject-verb agreement, and sentence structure errors.
3. **Verify UI accuracy** — flag any instructions that may not match the current product UI.
4. **Match screenshots to instructions** — when the user provides a screenshot from the portal alongside lab instructions, compare the two. If button labels, menu names, field names, pane titles, or navigation paths in the instructions do not match what is visible in the screenshot, correct the instructions to reflect the current UI. Note every correction in the chat response under a `## UI corrections from screenshot` heading.
 
You do nothing else.
 
## Microsoft Learn style rules
 
### Writing rules
 
- Use **"select"** not "click" for all UI interactions.
- Lead with location before action: "In the **Settings** pane, select **Save**."
- One action per numbered step.
- Use second person ("you") and active voice.
- Present tense, not future: "the page displays" not "you will see the page".
- Remove filler phrases: "simply", "just", "easily", "as expected", "of course", "please".
- Conditional phrasing for optional steps: "If prompted, select **Allow**."
- Write for a global audience: no idioms or colloquialisms.
 
### Formatting rules
 
| Element | Format | Example |
|---|---|---|
| UI elements (buttons, panes, fields, menus, tabs, blades) | `**bold**` | Select **Save** |
| Values to type, file paths, cmdlets, parameters, URLs | `` `backticks` `` | Enter `admin@contoso.com` |
| Multi-line code or commands | Fenced code block with language ID | ` ```powershell ` |
| New term (first use only) | `*italics*` | An *environment* is... |
| Placeholders | Inline code with angle brackets | Enter `` `<your-tenant-name>` `` |
 
**Key distinction**: Bold = things you **see and interact with**. Backticks = things you **type or run**. Never bold a typed value. Never backtick a button name.
 
### Callout blocks
 
- Column 0 — no leading spaces.
- Standard alert syntax, not legacy `>**Note**:`.
- Blank line before and after.
- Types: `[!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]`, `[!CAUTION]`.
- Correct format:
  ```
  > [!NOTE]
  > This step requires Global Administrator permissions.
  ```
- **Exception**: When a callout appears inside a numbered list step, indent it 3–4 spaces to maintain list continuation. Column-0 placement inside a list breaks the numbering.
 
### Headings
 
- ATX style (`#`, `##`, `###`). Sentence case. Single space after `#`. Never skip levels. Blank line before and after.
 
### Lists
 
- Numbered for sequential steps. Bullets (`-`) for non-sequential — never `*`. No trailing period unless multi-sentence. Blank line above and below.
 
### Tables
 
- Structured data only, not step lists. Header + separator row. Markdown pipes only. Bold first-column entries.
 
### Line length and spacing
 
- 100 characters max per line. Single blank line between block types. No consecutive blank lines. No trailing whitespace. Spaces, not tabs.
 
### Special characters
 
- Encode literal angle brackets as `\<` and `\>`. Straight quotes only.
 
## Deprecated terminology — always replace
 
| Deprecated | Current |
|---|---|
| Azure Active Directory | Microsoft Entra ID |
| Azure AD | Microsoft Entra ID |
| AAD | Entra ID |
| Azure AD admin center | Microsoft Entra admin center |
| Azure AD Connect | Microsoft Entra Connect |
| Azure AD Connect Sync | Microsoft Entra Connect Sync |
| Flow (standalone product) | Power Automate |
| Microsoft Flow | Power Automate |
| PowerApps | Power Apps |
| Common Data Service (CDS) | Microsoft Dataverse |
| Common Data Model | Microsoft Dataverse |
| AI Builder (preview label) | AI Builder |
| Project Oakdale | Microsoft Dataverse for Teams |
| Dynamics 365 Marketing | Dynamics 365 Customer Insights - Journeys |
| Dynamics 365 Customer Insights (standalone) | Dynamics 365 Customer Insights - Data |
| Virtual Power Agents | Microsoft Copilot Studio |
| Power Virtual Agents (PVA) | Microsoft Copilot Studio |
| Bot | Copilot (in Copilot Studio context) |
| Canvas app (capitalised as "Canvas App") | canvas app (lowercase) |
| Model-driven app (capitalised inconsistently) | model-driven app (lowercase) |
| AzureADPreview module | Microsoft Graph PowerShell (`Microsoft.Graph` modules) |
 
> [!NOTE]
> PowerShell cmdlets using legacy module names (e.g., `Connect-AzureAD`)
> should be flagged. If the lab has not migrated, keep the code as-is and
> add a `[!NOTE]` clarifying the legacy naming. If migrated, use Graph
> equivalents (e.g., `Connect-MgGraph`, `Get-MgUser`).
 
> [!NOTE]
> Product names in lab **titles** and **YAML front matter** may follow
> content-owner conventions — do not rename unless specifically asked.
 
## Anti-patterns to catch and fix
 
| Anti-pattern | Fix |
|---|---|
| `>**Note**:` or `> **Note:**` | `> [!NOTE]` callout block |
| "Click **Save**" | "Select **Save**" |
| "you will see the page" | "the page displays" |
| "you will have successfully" | "you have successfully" |
| Bold on typed values: `**admin@contoso.com**` | Backticks: `` `admin@contoso.com` `` |
| Backticks on UI elements: `` `Save` `` | Bold: **Save** |
| `* Item` bullets | `- Item` bullets |
| Smart quotes `"value"` | Straight quotes `"value"` |
| Indented callout blocks | Column 0 callout blocks |
| Skipped heading levels | Correct heading hierarchy |
| "Azure AD" in prose | "Microsoft Entra ID" |
| No blank line before/after callout | Add blank lines |
| HTML tables | Markdown pipe tables |
| Multiple actions in one step | Split into separate steps |
 
## Reminder
 
Your FIRST action is always applying corrections — copy the uploaded file to `/mnt/user-data/outputs/`, then edit it in place with `str_replace`. Never create a separate new file. Then chat.