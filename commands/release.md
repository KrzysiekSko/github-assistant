---
description: Prepare and create GitHub releases from Conventional Commits (changelog, version bump, tag, release)
argument-description: "[changelog | bump | tag | create | pr] [patch|minor|major|draft]"
---

# Release Command (`/release`)

Prepare a release from Conventional Commits since the last tag: changelog, semantic version bump, git tag, and GitHub release.

## Actions

### 1. Changelog (`/release changelog` or bare `/release`)
- Locate the most recent tag: `git describe --tags --abbrev=0` (or `HEAD~1` if no tags exist yet).
- List commits since that tag: `git log <last-tag>..HEAD --pretty=format:"%s"`.
- Group commits by conventional type:
  - `feat` → **Features**
  - `fix` → **Bug Fixes**
  - `docs` → **Documentation**
  - `refactor` → **Refactoring**
  - `perf` → **Performance**
  - `test` → **Tests**
  - `chore`, `build`, `ci` → **Chore & Maintenance**
- Render a markdown changelog with commit links (`[hash](<repo>/commit/<hash>)`).
- If no conventional commits are found, fall back to listing all commits since the tag.

### 2. Version bump (`/release bump [patch|minor|major]`)
- Read the current version from `.claude-plugin/plugin.json` (and `package.json`, `Cargo.toml`, `pyproject.toml` if present).
- Suggest the bump based on commit types since the last tag:
  - any `feat:` → **minor**
  - any `fix:` (and no `feat`) → **patch**
  - `BREAKING CHANGE:` in the footer or `!` after the scope (e.g. `feat(api)!:`) → **major**
- If an explicit level is given (`patch`/`minor`/`major`), use it instead.
- Print the proposed new version and confirm before writing it to any file.

### 3. Tag (`/release tag`)
- After a bump is confirmed, create an annotated tag:
  `git tag -a v<version> -m "Release v<version>"`.
- Push the tag: `git push origin v<version>`.
- Output the tag URL on GitHub.

### 4. Create release (`/release create [draft]`)
- Requires a pushed tag. Generate release notes from the changelog.
- Run `gh release create v<version> --notes "<notes>" [--draft]`.
- Output the release URL.
- Always append the attribution:
  `🤖 Generated with [Claude Code](https://claude.com/claude-code)`

### 5. Release PR (`/release pr`)
- Open a pull request containing the changelog and version bump for review before releasing.
- Delegates to the existing `/pr create` flow.
- Base branch: default branch (`main`/`master`); head: a `release/v<version>` branch.

## Notes
- Requires GitHub CLI (`gh`) authenticated (`gh auth login`), or the GitHub MCP server connected in Claude Code.
- All actions respect existing repository release templates if present.
