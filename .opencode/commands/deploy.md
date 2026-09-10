---
description: Deploy the current site to production (GitHub → CI/CD → Server).
agent: devops
---
# Deploy Command

Run this to deploy Airsoftlist.ru to production.

## Usage
```bash
opencode deploy
```

## What it does
1. Verifies all checks pass:
   - `seo-check` (sitemap, robots, meta tags, structured data)
   - `security-scan` (OWASP Top 10, secrets, dependencies)
   - `perf-baseline` (Core Web Vitals)
   - Unit tests + build
2. Pushes current branch to GitHub.
3. Triggers CI/CD pipeline (GitHub Actions).
4. Waits for pipeline to complete.
5. Runs `deploy-check` on the live site.
6. Reports deployment status and any issues found.

## Prerequisites
- GitHub repository configured with CI/CD workflow.
- All secrets stored in GitHub Secrets.
- Branch protection enabled on main.
- Vercel/Netlify integration configured.

## When to use
Run when ready to deploy to production. Always run after all checks pass.