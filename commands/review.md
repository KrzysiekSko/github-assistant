---
description: Perform multi-dimensional code review on git diff or pull request
argument-description: "[pr_number | branch | HEAD] [--fix] [--comment]"
---

# Code Review Command (`/review`)

Automated, rigorous code review for changes in the current branch, against a base branch, or on a specific pull request.

## Review Workflow

1. **Obtain Diff**:
   - If PR number provided: `gh pr diff <pr_number>` or use GitHub MCP `pull_request_read` with `method: "get_diff"`.
   - If branch provided: `git diff origin/<base-branch>...<branch>`.
   - Default: `git diff HEAD~1` or uncommitted working tree diff `git diff HEAD`.

2. **Review Dimensions**:
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

3. **Format Findings**:
   Present verified findings ordered by severity:
   - `Important`: Bugs, security vulnerabilities, or breaks that must be resolved before merging.
   - `Nit`: Style improvements, simplifications, minor polish.
   - `Pre-existing`: Latent defects observed in adjacent code not introduced by this diff.

   Format each finding as:
   - **Location**: `[path/to/file:line](path/to/file:line)`
   - **Issue**: Precise description of what will fail or behave incorrectly.
   - **Scenario**: Concrete input/condition triggering the defect.
   - **Suggested Fix**: Clear code snippet illustrating the recommended correction.

4. **Options Handling**:
   - If `--fix` was specified: Offer to apply the verified fixes directly to the local files.
   - If `--comment` was specified: Post the review comments to the pull request via `gh pr comment` or GitHub review API.
