---
name: github-review
description: Comprehensive code review across correctness, security, performance, and architecture with actionable inline suggestions.
metadata:
  type: project
---

# GitHub Code Review Skill

This skill delivers rigorous, constructive, and actionable code reviews on pull requests and git diffs.

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

## Review Checklist

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
