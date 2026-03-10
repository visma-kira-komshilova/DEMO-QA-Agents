# Bug Report Template

Output format for bug report documents. For analysis logic, see `bug-report-prompt.md`. For severity details, see `severity-criteria.md`.

---

## Document Structure

```markdown
# Bug Report: {Error Summary Title}

**Ticket:** <TICKET-ID>
**Reporter:** {agent: "@hb-bug-report" / person: enter your name}
**Date:** {YYYY-MM-DD}

## JIRA Fields

| Field | Value |
|-------|-------|
| **Summary** | {concise bug title, max 80 chars} |
| **Component** | {auto-detected from file paths per prompt rules} |
| **Severity** | {🔴 Critical / 🟠 High / 🟡 Medium / 🟢 Low} |
| **Labels** | {pattern + area + severity labels} |
| **Affects Version** | {from release branch `Release-<WEEK>/<YEAR>`, or `version.txt`, or `package.json`} |

---

<details>
<summary><strong>1. Error Summary</strong></summary>

> _30-50 words describing the bug concisely. State what is broken, where, and the immediate consequence._

{summary}

</details>

<details>
<summary><strong>2. Steps to Reproduce</strong></summary>

**Preconditions:**
- {precondition 1}
- {precondition 2}

**Steps:**
1. {step 1}
2. {step 2}
3. {step 3}
4. Observe: {what goes wrong}

**Frequency:** {Always / Intermittent (~X%) / Only under specific conditions}

</details>

<details>
<summary><strong>3. Expected vs. Actual Behavior</strong></summary>

| | Description |
|---|------------|
| **Expected** | {cite requirement or prior behavior} |
| **Actual** | {exact error message or UI state — quote verbatim} |

</details>

<details>
<summary><strong>4. Root Cause Analysis</strong></summary>

> _100-150 words explaining why the bug occurs._

**Confidence:** {🟢 High (90-100%) / 🟡 Medium (60-89%) / 🔴 Low (30-59%)} — {one sentence justification}

**Hotfix Pattern:** {pattern category from historical-bugfix-patterns.md}

{Analysis of the underlying cause — code path, why the logic fails, under what conditions.}

\`\`\`csharp
// File: {file-path}:{line-number}
{relevant code snippet, 5-10 lines max}
\`\`\`

</details>

<details>
<summary><strong>5. Impact Assessment</strong></summary>

| Factor | Assessment |
|--------|-----------|
| **Severity Justification** | {why this severity, based on severity-criteria.md} |
| **User Impact** | {who, how many, which workflows break} |
| **Data Risk** | {corruption, loss, incorrect records risk} |
| **Workaround Available?** | {Yes/No — describe if yes} |

</details>

<details>
<summary><strong>6. Pattern Scope Analysis</strong></summary>

| Question | Answer |
|----------|--------|
| **Isolated or pattern?** | {Isolated / Part of broader pattern} |
| **Similar code elsewhere?** | {file paths where same anti-pattern exists} |
| **Related PRs/Commits** | {commit hashes or PR numbers, or "None found"} |
| **Related JIRA tickets** | {link if known, or "Requires JIRA search"} |

> _If pattern: list all affected locations for comprehensive fix._

</details>

<details>
<summary><strong>7. Fix Recommendation</strong></summary>

| Option | Scope | Effort | Risk |
|--------|-------|--------|------|
| **Quick Fix** | {minimal change, address symptom} | {Low, 1-2h} | {Medium} |
| **Proper Fix** | {root cause in component} | {Medium, 4-8h} | {Low} |
| **Comprehensive Fix** | {root cause + all similar patterns} | {High, 8-16h} | {Very Low} |

**Recommended: {option}** — {one sentence justification}

\`\`\`csharp
// Recommended fix in {file-path}:{line-number} (3-5 lines max):
{code snippet}
\`\`\`

**Validation:** {how to verify the fix works}

</details>

<details>
<summary><strong>8. Test Data Requirements</strong></summary>

| Data Needed | Details |
|-------------|---------|
| {data type} | {specific values or conditions} |
| {data type} | {role and permissions required} |

**SQL/Setup (if applicable):**
\`\`\`sql
{query or setup steps}
\`\`\`

</details>

<details>
<summary><strong>9. Regression Test Recommendation</strong></summary>

### Manual Tests

| # | Scenario | Steps | Expected Result |
|---|----------|-------|-----------------|
| 1 | {primary bug scenario} | {steps} | {expected} |
| 2 | {edge case from pattern} | {steps} | {expected} |
| 3 | {negative/validation test} | {steps} | {expected} |

### E2E Automation Gaps

**If E2E automation is configured:**

| Framework | Repository | Coverage | Recommendation |
|-----------|------------|----------|----------------|
| Selenium UI (Python) | HealthBridge-Selenium-Tests | {Full/Partial/Gap/N/A} | {test name or recommendation} |
| Selenium Integration (Python) | HealthBridge-Selenium-Tests | {Full/Partial/Gap/N/A} | {test name or recommendation} |
| Playwright (TypeScript) | HealthBridge-E2E-Tests | {Full/Partial/Gap/N/A} | {test name or recommendation} |
| Mobile (WebdriverIO) | HealthBridge-Mobile-Tests | {Full/Partial/Gap/N/A} | {test name or recommendation} |

**If NO E2E automation (Q4.1 = "none"):**
> N/A — No E2E test automation configured for this project.

</details>
```

---

## Constraints

- **Max 900 words** total report length
- **Code snippets:** 5-10 lines (Section 4 defect) / 3-5 lines (Section 7 fix)
- **file:line references** required for all code mentions
- **Severity justified** against severity-criteria.md
- **Repro steps** numbered, specific, independently reproducible
- **Root cause** references actual code, not speculation
- **Confidence level** stated in Section 4

## Output Location

`reports/bug-report/<TICKET-ID>-bug-report.md`
(or `reports/bug-report/<ERROR-TYPE>-<DATE>-bug-report.md` if no ticket ID)
