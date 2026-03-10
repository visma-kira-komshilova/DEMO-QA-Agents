# Acceptance Tests Template

Output format for acceptance test documents. For analysis logic, see `acceptance-tests-prompt.md`.

---

## Document Structure

```markdown
# Acceptance Tests - <ID>

> **Feature:** <Feature Title>
> **Source:** <Branch/Ticket/Description>
> **Repository:** <repo-name> _(selected because: [reason])_
> **Generated:** <Date>
> **Status:** Draft - Ready for QA Review

---

## Requirements Validation Results

**If requirements were NOT provided:**
> Requirements Validation Skipped. Re-run with original JIRA requirements for validation.

**If requirements WERE provided:**

<details>
<summary><strong>Coverage Summary</strong> | <code>X% implemented</code> | <code>N requirements</code> | <code>Verdict: PASS/PARTIAL/FAIL</code></summary>

| Metric | Count | Percentage |
|--------|-------|------------|
| Fully Implemented | X | Z% |
| Partially Implemented | Y | Z% |
| Not Implemented | Y | Z% |
| Test Automation Added | Y | - |
| Extra Features | Y | - |

**Verdict:** PASS / PARTIAL / FAIL

</details>

<details>
<summary><strong>Detailed Validation</strong> | <code>R1-RN</code> | Implementation status per requirement</summary>

[Full Requirements Coverage Analysis from sub-agent]

</details>

<details>
<summary><strong>Immediate Actions Required</strong> | <code>N items</code> | PO, Dev, QA</summary>

[Actions for Developer, Product Owner, QA]

</details>

---

## Requirements Traceability Matrix

**If requirements were NOT provided:**
> Traceability Matrix Skipped. Re-run with original JIRA requirements to generate.

**If requirements WERE provided:**

<details>
<summary><strong>Traceability Matrix</strong> | <code>N requirements</code> | <code>M test scenarios</code> | All traced</summary>

| Req ID | Requirement | Test IDs | Unit | Integration | E2E | Manual | Status |
|--------|-------------|----------|------|-------------|-----|--------|--------|
| R1 | [desc] | T01, T02 | Yes/- | Yes/- | Selenium/- | - | Covered |

</details>

<details>
<summary><strong>Testability Summary</strong> | <code>X unit</code> | <code>Y integration</code> | <code>Z manual-only</code></summary>

[Testability classification breakdown]

</details>

<details>
<summary><strong>Automation Coverage</strong></summary>

[Automation coverage details]

</details>

<details>
<summary><strong>Gaps & Recommendations</strong> | <code>N items</code></summary>

[Gap analysis and recommendations]

</details>

---

<details>
<summary><strong>Overview</strong></summary>

[Brief description of the feature and what these tests validate]

</details>

## Prerequisites

<details>
<summary><strong>Environment</strong> | [short environment summary]</summary>

[Environment requirements]

</details>

<details>
<summary><strong>Test Data Setup</strong> | [short data summary]</summary>

[Test data setup steps]

</details>

<details>
<summary><strong>User Permissions</strong> | [roles needed]</summary>

[Permission requirements]

</details>

---

## Test Scenarios

### Happy Path Scenarios (minimum 3)

<details>
<summary><strong>T01: <Primary Success Flow></strong> | Priority: <code>High</code> | Automation: <code>Yes/No</code> | Req: <code>R1</code></summary>

**Given** <preconditions>
**And** <additional preconditions if any>
**When** <action performed by user>
**Then** <expected outcome>
**And** <additional verifications>

**Test Steps:**
1. <Step with specific values>

**Expected Results:**
- [ ] <Verification point>

**Test Data:**
- <Entity>: <specific test values>

</details>

---

### Alternative Flow Scenarios (minimum 2)

<details>
<summary><strong>T0N: <Alternative Flow Name></strong> | Priority: <code>Medium</code> | Automation: <code>Yes/No</code> | Req: <code>RN</code></summary>

[Same Given/When/Then structure as above]

</details>

---

### Error Handling Scenarios (minimum 3)

<details>
<summary><strong>T0N: <Error Scenario Name></strong> | Priority: <code>High</code> | Type: <code>Error/Security</code> | Req: <code>RN</code></summary>

[Same Given/When/Then structure as above]

</details>

---

### Edge Case Scenarios (minimum 3)

<details>
<summary><strong>T0N: <Edge Case Name></strong> | Priority: <code>Medium/High</code> | Type: <code>Boundary/Timing/etc</code> | Req: <code>RN</code></summary>

[Same Given/When/Then structure as above]

</details>

---

<details>
<summary><strong>Regression Test Checklist</strong> | <code>N cases</code> | Priority: <code>High-Low</code></summary>

| Area | Test Case | Priority | Last Passed |
|------|-----------|----------|-------------|
| <Feature> | <Specific test> | High/Medium/Low | Not Run |

</details>

---

## E2E Test Coverage Analysis

<details>
<summary><strong>Existing Automated Test Coverage</strong> | Coverage status per framework</summary>

**If E2E automation is configured:**

| Framework | Technology | Related Tests Found | Coverage Status |
|-----------|------------|---------------------|-----------------|
| Selenium UI | Python/Selenium | [UI tests or "None"] | Full/Partial/Gap/N/A |
| Selenium Integration | Python/Selenium | [API/Integration tests or "None"] | Full/Partial/Gap/N/A |
| Playwright | TypeScript/Playwright | [tests or "None"] | Full/Partial/Gap/N/A |
| Mobile | WebdriverIO | [tests or "None"] | Full/Partial/Gap/N/A |

**If NO E2E automation (Q4.1 = "none"):**
> N/A — No E2E test automation configured for this project.

</details>

<details>
<summary><strong>E2E Test Recommendations</strong> | <code>N candidates</code> | Priority: <code>P0-P2</code></summary>

| Scenario | Automate? | Framework | Priority | Effort | Justification |
|----------|-----------|-----------|----------|--------|---------------|
| Scenario 1 | Yes/No | Selenium/Playwright | P0/P1/P2 | Low/Med/High | [reason] |

**Priority Criteria:**
- **P0 (Critical):** Core business flow, high regression risk, frequently executed
- **P1 (High):** Important flow, medium regression risk, repeatable
- **P2 (Low):** Edge case, low regression risk, or one-time validation

</details>

<details>
<summary><strong>Suggested Test Implementation</strong> | Skeleton code</summary>

[Skeleton test code for Playwright (TypeScript) and Selenium (Python)]

</details>

---

<details>
<summary><strong>Automation Notes</strong> | <code>X automatable</code> | <code>Y manual-only</code></summary>

### Recommended for Automation
[List of automatable scenarios with justification]

### Manual Testing Required
[List of manual-only scenarios with reason]

</details>

---

<details>
<summary><strong>Data Cleanup</strong> | <code>N steps</code></summary>

Derived by reversing Test Data Setup. For each entity created during test execution:

1. <Cleanup step — entity type, table, removal method>

</details>

---

## Sign-off

| Role | Name | Date | Status |
|------|------|------|--------|
| QA Engineer | | | Pending |
| Developer | | | Pending |
| Product Owner | | | Pending |
```

---

## Output Location

`reports/acceptance-tests/<TICKET-ID>-acceptance-tests.md`
