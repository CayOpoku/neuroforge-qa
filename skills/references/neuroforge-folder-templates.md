# NeuroForge Folder Templates

These are the starter templates for each file created during a NeuroForge QA activation.
Use these when creating files from scratch. When files already exist, read and extend them.

---

## 01-project-analysis.md

```markdown
# Project Analysis

**Product:** [Name]
**Platform:** [Web / iOS / Android / Desktop / API / CLI / Other]
**Framework / Stack:** [Any — React, Vue, Django, Swift, etc. or "Unknown"]
**User Types:** [Who uses this? e.g. end users, admins, developers]
**Stage:** [Concept / MVP / Beta / Production]
**Input provided:** [Description / Screenshot / Wireframe / Code / API spec]
**Scanned:** [Date]

---

## Entry Points

[What are the main entry points into the product? e.g. Login, Home, API root]

## Key Flows

[List the primary user flows identified]

## Stack Notes

[Any observations about the technology — even if unknown/partial]

## Scope of This Audit

[What is in scope for this NeuroForge session]

## Out of Scope

[What is explicitly not being assessed this session]

## Open Questions

[Things that need clarification before analysis can be completed]
```

---

## 02-ux-audit.md

```markdown
# UX Audit

**Screen / Flow:** [What was audited]
**Input type:** [Description / Screenshot / Wireframe / Code]
**UX Health Score:** X / 10
**Audited:** [Date]

---

## Top Priority Fixes

1. **[Issue]** — [Law #N] — Severity: Critical / Major / Minor
2. **[Issue]** — [Law #N] — Severity: Critical / Major / Minor
3. **[Issue]** — [Law #N] — Severity: Critical / Major / Minor

---

## Law-by-Law Findings

### [Law Name] (#N) — Score: X/5

- **Finding:** [What was observed]
- **Why it matters:** [User impact]
- **Recommendation:** [Specific, actionable fix]

_(Only include laws that are meaningfully relevant)_

---

## Strengths

- [Law Name]: [What's working and why]

---

## Law Conflicts

- [e.g. "Occam's Razor (#19) vs. Tesler's Law (#27) — complexity hidden from user but shifted to
  backend config. Acceptable trade-off given user type."]

---

## Assumptions Made

- [Any assumptions due to limited input]
```

---

## 03-risk-register.md

```markdown
# Risk Register

**Updated:** [Date]

| ID       | Area   | Risk                  | Likelihood   | Impact       | Mitigation      |
| -------- | ------ | --------------------- | ------------ | ------------ | --------------- |
| RISK-001 | [Area] | [What could go wrong] | High/Med/Low | High/Med/Low | [How to reduce] |

---

## Edge Cases to Test

- [List specific edge cases identified during analysis]

## Accessibility Risks

- [List specific accessibility risks identified]

## Performance Risks

- [Any performance concerns — Doherty Threshold violations, heavy payloads, etc.]

## Security / Data Risks

- [Any concerns about exposed data, input validation, auth state]
```

---

## 04-qa-strategy.md

```markdown
# QA Strategy

**Product:** [Name]
**Coverage Goal:** [e.g. "Critical paths + full accessibility pass"]
**Environments:** [e.g. Chrome/Firefox/Safari, iOS 17+, Android 13+]
**Tools:** [e.g. Playwright, pytest, manual, axe-core]
**Updated:** [Date]

---

## Test Pyramid

| Layer                | Approach                   | Priority     |
| -------------------- | -------------------------- | ------------ |
| Unit                 | [If applicable]            | Low/Med/High |
| Integration          | [API contracts, data flow] | Low/Med/High |
| E2E                  | [Critical user journeys]   | High         |
| Manual / Exploratory | [Edge cases, UX, a11y]     | High         |

---

## Coverage Priorities (Pareto — 20% of tests covering 80% of risk)

1. [Most critical flow]
2. [Second most critical]
3. [Third]

## Deferred / Out of Scope

- [What's not being tested and why]

## Test Data Requirements

- [What data/state is needed to run tests]

## Known Gaps

- [Honest list of coverage gaps]
```

---

## 05-findings-log.md

```markdown
# Findings Log

Running log of all issues found across NeuroForge sessions. Never delete entries — mark as resolved.

| ID    | Date   | Source   | Description   | Severity             | Law / Criterion       | Status     | Resolved In    |
| ----- | ------ | -------- | ------------- | -------------------- | --------------------- | ---------- | -------------- |
| F-001 | [Date] | UX Audit | [Description] | Critical/Major/Minor | [Law #N / WCAG X.X.X] | Open/Fixed | [TC ref or PR] |

---

## Summary Stats

- Total findings: N
- Open: N
- Fixed: N
- Deferred: N
```

---

## ux-reports/UXR-001-[screen-name].md

```markdown
# UX Report: [Screen Name]

**Screen:** [Name]
**Input:** [Description / Screenshot / Wireframe]
**UX Health:** X / 10
**Audited:** [Date]

---

## Summary

[2–4 sentence overview for stakeholders — plain language, no jargon]

---

## Findings

### 🔴 Critical

- [Issue] — [Law violated] — [Recommendation]

### 🟠 Major

- [Issue] — [Law violated] — [Recommendation]

### 🟡 Minor

- [Issue] — [Law violated] — [Recommendation]

---

## Strengths

- [What's working well]

---

## Recommended Next Steps

1. [Specific action]
2. [Specific action]
```

---

## accessibility/AX-001-audit.md

```markdown
# Accessibility Audit

**Screen / Feature:** [Name]
**Standard:** WCAG 2.2 AA (AAA where noted)
**Tools used:** [axe-core / manual / VoiceOver / NVDA / contrast analyser / etc.]
**Audited:** [Date]

---

## Summary

[2–3 sentence overall accessibility health statement]

---

## Findings

| ID        | WCAG Criterion | Description   | Severity             | Recommendation |
| --------- | -------------- | ------------- | -------------------- | -------------- |
| AX-001-01 | 1.4.3 Contrast | [Description] | Critical/Major/Minor | [Fix]          |
| AX-001-02 | 2.1.1 Keyboard | [Description] | Critical/Major/Minor | [Fix]          |

---

## Keyboard Navigation

- Tab order: [Pass / Fail / Notes]
- Focus indicator visible: [Pass / Fail / Notes]
- All interactive elements reachable: [Pass / Fail / Notes]

## Screen Reader

- Semantic structure: [Pass / Fail / Notes]
- ARIA labels: [Pass / Fail / Notes]
- State changes announced: [Pass / Fail / Notes]

## Colour & Contrast

- Text contrast (4.5:1 minimum): [Pass / Fail / Notes]
- UI component contrast (3:1 minimum): [Pass / Fail / Notes]
- Not colour-only: [Pass / Fail / Notes]

## Touch Targets

- Minimum 44×44px: [Pass / Fail / Notes]

## Motion

- prefers-reduced-motion respected: [Pass / Fail / N/A]

---

## Recommendations

1. [Specific fix]
2. [Specific fix]
```
