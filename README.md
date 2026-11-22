# github-assistant

> Comprehensive GitHub Workflow Assistant plugin for [Claude Code](https://claude.com/claude-code).

`github-assistant` provides automated Pull Request creation, multi-dimensional code reviews, intelligent issue triage, and GitHub Actions CI diagnostics directly within your Claude Code sessions.

---

## Features

- **Automated PR Lifecycle (`/pr`)**:
  - Checks branch status, detects uncommitted changes, and enforces Conventional Commits.
  - Automatically discovers repository PR templates (`.github/PULL_REQUEST_TEMPLATE.md`).
  - Generates comprehensive PR descriptions with summaries, key changes, and verification steps.
  - Inspects check runs, review statuses, and handles squashing/merging.

- **Multi-Dimensional Code Review (`/review`)**:
  - In-depth review across correctness, security, performance, and architecture.
  - Generates concrete failure scenarios and line-specific clickable suggestions.
  - Supports `--fix` to automatically apply fixes to local files, and `--comment` to post directly to GitHub PRs.

- **Issue Triage & Management (`/issue`)**:
  - Search, filter, and list repository issues.
  - Triages bugs by locating the exact source files, isolating root causes, and formulating fix plans.
  - Creates structured bug reports or feature requests using repository issue templates.

- **GitHub Actions & CI Diagnostics (`/ci`)**:
  - Monitors live GitHub Actions workflows.
  - Downloads and parses failed step logs to extract stack traces.
  - Recommends exact fixes for CI-specific errors (matrix mismatches, lockfile issues, environment differences).

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
Once pushed to a GitHub repository (`<owner>/<repo>`), install it across your team with:
```powershell
claude plugin install github-assistant --marketplace <owner>/<repo>
```

---

## Slash Commands

| Command | Arguments | Description |
|---|---|---|
| `/pr` | `[create \| status \| ready \| merge]` | Create or manage pull requests |
| `/review` | `[pr_number \| branch] [--fix] [--comment]` | Review diffs, PRs, or local changes |
| `/issue` | `[create \| list \| search \| triage]` | Manage and triage GitHub issues |
| `/ci` | `[status \| view \| diagnose \| rerun]` | Monitor and debug GitHub Actions CI runs |

---

## Agent Skills Included

- **`github-pr`**: Deep PR workflow guidance, conventional commit formatting, and template compliance.
- **`github-review`**: Multi-dimensional code review checklists (correctness, security, performance).
- **`github-triage`**: Systematic bug reproduction and issue resolution workflows.
- **`github-ci`**: CI failure log extraction, stack trace parsing, and troubleshooting.

---

## Requirements

- [GitHub CLI (`gh`)](https://cli.github.com/) installed and authenticated (`gh auth login`), **OR** the GitHub MCP server connected in Claude Code.
- Git 2.25+ installed.

---

## License

MIT © Specjalista
