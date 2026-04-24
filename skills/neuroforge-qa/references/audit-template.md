# Deep Audit Template

Use this when producing the full written audit output for a screen or flow.
This feeds into `neuroforge/02-ux-audit.md` and `neuroforge/ux-reports/UXR-XXX-[name].md`.

---

## Audit Report: [Screen / Flow Name]

### 📋 Input Summary

> **Provided:** [text description / screenshot / wireframe / code]
> **Assumptions:** [list any assumptions made]
> **Context:** [platform, user type, product stage if known]
> **Framework / Stack:** [any — or "not applicable"]

---

### 🩺 UX Health Score

**Overall: X / 10**
_(Weighted average of law scores below, weighted by relevance to this UI type)_

---

### ⚡ Top Priority Fixes

_(Pareto Principle — biggest impact per unit of effort)_

1. **[Issue]** — [Law #N] — Severity: Critical / Major / Minor
2. **[Issue]** — [Law #N] — Severity: Critical / Major / Minor
3. **[Issue]** — [Law #N] — Severity: Critical / Major / Minor

---

### 📊 Law-by-Law Breakdown

Only include laws that are meaningfully relevant. Never force all 30.

#### [Law Name] (#N) — Score: X/5

- **Finding:** [What was observed]
- **Why it matters:** [User impact]
- **Recommendation:** [Specific, actionable fix]
- **Example:** [Good vs. bad if useful]

---

### ✅ Strengths

_(Well-applied laws — equally important to surface)_

- [Law Name]: [What's working and why]

---

### ⚠️ Risks & Edge Cases

- [Concerns about specific user groups, error states, accessibility, scale, or performance]

---

### ♿ Accessibility Pass

_(Abbreviated — full details go in `neuroforge/accessibility/AX-XXX-audit.md`)_

- Keyboard nav: [Pass / Concern / Unknown]
- Screen reader: [Pass / Concern / Unknown]
- Contrast: [Pass / Concern / Unknown]
- Touch targets: [Pass / Concern / N/A]

---

### 🔄 Law Interactions

_(Where multiple laws apply to the same element — fixing one may fix others)_

- [e.g. "Navigation violates Hick's Law (#10) and Miller's Law (#18) simultaneously —
  reducing item count resolves both."]

---

### ❓ Questions to Unlock Better Analysis

_(Max 3, most important first)_

1. [Question]
2. [Question]
3. [Question]

---

## QA Defect Log Format (for QA-oriented output)

| ID     | Screen   | Law / Criterion | Description   | Severity                 | Recommendation |
| ------ | -------- | --------------- | ------------- | ------------------------ | -------------- |
| UX-001 | [Screen] | [Law #N]        | [Description] | Critical / Major / Minor | [Fix]          |
| AX-001 | [Screen] | WCAG 1.4.3      | [Description] | Critical                 | [Fix]          |

**Severity definitions:**

- **Critical** — Causes task failure or abandonment for most users
- **Major** — Meaningfully degrades experience; significant users affected
- **Minor** — Polish-level issue; affects satisfaction but not completion

---

## Scoring Rubric

| Score | Meaning                               |
| ----- | ------------------------------------- |
| 5/5   | Excellent — textbook application      |
| 4/5   | Good — minor improvements possible    |
| 3/5   | Mixed — partially applied or neutral  |
| 2/5   | Violation — law is broken, impacts UX |
| 1/5   | Significant violation — fix urgently  |
| N/A   | Law not applicable to this UI         |
