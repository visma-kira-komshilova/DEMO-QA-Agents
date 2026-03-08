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

### Coverage Summary

| Metric | Count | Percentage |
|--------|-------|------------|
| Fully Implemented | X | Z% |
| Partially Implemented | Y | Z% |
| Not Implemented | Y | Z% |
| Test Automation Added | Y | - |
| Extra Features | Y | - |

**Verdict:** PASS / PARTIAL / FAIL

### Detailed Validation
[Full Requirements Coverage Analysis from sub-agent]

### Immediate Actions Required
[Actions for Developer, Product Owner, QA]

---

## Requirements Traceability Matrix

**If requirements were NOT provided:**
> Traceability Matrix Skipped. Re-run with original JIRA requirements to generate.

**If requirements WERE provided:**

| Req ID | Requirement | Test IDs | Unit | Integration | E2E | Manual | Status |
|--------|-------------|----------|------|-------------|-----|--------|--------|
| R1 | [desc] | T01, T02 | Yes/- | Yes/- | Selenium/- | - | Covered |

### Testability Summary
### Automation Coverage
### Gaps & Recommendations

---

## Overview
[Brief description of the feature and what these tests validate]

## Prerequisites

### Environment
### Test Data Setup
### User Permissions

---

## Test Scenarios

### Happy Path Scenarios (minimum 3)

#### Scenario 1: <Primary Success Flow>
**Priority:** High
**Automation Candidate:** Yes/No

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

---

### Alternative Flow Scenarios (minimum 2)
### Error Handling Scenarios (minimum 3)
### Edge Case Scenarios (minimum 3)

---

## Regression Test Checklist

| Area | Test Case | Priority | Last Passed |
|------|-----------|----------|-------------|
| <Feature> | <Specific test> | High/Medium/Low | Not Run |

---

## E2E Test Coverage Analysis

### Existing Automated Test Coverage

| Framework | Technology | Related Tests Found | Coverage Status |
|-----------|------------|---------------------|-----------------|
| Selenium UI | Python/Selenium | [UI tests or "None"] | Full/Partial/Gap/N/A |
| Selenium Integration | Python/Selenium | [API/Integration tests or "None"] | Full/Partial/Gap/N/A |
| Playwright | TypeScript/Playwright | [tests or "None"] | Full/Partial/Gap/N/A |
| Mobile | WebdriverIO | [tests or "None"] | Full/Partial/Gap/N/A |

### E2E Test Recommendations

| Scenario | Automate? | Framework | Priority | Effort | Justification |
|----------|-----------|-----------|----------|--------|---------------|
| Scenario 1 | Yes/No | Selenium/Playwright | P0/P1/P2 | Low/Med/High | [reason] |

**Priority Criteria:**
- **P0 (Critical):** Core business flow, high regression risk, frequently executed
- **P1 (High):** Important flow, medium regression risk, repeatable
- **P2 (Low):** Edge case, low regression risk, or one-time validation

### Suggested Test Implementation
[Skeleton test code for Playwright (TypeScript) and Selenium (Python)]

---

## Automation Notes
### Recommended for Automation
### Manual Testing Required

---

## Data Cleanup

Derived by reversing Test Data Setup. For each entity created during test execution:

1. <Cleanup step — entity type, table, removal method>

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
