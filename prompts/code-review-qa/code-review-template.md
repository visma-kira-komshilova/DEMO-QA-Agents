# Code Review Report Template

Output format for comprehensive code review reports. For analysis logic, see `code-review-qa.md`. For brief format, see `code-review-brief-template.md`.

**Related Documents:**
- `prompts/code-review-qa/code-review-qa.md` — PR analysis prompt
- `context/code-review-false-positive-prevention.md` — False positive prevention rules (Rules 1-6)

---

## Document Structure

```markdown
# Code Review: [TICKET-ID] - [Title]

**Branch:** `[branch-name]`
**Repository:** [Auto-detected]
**Review Date:** YYYY-MM-DD
**Risk Level:** Low | Medium | Critical

---

<details>
<summary><strong>1. Summary</strong></summary>

[2-3 sentences: What does this PR do? Business/clinical context? Max 50 words.]

</details>

<details>
<summary><strong>2. Risk Assessment</strong></summary>

| Factor | Assessment |
|--------|------------|
| Files Changed | X files (+Y/-Z lines) |
| Core Areas Affected | [list modules] |
| Database Changes | Yes / No |
| API Changes | Yes / No |
| Breaking Changes | Yes / No |
| Patient Safety Impact | Yes / No |

**Risk Level: Low | Medium | Critical**

**Justification:** [One sentence]

</details>

<details>
<summary><strong>3. Code Quality Review</strong></summary>

### 3.1 Standard Checks

| Check | Status | Notes |
|-------|--------|-------|
| Follows conventions | pass/fail | [details if issue] |
| No logic errors | pass/fail | [details if issue] |
| Error handling | pass/fail | [details if issue] |
| Security (OWASP) | pass/fail | [details if issue] |
| Performance | pass/fail | [details if issue] |
| No hardcoded values | pass/fail | [details if issue] |

### 3.2 Hotfix Pattern Prevention

**Use the pattern table matching the auto-detected repository** from `context/historical-bugfix-patterns.md`.

| Pattern | Status | Finding | Location |
|---------|--------|---------|----------|
| [Pattern 1 (XX%)] | pass/warn/fail | [specific finding] | [file:line] |
| [Pattern 2 (XX%)] | pass/warn/fail | [specific finding] | [file:line] |
| [Pattern 3 (XX%)] | pass/warn/fail | [specific finding] | [file:line] |
| [Pattern 4 (XX%)] | pass/warn/fail | [specific finding] | [file:line] |
| [Pattern 5 (XX%)] | pass/warn/fail | [specific finding] | [file:line] |
| [Pattern 6 (XX%)] | pass/warn/fail | [specific finding] | [file:line] |

</details>

<details>
<summary><strong>4. Test Coverage Analysis</strong></summary>

### 4.1 Unit Test Coverage

| Source File | Test File | Status | Complexity | Est. Effort |
|-------------|-----------|--------|------------|-------------|
| [file.cs] | [test file or "None"] | pass/fail/warn | Low/Medium/High | [hours] |

**Unit Test Complexity:**
- **Low** (1-2h): Pure functions, DTOs, simple logic
- **Medium** (2-4h): Business logic, mockable dependencies
- **High** (4-8h): Complex clinical logic, tightly coupled services

### 4.2 E2E Automation Impact

**E2E Test Repositories:**
- **Selenium:** `HealthBridge-Selenium-Tests/` (Python) — UI + Integration/API
- **Playwright:** `HealthBridge-E2E-Tests/` (TypeScript)
- **Mobile:** `HealthBridge-Mobile-Tests/` (WebdriverIO)

| Framework | Test File | Test Name | Action | Reason | Effort |
|-----------|-----------|-----------|--------|--------|--------|
| Selenium UI | [path] | [name] | UPDATE/DELETE/ADD/NONE | [why] | [hrs] |
| Selenium Integration | [path] | [name] | UPDATE/DELETE/ADD/NONE | [why] | [hrs] |
| Playwright | [path] | [name] | UPDATE/DELETE/ADD/NONE | [why] | [hrs] |
| Mobile | [path] | [name] | UPDATE/DELETE/ADD/NONE | [why] | [hrs] |

**E2E Effort Summary:**

| Repository | Update | Add | Delete | Total Effort |
|------------|--------|-----|--------|--------------|
| Selenium | [#] | [#] | [#] | [hours] |
| Playwright | [#] | [#] | [#] | [hours] |
| Mobile | [#] | [#] | [#] | [hours] |
| **TOTAL** | | | | **[hours]** |

### 4.3 Test Data Requirements

| Test Type | Data Needed | Source | Setup Required |
|-----------|-------------|--------|----------------|
| Unit Tests | [data] | Mock/Fixture | [steps] |
| Selenium | [data] | Test DB/Fixture | [steps] |
| Playwright | [data] | Fixture/API | [steps] |
| Mobile | [data] | Test Account | [steps] |

_If no special data needed: "Standard test data sufficient — no special setup required"_

</details>

<details>
<summary><strong>5. Regression Testing Impact</strong></summary>

| Impacted Area | Risk Level | Suggested Regression Tests |
|---------------|------------|---------------------------|
| [Feature] | Low/Medium/High | [specific scenarios] |

</details>

<details>
<summary><strong>5.5 Security Consistency Check</strong></summary>

**Trigger:** PR contains changes to authentication, token, session, cookie, or authorization handling in JS/TS files.

**If NOT triggered:** "N/A — No security code changes detected"

**If triggered:**

### 5.5.1 Client-Server Symmetry Analysis

| Check | Status | Finding |
|-------|--------|---------|
| Client changes have server counterpart? | pass/fail | [Details] |
| Disabled client feature has server validation updated? | pass/fail | [Details] |
| Token generation changes match validation logic? | pass/fail | [Details] |

### 5.5.2 Dependency Impact

| Dependent File | Dependency Type | Impact | Action Required |
|----------------|-----------------|--------|-----------------|
| [file.cs] | [type] | [impact] | [action] |

### 5.5.3 Security Impact Documentation

| Aspect | Status | Notes |
|--------|--------|-------|
| WHY is security feature being modified? | pass/fail | [Explanation] |
| Security implications documented? | pass/fail | [Details] |
| Alternative measures in place? | pass/fail | [Details] |

### 5.5.4 Testing Evidence

| Test Type | Status | Evidence Location |
|-----------|--------|-------------------|
| Manual testing with security feature | pass/fail | [location] |
| Critical security flows tested | pass/fail | [flows] |
| Error scenarios tested | pass/fail | [scenarios] |

</details>

<details>
<summary><strong>6. Issues Found</strong></summary>

**Quality Gate:** Only HIGH confidence (tool-verified) findings. UNVERIFIED → downgrade to Suggestion or move to Section 7.

### Critical (Must Fix)
1. [Issue with file:line] — **Evidence:** [tool output or code reference]

### Warning (Should Fix)
1. [Issue with file:line] — **Evidence:** [tool output or code reference]

### Suggestion (Nice to Have)
1. [Suggestion with file:line]

</details>

<details>
<summary><strong>7. Questions for Author</strong></summary>

1. [Specific question about the code]

</details>

<details>
<summary><strong>8. Recommendation</strong></summary>

- [ ] **Approve** — Ready to merge
- [ ] **Request Changes** — Issues must be addressed
- [ ] **Comment** — Questions need answers first

</details>

<details>
<summary><strong>9. Critical Test Scenarios</strong></summary>

**Manual Verification Required Before Merge:**

- [ ] **[Primary Flow]:** [Brief description]
- [ ] **[Edge Case]:** [Key boundary condition]
- [ ] **[Backward Compatibility]:** [Existing functionality check]
- [ ] **[Error Handling]:** [Error scenario to test]

**For comprehensive test planning:** `@hb-acceptance-tests <BRANCH-ID>`

</details>

<details>
<summary><strong>10. Developer Feedback</strong></summary>

**Mode:** Interactive (default) / Static (`--no-feedback`)

**Verdicts:**
- **Valid** — Finding is accurate and actionable
- **False Positive** — Finding is incorrect or not applicable
- **Won't Fix** — Finding is valid but won't be addressed

| # | Section | Finding | Verdict | Comment |
|---|---------|---------|---------|---------|
| 1 | [3.2/6] | [from report findings] | [from developer or empty] | [note] |

**Overall Accuracy:** ___/10

**Output:** Feedback also saved as JSON to `reports/feedback/<TICKET>-feedback.json`

</details>

---

*Generated: [date] | Branch: [branch] | Files: [count] | Risk: [level]*
```

---

## Section Constraints

| Section | Constraint |
|---------|-----------|
| 1. Summary | Maximum 50 words. No code details. |
| 3.2 Hotfix Patterns | Use repo-specific pattern table. Apply False Positive Prevention before including. |
| 4.3 Test Data | If no special data: "Standard test data sufficient — no special setup required" |
| 6. Issues Found | Critical/Warning MUST include Evidence. |
| 8. Recommendation | One checkbox only — match severity of issues. |
| 9. Test Scenarios | 3-5 scenarios. One line per scenario. |
| 10. Developer Feedback | Pre-populate with ALL findings from 3.2 (warn/fail) and 6 (all). **Excluded from word count.** |

---

## Constraints

- Max 1300 words (Section 10 excluded)
- Tables > prose for every finding
- file:line references for all issues
- Hotfix patterns MANDATORY per `context/historical-bugfix-patterns.md`

## Output Location

`reports/code-review/<TICKET>-code-review.md`
