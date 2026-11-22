---
name: github-release
description: Release management from Conventional Commits: automated changelog generation, semantic version bump, git tagging, and GitHub release creation.
metadata:
  type: project
---

# GitHub Release Skill

This skill prepares and executes GitHub releases driven by Conventional Commits, including automated end-to-end release pipelines.

## Release Policy

### 1. Versioning (Semantic Versioning)
The version bump is derived from commits since the last tag:
- `feat:` → **minor** bump (new feature)
- `fix:` → **patch** bump (bug fix)
- `BREAKING CHANGE:` in the commit body/footer, or `!` after the scope (e.g. `feat(api)!:`) → **major** bump

If multiple types appear, the highest severity wins: major > minor > patch.

### 2. Changelog
Commits are grouped by type into sections:
Features, Bug Fixes, Documentation, Refactoring, Performance, Tests, Chore & Maintenance.

Each entry links to its commit on GitHub.

### 3. Automated Release Pipeline (`/release auto`)
Executes the full release workflow:
1. Detects commits since last tag: `git log <last_tag>..HEAD`.
2. Computes the SemVer bump.
3. Updates manifest files.
4. Creates annotated git tag: `git tag -a v<ver> -m "Release v<ver>"`.
5. Pushes tag and creates GitHub release with generated release notes.

### 4. Pre-release checklist
- [ ] All target commits are merged into the release base branch (`main`/`master`).
- [ ] CI passes on the base branch.
- [ ] Changelog reviewed for accuracy.
- [ ] Version bump agreed (`patch`/`minor`/`major`).
- [ ] Tag created and pushed only after confirmation.

### 5. Release notes
Release notes are generated from the changelog and appended with:
```
---
🤖 Generated with [Claude Code](https://claude.com/claude-code)
```

## Related Commands
- `/release [auto | changelog | bump | tag | create | pr]` — see `commands/release.md`.
- `/pr` — used by `/release pr` to open the pre-release review PR.
