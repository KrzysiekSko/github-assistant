---
name: github-review
description: Comprehensive code review across correctness, security, performance, architecture, and Board-of-3 audit.
metadata:
  type: project
---

# GitHub Code Review Skill

This skill delivers rigorous, constructive, and actionable code reviews on pull requests and git diffs, including standard multi-dimensional analysis and the Board-of-3 audit methodology.

## Review Principles

1. **Focus on Substance over Pedantry**:
   - Prioritize correctness bugs, security vulnerabilities, race conditions, and architectural integrity over trivial whitespace or style that a linter handles.
2. **Be Concrete**:
   - Provide the specific failure scenario: under what inputs or conditions will the code break?
   - Offer the exact code snippet that fixes the issue.
3. **Respect Severity Levels**:
   - **Important**: Must-fix defects that block merge (crashes, logic bugs, security exposures, data loss).
   - **Nit**: Optional improvements, readability tweaks, non-blocking cleanups.
   - **Pre-existing**: Bugs found in surrounding code that were not introduced by this PR (file a separate issue/task).

## Board-of-3 Audit Methodology

For high-impact, infrastructure, or critical path changes, execute a three-auditor deliberation:
1. **Auditor 1: Security & Compliance**:
   - Checks: Injection vulnerabilities, secret scanner patterns, authorization boundaries, safe defaults.
2. **Auditor 2: Reliability & Concurrency**:
   - Checks: Deadlocks, timeout handling, race conditions, fail-closed design, crash loops, rollback readiness.
3. **Auditor 3: Architecture & Scope**:
   - Checks: Minimal diff integrity (no scope creep), DRY vs over-abstraction, backward compatibility, documentation parity.

Consolidate into an explicit verdict: `PASS`, `CONCERNS`, or `BLOCKER`.

## Standard Review Checklist

### 1. Correctness & Resilience
- [ ] Boundary conditions: empty arrays, 0, null, negative numbers, very large strings.
- [ ] Error propagation: are promises caught? Are error codes properly surfaced rather than silently swallowed?
- [ ] Concurrency: are there race conditions in async operations or state mutations?
- [ ] Resource cleanup: are database handles, file descriptors, and event listeners disposed?

### 2. Security & Secrets
- [ ] No hardcoded secrets, private keys, API tokens, or internal URLs.
- [ ] Input validation and sanitization for all external/user data.
- [ ] Authentication and authorization checks verified before state mutation.
- [ ] Safe dependency updates without known vulnerable versions.

### 3. Performance & Resource Efficiency
- [ ] No unindexed database queries or N+1 queries in loops.
- [ ] Proper caching strategies where repeated expensive calls occur.
- [ ] Memory footprint: no unbounded collection growth.

### 4. Tests & Observability
- [ ] Are new codepaths covered by unit or integration tests?
- [ ] Are edge cases and error handling branches tested?
- [ ] Meaningful logging or metrics on critical operations.
