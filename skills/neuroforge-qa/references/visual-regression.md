# Visual Regression Testing

Use Playwright's built-in `toHaveScreenshot()` for visual regression. No third-party tools needed.

---

## Full Page Screenshots

```ts
import { test, expect } from '@playwright/test';

test.describe('Visual Regression', () => {
  test('VR-001 — Home page matches baseline', async ({ page }) => {
    await page.goto('/');
    await page.waitForLoadState('networkidle');
    await expect(page).toHaveScreenshot('home-page.png', {
      maxDiffPixelRatio: 0.01,
      fullPage: true,
    });
  });

  test('VR-002 — Dashboard matches baseline', async ({ page }) => {
    await page.goto('/dashboard');
    await expect(page).toHaveScreenshot('dashboard.png', { fullPage: true });
  });
});
```

---

## Responsive Viewport Matrix

```ts
const viewports = [
  { name: 'mobile', width: 320, height: 568 },
  { name: 'tablet', width: 768, height: 1024 },
  { name: 'desktop', width: 1440, height: 900 },
];

for (const vp of viewports) {
  test(`VR-010 — Page renders at ${vp.name}`, async ({ page }) => {
    await page.setViewportSize({ width: vp.width, height: vp.height });
    await page.goto('/');
    await expect(page).toHaveScreenshot(`home-${vp.name}.png`);
  });
}
```

---

## Dark Mode / Theme Variants

```ts
test('VR-020 — Dark mode renders correctly', async ({ page }) => {
  await page.goto('/');
  await page.emulateMedia({ colorScheme: 'dark' });
  await expect(page).toHaveScreenshot('home-dark.png', { fullPage: true });
});

test('VR-021 — Light mode renders correctly', async ({ page }) => {
  await page.goto('/');
  await page.emulateMedia({ colorScheme: 'light' });
  await expect(page).toHaveScreenshot('home-light.png', { fullPage: true });
});
```

---

## Component-Level Screenshots

```ts
test('VR-030 — Navigation component', async ({ page }) => {
  await page.goto('/');
  const nav = page.getByRole('navigation');
  await expect(nav).toHaveScreenshot('nav-component.png');
});

test('VR-031 — Mobile menu open state', async ({ page }) => {
  await page.setViewportSize({ width: 375, height: 667 });
  await page.goto('/');
  await page.getByRole('button', { name: /menu/i }).click();
  await expect(page.getByRole('navigation')).toHaveScreenshot('mobile-menu-open.png');
});
```

---

## Masking Dynamic Content

```ts
test('VR-040 — Page with masked dynamic content', async ({ page }) => {
  await page.goto('/dashboard');
  await expect(page).toHaveScreenshot('dashboard-stable.png', {
    fullPage: true,
    mask: [
      page.locator('.timestamp'),     // Mask timestamps
      page.locator('.avatar'),        // Mask user avatars
      page.locator('[data-dynamic]'), // Mask any dynamic content
    ],
  });
});
```

---

## Updating Baselines

When visual changes are intentional, update baselines:
```bash
npx playwright test --update-snapshots
```

Store baselines in version control so the team can review visual changes in PRs.

---

## Playwright Config for Visual Regression

Add to `playwright.config.ts`:
```ts
expect: {
  toHaveScreenshot: {
    maxDiffPixelRatio: 0.01,    // Allow 1% pixel difference
    threshold: 0.2,              // Per-pixel color threshold
    animations: 'disabled',      // Disable CSS animations for stability
  },
},
```
