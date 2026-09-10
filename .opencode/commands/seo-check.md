---
description: Run a full SEO audit on the current site: indexability, metadata, structured data, and Core Web Vitals.
agent: seo-specialist
---
# SEO Check Command

Run this to audit a page or the whole site for SEO issues.

## Usage
```bash
opencode seo-check [URL or path]
```

Example:
```bash
opencode seo-check https://airsoftlist.ru/categories/guns
opencode seo-check
```

## What it does
1. Checks `robots.txt` and `sitemap.xml` availability.
2. For each page found:
   - Extracts `<title>`, `<meta name="description">`, canonical URL.
   - Verifies Open Graph and Twitter Card tags.
   - Checks heading hierarchy (`<h1>` count, order).
   - Verifies images have `alt` text.
   - Extracts JSON-LD structured data and validates against Google Rich Results Test.
   - Measures Core Web Vitals (via Lighthouse).
3. Reports issues in a markdown table: Issue | Severity | File/URL | Fix.
4. Provides a pre-launch SEO checklist sign-off.

## Output format
```markdown
### SEO Audit Summary
- Pages audited: N
- Critical issues: N
- Warnings: N
- Passed: N

| Issue | Severity | File | Fix |
|-------|----------|------|-----|
| Missing `<title>` on `/about` | Critical | `src/pages/about.tsx` | Add `<title>About Us</title>` |
| ... | ... | ... | ... |
```