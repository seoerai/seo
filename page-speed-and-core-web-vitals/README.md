# Page Speed & Core Web Vitals: LCP, INP & CLS Optimization

> **A first-principles engineering guide to Google Core Web Vitals (Largest Contentful Paint, Interaction to Next Paint, Cumulative Layout Shift) and front-end performance tuning.**

---

## 📌 Executive Summary

**Core Web Vitals (CWV)** are a set of standardized performance metrics defined by Google to measure real-world user experience (Field Data via Chrome UX Report / CrUX). Google uses Core Web Vitals as an explicit ranking factor. Optimizing CWV speeds up page rendering, improves conversion rates, and boosts organic search position.

```mermaid
flowchart LR
    A[User Clicks Link] --> B[TTFB: Time To First Byte < 0.8s]
    B --> C[LCP: Largest Contentful Paint < 2.5s]
    C --> D[INP: Interaction to Next Paint < 200ms]
    D --> E[CLS: Cumulative Layout Shift < 0.1]
```

---

## 1. Core Web Vitals Metrics & Target Thresholds

```text
┌───────────────────────────────────────────────────────────────────────────┐
│                     CORE WEB VITALS METRICS SCORECARD                     │
└───────────────────────────────────────────────────────────────────────────┘
   Metric   Full Name                       Good (Pass)      Needs Work      Poor
   LCP      Largest Contentful Paint        ≤ 2.5s           2.5s – 4.0s     > 4.0s
   INP      Interaction to Next Paint       ≤ 200ms          200ms – 500ms   > 500ms
   CLS      Cumulative Layout Shift         ≤ 0.10           0.10 – 0.25     > 0.25
```

---

## 2. Optimizing LCP (Largest Contentful Paint)

LCP measures the time it takes for the largest visual element (hero image, video poster, or large text block) to render on screen.

### 4 Key LCP Fixes
1. **Eliminate Render-Blocking Resources**: Defer or inline non-critical CSS/JS (`<script defer>`).
2. **Optimize & Preload Hero Image**:
   ```html
   <link rel="preload" fetchpriority="high" as="image" href="/hero.webp" type="image/webp">
   ```
3. **Use Modern Image Formats**: Serve WebP or AVIF formats compressed at 80% quality.
4. **Implement CDN Caching & Early Hints**: Use Cloudflare or Vercel Edge caching to deliver TTFB < 200ms.

---

## 3. Optimizing INP (Interaction to Next Paint)

INP replaced FID (First Input Delay) in March 2024 to measure overall page responsiveness during a user's entire visit.

### 3 Key INP Fixes
1. **Break Up Long Tasks (>50ms)**: Yield main-thread execution using `requestIdleCallback()` or `setTimeout()`.
2. **Minimize Main-Thread JS Overhead**: Reduce heavy client-side JavaScript bundle execution.
3. **Optimize Event Handlers**: Move non-UI calculations off the main thread into Web Workers.

---

## 4. Optimizing CLS (Cumulative Layout Shift)

CLS measures unexpected visual shifts in page layout during rendering (e.g., ads or images pushing text down suddenly).

### 3 Key CLS Fixes
1. **Set Explicit Width & Height on Images & Media**:
   ```html
   <img src="banner.webp" width="1200" height="630" alt="Banner" style="aspect-ratio: 1200 / 630;">
   ```
2. **Reserve Ad & iFrame Container Spaces**: Use CSS `min-height` on dynamic containers.
3. **Use `font-display: swap` or `optional`**: Prevent Flash of Unstyled Text (FOUT/FOIT) layout shifts.

---

## 5. Summary

Core Web Vitals optimization is pure engineering. By preloading hero images for LCP < 2.5s, breaking up long JavaScript tasks for INP < 200ms, and reserving media container aspect ratios for CLS < 0.1, you achieve 100 PageSpeed scores and pass Google's ranking thresholds.
