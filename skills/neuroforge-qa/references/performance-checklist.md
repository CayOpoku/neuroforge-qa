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
