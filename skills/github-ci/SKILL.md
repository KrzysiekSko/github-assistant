---
name: github-ci
description: GitHub Actions workflow monitoring, failed step log extraction, automated stack trace diagnosis, and CI fix recommendations.
metadata:
  type: project
---

# GitHub CI/CD Diagnostics Skill

This skill diagnoses GitHub Actions pipeline failures, extracts actionable stack traces from logs, and guides local reproduction and automated auto-heal fixes.

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

### 3. Categorize the Failure Mode (CI Doctor)
Common CI failure categories and automated remedies:
1. **Flaky Test / Runner Outage**: Fails intermittently or on network timeouts. Remedy: trigger automated re-run via `gh run rerun <run_id> --failed`.
2. **Environment & Matrix Discrepancy**: Works locally on Windows/macOS but fails on Ubuntu. Check case-sensitivity in file paths, line endings (`\r\n` vs `\n`), or Node/Python versions.
3. **Secret Scanner / Gitleaks Failure**: Triggered by heuristic matches on documentation mock keys. Remedy: sanitize examples to neutral placeholders, amend commit, and force-push.
4. **Missing Environment Secrets**: Missing API keys or test credentials in GitHub repository secrets (`secrets.MY_TOKEN`).
5. **Linter / Formatting**: Pre-commit hooks not run locally. Run `npm run lint` or `prettier --check .`.
6. **Dependency Cache Mismatch**: Outdated lockfile (`package-lock.json`, `poetry.lock`, `Cargo.lock`). Run lockfile update.

### 4. Fix & Verify
- Reproduce the exact command locally in the same environment settings.
- Apply the fix.
- Re-run the tests locally before pushing to save CI minutes.
- Push the fix and monitor the new run with `gh run watch`.
