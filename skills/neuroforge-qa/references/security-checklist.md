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
