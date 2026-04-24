# API Testing Reference

Use Playwright's built-in `request` context for API testing. No extra dependencies needed.

---

## REST API — CRUD Test Patterns

```ts
import { test, expect } from '@playwright/test';

test.describe('API: /api/users', () => {
  let authToken: string;

  test.beforeAll(async ({ request }) => {
    // Login to get a token (adapt to your auth flow)
    const loginRes = await request.post('/api/auth/login', {
      data: { email: 'test@example.com', password: 'password123' },
    });
    const body = await loginRes.json();
    authToken = body.token;
  });

  const authHeaders = () => ({ Authorization: `Bearer ${authToken}` });

  // --- Happy Path ---

  test('API-001 — GET returns 200 with valid token', async ({ request }) => {
    const res = await request.get('/api/users', { headers: authHeaders() });
    expect(res.status()).toBe(200);
    const body = await res.json();
    expect(body).toHaveProperty('data');
    expect(Array.isArray(body.data)).toBe(true);
  });

  test('API-002 — POST creates a resource', async ({ request }) => {
    const res = await request.post('/api/users', {
      headers: authHeaders(),
      data: { name: 'Jane Doe', email: 'jane@example.com' },
    });
    expect(res.status()).toBe(201);
    const body = await res.json();
    expect(body).toHaveProperty('id');
    expect(body.name).toBe('Jane Doe');
  });

  // --- Auth ---

  test('API-010 — GET returns 401 without token', async ({ request }) => {
    const res = await request.get('/api/users');
    expect(res.status()).toBe(401);
  });

  test('API-011 — GET returns 403 for forbidden resource', async ({ request }) => {
    const res = await request.get('/api/admin/users', { headers: authHeaders() });
    expect([401, 403]).toContain(res.status());
  });

  // --- Validation ---

  test('API-020 — POST returns 400 for missing required fields', async ({ request }) => {
    const res = await request.post('/api/users', {
      headers: authHeaders(),
      data: { name: '' }, // missing email
    });
    expect(res.status()).toBe(400);
    const body = await res.json();
    expect(body.errors || body.message).toBeDefined();
  });

  // --- Error Response Shape ---

  test('API-030 — Error responses have consistent shape', async ({ request }) => {
    const res = await request.get('/api/nonexistent');
    expect(res.status()).toBe(404);
    const body = await res.json();
    // Should have error message
    expect(body.error || body.message).toBeDefined();
    // Must NOT leak stack traces
    const bodyStr = JSON.stringify(body);
    expect(bodyStr).not.toContain('at Object');
    expect(bodyStr).not.toContain('node_modules');
    expect(bodyStr).not.toContain('TypeError');
  });
});
```

---

## Schema Validation Helper

```ts
function validateSchema(body: any, schema: Record<string, string>) {
  for (const [key, type] of Object.entries(schema)) {
    expect(body).toHaveProperty(key);
    expect(typeof body[key]).toBe(type);
  }
}

test('API-040 — User response matches expected schema', async ({ request }) => {
  const res = await request.get('/api/users/1', { headers: authHeaders() });
  const user = await res.json();
  validateSchema(user, {
    id: 'number',
    name: 'string',
    email: 'string',
    createdAt: 'string',
  });
});
```

---

## Rate Limiting Check

```ts
test('API-050 — Rate limiting is enforced', async ({ request }) => {
  const promises = Array.from({ length: 50 }, () =>
    request.get('/api/users', { headers: authHeaders() })
  );
  const responses = await Promise.all(promises);
  const statuses = responses.map(r => r.status());
  const hasRateLimit = statuses.includes(429);
  // Log finding if no rate limiting exists
  if (!hasRateLimit) {
    console.warn('⚠️ No rate limiting detected on /api/users — flag as security concern');
  }
});
```

---

## Response Time Check

```ts
test('API-060 — Response time under 500ms', async ({ request }) => {
  const start = Date.now();
  await request.get('/api/users', { headers: authHeaders() });
  const duration = Date.now() - start;
  expect(duration).toBeLessThan(500);
});
```

---

## Checklist for API Test Coverage

For each API endpoint, test:

- [ ] **2xx** — Happy path returns correct status and shape
- [ ] **401** — Unauthenticated request is rejected
- [ ] **403** — Unauthorized request is rejected (IDOR check)
- [ ] **400** — Invalid input returns validation errors
- [ ] **404** — Non-existent resource returns 404
- [ ] **500** — Server errors don't leak stack traces
- [ ] **Schema** — Response matches expected field types
- [ ] **Speed** — Response time is under threshold
