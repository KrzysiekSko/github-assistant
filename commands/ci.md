---
description: Inspect, monitor, and diagnose GitHub Actions CI workflow runs
argument-description: "[status | view | diagnose | doctor | fix-leaks | rerun] [run_id]"
---

# CI & Actions Diagnostics Command (`/ci`)

Monitor GitHub Actions pipelines, inspect failed workflows, and automatically diagnose build, test, or secret scanner errors.

## Actions

### 1. Check CI Status (`/ci status` or bare `/ci`)
- View recent runs on current branch or PR:
  `gh run list --limit 10`
- Display status indicators:
  - ✓ Success
  - ✗ Failure
  - ◷ In Progress / Queued
- If a pull request is open, inspect checks with `gh pr checks`.

### 2. View Failed Run (`/ci view [run_id]`)
- If `run_id` omitted, target the most recent failed run on the current branch.
- Inspect workflow summary:
  `gh run view <run_id>`
- List all jobs within the run and highlight which specific steps failed.

### 3. Diagnose Failures (`/ci diagnose [run_id]`)
- Retrieve failure logs:
  `gh run view <run_id> --log-failed`
- Parse log output:
  - Filter out boilerplate setup/teardown output.
  - Isolate the stack trace, compiler error, or test failure assertion.
  - Locate the corresponding source files in the local repository.
- Provide automated diagnosis:
  - **Failing Step / Test**: Exact name and error message.
  - **Error Analysis**: Why it failed in the CI environment (e.g., missing environment variable, platform difference, dependency version mismatch, broken assertion).
  - **Remediation**: Specific patch or command to resolve the issue locally before re-pushing.

### 4. CI Doctor & Auto-Heal (`/ci doctor [run_id]`)
Performs automatic categorization of the failure into one of 4 root causes:
1. **Infra / Flaky Runner Failure**:
   - Symptoms: Network timeout, runner disconnect, package manager download 503/429 rate limit.
   - Action: Offers automated re-run via `gh run rerun <run_id> --failed`.
2. **Test Assertion Failure**:
   - Symptoms: Unit/integration test failure, exit code 1 in pytest/jest/vitest.
   - Action: Pinpoints exact file and test method, analyzes expected vs actual values, drafts local fix.
3. **Dependency / Lockfile Drift**:
   - Symptoms: `package-lock.json out of date`, npm ci error EUSAGE, poetry/pipenv lock mismatch.
   - Action: Suggests exact local command to regenerate lockfile cleanly (`npm install --package-lock-only`, `pnpm install`, etc.).
4. **Secret Scanner / Gitleaks Failure**:
   - Symptoms: `Gitleaks secret scan fail`, matches on documentation samples.
   - Action: Recommends running `/ci fix-leaks`.

### 5. Fix Secret Scanner Leaks (`/ci fix-leaks [run_id]`)
Targeted recovery workflow when Gitleaks or secret scanners fail on documentation examples or mock keys:
1. Fetches the exact rule and line from `gh run view <run_id> --log-failed`.
2. Locates the offending pattern (e.g. `sk-`, `Authorization: Bearer`, high-entropy Base64).
3. Replaces it with neutral placeholders (`<API_KEY_PLACEHOLDER>`, `<TOKEN>`).
4. Replaces the commit via `git commit --amend` and applies a safe `--force` update to the PR branch.
5. Monitors `gh pr checks` until the scan passes.

### 6. Rerun Failed Jobs (`/ci rerun [run_id]`)
- Trigger a rerun for failed jobs only:
  `gh run rerun <run_id> --failed`
- Or rerun all jobs:
  `gh run rerun <run_id>`
- Provide workflow URL so the user can watch execution.
