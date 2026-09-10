---
name: seo-best-practices
description: SEO best practices for Next.js airsoftlist.ru: meta tags, structured data, crawlability, Core Web Vitals, and sitemap/robots management.
---
# SEO Best Practices Skill

## Purpose
Provide guidance and audit checklist for ensuring Airsoftlist.ru pages are crawlable, indexable, and optimized in Google and other search engines.

## Meta tags per route
Every page must have, in `<head>`:
1. `<title>` — unique, ≤ 60 chars, primary keyword first.
2. `<meta name="description" content="...">` — unique, ≤ 160 chars, compelling call-to-action.
3. `<link rel="canonical" href="...">` — absolute URL, self-referencing (or canonical to the correct variant).
4. Open Graph: `og:title`, `og:description`, `og:image`, `og:type`, `og:locale`.
5. Twitter Card: `twitter:card`, `twitter:title`, `twitter:description`, `twitter:image`.

## Structured data (JSON-LD)
Add where relevant:
- **Listing pages** → `Product` schema: `name`, `description`, `price`, `availability`, `image`, `brand`, `sku`, `review` (aggregateRating).
- **Category pages** → `BreadcrumbList` + `ItemList` schemas.
- **Search results page** → `BreadcrumbList` schema (or noindex on query variants).
- **FAQ/contact pages** → `FAQPage` schema.

## Images
- Always use `next/image`.
- Always provide descriptive `alt` text.
- Specify `width` and `height` to prevent layout shift.
- Prefer AVIF/WebP with automatic fallback (`type` attribute).

## Robots & sitemap
- `robots.txt`: allow all public pages; disallow `/account/*`, `/admin/*`, `/search?q=`.
- `sitemap.xml`: auto-generated via `next-sitemap` or Next.js `generateSitemap`; submit to GSC.
- All public listing and category pages must be in the sitemap.

## Core Web Vitals SEO link
- LCP < 2.5s — images optimized, server fast, critical CSS inlined.
- CLS < 0.1 — images have width/height, fonts via `next/font`.
- INP < 200ms — no heavy client hydration on initial load, `use client` only when needed.

## Tools
- Google Search Console (index coverage, Core Web Vitals)
- Google Rich Results Test
- PageSpeed Insights / Lighthouse
- Screaming Frog / Sitebulb (crawl)
- `next build` + sitemap verification

## When to trigger
Use when: designing a new route, adding a new page type, before launch, after any meta/title change, or when SEO traffic drops.