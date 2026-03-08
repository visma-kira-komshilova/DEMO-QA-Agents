# Bug Report Analysis Prompt

You are analyzing an error or exception to generate a ticket-ready bug report. Follow this analysis framework for each phase.

---

## Phase 1: Error Classification

**Input Types:**
1. **Stack Trace** — Full exception with file:line references
2. **Error Message Only** — User-reported error text
3. **Symptom Description** — Behavior without technical details
4. **Code Issue** — Developer-reported code problem

**Actions:**
1. Identify error type (NullRef, IndexOut, Logic, Auth, etc.)
2. Extract file path and line number (if available)
3. Determine repository from namespace/stack trace:

| Error Pattern | Repository | Technology |
|---------------|------------|------------|
| `*.cs`, Web namespace | `HealthBridge-Web` | C# / ASP.NET Core |
| `*.cs`, Portal namespace | `HealthBridge-Portal` | C# / .NET Core |
| `*.dart`, Flutter stack | `HealthBridge-Mobile` | Flutter / Dart |
| `*.cs`, Api namespace | `HealthBridge-Api` | C# / .NET Core |
| Claims, insurance processing | `HealthBridge-Claims-Processing` | C# / .NET Core |
| Prescriptions namespace | `HealthBridge-Prescriptions-Api` | C# / .NET Core |
| Browser/client error | `HealthBridge-Web` (WebInterface) | ASP.NET Core |

4. Select correct pattern table from `context/historical-bugfix-patterns.md`

---

## Phase 2: Code Location

**If Stack Trace Available:**
```bash
cd <repository-path>
# Read the file at error location (+/- 20 lines)
```

**If Error Message Only:**
```bash
git grep -n "<error message>" origin/main -- "*.cs" "*.ts" "*.dart"
```

**If Symptom Description Only:**
```bash
git grep -n "<feature keyword>" origin/main -- "*.cs"
```

---

## Phase 3: Root Cause Analysis

### Step 1: Read and Understand Code

- Read +/-20 lines around error line
- Understand function purpose and input parameters
- Trace data flow
- Look for: missing null checks, incorrect conditions, boundary issues, unhandled edge cases, logic errors

### Step 2: Match Hotfix Pattern

Read `context/historical-bugfix-patterns.md` and use the routing table to select the correct pattern table. Common indicators:

- **NULL Handling:** NullReferenceException, accessing properties without null check
- **Edge Cases:** IndexOutOfRangeException, empty collections, date boundaries
- **Authorization:** Unauthorized access, role bypass, missing access control
- **Logic/Condition:** Wrong operators (`&&` vs `||`), wrong variable, copy-paste errors
- **Data Validation:** Invalid formats, type conversion failures
- **Missing Implementation:** NotImplementedException, TODO comments, stubs

### Step 3: Git History Check

```bash
git log origin/main --oneline -n 10 -- "<file-path>"
git blame origin/main -- "<file-path>" -L <line>,<line>
git log --all --grep="<error-type>" --oneline
```

Answer: Is this new or legacy code? Recently changed (regression)? Similar past issues?

### Step 4: Codebase Pattern Search (CRITICAL)

Search for the same bug pattern elsewhere:

```bash
git grep -n "problematic_pattern" origin/main -- "*.cs" "*.ts" "*.dart"
git grep -n "SuspiciousMethod(" origin/main
```

Determine:
1. Same bug elsewhere? How many occurrences?
2. Copy-paste origin? Shared base class/utility?
3. Isolated or systemic?

---

## Phase 4: Severity Assessment

**Apply criteria from `severity-criteria.md`.** Quick reference:

| Severity | Priority | Key Criteria |
|----------|----------|--------------|
| Critical | P1 | Data loss, security breach, system unavailable, >50% users, HIPAA/patient safety |
| High | P2 | Core feature broken, >10% users, data integrity risk, workaround exists but complex |
| Medium | P3 | Non-critical feature broken, <10% users, easy workaround |
| Low | P4 | Cosmetic, no functional impact |

**Assessment questions:** Can users complete their task? Is patient data at risk? How many affected? Workaround available? Clinical feature?

For detailed criteria, decision tree, escalation rules, and edge cases → read `severity-criteria.md`.

---

