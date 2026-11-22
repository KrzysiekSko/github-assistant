# github-assistant

> Comprehensive GitHub Workflow Assistant plugin for [Claude Code](https://claude.com/claude-code).

`github-assistant` provides automated Pull Request creation with Gitleaks preflight checks, Board-of-3 code reviews, issue triage & automated implementation, CI/CD auto-heal diagnostics, and semantic release pipelines directly within your Claude Code sessions.

---

## Features

- **Automated PR Lifecycle & Preflight (`/pr`)**:
  - Checks branch status, detects uncommitted changes, and enforces Conventional Commits.
  - **Preflight Sanitizer (`/pr preflight`)**: Scans staged changes against Gitleaks secret pattern heuristics (`sk-`, `Authorization: Bearer`, DPAPI signatures) and cross-platform hygiene (LF line endings, machine-specific paths).
  - Automatically discovers repository PR templates (`.github/PULL_REQUEST_TEMPLATE.md`).
  - Generates comprehensive PR descriptions with summaries, key changes, and verification steps.
  - Inspects check runs, review statuses, and handles squashing/merging.

- **Multi-Dimensional & Board-of-3 Review (`/review`)**:
  - In-depth review across correctness, security, performance, and architecture.
  - **Board-of-3 Audit (`/review --board`)**: Runs three specialist auditor personas in parallel (Security & Compliance, Reliability & Concurrency, Architecture & Scope) and outputs a consolidated verdict (`PASS` / `CONCERNS` / `BLOCKER`).
  - Supports `--fix` to automatically apply fixes to local files, and `--comment` to post directly to GitHub PRs.

- **Issue Triage & Automated Implementation (`/issue`)**:
  - Search, filter, and list repository issues.
  - Triages bugs by locating the exact source files, isolating root causes, and formulating fix plans.
  - **End-to-End Implementation (`/issue implement <NUM>`)**: Automatically creates a topic branch, applies minimal fix, adds regression test, and opens a PR referencing `Closes #<NUM>`.
  - Creates structured bug reports or feature requests using repository issue templates.

- **GitHub Actions & CI Auto-Heal (`/ci`)**:
  - Monitors live GitHub Actions workflows and PR checks.
  - **CI Doctor (`/ci doctor`)**: Automatically categorizes failures into 4 quadrants (Infra/Flaky, Test Assertion, Dependency Drift, Secret Scanner).
  - **Secret Scanner Healing (`/ci fix-leaks`)**: Downloads failure logs, neutralizes sample keys, amends commits, and force-pushes clean history.
  - Re-runs failed jobs only with `gh run rerun <id> --failed`.

- **Release Management (`/release`)**:
  - **One-Shot Release Pipeline (`/release auto`)**: Calculates SemVer bump, generates changelog, tags release, and creates GitHub release in a single command.
  - Generates changelogs from Conventional Commits since the last tag.
  - Supports manual version bumps (patch / minor / major) and draft releases.
  - Opens a pre-release review PR via `/release pr`.

---

## Installation

### Method 1: Local Development
Load the plugin in your Claude Code session by pointing to this directory:
```powershell
claude --plugin-dir C:\Projekty\NaszProjektAgentowy
```

Or set the environment variable:
```powershell
$env:CLAUDE_CODE_PLUGIN_DIRS = "C:\Projekty\NaszProjektAgentowy"
```

### Method 2: Via GitHub Marketplace
Install directly in any Claude Code session via the repository marketplace:
```powershell
claude plugin install github-assistant --marketplace KrzysiekSko/github-assistant
```
Or use the slash command inside Claude Code:
```
/plugin install github-assistant --marketplace KrzysiekSko/github-assistant
```

---

## Slash Commands

| Command | Arguments | Description |
|---|---|---|
| `/pr` | `[create \| status \| preflight \| ready \| merge]` | Create or manage PRs with secret preflight checks |
| `/review` | `[pr_number \| branch] [--board] [--fix] [--comment]` | Review diffs, PRs, or run Board-of-3 audit |
| `/issue` | `[create \| list \| search \| triage \| implement]` | Manage, triage, and implement GitHub issues |
| `/ci` | `[status \| view \| diagnose \| doctor \| fix-leaks \| rerun]` | Monitor and auto-heal GitHub Actions CI runs |
| `/release` | `[auto \| changelog \| bump \| tag \| create \| pr]` | Automated SemVer releases from Conventional Commits |

---

## Agent Skills Included

- **`github-pr`**: Deep PR workflow guidance, conventional commit formatting, template compliance, and preflight checks.
- **`github-sanitizer`**: Secret scanner heuristic sanitization (Gitleaks false-positive prevention) and cross-platform git hygiene.
- **`github-review`**: Multi-dimensional code review checklists and Board-of-3 audit deliberation.
- **`github-triage`**: Systematic bug reproduction, issue investigation, and automated implementation.
- **`github-ci`**: CI failure log extraction, stack trace parsing, CI doctor categorization, and auto-heal workflows.
- **`github-release`**: Automated release pipelines from Conventional Commits — changelog, semantic version bump, tagging, and GitHub release creation.

---

## Requirements

- [GitHub CLI (`gh`)](https://cli.github.com/) installed and authenticated (`gh auth login`), **OR** the GitHub MCP server connected in Claude Code.
- Git 2.25+ installed.

---

## License

MIT © Specjalista
