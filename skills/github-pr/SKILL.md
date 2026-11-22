---
name: github-pr
description: End-to-end pull request management: drafting conventional commit messages, formatting PR descriptions from templates, verifying CI checks, and executing merges.
metadata:
  type: project
---

# GitHub Pull Request Workflow Skill

This skill guides automated, high-quality Pull Request creation and maintenance.

## Best Practices

### 1. Branch Strategy & Conventional Commits
- Always work on a descriptive branch (`feature/`, `bugfix/`, `refactor/`, `chore/`).
- Commit messages should follow the Conventional Commits specification:
  - `feat(<scope>): <description>`: Introduces a new feature
  - `fix(<scope>): <description>`: Fixes a bug
  - `docs(<scope>): <description>`: Documentation changes
  - `refactor(<scope>): <description>`: Code change that neither fixes a bug nor adds a feature
  - `test(<scope>): <description>`: Adding or correcting tests
  - `chore(<scope>): <description>`: Build process, auxiliary tools, dependencies

### 2. PR Body Structure
When creating a pull request, format the body systematically:
```markdown
## Summary
Brief description of the problem solved or functionality added.

## Motivation & Context
Why is this change required? What problem does it solve?
Fixes #<issue_number> (if applicable)

## Key Changes
- List of specific technical changes
- Architectural decisions or modifications

## Verification & Testing
- How were these changes verified?
- Commands run (e.g. `npm test`, `pytest`, `cargo test`)
- Test results summary

## Screenshots / Artifacts (if applicable)
Visual evidence for UI changes.

---
🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

### 3. Pre-flight Checklist Before Creating PR
1. Ensure all local changes are committed or stashed.
2. Run local tests and linters to prevent immediate CI failures.
3. Push branch to remote with upstream tracking: `git push -u origin <branch-name>`.
4. Check if a repository template exists in `.github/PULL_REQUEST_TEMPLATE/` or `.github/pull_request_template.md`.
5. Create PR via `gh pr create` or GitHub MCP server tools.
