# Automated Accessibility Testing

Generate executable accessibility tests alongside the manual WCAG audit.
Every `AX-XXX-audit.md` should have a companion `.spec.ts` file.

---

## axe-core with Playwright

The primary tool for automated accessibility testing. Install: `npm i -D @axe-core/playwright`

### Full Page Audit

```ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test.describe('Accessibility Audit', () => {
  test('AX-001 — Home page has no WCAG 2.2 AA violations', async ({ page }) => {
    await page.goto('/');
    const results = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa', 'wcag22aa'])
      .analyze();

    // Log violations for debugging
    if (results.violations.length > 0) {
      console.log('Violations:', JSON.stringify(results.violations, null, 2));
    }

    expect(results.violations).toEqual([]);
  });

  test('AX-002 — Login page has no violations', async ({ page }) => {
    await page.goto('/login');
    const results = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa'])
      .exclude('.third-party-widget') // Exclude elements you don't control
      .analyze();

    expect(results.violations).toEqual([]);
  });
});
```

### Specific Rule Checks

```ts
test('AX-003 — All images have alt text', async ({ page }) => {
  await page.goto('/');
  const results = await new AxeBuilder({ page })
    .withRules(['image-alt'])
    .analyze();
  expect(results.violations).toEqual([]);
});

test('AX-004 — Form inputs have labels', async ({ page }) => {
  await page.goto('/login');
  const results = await new AxeBuilder({ page })
    .withRules(['label', 'input-image-alt'])
    .analyze();
  expect(results.violations).toEqual([]);
});
```

---

## Keyboard Navigation Tests

```ts
test('AX-010 — All interactive elements reachable by Tab', async ({ page }) => {
  await page.goto('/');
  const interactiveElements = await page.locator(
    'a[href], button, input, select, textarea, [tabindex]:not([tabindex="-1"])'
  ).count();

  const reached = new Set<string>();
  for (let i = 0; i < interactiveElements + 5; i++) {
    await page.keyboard.press('Tab');
    const focused = await page.evaluate(() => {
      const el = document.activeElement;
      return el ? `${el.tagName}:${el.textContent?.trim().slice(0, 30)}` : null;
    });
    if (focused) reached.add(focused);
  }

  expect(reached.size).toBeGreaterThanOrEqual(interactiveElements * 0.9);
});

test('AX-011 — Focus indicator is visible', async ({ page }) => {
  await page.goto('/');
  await page.keyboard.press('Tab');

  const focusVisible = await page.evaluate(() => {
    const el = document.activeElement;
    if (!el) return false;
    const styles = window.getComputedStyle(el);
    const outline = styles.outline;
    const boxShadow = styles.boxShadow;
    return outline !== 'none' || boxShadow !== 'none';
  });

  expect(focusVisible).toBe(true);
});

test('AX-012 — Escape closes modal', async ({ page }) => {
  await page.goto('/');
  // Open a modal (adjust selector to match project)
  const trigger = page.getByRole('button', { name: /open|menu|modal/i });
  if (await trigger.isVisible()) {
    await trigger.click();
    await page.keyboard.press('Escape');
    // Modal should be closed
    await expect(page.getByRole('dialog')).not.toBeVisible();
  }
});
```

---

## Contrast Ratio Check

```ts
function luminance(r: number, g: number, b: number): number {
  const [rs, gs, bs] = [r, g, b].map(c => {
    c = c / 255;
    return c <= 0.03928 ? c / 12.92 : Math.pow((c + 0.055) / 1.055, 2.4);
  });
  return 0.2126 * rs + 0.7152 * gs + 0.0722 * bs;
}

function contrastRatio(rgb1: number[], rgb2: number[]): number {
  const l1 = luminance(rgb1[0], rgb1[1], rgb1[2]);
  const l2 = luminance(rgb2[0], rgb2[1], rgb2[2]);
  const lighter = Math.max(l1, l2);
  const darker = Math.min(l1, l2);
  return (lighter + 0.05) / (darker + 0.05);
}

function parseRgb(color: string): number[] {
  const match = color.match(/\d+/g);
  return match ? match.map(Number) : [0, 0, 0];
}

test('AX-020 — Primary CTA meets 4.5:1 contrast', async ({ page }) => {
  await page.goto('/');
  const button = page.getByRole('button').first();
  const styles = await button.evaluate((el) => {
    const cs = window.getComputedStyle(el);
    return { color: cs.color, background: cs.backgroundColor };
  });

  const fg = parseRgb(styles.color);
  const bg = parseRgb(styles.background);
  const ratio = contrastRatio(fg, bg);

  expect(ratio).toBeGreaterThanOrEqual(4.5);
});
```

---

## Vue Component Accessibility (jest-axe)

```ts
import { axe, toHaveNoViolations } from 'jest-axe';
import { mount } from '@vue/test-utils';
import MyComponent from '@/components/MyComponent.vue';

expect.extend(toHaveNoViolations);

it('AX-030 — component has no a11y violations', async () => {
  const wrapper = mount(MyComponent);
  const results = await axe(wrapper.element);
  expect(results).toHaveNoViolations();
});
```

---

## Required Dependencies

| Package | Purpose | Install |
|---------|---------|---------|
| `@axe-core/playwright` | Automated WCAG testing with Playwright | `npm i -D @axe-core/playwright` |
| `jest-axe` | Automated WCAG testing with Jest/Vitest | `npm i -D jest-axe` |
