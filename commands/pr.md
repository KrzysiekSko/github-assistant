---
description: Create, inspect, or manage pull requests on GitHub
argument-description: "[create | status | preflight | ready | merge] [target-branch]"
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
2. **Preflight Sanitization & Cross-Platform Check**:
   - Automatically run preflight checks (see `/pr preflight`) to prevent CI breaks caused by secret scanner false positives (Gitleaks) or CRLF/path issues.
3. **Commit & Push**:
   - Verify all relevant commits follow Conventional Commits format (`feat:`, `fix:`, `refactor:`, `docs:`, `test:`, etc.).
   - Ensure branch is pushed to remote: `git push -u origin <branch-name>`.
4. **Template Discovery**:
   - Check for PR templates in `.github/PULL_REQUEST_TEMPLATE.md`, `.github/PULL_REQUEST_TEMPLATE/`, or `pull_request_template.md`.
   - Adhere to the template structure if found.
5. **Generate Description**:
   - Title: concise imperative statement with conventional commit prefix (e.g. `feat(auth): add OAuth2 provider support`).
   - Summary: clear overview of *why* and *what* changed.
   - Key Changes: bullet points highlighting architectural changes and new files.
   - Verification: how the changes were verified (tests run, manual testing).
6. **Execute Creation**:
   - Run `gh pr create --title "<title>" --body "<body>"` (or use GitHub MCP `create_pull_request`).
   - If draft requested, add `--draft`.
   - Always append the attribution:
     `🤖 Generated with [Claude Code](https://claude.com/claude-code)`
   - Output the clickable URL of the newly created PR.

### 2. Preflight Safety Check (`/pr preflight`)
Scan staged changes or branch diff before opening a PR:
- **Secret Scanner / Gitleaks Preflight**:
  - Scan diff for mock keys matching heuristic rules: `sk-[a-zA-Z0-9_\-]{8,}`, `Authorization:\s*Bearer`, `__sealed__:[a-zA-Z0-9+/=]{20,}`.
  - Warn if documentation or runbooks contain realistic-looking tokens; recommend neutral placeholders like `<API_KEY_PLACEHOLDER>`.
- **Cross-Platform Hygiene**:
  - Ensure shell scripts (`*.sh`), workflow YAMLs (`.github/workflows/*.yml`), and Dockerfiles use LF line endings, not CRLF.
  - Detect hardcoded absolute paths specific to the developer's machine (`C:\Users\...`, `/home/...`).

### 3. Status & CI Checks (`/pr status`)
- Check current PR status: `gh pr status`.
- Check live CI status and checks for the active PR: `gh pr checks`.
- Summarize passing, failing, and pending checks.
- If checks are failing, suggest running `/ci diagnose` to inspect the failure logs.

### 4. Mark as Ready for Review (`/pr ready`)
- Convert a draft PR to ready for review: `gh pr ready`.
- Request reviewers if specified: `gh pr edit --add-reviewer <username>`.

### 5. Merge PR (`/pr merge`)
- Ensure all CI checks have passed: `gh pr checks`.
- Merge using preferred strategy:
  - Squash and merge (recommended for feature branches): `gh pr merge --squash --delete-branch`.
  - Rebase: `gh pr merge --rebase --delete-branch`.
  - Standard merge commit: `gh pr merge --merge --delete-branch`.
- Inform user of successful merge and branch deletion.
