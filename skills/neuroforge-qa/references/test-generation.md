# Test Code Generation — Reference Templates

When generating tests, always produce **both** a markdown test case document (`TC-XXX-feature.md`)
AND an executable test file (`.spec.ts` or `.test.ts`). The markdown doc is for human review and
traceability; the executable file is the actual test.

---

## Selector Strategy (Accessible-First Hierarchy)

Always prefer accessible selectors. Fall through this list in order:

1. **`getByRole()`** — Best. Uses ARIA roles. `page.getByRole('button', { name: /submit/i })`
2. **`getByLabel()`** — For form inputs. `page.getByLabel('Email address')`
3. **`getByPlaceholder()`** — When label is missing. `page.getByPlaceholder('Enter email')`
4. **`getByText()`** — For visible text content. `page.getByText('Welcome back')`
5. **`getByTestId()`** — When no semantic option exists. `page.getByTestId('user-avatar')`
6. **CSS selectors** — Last resort only. Document why no accessible selector was possible.

> **Never use CSS selectors as a first choice.** If a component lacks accessible selectors,
> flag it as an accessibility finding in the audit.

---

## Test File Naming Conventions

| Framework | Unit/Component Tests | E2E Tests |
|-----------|---------------------|-----------|
| Vue / Nuxt (Vitest) | `[component].test.ts` | `[feature].spec.ts` |
| React / Next (Jest) | `[Component].test.tsx` | `[feature].spec.ts` |
| React / Next (Vitest) | `[Component].test.tsx` | `[feature].spec.ts` |
| Angular (Karma/Jest) | `[component].spec.ts` | `[feature].e2e-spec.ts` |
| Svelte (Vitest) | `[component].test.ts` | `[feature].spec.ts` |
| Framework-agnostic | N/A | `[feature].spec.ts` |

---

## Test File Placement

- **E2E tests (Playwright):** Place in `e2e/` or `tests/e2e/` at the project root.
  Match the existing directory if one exists.
- **Unit/component tests:** Place alongside the component file or in `__tests__/` adjacent
  to it. Match the project's existing pattern.
- **NeuroForge test cases (markdown):** Always in `neuroforge/test-cases/TC-XXX-[name].md`.

---

## Template 1: Playwright E2E Test

Use for any framework. Playwright is the universal E2E runner.

```ts
import { test, expect } from '@playwright/test';

test.describe('[Feature Name] — [TC-XXX]', () => {
  test.beforeEach(async ({ page }) => {
    // Navigate to the page under test
    await page.goto('/target-page');
  });

  // --- Happy Path ---

  test('TC-XXX-001 — [descriptive name]', async ({ page }) => {
    // Given: [preconditions described here]

    // When: [user action]
    await page.getByRole('button', { name: /submit/i }).click();

    // Then: [expected outcome]
    await expect(page.getByText('Success')).toBeVisible();
  });

  // --- Edge Cases ---

  test('TC-XXX-010 — handles empty input gracefully', async ({ page }) => {
    await page.getByRole('button', { name: /submit/i }).click();
    await expect(page.getByText(/required/i)).toBeVisible();
  });

  test('TC-XXX-011 — handles max-length input', async ({ page }) => {
    const longText = 'A'.repeat(10000);
    await page.getByRole('textbox', { name: /name/i }).fill(longText);
    await page.getByRole('button', { name: /submit/i }).click();
    // Should either truncate or show validation error
    await expect(page.getByText(/too long|character limit/i)).toBeVisible();
  });

  // --- Error States ---

  test('TC-XXX-020 — shows error on network failure', async ({ page }) => {
    // Simulate network failure
    await page.route('**/api/**', route => route.abort());
    await page.getByRole('button', { name: /submit/i }).click();
    await expect(page.getByText(/error|failed|try again/i)).toBeVisible();
  });

  // --- Accessibility ---

  test('TC-XXX-030 — keyboard navigation works', async ({ page }) => {
    // Tab through interactive elements
    await page.keyboard.press('Tab');
    const firstFocused = page.locator(':focus');
    await expect(firstFocused).toBeVisible();

    // Enter submits focused button
    await page.keyboard.press('Tab');
    await page.keyboard.press('Enter');
  });
});
```

---

## Template 2: Vitest Component Test (Vue / Nuxt)

Use when the project has `vitest.config.ts` or uses Vue/Nuxt.

