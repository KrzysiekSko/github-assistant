---
name: github-ci
description: GitHub Actions workflow monitoring, failed step log extraction, automated stack trace diagnosis, and CI fix recommendations.
metadata:
  type: project
---

# GitHub CI/CD Diagnostics Skill

This skill diagnoses GitHub Actions pipeline failures, extracts actionable stack traces from logs, and guides local reproduction and fixes.

## CI Failure Analysis Protocol

### 1. Identify the Failing Run & Job
- List recent runs: `gh run list --limit 5`.
- Inspect the failed run: `gh run view <run_id>`.
- Identify the exact failed job and step within the job matrix (e.g. Node 18, Node 20, Ubuntu, Windows).

### 2. Extract & Filter Logs
- Fetch failure logs directly:
  `gh run view <run_id> --log-failed`
- Filter noise (runner initialization, checkout, cache download) to pinpoint the exact failure line:
  - Look for keywords: `FAIL`, `Error:`, `AssertionError`, `TypeError`, `exit code 1`, `SyntaxError`.
  - Extract the test file name and line number: `path/to/test.spec.ts:45`.

### 3. Categorize the Failure Mode
Common CI failure categories:
1. **Flaky Test / Race Condition**: Fails intermittently or only on slower CI machines. Solution: eliminate timing dependencies, use proper async await / polling.
2. **Environment & Matrix Discrepancy**: Works locally on OS X/Windows but fails on Ubuntu (or vice versa). Check case-sensitivity in file paths, line endings (`\r\n` vs `\n`), or Node/Python versions.
3. **Missing Environment Secrets**: Missing API keys or test credentials in GitHub repository secrets (`secrets.MY_TOKEN`).
4. **Linter / Formatting**: Pre-commit hooks not run locally. Run `npm run lint` or `prettier --check .`.
5. **Dependency Cache Mismatch**: Outdated lockfile (`package-lock.json`, `poetry.lock`, `Cargo.lock`).

### 4. Fix & Verify
- Reproduce the exact command locally in the same environment settings.
- Apply the fix.
- Re-run the tests locally before pushing to save CI minutes.
- Push the fix and monitor the new run with `gh run watch`.
