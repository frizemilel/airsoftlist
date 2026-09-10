---
description: Measure current Core Web Vitals and performance baseline.
agent: perf-engineer
---
# Performance Baseline Command

Run this to establish and track Core Web Vitals and overall performance metrics.

## Usage
```bash
opencode perf-baseline [URL or path]
```

Example:
```bash
opencode perf-baseline https://airsoftlist.ru
opencode perf-baseline
```

## What it does
1. Runs Lighthouse audit (Chrome DevTools Protocol) for:
   - LCP, CLS, INP, FCP, TBT, Speed Index, Total Blocking Time.
   - Mobile and desktop.
2. Reports current values alongside any previous baseline (from `.perf-baseline.json` if it exists).
3. Provides a prioritized optimization list with estimated impact.
4. Checks image optimization, code splitting, and caching headers.

## Output format
```json
{
  "url": "https://airsoftlist.ru",
  "lcp": "2.1s",
  "cls": "0.05",
  "inp": "150ms",
  "fcp": "1.4s",
  "tbt": "80ms",
  "speedIndex": "3.2s",
  "issues": [
    "Hero image not optimized — serve AVIF/WebP",
    "No width/height on below-the-fold images causes CLS",
    "Third-party script (analytics) blocks main thread"
  ],
  "recommendations": [
    { "priority": "high", "effort": "low", "impact": "high", "action": "Add next/image with formatting: [avif,webp]" },
    { "priority": "medium", "effort": "medium", "impact": "medium", "action": "Code-split the map component with next/dynamic" }
  ]
}
```

## When to use
Run before launch, after any major UI change, after adding new third-party scripts, when Core Web Vitals traffic drops in GSC.
```