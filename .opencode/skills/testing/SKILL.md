---
name: testing
description: Testing and QA practices for Airsoftlist.ru: unit tests, integration tests, E2E tests, accessibility testing, performance budgets, and regression prevention.
---
# Testing & QA Skill

## Purpose
Ensure Airsoftlist.ru is thoroughly tested at every level — unit, integration, E2E, accessibility, performance — to prevent regressions and maintain reliability.

## Unit testing (Vitest)
- Test all utility functions, custom hooks, and components.
- Use `describe`/`it`/`expect` pattern.
- Test edge cases: empty arrays, null values, undefined, errors.
- Mock external dependencies (API calls, database).
- Target: **≥ 80%** code coverage.
- Example:
  ```ts
  describe('formatPrice', () => {
    it('formats rubles correctly', () => {
      expect(formatPrice(1000)).toBe('1 000 ₽');
    });
    it('returns empty string for 0', () => {
      expect(formatPrice(0)).toBe('');
    });
  });
  ```

## Integration testing
- Test API routes with Supertest.
- Test database operations with test database (separate from prod).
- Test auth flows: login, registration, logout, password reset.
- Test payment/webhook integration (Stripe test mode).
- Target: **100%** coverage on critical paths.

## E2E testing (Playwright)
- Test critical user journeys:
  1. Home page loads and displays listings.
  2. Category filtering works.
  3. Search returns correct results.
  4. Product page renders with all data.
  5. Add to cart / contact form submission.
  6. User registration and login.
  7. Admin dashboard access.
- Cross-browser: Chrome, Firefox, Safari.
- Mobile responsive: viewport 375px, 768px, 1440px.
- Accessibility: `axe-core` on every page.
- Visual regression: screenshot comparisons for critical pages.

## Performance testing (Lighthouse CI)
- Set budgets in `lighthouserc.js`:
  ```js
  module.exports = {
    ci: {
      collect: { url: ['http://localhost:3000'] },
      assert: {
        assertions: {
          'categories:performance': ['error', { minScore: 0.9 }],
          'largest-contentful-paint': ['error', { maxNumericValue: 2500 }],
          'cumulative-layout-shift': ['error', { maxNumericValue: 0.1 }],
          'interaction-to-next-paint': ['error', { maxNumericValue: 200 }],
          'total-blocking-time': ['error', { maxNumericValue: 200 }],
          'first-contentful-paint': ['error', { maxNumericValue: 1800 }],
        },
      },
    },
  };
  ```
- Bundle size limits:
  - Initial JS: max 250KB
  - CSS: max 50KB
  - Largest image: max 2000px

## Accessibility testing
- Run `axe-core` on every E2E test page.
- Keyboard navigation test: tab through all elements, verify focus indicators.
- Color contrast: ≥ 4.5:1 for normal text, ≥ 3:1 for large text.
- Alt text on all images.
- ARIA labels on interactive elements.

## Regression prevention
- Full test suite runs on every PR in CI.
- Any test failure blocks merge.
- Maintain a `test-baseline` for performance thresholds.
- Never skip or disable tests.
- After release, monitor with Sentry/RUM for regressions.

## Test file conventions
- `src/**/*.test.ts` or `src/**/*.spec.ts` for unit/integration.
- `e2e/**/*.spec.ts` for E2E tests.
- `tests/fixtures/` for test data.
- `tests/helpers/` for utility functions.

## Commands
- `npm test` — run all unit/integration tests.
- `npm test:coverage` — run tests with coverage report.
- `npm run test:e2e` — run E2E tests (Playwright).
- `npm run test:accessibility` — run accessibility audit.
- `npm run test:performance` — run Lighthouse CI.

## When to trigger
Use when: writing new features, fixing bugs, adding API endpoints, making UI changes, before merge, before launch, and after any regression is reported.