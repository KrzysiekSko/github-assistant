---
name: github-triage
description: Systematically triage, reproduce, investigate root causes, and execute end-to-end implementation workflows from GitHub issues.
metadata:
  type: project
---

# GitHub Issue Triage & Implementation Skill

This skill guides bug reproduction, issue investigation, root cause diagnosis, and automated end-to-end implementation from issue to pull request.

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

### Step 4: End-to-End Implementation Workflow
Execute full transition from issue to pull request:
1. **Branch Checkout**:
   - Create and check out a dedicated branch: `git checkout -b fix/issue-<number>-<slug>` (or `feat/...`).
2. **Apply Minimal Fix**:
   - Make the targeted fix following the repository's architecture and coding standards.
3. **Add Regression Test**:
   - Add a unit or integration test proving the bug is fixed and cannot regress.
4. **Pull Request Automation**:
   - Commit with Conventional Commits referencing the issue.
   - Push and open a PR with `Closes #<issue_number>` in the body so GitHub automatically closes the issue upon merge.
