---
description: Verify production deployment: SEO, security, and performance checks on live site.
agent: devops
---
# Deploy Check Command

Run this to verify a production deployment.

## Usage
```bash
opencode deploy-check
```

## What it does
1. **SEO verification**:
   - Check `sitemap.xml` is accessible and up to date.
   - Check `robots.txt` is correct.
   - Verify canonical URLs point to correct domain.
   - Check key pages have meta tags and structured data.
   - Verify no broken links.
2. **Security verification**:
   - Check security headers (CSP, HSTS, X-Frame-Options, etc.).
   - Run `npm audit` on production build.
   - Verify no secrets leaked in source.
3. **Performance verification**:
   - Run `perf-baseline` on live site.
   - Check Core Web Vitals (LCP, CLS, INP).
   - Check caching headers on static assets.
4. **Uptime check**:
   - Verify site is responding (HTTP 200).
   - Check response time.
5. **Report**: Markdown summary of all checks with pass/fail status.

## When to use
Run after every production deployment, after major releases, and periodically (daily/weekly) for ongoing monitoring.