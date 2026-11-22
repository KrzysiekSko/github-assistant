---
name: github-sanitizer
description: Preflight validation, secret scanner heuristic sanitization (Gitleaks), and cross-platform git hygiene.
metadata:
  type: project
---

# GitHub Sanitizer & Preflight Skill

This skill guides pre-commit preflight scanning, Gitleaks false-positive prevention, cross-platform parity checks, and automated secret recovery in pull requests.

## Core Capabilities

### 1. Secret Scanner / Gitleaks Heuristic Sanitization
When authoring documentation, tutorials, runbooks, or test mocks, scanners like Gitleaks trigger on realistic pattern heuristics:
- `generic-api-key`: Patterns like `sk-<hex-or-base64>` (e.g. mock OpenAI or API tokens).
- `curl-auth-header`: Patterns matching `curl -H "Authorization: Bearer <token>"`.
- `base64-entropy`: Long Base64 strings or encrypted signatures (e.g. DPAPI `__sealed__:v1:AQAAAN...`).

#### Safe Placeholder Standard
Always sanitize examples before committing:
| Unsafe Heuristic Pattern | Sanitized Neutral Replacement |
| :--- | :--- |
| `sk-c189df04fe78eff8-4ee759` | `<API_KEY_PLACEHOLDER>` or `<TOKEN>` |
| `curl -H "Authorization: Bearer ***"` | `curl "http://api.local/v1" -H "X-API-Key: <TOKEN>"` |
| `__sealed__:v1:AQAAANCMnd8BF...` | `__sealed__:v1:<SEALED_DPAPI_BASE64>` |

#### Pre-commit Preflight Scan
```bash
git diff --staged | grep -E "sk-[a-zA-Z0-9_\-]{8,}|Authorization:\s*Bearer|__sealed__:[a-zA-Z0-9+/=]{20,}"
# Expected output: Empty (clean)
```

### 2. Cross-Platform Parity Guard
Ensure parity between Windows developpers and Linux GitHub Actions runners:
1. **Line Endings:**
   - Shell scripts (`*.sh`), GitHub Actions workflows (`.github/workflows/*.yml`), and Dockerfiles must use LF (`\n`), not CRLF (`\r\n`).
   - Run verification:
     ```bash
     file scripts/*.sh .github/workflows/*.yml | grep CRLF
     ```
2. **Absolute Path Guard:**
   - Diff must never contain machine-specific absolute paths:
     ```bash
     git diff --staged | grep -E "C:\\\\Users\\\\[a-zA-Z0-9_-]+|/home/[a-zA-Z0-9_-]+"
     ```
   - Replace with dynamic environment variables (`%USERPROFILE%`, `$HOME`, `%LOCALAPPDATA%`).

### 3. CI Gitleaks Failure Recovery Runbook
If a PR CI fails with `Gitleaks secret scan fail`:
1. **Locate finding:** `gh run view <run_id> --log-failed` to identify file and line.
2. **Sanitize:** Replace offending tokens with neutral placeholders.
3. **Amend:** Do NOT create a second "fix" commit (Gitleaks scans the whole commit history). Run:
   ```bash
   git add <sanitized-files>
   git commit --amend -m "docs: sanitize mock credentials against secret scanner heuristics"
   ```
4. **Force Push:** `git push --force origin <feature-branch>`.
5. **Verify:** Check status with `gh pr checks`.
