---
name: github-pr
description: End-to-end pull request management: drafting conventional commit messages, preflight sanitization, PR templates, CI verification, and merging.
metadata:
  type: project
---

# GitHub Pull Request Workflow Skill

This skill guides automated, high-quality Pull Request creation, preflight sanitization, and lifecycle maintenance.

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
1. **Uncommitted Changes:** Ensure all local changes are committed or stashed (`git status`).
2. **Local Tests & Linters:** Run test suite and linters to prevent immediate CI failures.
3. **Secret Scanner Preflight:** Check staged diff for mock keys or regex triggers matching Gitleaks heuristics (`sk-`, `Authorization: Bearer`, DPAPI Base64).
4. **Cross-Platform Hygiene:** Check line endings (ensure LF on `.sh`, `.yml`, Dockerfiles) and verify no hardcoded machine paths exist in diff.
5. **Push with Upstream:** Push branch to remote with upstream tracking: `git push -u origin <branch-name>`.
6. **Template Discovery:** Adhere to `.github/PULL_REQUEST_TEMPLATE.md` if present.
7. **Create PR:** Create PR via `gh pr create` or GitHub MCP server tools.
