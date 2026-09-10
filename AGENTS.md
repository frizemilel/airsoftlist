# AGENTS.md — Airsoftlist.ru

## Project
Airsoftlist.ru is an airsoft gear marketplace / listing site. Goals: fast, SEO-optimized, secure.

## Tech stack (intended)
- Next.js 14+ (App Router), React, TypeScript
- Tailwind CSS
- PostgreSQL (via Prisma or similar)
- Vercel (or equivalent) deployment
- GitHub (source control + CI/CD)

## Cross-cutting rules

### SEO (non-negotiable)
- Every dynamic route must have a `generateStaticParams` or a `generateSitemap` entry; never ship a page that is indexable but un-crawlable.
- Every page/component must have: `<title>`, `<meta name="description">`, canonical `<link rel="canonical">`, Open Graph tags, and Twitter card tags.
- Use semantic HTML: `<main>`, `<article>`, `<section>`, `<nav>`, `<h1>`..`<h6>` in order (no skipping levels).
- Add JSON-LD structured data (`Article`, `Product`, `BreadcrumbList`, `FAQPage`) where relevant.
- Images: always `alt`, `width`/`height` or explicit `sizes`, prefer Next/Image or `<img loading="lazy">`.
- URLs: kebab-case, stable, descriptive, no query params for content variants (use static paths or canonical).
- Internal linking: every page links to at least 2-3 related pages.
- Avoid client-only rendering for primary content; use SSR/SSG/ISR so bots see content.

### Security (non-negotiable)
- Never hardcode secrets; use environment variables only. Add `.env.example`.
- Validate and sanitize all user input (server-side). Use a schema validator (zod/io-ts).
- Escape output to prevent XSS. React escapes by default — do not use `dangerouslySetInnerHTML` without sanitization (use DOMPurify).
- Use parameterized queries / ORM — never string-built SQL.
- Authentication: use established libraries (NextAuth, Supabase Auth, Clerk). Never roll your own.
- Set security headers: CSP, X-Content-Type-Options, X-Frame-Options, Referrer-Policy, Strict-Transport-Security.
- Rate limit all user-facing endpoints.
- Dependencies: check for known vulnerabilities (`npm audit`, `snyk`) before merging.
- File uploads: validate type, size, scan for malware, store outside web root, use signed URLs.

### Performance
- Target: LCP < 2.5s, CLS < 0.1, INP < 200ms on 4G.
- Code-split routes; lazy-load non-critical JS.
- Compress images (AVIF/WebP with fallbacks).
- Use `next/font` for self-hosted fonts (no layout shift).
- Cache API responses and static assets.

### Accessibility
- WCAG 2.1 AA minimum: keyboard navigation, focus indicators, ARIA labels where needed, color contrast ≥ 4.5:1.

## DevOps & CI/CD
- GitHub is the source of truth. Protect main branch with required reviews and status checks.
- All secrets go in GitHub Secrets (Settings → Secrets → Actions). Never commit `.env` files.
- CI/CD pipeline must run: tests → lint → build → `seo-check` → `security-scan` → `perf-baseline` → deploy.
- Fail the build if any SEO, security, or performance check fails.
- Deploy preview environments for every PR. Deploy production only on merge to main.
- Use Vercel (recommended) or Netlify for deployment with global CDN.
- Monitor: uptime, Core Web Vitals (GSC), error tracking (Sentry), SEO performance (GSC).
- Use Dependabot for automated dependency updates.

## Testing & QA
- Every feature and bug fix must include tests (unit + integration + E2E).
- Critical user journeys must have E2E tests (home, search, filter, product page, auth, admin).
- Target ≥ 80% code coverage on new code; 100% on critical paths.
- Accessibility audit (axe-core) on every page in E2E tests.
- Performance budgets enforced in CI: LCP < 2.5s, CLS < 0.1, INP < 200ms, bundle size limits.
- Visual regression checks for critical pages.
- Any test failure blocks merge.
- Never skip or disable tests.
- Run `test:debug` when a test fails to find root cause and fix.

## Agent responsibilities
- **architect**: design before code; produce ADRs for major decisions.
- **designer**: UX/UI wireframes, mockups, design system, accessibility from design perspective.
- **frontend-dev**: implement UI + SEO markup; run `seo-check` after.
- **backend-dev**: implement API + data layer; run `security-scan` after.
- **seo-specialist**: audit, report, verify fixes.
- **security-auditor**: audit, report, verify fixes.
- **perf-engineer**: measure, optimize, verify.
- **devops**: GitHub setup, CI/CD pipeline, deployment, monitoring, infrastructure.
- **qa-engineer**: write tests, maintain test suite, verify coverage, debug failures, regression prevention.

## Workflow
1. `architect` designs the route map, data model, and SEO/security strategy.
2. `designer` creates wireframes, mockups, and design system for the UI.
3. `frontend-dev` and `backend-dev` implement in parallel.
4. `qa-engineer` writes tests alongside code (unit, integration, E2E).
5. `seo-specialist` and `security-auditor` review before merge.
6. CI runs: tests → `seo-check` → `security-scan` → `perf-baseline` → deploy preview.
7. On merge to main: deploy production + `deploy-check`.
8. `perf-engineer` measures and optimizes before launch.
9. `devops` monitors production and manages rollback.