```ts
import { describe, it, expect, beforeEach } from 'vitest';
import { mount, VueWrapper } from '@vue/test-utils';
import { createI18n } from 'vue-i18n';
import { createTestingPinia } from '@pinia/testing';
import ComponentName from '@/components/ComponentName.vue';

describe('ComponentName', () => {
  let wrapper: VueWrapper;

  const createWrapper = (props = {}, options = {}) => {
    const i18n = createI18n({
      legacy: false,
      locale: 'en',
      messages: { en: { /* relevant keys */ } },
    });

    return mount(ComponentName, {
      props,
      global: {
        plugins: [i18n, createTestingPinia()],
        stubs: {
          // Stub child components if needed
          // RouterLink: true,
          // NuxtLink: true,
        },
        ...options,
      },
    });
  };

  beforeEach(() => {
    wrapper = createWrapper();
  });

  // --- Happy Path ---

  it('TC-XXX-001 — renders correctly with default props', () => {
    expect(wrapper.find('[data-testid="component-root"]').exists()).toBe(true);
  });

  it('TC-XXX-002 — displays provided data', () => {
    wrapper = createWrapper({ title: 'Hello World' });
    expect(wrapper.text()).toContain('Hello World');
  });

  // --- Edge Cases ---

  it('TC-XXX-010 — handles empty props gracefully', () => {
    wrapper = createWrapper({ items: [] });
    expect(wrapper.find('[data-testid="empty-state"]').exists()).toBe(true);
  });

  it('TC-XXX-011 — handles undefined props without crashing', () => {
    expect(() => createWrapper({ items: undefined })).not.toThrow();
  });

  // --- Events ---

  it('TC-XXX-015 — emits event on button click', async () => {
    await wrapper.find('button').trigger('click');
    expect(wrapper.emitted('submit')).toHaveLength(1);
  });

  // --- Reactivity ---

  it('TC-XXX-016 — updates when props change', async () => {
    await wrapper.setProps({ title: 'Updated' });
    expect(wrapper.text()).toContain('Updated');
  });
});
```

---

## Template 3: Jest + React Testing Library

Use when the project has `jest.config.ts` or uses React/Next.

```tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import ComponentName from './ComponentName';

describe('ComponentName', () => {
  const defaultProps = {
    title: 'Test Title',
    onSubmit: jest.fn(),
  };

  const renderComponent = (props = {}) => {
    return render(<ComponentName {...defaultProps} {...props} />);
  };

  beforeEach(() => {
    jest.clearAllMocks();
  });

  // --- Happy Path ---

  it('TC-XXX-001 — renders correctly', () => {
    renderComponent();
    expect(screen.getByRole('heading', { name: /test title/i })).toBeInTheDocument();
  });

  it('TC-XXX-002 — submits form with valid data', async () => {
    const user = userEvent.setup();
    renderComponent();

    await user.type(screen.getByLabelText(/email/i), 'test@example.com');
    await user.click(screen.getByRole('button', { name: /submit/i }));

    expect(defaultProps.onSubmit).toHaveBeenCalledWith(
      expect.objectContaining({ email: 'test@example.com' })
    );
  });

  // --- Edge Cases ---

  it('TC-XXX-010 — shows empty state when no data', () => {
    renderComponent({ items: [] });
    expect(screen.getByText(/no items/i)).toBeInTheDocument();
  });

  // --- Error States ---

  it('TC-XXX-020 — shows validation error for invalid email', async () => {
    const user = userEvent.setup();
    renderComponent();

    await user.type(screen.getByLabelText(/email/i), 'not-an-email');
    await user.click(screen.getByRole('button', { name: /submit/i }));

    expect(screen.getByText(/valid email/i)).toBeInTheDocument();
    expect(defaultProps.onSubmit).not.toHaveBeenCalled();
  });

  // --- Loading States ---

  it('TC-XXX-025 — shows loading indicator during submission', async () => {
    const user = userEvent.setup();
    defaultProps.onSubmit.mockImplementation(
      () => new Promise(resolve => setTimeout(resolve, 1000))
    );
    renderComponent();

    await user.click(screen.getByRole('button', { name: /submit/i }));
    expect(screen.getByRole('progressbar')).toBeInTheDocument();
  });
});
```

---

## Template 4: Nuxt E2E with @nuxt/test-utils

Use when the project is Nuxt 3 and has `@nuxt/test-utils` installed.

```ts
import { describe, it, expect } from 'vitest';
import { setup, createPage, url } from '@nuxt/test-utils/e2e';

describe('Dashboard Page — [TC-XXX]', async () => {
  await setup({
    host: 'http://localhost:3000',
    browser: true,
  });

  it('TC-XXX-001 — renders dashboard heading', async () => {
    const page = await createPage('/dashboard');
    const heading = page.getByRole('heading', { level: 1 });
    await expect(heading).toBeVisible();
    await page.close();
  });

  it('TC-XXX-002 — sidebar navigation works', async () => {
    const page = await createPage('/dashboard');
    await page.getByRole('link', { name: /settings/i }).click();
    await expect(page).toHaveURL(/settings/);
    await page.close();
  });
});
```

---

## Test Generation Rules (for the AI agent)

1. **Always generate four quadrants:** Happy path, edge cases, error states, accessibility.
2. **Match the project's existing patterns.** If they use `data-testid`, use it. If they use
   `getByRole`, use it. Consistency > purity.
3. **Include setup/teardown.** If a test requires auth, include login in `beforeEach` or use
   Playwright's `storageState` for session reuse.
4. **Use realistic test data.** Not `"test"` or `"asdf"` — use `"jane.doe@example.com"`,
   `"123 Main Street"`, etc.
5. **Comment the Given/When/Then** inside the test code for traceability.
6. **Set appropriate timeouts.** Default Playwright timeout is 30s. Only increase if the
   feature genuinely requires it (file uploads, heavy API calls).
7. **One describe block per feature.** One test file per feature. Never put unrelated tests
   in the same file.
8. **Link to NeuroForge docs.** Add a comment at the top of each test file:
   `// NeuroForge ref: neuroforge/test-cases/TC-XXX-[feature].md`
