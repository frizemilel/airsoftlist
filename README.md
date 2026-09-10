# Airsoftlist.ru

Airsoft gear marketplace — fast, SEO-optimized, secure.

## Tech Stack
- Next.js 14+ (App Router), React, TypeScript
- Tailwind CSS
- PostgreSQL (Prisma)
- Vercel deployment
- GitHub Actions CI/CD

## Getting Started

### Prerequisites
- Node.js 20+
- npm 9+ or Yarn/Pnpm
- PostgreSQL database

### Setup
```bash
git clone https://github.com/airsoftlist/airsoftlist.git
cd airsoftlist
cp .env.example .env
# Edit .env with your values
npm install
npm run dev
```

### Available Scripts
```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run start        # Start production server
npm run lint         # Run linter
npm test             # Run unit & integration tests
npm test:coverage    # Run tests with coverage
npm run test:e2e     # Run E2E tests (Playwright)
npm run test:perf    # Run performance audit (Lighthouse CI)
```

### Commands
```bash
opencode ci-setup     # Set up GitHub Actions workflow
opencode deploy       # Deploy to production
opencode deploy-check # Verify production deployment
opencode test         # Run all tests with coverage
opencode test:e2e     # Run E2E tests
opencode seo-check    # SEO audit
opencode security-scan # Security audit
opencode perf-baseline # Performance baseline
```

## Project Structure
```
airsoftlist/
├── .github/workflows/ci.yml
├── .opencode/agents/      # AI agent definitions
├── .opencode/skills/      # Skills (SEO, security, testing, CI/CD)
├── .opencode/commands/    # Available commands
├── src/                   # Next.js App Router, components, lib
├── e2e/                   # E2E tests (Playwright)
├── tests/                 # Unit & integration tests
└── public/                # Static assets
```

## SEO & Security
- Every page has meta tags, structured data, semantic HTML
- Security headers enforced (CSP, HSTS, X-Frame-Options)
- Input validation with zod, parameterized queries only
- Rate limiting on all user-facing endpoints
- Performance budgets: LCP < 2.5s, CLS < 0.1, INP < 200ms

## License
MIT
