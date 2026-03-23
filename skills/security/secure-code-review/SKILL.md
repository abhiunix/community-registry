---
name: secure-code-review
description: "Security-focused code review for staged changes or specified files. Checks for injection, auth flaws, crypto misuse, data exposure, and insecure patterns. Works with any language."
license: MIT
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
  - AskUserQuestion
---

# Secure Code Review

When asked to do a security review, analyze the specified code (or staged git changes) for security vulnerabilities. This is not a general code review — focus exclusively on security.

## Step 1: Gather the Code

If reviewing staged changes:
```bash
git diff --cached --stat    # see which files changed
git diff --cached           # full diff
```

If reviewing specific files or directories: read them directly.

If nothing specified, ask the user what to review.

## Step 2: Identify the Context

Before reviewing, understand:
- What language(s) and framework(s) are in use?
- Is this user-facing code, internal tooling, or infrastructure?
- Does it handle user input, auth, payments, or file operations?
- What's the data sensitivity? (PII, financial, healthcare, etc.)

This context determines what to focus on.

## Step 3: Security Checks

Run through these checks for every piece of code reviewed. Skip categories that don't apply (e.g., skip SQL injection for code that doesn't touch databases).

### A. Injection (OWASP A03)

**SQL Injection**
- String concatenation or interpolation in SQL queries
- Raw queries without parameterization
- ORM methods that accept raw SQL (`.extra()`, `.raw()`, `$queryRawUnsafe`)
- Dynamic table/column names from user input

**Command Injection**
- User input passed to shell commands (`exec`, `system`, `popen`, backticks)
- `shell=True` with variable arguments
- `eval()`, `Function()`, `exec()` with any non-literal input

**XSS**
- `innerHTML`, `dangerouslySetInnerHTML`, `v-html` with user data
- Template auto-escaping disabled
- User input in `href`, `src`, `onclick`, or `style` attributes
- URL construction from user input without scheme validation

**Path Traversal**
- File operations with user-controlled path segments
- Missing path canonicalization or containment check
- Zip extraction without path validation (zip slip)

**Template Injection (SSTI)**
- User input rendered as part of a template string (server-side)
- Template engines invoked with user-controlled template content

### B. Authentication (OWASP A07)

- Weak password hashing (MD5, SHA-1, plain SHA-256 without bcrypt/argon2)
- Hardcoded credentials or default passwords
- Missing authentication on endpoints
- JWT: `alg: none` allowed, missing expiration validation, weak signing secret
- Session fixation: session ID not regenerated after login
- Timing attacks on token/password comparison (use constant-time compare)

### C. Authorization (OWASP A01)

- Missing authorization checks on state-changing operations
- IDOR: resource access without ownership verification
- Mass assignment: accepting user-controlled fields for role, admin, permissions
- Client-side authorization checks without server-side enforcement
- Privilege escalation paths: can a user modify their own role?

### D. Cryptography (OWASP A02)

- Insecure algorithms: MD5, SHA-1, DES, RC4, ECB mode
- Non-cryptographic RNG for security values: `Math.random()`, `random.random()`, `rand()`
- Hardcoded encryption keys or IVs
- IV/nonce reuse
- Missing HMAC verification before decryption
- Disabled certificate validation

### E. Data Exposure (OWASP A04)

- Sensitive data in logs (passwords, tokens, PII, credit cards)
- Stack traces or internal errors exposed to users
- Verbose error messages that reveal system internals
- API responses returning more data than needed (over-fetching)
- Sensitive data in URL query parameters (gets logged in access logs)
- Missing `Cache-Control: no-store` for sensitive responses

### F. Configuration

- Debug mode enabled in production configs
- CORS wildcard (`*`) on authenticated endpoints
- Missing security headers (CSP, HSTS, X-Frame-Options)
- Default credentials in config files
- Secrets in environment variable defaults or config templates
- `TODO`/`FIXME`/`HACK` comments around security-sensitive code

### G. Deserialization & Data Parsing

- Unsafe deserialization: `pickle.loads()`, `yaml.load()` (not `safe_load`), Java `ObjectInputStream`, PHP `unserialize()` with untrusted input
- XML parsing without disabling external entities (XXE)
- JSON parsing of very large payloads without size limits

### H. Race Conditions & Concurrency

- Time-of-check to time-of-use (TOCTOU) in file operations
- Double-spend or duplicate action possibilities without locking
- Non-atomic check-then-act patterns in auth or payment flows

## Step 4: Report Findings

Structure your review as:

```
## Security Review

### Summary
One-line summary. Overall risk: Low / Medium / High / Critical.

### Findings

#### [SEV: Critical/High/Medium/Low] Finding title
- **File:** path/to/file:line_number
- **Category:** Injection / Auth / Authz / Crypto / Data Exposure / Config
- **Issue:** What's wrong
- **Risk:** What an attacker could do with this
- **Fix:**
  ```
  // concrete code fix or approach
  ```

### No Issues Found In
- List areas you checked that looked good (so the user knows you checked them)
```

## Rules
- Only report actual security issues — not style, performance, or general code quality
- Verify every finding by reading the surrounding code for context — don't report based on grep matches alone
- A function called `escape()` might already be handling the sanitization you're worried about
- Rate severity honestly. Not everything is critical. A reflected XSS in an admin-only tool is different from one on a public login page.
- If something looks risky but you can't confirm it's exploitable, flag it as "needs manual review" rather than a confirmed vulnerability
- Provide concrete fixes, not just "sanitize this"
- If the code handles the vulnerability correctly, say so — it builds trust in the review
- Keep the review focused and actionable. 5 real findings are worth more than 30 theoretical ones.
