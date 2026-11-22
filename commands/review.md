---
description: Perform multi-dimensional code review on git diff or pull request
argument-description: "[pr_number | branch | HEAD] [--board] [--fix] [--comment]"
---

# Code Review Command (`/review`)

Automated, rigorous code review for changes in the current branch, against a base branch, or on a specific pull request.

## Review Workflow

1. **Obtain Diff**:
   - If PR number provided: `gh pr diff <pr_number>` or use GitHub MCP `pull_request_read` with `method: "get_diff"`.
   - If branch provided: `git diff origin/<base-branch>...<branch>`.
   - Default: `git diff HEAD~1` or uncommitted working tree diff `git diff HEAD`.

2. **Standard Review Dimensions**:
   Evaluate changes across four core dimensions:
   - **Correctness & Edge Cases**:
     - Off-by-one errors, null/undefined dereferences, unhandled exception paths.
     - Race conditions, concurrent mutation, missing locks or synchronization.
     - Type mismatch or silent coercion bugs.
   - **Security & Data Safety**:
     - Hardcoded credentials, API tokens, sensitive environment data.
     - Injection vectors (SQL, command, prompt injection, XSS).
     - Insecure defaults or missing permission checks.
   - **Performance & Efficiency**:
     - Accidental N+1 queries or repetitive disk/network I/O in loops.
     - Unbounded array/memory allocations or leaks.
   - **Maintainability & Conventions**:
     - Adherence to project conventions and design patterns.
     - Clear naming, proper error wrapping, documentation updates.

3. **Board-of-3 Review Mode (`/review --board`)**:
   Runs three simulated specialist auditors in parallel for critical PRs:
   - **Auditor 1: Security & Compliance**:
     - Focus: Injection, authentication/authorization boundaries, credential leakage heuristics, safe data serialization.
   - **Auditor 2: Reliability & Concurrency**:
     - Focus: Deadlocks, timeout limits, race conditions, fail-closed handling, crash loops, rollback readiness.
   - **Auditor 3: Architecture & Scope**:
     - Focus: Minimal diff integrity (no scope creep), DRY vs over-abstraction, backward compatibility, documentation parity.
   - **Output**: Generates a consolidated deliberation table with individual verdicts and a unified synthesis:
     `VERDICT: [PASS | CONCERNS | BLOCKER]`

4. **Format Findings**:
   Present verified findings ordered by severity:
   - `Important`: Bugs, security vulnerabilities, or breaks that must be resolved before merging.
   - `Nit`: Style improvements, simplifications, minor polish.
   - `Pre-existing`: Latent defects observed in adjacent code not introduced by this diff.

   Format each finding with:
   - **Location**: `path/to/file.ext:line_number`
   - **Issue**: Specific description of the flaw.
   - **Concrete Scenario**: An exact edge case, input, or environment condition that triggers the bug.
   - **Suggested Fix**: Clear code snippet illustrating the recommended correction.

5. **Action Flags**:
   - `--fix`: Automatically apply the suggested fixes directly to the local files. Run git diff afterwards to confirm corrections.
   - `--comment`: Format the review findings as inline GitHub review comments and submit them via `gh pr review --comment --body "<summary>"` (or GitHub MCP).
