---
description: Run a security audit: OWASP Top 10, secrets, dependency risks, and insecure patterns.
agent: security-auditor
---
# Security Scan Command

Run this to audit code and configuration for security vulnerabilities.

## Usage
```bash
opencode security-scan [path or URL]
```

Example:
```bash
opencode security-scan
opencode security-scan src/pages/listings
```

## What it does
1. **Secrets scan** — greps codebase for hardcoded API keys, passwords, tokens, `apiKey`, `secret`.
2. **Dependency scan** — runs `npm audit --json` and reports any high/critical vulnerabilities.
3. **OWASP Top 10 review** — checks for:
   - A01: Broken Access Control (auth middleware, role checks)
   - A03: Injection (parameterized queries, no string-built SQL)
   - A07: Identification/Auth failures (hardcoded creds, session timeouts)
   - A09: Security Logging (events logged, no PII in logs)
   - And others as applicable
4. **Headers check** — uses `curl -I` to verify security headers on the target URL.
5. **File upload validation** — checks for MIME type, size, storage location.
6. **Output**: markdown report with findings table and remediation priority.

## Output format
```markdown
### Security Audit Summary
- Critical findings: N
- High findings: N
- Medium findings: N
- Low findings: N

| ID | Severity | OWASP Category | File:Line | Description | Remediation |
|----|----------|---------------|-----------|-------------|-------------|
| ... | ... | ... | ... | ... | ... |
```

## When to use
Run before every merge, before launch, after adding new dependencies, after adding file upload functionality, after adding new auth flows.
```