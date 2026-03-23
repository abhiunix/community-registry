---
name: security-audit
description: "Run a comprehensive security audit against a codebase. Covers OWASP Top 10, secrets exposure, dependency vulnerabilities, misconfigurations, and insecure patterns. Language-agnostic."
license: MIT
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
  - AskUserQuestion
---

# Security Audit

When asked to run a security audit, follow this systematic process. Adapt checks to the languages and frameworks detected in the project.

## Step 1: Reconnaissance

Identify the tech stack before diving in.

1. Check for package manifests to identify languages and frameworks:
   - `package.json`, `yarn.lock` (Node.js)
   - `requirements.txt`, `Pipfile`, `pyproject.toml` (Python)
   - `go.mod` (Go)
   - `Cargo.toml` (Rust)
   - `Gemfile` (Ruby)
   - `pom.xml`, `build.gradle` (Java/Kotlin)
   - `*.csproj` (C#/.NET)
   - `composer.json` (PHP)
2. Identify entry points: API routes, web handlers, CLI entry points
3. Map external integrations: databases, caches, message queues, third-party APIs
4. Check for deployment configs: Dockerfile, docker-compose, k8s manifests, terraform, CI/CD configs

## Step 2: Secrets & Credentials Scan

Search for hardcoded secrets. These are critical findings.

```
# Patterns to grep for (case-insensitive):
- API_KEY, APIKEY, api_key
- SECRET, SECRET_KEY, CLIENT_SECRET
- PASSWORD, PASSWD, DB_PASS
- TOKEN, ACCESS_TOKEN, AUTH_TOKEN, BEARER
- PRIVATE_KEY, private_key, -----BEGIN
- AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY
- JDBC connection strings with credentials
- mongodb://, postgres://, mysql:// with embedded passwords
- Base64-encoded strings that are suspiciously long (>40 chars)
```

Check:
- Source code files
- Configuration files (`.env` files that are NOT gitignored, `config.yaml`, etc.)
- `.gitignore` — verify it excludes `.env`, `*.pem`, `*.key`
- Git history: `git log --diff-filter=D -- "*.env" "*.key" "*.pem"` (deleted but in history)
- CI/CD configs for inline secrets
- Docker files for `ARG` or `ENV` with secret values

## Step 3: Injection Vulnerabilities

### SQL Injection
Search for string concatenation or interpolation in database queries:
- Template literals or f-strings in SQL context
- String concatenation with `+` near SQL keywords (SELECT, INSERT, UPDATE, DELETE, WHERE)
- Raw query methods without parameterization

### Command Injection
Search for:
- `system()`, `exec()`, `popen()`, `spawn()`, `os.system()` with variable arguments
- `shell=True` (Python), backtick execution (Ruby), `child_process.exec` (Node.js)
- `eval()`, `Function()`, `exec()` with user-derived data

### XSS
Search for:
- `innerHTML`, `outerHTML`, `document.write()`, `insertAdjacentHTML()`
- `dangerouslySetInnerHTML` (React), `v-html` (Vue), `[innerHTML]` (Angular)
- Template expressions that disable auto-escaping: `| safe` (Jinja2), `<%== %>` (ERB), `{{{ }}}` (Handlebars)

### Path Traversal
Search for:
- File operations using user input without path canonicalization
- `../` not being filtered in file path logic
- `os.path.join()` or `path.join()` with user-controlled segments without containment check

## Step 4: Authentication & Session Management

Check:
- Password hashing: verify bcrypt/argon2/scrypt is used, not MD5/SHA
- Session configuration: HttpOnly, Secure, SameSite flags on cookies
- JWT validation: algorithm verification, expiration check, secret strength
- Login endpoints: rate limiting, account lockout, generic error messages
- Session invalidation on logout
- Password reset: token expiration, single-use, delivered over secure channel

## Step 5: Authorization & Access Control

Check:
- Every API endpoint has authorization middleware/checks
- Resource access verifies ownership (IDOR checks)
- Admin endpoints are protected and not accessible via URL guessing
- Mass assignment protection: explicit field allowlists on update operations
- File upload/download authorization
- Multi-tenant data isolation

## Step 6: Cryptography Review

Check:
- No use of MD5, SHA-1, DES, RC4, ECB mode for security purposes
- Random values generated with cryptographic RNG (not Math.random or rand())
- TLS version >= 1.2 in any custom TLS configuration
- Certificate validation not disabled
- Encryption keys not hardcoded

## Step 7: Dependency Audit

Run the appropriate vulnerability scanner:
```bash
# Node.js
npm audit --json 2>/dev/null || true

# Python
pip audit 2>/dev/null || true

# Ruby
bundle audit check 2>/dev/null || true

# Go
govulncheck ./... 2>/dev/null || true

# Rust
cargo audit 2>/dev/null || true
```

Also check:
- Outdated packages with known CVEs
- Packages with very few maintainers or no recent updates
- Lock file presence and integrity

## Step 8: Configuration & Infrastructure

Check:
- Debug mode disabled in production configs
- CORS configuration: no wildcard origins on authenticated endpoints
- Security headers: CSP, HSTS, X-Content-Type-Options, X-Frame-Options
- Database: no default credentials, no unnecessary ports exposed
- Docker: not running as root, no unnecessary capabilities
- Environment variables: secrets not in docker-compose.yml or CI configs

## Step 9: Error Handling & Logging

Check:
- No stack traces exposed to users in production
- Generic error messages for auth failures
- Sensitive data not logged (passwords, tokens, PII)
- Security events are logged (auth failures, access control violations)

## Output Format

Structure your audit report as:

```
## Security Audit Report

### Summary
- **Risk Level:** Critical / High / Medium / Low
- **Tech Stack:** [detected languages, frameworks, databases]
- **Findings:** X critical, Y high, Z medium, W low

### Critical Findings
#### [CRIT-1] Finding title
- **Category:** Injection / Auth / Crypto / Config / etc.
- **Location:** file:line
- **Description:** What the vulnerability is
- **Impact:** What an attacker could do
- **Remediation:** How to fix it
- **Reference:** OWASP/CWE link if applicable

### High Findings
...

### Medium Findings
...

### Low Findings / Informational
...

### Positive Observations
- Things the project does well
```

## Rules
- Always verify findings before reporting — read the actual code, don't guess from file names
- Rate severity honestly: not everything is critical
- Provide specific file paths and line numbers
- Include concrete remediation steps, not just "fix this"
- Note when something looks suspicious but you can't confirm (flag as "needs manual review")
- If the project is small, the audit may be quick — that's fine, don't inflate findings
- Prioritize findings by exploitability and impact, not by count
