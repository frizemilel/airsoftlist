---
description: CI/CD and DevOps best practices for Airsoftlist.ru: GitHub Actions, deployment automation, infrastructure as code, and monitoring.
---
# CI/CD & DevOps Skill

## Purpose
Ensure Airsoftlist.ru has a robust CI/CD pipeline that enforces SEO, security, and performance standards before any code reaches production.

## GitHub Actions workflow template
Create `.github/workflows/ci.yml`:

```yaml
name: CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install dependencies
        run: npm ci
      - name: Lint
        run: npm run lint
      - name: Unit tests
        run: npm test
      - name: Build
        run: npm run build
        env:
          NEXT_PUBLIC_API_URL: ${{ secrets.NEXT_PUBLIC_API_URL }}
          DATABASE_URL: ${{ secrets.DATABASE_URL }}

  seo-check:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      - name: Install dependencies
        run: npm ci
      - name: Build site
        run: npm run build
      - name: SEO audit
        run: opencode seo-check
        continue-on-error: false

  security-scan:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      - name: Install dependencies
        run: npm ci
      - name: Security audit
        run: opencode security-scan
        continue-on-error: false

  perf-baseline:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      - name: Build site
        run: npm run build
      - name: Performance baseline
        run: opencode perf-baseline
        continue-on-error: false

  deploy-preview:
    runs-on: ubuntu-latest
    needs: [test, seo-check, security-scan, perf-baseline]
    if: github.event_name == 'pull_request'
    steps:
      - uses: actions/checkout@v4
      - name: Deploy preview
        run: vercel --scope=${{ secrets.VERCEL_TEAM }} --token=${{ secrets.VERCEL_TOKEN }}
        env:
          VERCEL_TOKEN: ${{ secrets.VERCEL_TOKEN }}

  deploy-production:
    runs-on: ubuntu-latest
    needs: [test, seo-check, security-scan, perf-baseline]
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - name: Deploy production
        run: vercel --scope=${{ secrets.VERCEL_TEAM }} --token=${{ secrets.VERCEL_TOKEN }} --prod
        env:
          VERCEL_TOKEN: ${{ secrets.VERCEL_TOKEN }}
      - name: Verify deployment
        run: opencode deploy-check
```

## Environment variables
Store all secrets in GitHub Secrets (Settings → Secrets → Actions):
- `DATABASE_URL`
- `NEXT_PUBLIC_API_URL`
- `NEXT_PUBLIC_GA_ID`
- `VERCEL_TOKEN`
- `VERCEL_TEAM`
- `GITHUB_TOKEN` (auto-provided)
- Any API keys, OAuth credentials, etc.

## Branch protection
On main branch:
- Require pull request reviews (at least 1)
- Require status checks to pass before merging
- Dismiss stale approvals when new commits are pushed
- Restrict pushes to administrators
- Require linear history (optional)

## Deployment targets
- **Vercel** (recommended): automatic builds, preview deployments, global CDN.
- **Netlify**: similar to Vercel, with deploy previews.
- **AWS**: EC2 + RDS + S3 + CloudFront, with Terraform for IaC.

## Monitoring
- **Uptime**: UptimeRobot or similar.
- **Performance**: Google Search Console Core Web Vitals, Chrome UX Report.
- **Error tracking**: Sentry, LogRocket.
- **SEO**: Google Search Console, Ahrefs/SEMrush.
- **Security**: Dependabot, Snyk, periodic `npm audit`.

## When to trigger
Use when: setting up CI/CD, deploying to production, configuring GitHub Actions, adding environment variables, setting up monitoring, troubleshooting deployment issues.