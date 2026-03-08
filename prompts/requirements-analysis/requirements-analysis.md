# Requirements Analysis Prompt

Analysis logic for Phase 1 of the Requirements Analysis Agent. For orchestration, see `agents/vscode-chat-participants/requirements-analysis.md`. For output format, see `requirements-analysis-template.md`.

---

## Context

HealthBridge is a health management platform with multiple interconnected repositories. Most features require coordinated changes across repos. Based on historical bugfix patterns (see `context/historical-bugfix-patterns.md`), the top hotfix causes — edge cases, authorization gaps, NULL handling — often stem from incomplete requirements.

---

## Analysis Required

### 1. Requirements Summary

- **What:** Core functionality being requested
- **Who:** User personas (physicians, nurses, admins, patients)
- **Why:** Business value / clinical problem solved
- **Where:** Affected modules/areas

### 2. Business Gap Analysis

| Gap Type | Question | Impact if Unaddressed |
|----------|----------|----------------------|
| Business Rule | [What rule is unclear?] | [What could go wrong?] |
| User Journey | [What flow is missing?] | [User confusion/errors] |
| Data | [What data is undefined?] | [Integration failures] |
| Compliance | [What regulations apply?] | [Regulatory/safety risk] |

**Key questions to answer:**
- What happens if the user cancels midway?
- What are the clinical limits/thresholds?
- Are there time-based constraints (prescription validity, appointment windows)?
- What audit trail is required?
- Who can perform this action (role-based permissions)?

### 3. Edge Cases & Exception Scenarios

**CRITICAL:** Edge cases are the #1 hotfix pattern across repositories.

| Category | Scenario | Expected Behavior | Defined? |
|----------|----------|-------------------|----------|
| Empty/Null States | No patient data exists | [behavior?] | Yes/No |
| Boundary Values | Maximum/minimum limits | [behavior?] | Yes/No |
| Concurrent Access | Multiple clinicians editing same record | [behavior?] | Yes/No |
| Timing Issues | Prescription expiry at midnight | [behavior?] | Yes/No |
| Partial Data | Incomplete patient records | [behavior?] | Yes/No |
| Permission Edge | User loses access during operation | [behavior?] | Yes/No |

**Edge Case Checklist (single source — do not duplicate elsewhere):**
- [ ] What if the patient list/collection is empty?
- [ ] What if the value is zero, negative, or exceeds maximum?
- [ ] What if the date is end of month/year/leap year?
- [ ] What if the user has no permission for this department?
- [ ] What if the external system is unavailable?
- [ ] What if the operation times out?
- [ ] What if the record was modified by another clinician?
- [ ] What if the entity was deleted during the operation?

### 4. Error Handling Requirements

| Error Scenario | User Message | System Action | Retry? | Logging |
|----------------|--------------|---------------|--------|---------|
| [scenario] | [message text] | [rollback/partial/continue] | Yes/No | [level] |

### 5. Integration Impact Analysis

**CRITICAL:** Analyze impact on ALL repositories, not just one.

**Internal — per repo:** Module, how affected, integration points, risk level.

**External integrations to check:**
- National e-Prescription Registry, insurance providers, lab systems (HL7/FHIR), EHR exchanges

**Database impact:** Tables, change types, migration needs, backward compatibility.

### 6. Multi-Repository Impact

For each repo (HealthBridge-Web, Portal, Api, Mobile), analyze: business logic, database layer, UI, API endpoints, permissions. Include cross-repository deployment coordination.

### 7. Domain Research

For every analysis:
1. Cross-reference requirements against domain context file rules
2. WebSearch for current regulations if domain file doesn't cover a topic
3. Flag regulatory gaps in Gap Analysis under "Domain / Regulatory Gaps"
4. Add domain-specific edge cases to Edge Cases section

### 8. Data Requirements

Fields, types, validation rules, required status, defaults.

### 9. Existing Code Impact

Search codebase, identify affected files/components, change complexity.

### 10. Missing Requirements Checklist

Happy path, error scenarios, edge cases, permissions, validation, UI/UX, mobile, API, database, migration, rollback, performance, audit/logging.

### 11. Risk Assessment & Recommendations

Risks with likelihood, impact, mitigation. Specific questions for PO and developers.

---

## Requirements Readiness Scoring

**Use the 7-dimension weighted scoring model. Canonical definition: `requirements-analysis-template.md`, Section 3.**

Score each dimension 0-10, apply weights, calculate total. Reference the template for the full scoring table structure and weight values.

**Decision:**
- Score >= 7/10 → Generate all 3 documents
- Score < 7/10 → Generate only Requirements Analysis, list critical questions for PO

---

## Word Count Enforcement

| Document | Limit |
|----------|-------|
| Requirements Analysis | 1500 words |
| Acceptance Tests | No hard cap — follow `acceptance-tests-prompt.md` constraints |
| Dev Estimation | 1500 words |

**If approaching limit, trim:** (1) N/A sections, (2) low-severity gaps, (3) data requirements for simple features. Never trim scoring, edge cases, or integration impact.

---

## Pre-Submission Checklists

### Phase 1: Requirements Analysis

- [ ] Executive Summary (1-2 sentences)
- [ ] Scoring table with all 7 criteria (per template canonical definition)
- [ ] Score breakdown with justifications
- [ ] Verdict: READY (>=7) or NOT READY (<7)
- [ ] Blocking issues list (if <7)
- [ ] Requirements Summary (What/Who/Why/Where)
- [ ] Gap Analysis with Business + Domain/Regulatory gaps
- [ ] Edge Cases table with defined/undefined status
- [ ] Integration Impact — all core repos assessed + relevant microservice APIs
- [ ] Error Handling table
- [ ] Data Requirements table
- [ ] Missing Requirements Checklist
- [ ] Risk Assessment table
- [ ] Questions for PO and Developers (specific, not generic)
- [ ] Word count ≤ 1500

### Phase 2: Acceptance Tests (No-Codebase Mode)

- [ ] Given/When/Then scenarios generated (minimum: 3 happy, 2 alternative, 3 error, 3 edge)
- [ ] Feature Analyzer, Data Analyzer, Scenario Generator sub-agents completed
- [ ] Traceability Matrix maps each requirement to test IDs
- [ ] E2E Coverage Analysis skipped (note: "No code analysis — pre-development")
- [ ] Overview notes: "Generated from requirements description only"
- [ ] Saved to `reports/requirements-analysis/<TICKET>-acceptance-tests.md`

### Phase 3: Dev Estimation

- [ ] Only impacted repositories included
- [ ] Specific file paths identified via code search
- [ ] Unit test framework identified per repo
- [ ] Task-level breakdown with hours
- [ ] Risk buffers applied per template Section 7.2
- [ ] Cross-repo dependencies documented (if multi-repo)
- [ ] Word count ≤ 1500
