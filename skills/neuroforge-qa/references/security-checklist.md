# NeuroForge Security Checklist (OWASP Top 10)

Use this checklist during Step 4 (Code Analysis) and Step 6 (Live Testing) to identify security risks.

---

## 🔴 OWASP Top 10 Mapping

### 1. Broken Access Control
- **Check:** Can a user access another user's data by changing a URL ID? (IDOR)
- **Check:** Are admin routes accessible to non-admin users?
- **Check:** Is sensitive data exposed via public APIs?

### 2. Cryptographic Failures
- **Check:** Is sensitive data (passwords, PII) transmitted over HTTP? (Must be HTTPS)
- **Check:** Are secrets (API keys, DB creds) hardcoded in the frontend or public repo?

### 3. Injection
- **Check:** Does the application allow raw HTML in inputs? (XSS)
- **Check:** Are database queries built by concatenating user input? (SQLi)
- **Check:** Are search fields or form inputs vulnerable to script tags?

### 4. Insecure Design
- **Check:** Does the workflow allow skipping critical steps (e.g., payment before checkout)?
- **Check:** Are error messages too detailed (revealing stack traces or server info)?

### 5. Security Misconfiguration
- **Check:** Are default passwords used?
- **Check:** Is the `.env` file or `.git` directory exposed?
- **Check:** Are CORS policies overly permissive (`Access-Control-Allow-Origin: *`)?

### 6. Vulnerable and Outdated Components
- **Check:** Are dependencies (npm, pip, etc.) outdated or known to have CVEs?

### 7. Identification and Authentication Failures
- **Check:** Does the app allow weak passwords?
- **Check:** Is there a lockout mechanism after failed login attempts?
- **Check:** Are session tokens long-lived or not invalidated on logout?

### 8. Software and Data Integrity Failures
- **Check:** Does the app verify the source of updates or external scripts?

### 9. Security Logging and Monitoring Failures
- **Check:** Are failed login attempts and critical actions logged?

### 10. Server-Side Request Forgery (SSRF)
- **Check:** Can the user force the server to make requests to internal resources via URL inputs?

---

## 🔒 Security Test Cases (Manual)

| ID | Test Case | Target |
|----|-----------|--------|
| SEC-001 | XSS Injection | Search bars, comment fields, profile names |
| SEC-002 | IDOR Check | URL IDs (`/user/123` -> `/user/124`) |
| SEC-003 | Auth Bypass | Accessing `/dashboard` without being logged in |
| SEC-004 | Secret Exposure | Scanning `main.js` or `app.js` for keys |
| SEC-005 | Form Validation | Bypassing client-side limits via Postman |

---

## 🔧 Executable Security Test Patterns

### XSS Injection Test

```ts
import { test, expect } from '@playwright/test';

test.describe('Security: XSS Prevention', () => {
  const xssPayloads = [
    '<script>alert(1)</script>',
    '<img src=x onerror=alert(1)>',
    '"><script>alert(1)</script>',
    "';alert(1);//",
    '<svg onload=alert(1)>',
    'javascript:alert(1)',
    '<iframe src="javascript:alert(1)">',
    '{{constructor.constructor("alert(1)")()}}',
  ];

  test('SEC-001 — Input fields sanitize XSS payloads', async ({ page }) => {
    await page.goto('/');
    const inputs = page.locator('input[type="text"], input[type="search"], textarea');
    const count = await inputs.count();

    for (let i = 0; i < Math.min(count, 3); i++) {
      for (const payload of xssPayloads) {
        await inputs.nth(i).fill(payload);
        await inputs.nth(i).press('Enter');
        const content = await page.content();
        expect(content).not.toContain('<script>alert(1)</script>');
      }
    }
  });
});
```

### IDOR (Insecure Direct Object Reference) Test

```ts
test('SEC-002 — Cannot access other user data via URL manipulation', async ({ page }) => {
  // After logging in as user A, try to access user B's resources
  const sensitiveRoutes = [
    '/api/users/999/profile',
    '/api/users/999/settings',
    '/api/orders/999',
  ];

  for (const route of sensitiveRoutes) {
    const response = await page.request.get(route);
    expect([401, 403, 404]).toContain(response.status());
  }
});
```

### Auth Bypass Test

```ts
test('SEC-003 — Protected routes redirect to login when unauthenticated', async ({ page }) => {
  // Clear all cookies/storage to ensure no auth
  await page.context().clearCookies();

  const protectedRoutes = ['/dashboard', '/settings', '/admin', '/profile'];
  for (const route of protectedRoutes) {
    await page.goto(route);
    // Should redirect to login or show 401
    const url = page.url();
    expect(url).toMatch(/login|signin|auth|unauthorized/i);
  }
});
```

### Cookie Security Check

```ts
test('SEC-004 — Auth cookies have secure flags', async ({ page }) => {
  await page.goto('/login');
  // Perform login...
  const cookies = await page.context().cookies();
  const authCookies = cookies.filter(c =>
    /token|session|auth|jwt/i.test(c.name)
  );

  for (const cookie of authCookies) {
    expect(cookie.httpOnly).toBe(true);   // Not accessible via JS
    expect(cookie.secure).toBe(true);      // HTTPS only
    expect(cookie.sameSite).not.toBe('None'); // CSRF protection
  }
});
```

### Secret Scanning Patterns (for codebase analysis)

When scanning the codebase, use these regex patterns to find exposed secrets:

```
API keys:     (?:api[_-]?key|apikey)\s*[:=]\s*['"][A-Za-z0-9+/=]{20,}['"]
AWS keys:     (?:AKIA|ABIA|ACCA|ASIA)[A-Z0-9]{16}
JWT tokens:   eyJ[A-Za-z0-9_-]+\.eyJ[A-Za-z0-9_-]+\.[A-Za-z0-9_-]+
Private keys: -----BEGIN (?:RSA |EC )?PRIVATE KEY-----
Passwords:    (?:password|passwd|pwd)\s*[:=]\s*['"][^'"]{4,}['"]
Generic:      (?:secret|token|credential)\s*[:=]\s*['"][A-Za-z0-9+/=]{8,}['"]
```

Also check for:
- `.env` files committed to git (check `.gitignore`)
- Source maps enabled in production (`*.js.map` accessible)
- `console.log` statements leaking sensitive data
- CORS set to `Access-Control-Allow-Origin: *`
