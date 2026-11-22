---
description: Create, search, list, or triage GitHub issues
argument-description: "[create | list | search | triage] [query | issue_number]"
---

# Issue Management Command (`/issue`)

Manage GitHub issues, triage bugs, and link tasks to code locations.

## Actions

### 1. List / Search (`/issue list` or `/issue search <query>`)
- List open issues: `gh issue list --limit 15` or use GitHub MCP `list_issues`.
- Search with filters: `gh issue list --search "<query>"` or GitHub MCP `search_issues`.
- Render a concise summary table:
  - `#ID` | `Title` | `Author` | `Labels` | `Updated`

### 2. Triage Issue (`/issue triage <issue_number>`)
- Fetch full issue details and comments: `gh issue view <issue_number> --comments` or GitHub MCP `issue_read`.
- Locate affected files and code paths using `Grep` or `Glob`.
- Identify root cause or reproduction conditions.
- Produce a structured triage summary:
  - **Summary**: Concise explanation of the reported behavior vs expected behavior.
  - **Suspected Root Cause**: Specific files, functions, or configurations.
  - **Proposed Fix Steps**: Ordered action items to resolve the issue.
  - **Test Plan**: How to reproduce and verify the fix.

### 3. Create Issue (`/issue create`)
- Check for issue templates in `.github/ISSUE_TEMPLATE/` (e.g., `bug_report.md`, `feature_request.md`).
- Prompt user for title and key details if not provided in arguments.
- Format body using standard template sections:
  - Problem description / motivation
  - Steps to reproduce (for bugs) / Proposed solution (for features)
  - Relevant logs, error outputs, or environment information
- Run `gh issue create --title "<title>" --body "<body>"` (or GitHub MCP `issue_write`).
- Return the created issue URL and ID.
