---
name: security-guidelines
description: Security guidelines for Airsoftlist.ru: OWASP Top 10, input validation, secrets handling, authentication, security headers, and dependency management.
---
# Security Guidelines Skill

## Purpose
Ensure every line of code and configuration for Airsoftlist.ru follows security best practices and avoids OWASP Top 10 vulnerabilities.

## Input validation
- Every user-facing input must be validated with a **zod** schema before processing.
- Reject unknown fields, enforce types, limit length.
- Server-side validation is mandatory — never trust client-side only.

## Authentication & sessions
- Use **NextAuth.js**, **Clerk**, or **Supabase Auth** — never roll your own.
- Verify session on every protected route via middleware.
- Roles/permissions checked server-side on every request.
- Session timeout configured (e.g., 30 days max, re-auth on sensitive actions).
- Password reset tokens: single-use, time-limited (e.g., 24h).

## SQL & database safety
- Use ORM parameterized queries **only** — never string-build SQL.
- Apply least-privilege DB user (no SUPERUSER, no DDL from app).
- Transactions for multi-step writes.
- Soft-delete with `deletedAt` flag (never hard-delete user data).

## Output & XSS prevention
- React escapes by default — do **not** use `dangerouslySetInnerHTML` unless output is first sanitized with **DOMPurify**.
- Set `X-Content-Type-Options: nosniff` on all responses.
- Set `Content-Security-Policy` header (see headers config below).

## Secrets & environment
- **Never** hardcode API keys, passwords, or tokens in source.
- Store all secrets in `.env` (git-ignored) and reference via `process.env.VAR`.
- Add `.env.example` with placeholder values.
- Never log secrets, tokens, or PII to any output (logs, error reports, sentry).

## Security headers (next.config.js)
```js
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'X-Frame-Options', value: 'DENY' },
          { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
          {
            key: 'Content-Security-Policy',
            value: "default-src 'self'; script-src 'self' 'unsafe-inline' https://*.google.com; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; connect-src 'self'; frame-src 'none';",
          },
          { key: 'Strict-Transport-Security', value: 'max-age=63072000; includeSubDomains; preload' },
        ],
      },
    ]
  },
}
```

## Dependencies
- Run `npm audit` before merging any PR.
- Pin versions in `package-lock.json` (committed).
- Avoid deprecated/unmaintained packages.
- If using `next-compose-plugins` or similar, audit those too.

## File uploads
- Validate MIME type **and** magic bytes.
- Enforce size limit (e.g., max 5MB per file).
- Store in object storage (S3/R2) outside web root.
- Use signed URLs for access; never expose internal storage paths.
- Optionally scan for malware.

## Secrets scan (pre-commit / CI)
- Grep codebase for patterns: `apiKey`, `password`, `secret`, `token`, `.env`.
- Exclude `.env.example` and test fixture files.
- Fail CI if any match found in `.ts`, `.tsx`, `.js`, `.json` files.

## Severity map
- **Critical**: Auth bypass, SQL injection, RCE, data exposure
- **High**: XSS, IDOR, SSRF, sensitive data in URLs
- **Medium**: Missing rate limit, weak crypto, missing CSP fallback
- **Low**: Info disclosure, deprecated patterns, missing HSTS

## When to trigger
Use for: pre-merge code review, security audit, dependency update, adding new file upload feature, adding new auth flow, before launch.