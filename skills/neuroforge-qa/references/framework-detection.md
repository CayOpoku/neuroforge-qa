# Framework Detection & Auto-Configuration

Before generating any test code, detect the project's framework and existing test infrastructure.

---

## Step 0 — Detect Framework (Run Before Step 1)

### 1. Check `package.json` dependencies

| Dependency Found | Framework | Confidence |
|-----------------|-----------|:----------:|
| `nuxt` | Nuxt 3 | High |
| `next` | Next.js | High |
| `vue` (without `nuxt`) | Vue (standalone) | High |
| `react` (without `next`) | React (standalone) | High |
| `@angular/core` | Angular | High |
| `svelte` or `@sveltejs/kit` | SvelteKit | High |
| None of the above | Unknown / vanilla | — |

### 2. Check for existing test infrastructure

| File Found | Indicates |
|-----------|-----------|
| `playwright.config.ts` | Playwright E2E already configured |
| `vitest.config.ts` | Vitest already configured |
| `jest.config.ts` or `jest` key in `package.json` | Jest already configured |
| `cypress.config.ts` | Cypress already configured |

### 3. Check for existing test directories and patterns

Look for `tests/`, `e2e/`, `__tests__/`, `spec/`, or co-located `*.spec.ts` / `*.test.ts`.
If found, read 2-3 files and match their import style, selector strategy, and assertion patterns.

**Rule: Match the existing pattern. Never introduce a conflicting test style.**

---

## Framework → Test Tooling Matrix

| Framework | Unit Runner | E2E Runner | Component Library | Install Command |
|-----------|------------|------------|-------------------|----------------|
| **Nuxt 3** | Vitest | Playwright | `@nuxt/test-utils` + `@vue/test-utils` | `npm i -D vitest @vue/test-utils @nuxt/test-utils @playwright/test` |
| **Vue (Vite)** | Vitest | Playwright | `@vue/test-utils` | `npm i -D vitest @vue/test-utils @playwright/test` |
| **Next.js** | Jest | Playwright | `@testing-library/react` | `npm i -D jest @testing-library/react @testing-library/jest-dom @playwright/test` |
| **React (Vite)** | Vitest | Playwright | `@testing-library/react` | `npm i -D vitest @testing-library/react @playwright/test` |
| **SvelteKit** | Vitest | Playwright | `@testing-library/svelte` | `npm i -D vitest @testing-library/svelte @playwright/test` |
| **Unknown** | — | Playwright | — | `npm i -D @playwright/test` |

---

## Scaffolding: Config Files If Missing

### Playwright Config

```ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  retries: process.env.CI ? 2 : 0,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox', use: { ...devices['Desktop Firefox'] } },
    { name: 'mobile-chrome', use: { ...devices['Pixel 5'] } },
  ],
  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

### Vitest Config (Vue/Nuxt)

```ts
import { defineConfig } from 'vitest/config';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
  plugins: [vue()],
  test: {
    globals: true,
    environment: 'jsdom',
    include: ['**/*.{test,spec}.{ts,tsx}'],
    exclude: ['e2e/**', 'node_modules/**'],
  },
});
```

### Nuxt-Specific Vitest Config

```ts
import { defineVitestConfig } from '@nuxt/test-utils/config';

export default defineVitestConfig({
  test: {
    globals: true,
    environment: 'nuxt',
  },
});
```

---

## Directory Scaffolding

```
project-root/
  e2e/                          ← Playwright E2E tests
    [feature].spec.ts
  tests/                        ← Unit / component tests
    components/
      [ComponentName].test.ts
    composables/
      [useComposable].test.ts
  playwright.config.ts
  vitest.config.ts
```

---

## Decision Flow

```
Has playwright.config.ts? → YES: use it. NO: generate one (ask user for baseURL).
Has vitest/jest config?   → YES: use it. NO: Vue/Nuxt/Svelte/Vite? → generate vitest.config.ts.
                                              React CRA/Next? → use Jest.
                                              Unknown? → Playwright E2E only.
Has existing test files?  → YES: read and match style. NO: use templates from test-generation.md.
```

**Always ask before installing dependencies. Never install silently.**
