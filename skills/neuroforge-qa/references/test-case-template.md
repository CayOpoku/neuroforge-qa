# Test Case Template

Use this template for every file in `neuroforge/test-cases/TC-XXX-[feature-name].md`.
Adapt format to the user's framework if specified (Playwright, Cypress, pytest, Jest, XCTest, etc.).

---

# Test Cases: [Feature / Flow Name]

**File:** `TC-XXX-[feature-name].md`
**Feature:** [What is being tested]
**Platform:** [Web / iOS / Android / API / Desktop / Any]
**Framework:** [Plain language / Playwright / pytest / Jest / etc.]
**NeuroForge ref:** [Link to related `02-ux-audit.md` finding or `03-risk-register.md` entry]
**Date:** [Created / Last updated]
**Status:** Draft / In Review / Approved

---

## Coverage Map

| Quadrant           | Count | Status   | Result (P/F) |
| ------------------ | ----- | -------- | ------------ |
| Happy Path         | N     | ⬜ Draft | ⬛ -          |
| Edge Cases         | N     | ⬜ Draft | ⬛ -          |
| Error States       | N     | ⬜ Draft | ⬛ -          |
| UX / Accessibility | N     | ⬜ Draft | ⬛ -          |

---

## Happy Path Tests

### TC-XXX-001 — [Short descriptive name]

- **Given:** [Preconditions / state]
- **When:** [User action or system event]
- **Then:** [Expected outcome]
- **Covers:** [Law / requirement / risk reference]
- **Priority:** P1 / P2 / P3
- **Result:** ⬛ NOT TESTED / 🟩 PASS / 🟥 FAIL
- **Notes:** [Any relevant context, platform caveats, etc.]

### TC-XXX-002 — [Short descriptive name]

- **Given:**
- **When:**
- **Then:**
- **Covers:**
- **Priority:**
- **Result:**

_(Add as many as needed)_

---

## Edge Case Tests

### TC-XXX-010 — [Short descriptive name]

- **Given:**
- **When:**
- **Then:**
- **Covers:**
- **Priority:**
- **Result:**
- **Notes:** [Boundary condition, large input, empty state, concurrent user, etc.]

_(Common edge cases to consider: empty state, max-length input, special characters,
slow network, expired session, concurrent modification, unsupported browser/device)_

---

## Error State Tests

### TC-XXX-020 — [Short descriptive name]

- **Given:**
- **When:** [Invalid input / network failure / permission denied / timeout]
- **Then:** [Expected error message, recovery path, no data leak]
- **Covers:**
- **Priority:**
- **Result:**

_(Common error states: 400/401/403/404/500 responses, validation failures, network offline,
timeout, race condition, partial failure, retry behaviour)_

---

## UX & Accessibility Tests

### TC-XXX-030 — Keyboard navigation

- **Given:** User navigates without a mouse
- **When:** Tabs through [feature]
- **Then:** All interactive elements are reachable, focus order is logical, focus indicator is visible
- **Covers:** WCAG 2.1.1 / Fitts's Law (#7)
- **Priority:** P1

### TC-XXX-031 — Screen reader compatibility

- **Given:** Screen reader active (NVDA / VoiceOver / TalkBack)
- **When:** User navigates [feature]
- **Then:** All elements have meaningful labels, state changes announced, no orphaned ARIA
- **Covers:** WCAG 1.3.1, 4.1.2
- **Priority:** P1

### TC-XXX-032 — Colour contrast

- **Given:** [Screen / component]
- **When:** Checked with contrast analyser
- **Then:** Text ≥ 4.5:1, UI components ≥ 3:1 (WCAG 1.4.3 / 1.4.11)
- **Covers:** WCAG 1.4.3, Aesthetic-Usability Effect (#1)
- **Priority:** P1

### TC-XXX-033 — Touch target size (mobile)

- **Given:** Mobile viewport
- **When:** Measuring interactive elements
- **Then:** All tap targets ≥ 44×44px
- **Covers:** Fitts's Law (#7) / WCAG 2.5.8
- **Priority:** P1

### TC-XXX-034 — Responsive layout

- **Given:** [Feature]
- **When:** Viewed at 320px, 768px, 1280px, 1920px
- **Then:** No overflow, no broken layout, primary actions always reachable
- **Covers:** Cognitive Load (#5), Jakob's Law (#11)
- **Priority:** P2
- **Result:**

_(Add UX-specific tests as needed — law violations found in the UX audit become test cases here)_

---

## Defect Log

Defects found during test execution go here, not in a separate file.

| ID      | TC Ref     | Description   | Severity                 | Status       | Notes |
| ------- | ---------- | ------------- | ------------------------ | ------------ | ----- |
| BUG-001 | TC-XXX-020 | [Description] | Critical / Major / Minor | Open / Fixed |       |

**Severity definitions:**

- **Critical** — Task failure or data loss for most users
- **Major** — Significantly degrades experience; many users affected
- **Minor** — Polish issue; affects satisfaction but not completion

---

## Sign-off

| Role     | Name | Status     | Date |
| -------- | ---- | ---------- | ---- |
| QA Lead  |      | ⬜ Pending |      |
| Designer |      | ⬜ Pending |      |
| Dev Lead |      | ⬜ Pending |      |
