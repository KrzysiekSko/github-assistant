---
description: Inspect, monitor, and diagnose GitHub Actions CI workflow runs
argument-description: "[status | view | diagnose | rerun] [run_id]"
---

# CI & Actions Diagnostics Command (`/ci`)

Monitor GitHub Actions pipelines, inspect failed workflows, and automatically diagnose build or test errors.

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

### 4. Rerun Failed Jobs (`/ci rerun [run_id]`)
- Trigger a rerun for failed jobs only:
  `gh run rerun <run_id> --failed`
- Or rerun all jobs:
  `gh run rerun <run_id>`
- Provide workflow URL so the user can watch execution.