## Phase 5: Test Coverage Analysis

### Unit Tests
```bash
git ls-tree -r --name-only origin/main | grep -E "(Test|Tests)\.(cs|dart)$"
git grep -n "Test<FunctionName>" origin/main -- "*Test*"
```

Assessment: **Exist** / **Partial** / **Missing**

### E2E Tests

Consult `context/e2e-test-coverage-map.md` for framework mapping. Fetch all E2E repos per CLAUDE.md protocol, then keyword-first search:

```bash
git grep -n "<feature-keyword>" origin/main -- "*.py"       # Selenium
git grep -n "<feature-keyword>" origin/main -- "*.spec.ts"  # Playwright
git grep -n "<feature-keyword>" origin/main -- "*.js"       # Mobile
```

**Selenium row assignment:** `HBIntegrationTests/` → Selenium Integration row. All other folders → Selenium UI row.

### Automation Priority

| Priority | Indicators |
|----------|-----------|
| High | Core clinical feature, patient safety, data integrity, high regression risk |
| Medium | Secondary feature, UI validation, edge case |
| Low | Cosmetic, rare edge case, one-time issue |

---

## Phase 6: Fix Recommendation

Generate 3 fix options:

| Option | Scope | Effort | Risk |
|--------|-------|--------|------|
| **Quick Fix** | Minimal change, address symptom | Low (1-2h) | Medium (may not address root cause) |
| **Proper Fix** | Address root cause in component | Medium (4-8h) | Low (targeted) |
| **Comprehensive Fix** | Root cause + all similar patterns | High (8-16h) | Very Low (prevents recurrence) |

For each option, provide specific code-level guidance. Include a code snippet for the recommended option (3-5 lines).

---

## Phase 7: Report Generation

Use `bug-report-template.md`. Follow these guidelines:

**Be Specific:** "Add null check at PrescriptionService.cs:234" not "Fix the error"
**Be Concise:** Bullet points, short sentences, clear structure
**Be Actionable:** Exact file:line, code fix, specific tests, numbered repro steps

### Ticket Field Auto-Detection

**Component** — derive from file path:

| Path Pattern | Component |
|--------------|-----------|
| `*/prescriptions/*` | Prescriptions |
| `*/patients/*` | Patient Records |
| `*/appointments/*` | Scheduling |
| `*/billing/*` | Billing |
| `*/insurance/*` | Insurance Claims |
| `*/lab/*` | Lab Results |
| `*/admin/*` | Administration |
| `*/auth/*` | Authentication |
| `*/api/*` | API |
| `*/mobile/*` | Mobile |

**Labels** — auto-generate from:
- Pattern: `null-handling`, `edge-case`, `authorization-gap`, `logic-error`, `data-validation`, `missing-implementation`
- Area: `prescription`, `patient-record`, `appointment`, `billing`, `lab-results`
- Severity: `critical-severity`, `high-severity`, `medium-severity`, `low-severity`

### Section 9 Manual Scenarios

Derive 3 scenarios: (1) primary bug reproduction, (2) edge case from matched pattern category, (3) negative/validation test.

### Confidence Levels

State confidence in root cause analysis:

| Level | Range | Indicators |
|-------|-------|-----------|
| High | 90-100% | Clear stack trace, obvious issue, easy to reproduce, similar past issues |
| Medium | 60-89% | Error located but complex logic, multiple potential causes, intermittent |
| Low | 30-59% | No stack trace, complex system interaction, cannot reproduce reliably |

---

## Pre-Submission Checklist

Before submitting, verify ALL:

- [ ] All 9 sections completed per template
- [ ] JIRA fields populated (Summary ≤80 chars, Component, Severity, Labels, Affects Version)
- [ ] Severity justified against criteria
- [ ] Correct pattern table used for repository
- [ ] Root cause with code snippet (5-10 lines) and confidence level
- [ ] Codebase searched for similar patterns — scope reported (isolated vs cluster)
- [ ] 3 fix options with recommended option and code snippet (3-5 lines)
- [ ] Repro steps numbered and specific
- [ ] E2E table has 4 rows (Selenium UI, Selenium Integration, Playwright, Mobile)
- [ ] Word count ≤ 900

---

**File Location:** `prompts/bug-report/bug-report-prompt.md`
