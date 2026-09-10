# Pull Request Template

## Description
Brief description of what this PR does and why.

## Type of change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update
- [ ] Performance optimization
- [ ] Security fix

## SEO checklist
- [ ] No `noindex` on pages that should be indexed
- [ ] All meta tags present (title, description, canonical, OG, Twitter Card)
- [ ] Images have `alt` text
- [ ] Heading hierarchy is logical (h1 → h2 → h3, no skips)
- [ ] Structured data (JSON-LD) is correct and validated
- [ ] URLs are kebab-case and descriptive
- [ ] Sitemap is up to date

## Security checklist
- [ ] No hardcoded secrets or credentials
- [ ] Input validation with zod schemas
- [ ] No string-built SQL (parameterized queries only)
- [ ] Auth middleware on protected routes
- [ ] Rate limiting on user-facing endpoints
- [ ] Security headers set (CSP, HSTS, X-Frame-Options)
- [ ] Dependencies checked with `npm audit`
- [ ] File uploads validated (type, size, magic bytes)

## Performance checklist
- [ ] LCP < 2.5s
- [ ] CLS < 0.1
- [ ] INP < 200ms
- [ ] Images optimized (AVIF/WebP with fallbacks)
- [ ] No layout shift (explicit width/height on images)
- [ ] Bundle size within budget
- [ ] Code splitting on routes

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] E2E tests added/updated (if critical user journey changed)
- [ ] Accessibility audit passed (axe-core)
- [ ] Performance budget met

## How to test
1. `npm install && npm run build`
2. `npm test`
3. `opencode seo-check`
4. `opencode security-scan`
5. `opencode perf-baseline`

## Related issues
Closes #XXX
