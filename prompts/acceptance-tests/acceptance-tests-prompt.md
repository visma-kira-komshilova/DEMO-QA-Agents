# Acceptance Tests Prompt

Analysis logic for generating acceptance test scenarios. For output format, see `acceptance-tests-template.md`.

---

## Sub-Agent Analysis Instructions

### A. Feature Analyzer

Identify: core functionality, user personas, business rules, edge cases, error conditions, integration points.

### B. Data Analyzer

Identify: test data entities, data states, database tables, setup/cleanup steps. Derive cleanup steps by reversing setup — for each entity created during test, specify how to remove it (by table/entity type).

### C. Scenario Generator

Generate Given/When/Then scenarios for: happy path, alternative paths, error handling, boundary/edge cases.

**Minimum output gate (enforced before synthesis):**
- At least 3 happy path scenarios
- At least 3 error handling scenarios
- At least 3 edge case scenarios

If any minimum is not met, re-run the Scenario Generator with explicit instruction to add the missing category. Do NOT proceed to synthesis with insufficient scenarios.

### D. Regression Identifier

Identify: related features sharing code, downstream consumers, UI flows, integration points.

### E. E2E Coverage Analyzer

Read `context/e2e-test-coverage-map.md` and use it as follows:
1. Identify the functional area affected by the feature
2. Look up the Quick Reference Table to determine which frameworks are in scope
3. For each in-scope framework, use the Search Keywords from the Detailed tables to search across ALL test directories
4. For out-of-scope frameworks, report "N/A"

**E2E Coverage Status Definitions:**

| Status | Definition |
|--------|-----------|
| **Full** | Tests exist covering the happy path AND at least one edge case relevant to the feature |
| **Partial** | Tests exist but only cover the happy path, or don't cover the specific scenario being tested |
| **Gap** | Framework covers this functional area (per coverage map) but no tests exist for this specific feature |
| **N/A** | Framework doesn't cover this functional area (per coverage map) |

### F. Traceability Matrix Builder (only if requirements provided)

Runs AFTER Scenario Generator completes. Maps each requirement to specific test scenario IDs.

**Testability Classification:**

| Signal in Code/Requirement | Classification |
|----------------------------|----------------|
| Pure calculation, business logic | Unit Testable |
| Database query, data access | Integration Testable |
| UI flow, form submission, user interaction | E2E Testable |
| Permission/role check via UI | E2E Testable |
| Permission/role check via API/service | Unit/Integration Testable |
| External system (email, SMS, API call) | Manual or Mock |
| Visual/layout, PDF output, print format | Manual Only |

**Status Legend:**
- **Covered** — This test plan includes scenarios that fully verify the requirement
- **NOT COVERED** — No scenario in this plan verifies the requirement (add one)
- **Manual Only** — Can only be verified manually

**IMPORTANT:** Do NOT use "PARTIAL". If a test scenario covers the requirement, it is COVERED. If no scenario covers it, add one.

---

## Requirements Validation Logic

**This section only applies when user provides original requirements text.**

Compare code implementation (or feature description) against original requirements. Identify:

1. **Implemented Features**: Requirements correctly implemented
2. **Missing Features**: Requirements NOT implemented
3. **Modified Behavior**: Requirements implemented differently than specified
4. **Test Automation**: Test files added/modified to verify the feature
5. **Extra Features**: Code changes not mentioned in requirements (excluding test files)

**IMPORTANT:** Test files (`*Test.cs`, `*Tests.cs`, `*_test.dart`, `*.spec.ts`) are NEVER "Extra Features". Tests are automation coverage. Categorize them under "Test Automation Added".

### Requirements Counting Rules

1. **What counts as one requirement:** Each acceptance criteria bullet point or numbered item in the JIRA ticket counts as one requirement. If there are no explicit acceptance criteria, each distinct functional behavior described in the Description counts as one.
2. **Sub-points:** If a requirement has sub-points (e.g., "Support types: A, B, C"), count the parent as 1 requirement, not 3. Sub-points are verification details within that requirement.
3. **Scoring implementation status:**
   - Fully Implemented = 1.0 (all verification points pass)
   - Partially Implemented = 0.5 (some verification points pass, others missing)
   - Not Implemented = 0.0 (no evidence of implementation)
4. **Coverage % = (sum of scores / total requirements) x 100**

### Validation Verdict Thresholds

- **PASS**: 90-100% coverage score
- **PARTIAL**: 70-89% coverage score
- **FAIL**: <70% coverage score

---

## Constraints

- Every scenario must use Given/When/Then format without exception
- Minimum scenarios: 3 happy path, 2 alternative flow, 3 error handling, 3 edge case
- Reference specific `file:line` when linking scenarios to code changes (branch-based flow only)
- Check E2E coverage map before recommending new automation
- Requirements validation is OPTIONAL — if not provided, skip and add note
- Acceptance tests are ALWAYS generated — validation is supplementary
- If validation shows FAIL verdict, still generate tests but prioritize missing requirements
- Repository selection MUST be reported in the document header
- **Soft per-section targets:** Overview ~100 words, each scenario ~80-120 words, E2E analysis ~200 words, total ~3000-4000 words for a full run with requirements
