---
description: Set up CI/CD pipeline for the project (GitHub Actions).
agent: devops
---
# CI Setup Command

Run this to configure CI/CD for Airsoftlist.ru.

## Usage
```bash
opencode ci-setup
```

## What it does
1. Creates `.github/workflows/ci.yml` with the complete pipeline:
   - Install dependencies
   - Lint + unit tests
   - Production build
   - `seo-check` (fail on critical issues)
   - `security-scan` (fail on high/critical)
   - `perf-baseline` (fail if targets not met)
   - Deploy preview environment for PRs
   - Deploy production on merge to main
   - Verify production deployment
2. Creates `.env.example` with required environment variables.
3. Creates `.github/PULL_REQUEST_TEMPLATE.md` with SEO/security checklist.
4. Creates `.gitignore` with proper exclusions.
5. Provides instructions for:
   - Enabling branch protection on main
   - Adding GitHub Secrets
   - Enabling Dependabot
   - Setting up Vercel/Netlify integration

## Output
- `.github/workflows/ci.yml`
- `.env.example`
- `.github/PULL_REQUEST_TEMPLATE.md`
- `.gitignore`
- Setup instructions in markdown