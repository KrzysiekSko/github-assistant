---
description: Create, inspect, or manage pull requests on GitHub
argument-description: "[create | status | ready | merge] [target-branch]"
---

# Pull Request Command (`/pr`)

Use this command to manage pull requests efficiently using the GitHub CLI (`gh`) or GitHub MCP tools.

## Usage Modes

### 1. Create PR (`/pr create` or bare `/pr`)
When asked to create a pull request:
1. **Branch Verification**:
   - Check current branch: `git branch --show-current`.
   - If on the default branch (`main` or `master`), warn the user and suggest creating a feature branch first (`git checkout -b feature/your-feature-name`).
   - Check for uncommitted changes: `git status --porcelain`. If changes exist, ask whether to commit them first.
2. **Commit & Push**:
   - Verify all relevant commits follow Conventional Commits format (`feat:`, `fix:`, `refactor:`, `docs:`, `test:`, etc.).
   - Ensure branch is pushed to remote: `git push -u origin <branch-name>`.
3. **Template Discovery**:
   - Check for PR templates in `.github/PULL_REQUEST_TEMPLATE.md`, `.github/PULL_REQUEST_TEMPLATE/`, or `pull_request_template.md`.
   - Adhere to the template structure if found.
4. **Generate Description**:
   - Title: concise imperative statement with conventional commit prefix (e.g. `feat(auth): add OAuth2 provider support`).
   - Summary: clear overview of *why* and *what* changed.
   - Key Changes: bullet points highlighting architectural changes and new files.
   - Verification: how the changes were verified (tests run, manual testing).
5. **Execute Creation**:
   - Run `gh pr create --title "<title>" --body "<body>"` (or use GitHub MCP `create_pull_request`).
   - If draft requested, add `--draft`.
   - Always append the attribution:
     `🤖 Generated with [Claude Code](https://claude.com/claude-code)`
   - Output the clickable URL of the newly created PR.

### 2. Status & CI Checks (`/pr status`)
- Check current PR status: `gh pr status` or `gh pr view --json number,title,state,url,statusCheckRollup`.
- Display summary table of checks (Passed / Pending / Failed).
- Report any blocking review comments or conflicts.

### 3. Mark Ready for Review (`/pr ready`)
- If the PR is in draft state: `gh pr ready`.
- Confirm transition to reviewers.

### 4. Merge (`/pr merge`)
- Verify all CI checks pass and reviews are approved.
- Ask user for preferred merge strategy: `squash` (default), `rebase`, or `merge`.
- Run `gh pr merge --squash --delete-branch` upon user confirmation.
