---
name: github-triage
description: Systematically triage, reproduce, investigate root causes, and structure GitHub issues into actionable implementation tasks.
metadata:
  type: project
---

# GitHub Issue Triage Skill

This skill guides bug reproduction, issue investigation, labeling, and implementation planning.

## Triage Procedure

### Step 1: Ingest & Understand
- Read the issue description, reproduction steps, environment details, and user comments.
- Check if related or duplicate issues already exist (`gh issue list --search "<keywords>"`).

### Step 2: Codebase Localization
- Search for error messages, function names, route endpoints, or UI components mentioned in the issue.
- Identify the exact files and lines responsible for the reported behavior.

### Step 3: Reproduce & Diagnose
- Formulate a hypothesis for why the defect occurs under the reported conditions.
- If possible, formulate a failing test case that demonstrates the bug.
- Check git blame/history (`git log -S "<symbol>"`) to determine when and why the behavior changed.

### Step 4: Formulate the Resolution Plan
- Define the minimal, cleanest fix that solves the problem without breaking existing behavior.
- Determine required test cases to prevent regression.
- Estimate impact across dependent modules or API clients.

### Step 5: Document and Link
- When opening a PR to resolve the issue, include `Fixes #<issue_number>` or `Closes #<issue_number>` in the PR body.
- This ensures GitHub automatically closes the issue upon merging.
