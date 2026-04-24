# NeuroForge Performance Checklist

Grounded in Google's Core Web Vitals and Roadmap standards.

---

## 📊 Core Web Vitals (The Big Three)

### 1. Largest Contentful Paint (LCP) - Loading Performance
- **Target:** ≤ 2.5 seconds
- **Check:** Large images, hero sections, video backgrounds.
- **Fixes:** Compression, lazy loading, WebP format, CDN usage.

### 2. Interaction to Next Paint (INP) - Responsiveness
- **Target:** ≤ 200 milliseconds
- **Check:** Delays after clicking buttons, opening menus, or typing.
- **Fixes:** Optimizing JS execution, avoiding main thread blocking.

### 3. Cumulative Layout Shift (CLS) - Visual Stability
- **Target:** ≤ 0.1
- **Check:** Elements jumping around during load (especially ads or late-loading images).
- **Fixes:** Define width/height for images, reserve space for dynamic content.

---

## ⚡ Additional Metrics

- **First Contentful Paint (FCP):** Time until the first piece of content is visible.
- **Time to First Byte (TTFB):** Server response speed.
- **Total Blocking Time (TBT):** How long the page is "frozen" during load.

---

## 🛠️ Performance Audit Steps

1. **Lighthouse Run:** Generate a baseline report.
2. **Network Audit:** Check for heavy assets (>500KB) and uncompressed files.
3. **Doherty Threshold Check:** Do interactions feel instant (<400ms)?
4. **Mobile Performance:** Test on 4G/3G throttling in DevTools.
5. **Caching:** Check if assets have appropriate `Cache-Control` headers.

---

## 🚩 Performance Red Flags

- **Unoptimized Images:** PNGs/JPEGs where WebP could be used.
- **Render-Blocking CSS/JS:** Massive files in the `<head>`.
- **Memory Leaks:** Increasing memory usage during long sessions.
- **Excessive DOM Size:** Too many elements on one page (Hick's Law violation).

---

## 🔧 Executable Performance Test Patterns

### Core Web Vitals — LCP

```ts
import { test, expect } from '@playwright/test';

test('PERF-001 — LCP under 2.5s', async ({ page }) => {
  await page.goto('/');

  const lcp = await page.evaluate(() => {
    return new Promise<number>((resolve) => {
      new PerformanceObserver((list) => {
        const entries = list.getEntries();
        resolve(entries[entries.length - 1].startTime);
      }).observe({ type: 'largest-contentful-paint', buffered: true });
      setTimeout(() => resolve(Infinity), 10000);
    });
  });

  expect(lcp).toBeLessThan(2500);
});
```

### Core Web Vitals — CLS

```ts
test('PERF-002 — CLS under 0.1', async ({ page }) => {
  await page.goto('/');

  const cls = await page.evaluate(() => {
    return new Promise<number>((resolve) => {
      let clsValue = 0;
      new PerformanceObserver((list) => {
        for (const entry of list.getEntries() as any[]) {
          if (!entry.hadRecentInput) clsValue += entry.value;
        }
      }).observe({ type: 'layout-shift', buffered: true });
      setTimeout(() => resolve(clsValue), 5000);
    });
  });

  expect(cls).toBeLessThan(0.1);
});
```

### Page Weight Check

```ts
test('PERF-003 — Total page weight under 2MB', async ({ page }) => {
  let totalBytes = 0;
  page.on('response', (response) => {
    const len = parseInt(response.headers()['content-length'] || '0');
    totalBytes += len;
  });

  await page.goto('/');
  await page.waitForLoadState('networkidle');

  expect(totalBytes).toBeLessThan(2 * 1024 * 1024);
});
```

### Doherty Threshold — Interaction Speed

```ts
test('PERF-004 — Interaction responds within 400ms', async ({ page }) => {
  await page.goto('/');
  const button = page.getByRole('button').first();

  if (await button.isVisible()) {
    const start = Date.now();
    await button.click();
    // Wait for any visible feedback (new content, loading state, navigation)
    await page.waitForLoadState('domcontentloaded');
    const duration = Date.now() - start;

    expect(duration).toBeLessThan(400);
  }
});
```

### Navigation Timing

```ts
test('PERF-005 — Page load completes under 3s', async ({ page }) => {
  await page.goto('/');
  const timing = await page.evaluate(() => {
    const nav = performance.getEntriesByType('navigation')[0] as PerformanceNavigationTiming;
    return {
      domContentLoaded: nav.domContentLoadedEventEnd - nav.startTime,
      loadComplete: nav.loadEventEnd - nav.startTime,
      ttfb: nav.responseStart - nav.requestStart,
    };
  });

  expect(timing.domContentLoaded).toBeLessThan(3000);
  expect(timing.ttfb).toBeLessThan(600);
});
```
