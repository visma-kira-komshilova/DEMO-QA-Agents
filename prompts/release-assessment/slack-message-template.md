# Slack Release Assessment Message Template

Output format for Slack release notifications. For analysis logic (coverage formula, decision criteria), see `release-assessment-prompt.md`. For orchestration, see `agents/vscode-chat-participants/release-analysis.md`.

---

## Message Structure

```
**[Release Name] Release Assessment**

**Repository:** [HealthBridge-Web | HealthBridge-Api | HealthBridge-Mobile]
**Planned Release:** [Date]
**PRs:** [X] | **Files:** [XXX] (+X,XXX/-X,XXX)
**Overall Risk:** Low | Medium | Critical

---

## HIGHLIGHTS

- **[HM-XXXXX]:** [Feature name] — [One line benefit/status]
- **[HM-XXXXX]:** [Feature name] — [One line benefit/status]
- **[HM-XXXXX]:** [Feature name] — [One line benefit/status]

---

## RISKS & GAPS

**Critical:** [X] PR(s) — [Brief description of blocking issues]
**Medium:** [X] PR(s) — [Brief description of concerns]
**Low:** [X] PR(s) — [Brief status]

**Key Issues:**
- **[HM-XXXXX]:** [Specific issue]
- **[HM-XXXXX]:** [Specific issue]

---

## E2E REGRESSION COVERAGE

**Impacted Areas:** [X total areas affected by this release]
**E2E Coverage (this release):** [X/Y impacted areas covered] = [XX]%

**Covered Areas:**
- [Area 1] — [Framework: Selenium/Playwright/Mobile]
- [Area 2] — [Framework: Selenium/Playwright/Mobile]

**Gaps (Manual Testing Required):**
- [Area 3] — No E2E tests
- [Area 4] — No E2E tests

---

## MANUAL TESTING REQUIRED

- **[HM-XXXXX]:** [Specific scenario] — [Priority: Critical/High/Medium]
- **[HM-XXXXX]:** [Specific scenario] — [Priority: Critical/High/Medium]

---

## RECOMMENDATION

[GO | CONDITIONAL GO | NO-GO]

**Action Required:**
- [ ] [Specific action 1]
- [ ] [Specific action 2]

---

**Full Report:** `reports/release-analysis/Release-XX-YYYY-Risk-Assessment.md`

*Generated: [YYYY-MM-DD HH:MM]*
```

---

## Section Constraints

| Section | Constraint |
|---------|-----------|
| Highlights | 3–5 items. START WITH WINS. No infrastructure items. |
| Risks & Gaps | Critical first. Specific ticket IDs and issues. |
| E2E Coverage | Use LISTS, not tables (Slack doesn't render tables). |
| Manual Testing | Specific scenarios with priorities. |
| Recommendation | Exactly one of GO / CONDITIONAL GO / NO-GO. |

---

## Formatting Rules

- **No tables** — Slack doesn't render markdown tables well. Use bullet lists.
- **No time estimates** — Focus on what needs to be done. Effort estimates belong in the full Risk Assessment (Section 4.5) only.
- Start with positive highlights, THEN problems
- Include specific ticket IDs
- Keep total length under 500 words (target 250–350)

---

## Output Location

`reports/release-analysis/Release-<XX>-<YYYY>-Slack-Message.md`
