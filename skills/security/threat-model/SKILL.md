---
name: threat-model
description: "Generate a STRIDE-based threat model from codebase and architecture analysis. Identifies assets, trust boundaries, data flows, threats, and mitigations. Language-agnostic."
license: MIT
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
  - AskUserQuestion
---

# Threat Model Generator

When asked to create a threat model, analyze the codebase and generate a structured threat model using the STRIDE methodology.

## Step 1: System Discovery

Map the system by examining the codebase:

1. **Entry points**: API routes, web handlers, CLI commands, message consumers, cron jobs
2. **Data stores**: Databases, caches, file systems, object storage, message queues
3. **External services**: Third-party APIs, OAuth providers, payment gateways, email services
4. **Authentication mechanisms**: How users prove identity (JWT, sessions, API keys, OAuth)
5. **User roles**: Admin, regular user, anonymous, service accounts, internal services
6. **Deployment**: Docker, k8s, cloud services (check Dockerfile, docker-compose, terraform, CI configs)

## Step 2: Identify Assets

List what an attacker would want:

- **Data assets**: User PII, credentials, payment data, business data, API keys, session tokens
- **System assets**: Server access, database access, admin panels, CI/CD pipelines
- **Reputation assets**: Brand trust, user confidence, compliance status

For each asset, note:
- Where it's stored
- How it's transmitted
- Who has access
- What protection exists

## Step 3: Map Trust Boundaries

Identify where trust levels change:

- **External → Application**: User browsers/clients to your API
- **Application → Database**: App server to data stores
- **Application → External Service**: Your app to third-party APIs
- **Public → Authenticated**: Before and after login
- **User → Admin**: Regular user context vs admin context
- **Service → Service**: Internal microservice communication
- **CI/CD → Production**: Build pipeline to deployment

## Step 4: STRIDE Analysis

For each trust boundary and data flow, analyze threats using STRIDE:

### S — Spoofing (Identity)
Can an attacker pretend to be someone else?
- Check: authentication strength, token forgery, session hijacking, credential stuffing
- Look for: weak JWT validation, missing auth middleware, shared API keys

### T — Tampering (Data Integrity)
Can an attacker modify data they shouldn't?
- Check: input validation, mass assignment, unsigned data, SQL injection
- Look for: unprotected API fields, missing CSRF tokens, unsigned cookies

### R — Repudiation (Accountability)
Can an attacker deny their actions?
- Check: audit logging, log integrity, transaction records
- Look for: missing security event logging, no request correlation IDs, deletable logs

### I — Information Disclosure
Can an attacker access data they shouldn't see?
- Check: error messages, verbose logging, debug endpoints, directory listing, IDOR
- Look for: stack traces in responses, PII in logs, exposed internal endpoints

### D — Denial of Service
Can an attacker make the system unavailable?
- Check: rate limiting, resource limits, input size limits, query complexity
- Look for: unbounded queries, missing rate limits, no pagination, no request timeouts

### E — Elevation of Privilege
Can an attacker gain higher access than intended?
- Check: authorization checks, role validation, admin endpoint protection
- Look for: missing authz middleware, IDOR, mass assignment of role fields, path traversal

## Step 5: Risk Rating

Rate each threat using:

| Factor | Low (1) | Medium (2) | High (3) |
|--------|---------|------------|----------|
| **Likelihood** | Requires internal access + deep knowledge | Requires some skill or access | Exploitable by anyone with basic tools |
| **Impact** | Minor inconvenience | Data loss or partial compromise | Full system compromise or major data breach |

**Risk = Likelihood x Impact**
- 1-2: Low risk
- 3-4: Medium risk
- 6: High risk
- 9: Critical risk

## Output Format

```
## Threat Model: [Application Name]

### 1. System Overview
Brief description of what the system does and its architecture.

### 2. Assets
| Asset | Location | Sensitivity | Current Protection |
|-------|----------|-------------|-------------------|
| User passwords | PostgreSQL users table | Critical | bcrypt hashed |
| ... | ... | ... | ... |

### 3. Trust Boundaries
- [Boundary 1]: Description and what crosses it
- [Boundary 2]: ...

### 4. Threats

#### [T-001] Threat title
- **STRIDE Category:** Spoofing / Tampering / Repudiation / Info Disclosure / DoS / Elevation
- **Trust Boundary:** Where this threat applies
- **Attack Scenario:** How an attacker would exploit this
- **Likelihood:** Low / Medium / High
- **Impact:** Low / Medium / High
- **Risk:** Low / Medium / High / Critical
- **Existing Mitigations:** What's already in place
- **Recommended Mitigations:** What should be added
- **Priority:** Fix now / Next sprint / Backlog

### 5. Data Flow Diagram (Text)
Describe the main data flows in text form:
  User → [HTTPS] → Load Balancer → [HTTP] → App Server → [TLS] → Database

### 6. Recommendations Summary
Ordered by priority:
1. Critical: ...
2. High: ...
3. Medium: ...
```

## Rules
- Base the model on actual code, not assumptions — read the codebase
- Focus on realistic threats, not theoretical edge cases
- If you can't determine something from the code, ask the user
- Keep the model proportional to the system size — a small CRUD app doesn't need 50 threats
- Call out what the project does well — good security decisions should be acknowledged
- Prioritize actionable mitigations over theoretical concerns
- If architecture diagrams or docs exist in the repo, reference them
