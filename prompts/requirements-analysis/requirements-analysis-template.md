# Requirements Analysis Template

Output format for requirements analysis reports (Phase 1). For analysis logic, see `requirements-analysis.md`. For orchestration, see `agents/vscode-chat-participants/requirements-analysis.md`.

---

## Document Structure

```markdown
# Requirements Analysis: <TICKET-ID>

**Ticket:** <TICKET-ID>
**Title:** {ticket title}
**Date:** YYYY-MM-DD

---

<details>
<summary><strong>1. Requirements Summary</strong></summary>

| Aspect | Details |
|--------|---------|
| **What** | {what is being built/changed} |
| **Who** | {target users/roles} |
| **Why** | {business justification} |
| **Where** | {affected areas/modules} |

### JIRA Context (if available)

- Reporter: {reporter}
- Priority: {priority}
- Labels: {labels}

</details>

<details>
<summary><strong>2. Business Gap Analysis</strong></summary>

| Gap Type | Description / Question | Impact if Unaddressed | Severity |
|----------|----------------------|----------------------|----------|
| Missing Requirement | {what's missing} | {what could go wrong} | Low/Medium/High |
| Ambiguous Requirement | {what's unclear} | {user confusion/errors} | Low/Medium/High |
| Conflicting Requirement | {what conflicts} | {integration failures} | Low/Medium/High |

### Domain / Regulatory Gaps

| Gap | Regulation / Source | Impact | Priority |
|-----|---------------------|--------|----------|
| {what's missing} | {law/regulation} | {consequence} | Critical/Medium/Low |

_Use "N/A — no regulatory gaps identified" if not applicable._

### Critical Questions

1. {specific question about a gap}
2. {specific question about ambiguity}

</details>

<details>
<summary><strong>3. Requirements Readiness Score</strong></summary>

**This is the canonical scoring definition.** All other documents reference this table — update weights and dimensions here only.

| Criteria | Weight | Score | Weighted |
|----------|--------|-------|----------|
| **Completeness** | 20% | X/10 | X.XX |
| **Clarity** | 15% | X/10 | X.XX |
| **Testability** | 15% | X/10 | X.XX |
| **Feasibility** | 15% | X/10 | X.XX |
| **Edge Cases Defined** | 10% | X/10 | X.XX |
| **Integration Impact Defined** | 10% | X/10 | X.XX |
| **Domain Compliance** | 15% | X/10 | X.XX |
| **Total** | 100% | - | **X.X/10** |

### Score Breakdown

| Criteria | Score | Justification |
|----------|-------|---------------|
| **Completeness** | X/10 | {Are all business rules defined?} |
| **Clarity** | X/10 | {Is the user story unambiguous?} |
| **Testability** | X/10 | {Can acceptance criteria be tested?} |
| **Feasibility** | X/10 | {Is it technically achievable?} |
| **Edge Cases Defined** | X/10 | {What % of edge cases have defined behavior?} |
| **Integration Impact** | X/10 | {Are cross-system impacts understood?} |
| **Domain Compliance** | X/10 | {Do requirements align with healthcare regulations?} |

### Readiness Decision

| Score | Verdict |
|-------|---------|
| **>= 7/10** | READY FOR DEVELOPMENT — proceed to QA Test Plan + DEV Estimation |
| **< 7/10** | NOT READY — return to Product Owner with gap list |

**Verdict: {READY / NOT READY} ({score}/10)**

### Blocking Issues (if score < 7/10)

1. {Issue preventing development}
2. {Issue preventing development}

**Action Required:** {PO must clarify X, Y, Z before QA/DEV can proceed}

</details>

<details>
<summary><strong>4. Edge Cases & Exception Scenarios</strong></summary>

### Null/Empty Data

| Scenario | Expected Behavior |
|----------|-------------------|
| {null scenario} | {expected handling} |

### Boundary Values

| Scenario | Expected Behavior |
|----------|-------------------|
| {boundary scenario} | {expected handling} |

### Timing/Concurrency

| Scenario | Expected Behavior |
|----------|-------------------|
| {timing scenario} | {expected handling} |

### External System Failures

| Scenario | Expected Behavior |
|----------|-------------------|
| {failure scenario} | {expected handling} |

</details>

<details>
<summary><strong>5. Integration Impact</strong></summary>

| Repository | Affected? | Changes Needed |
|------------|-----------|----------------|
| HealthBridge-Web | yes/no | {description} |
| HealthBridge-Portal | yes/no | {description} |
| HealthBridge-Api | yes/no | {description} |
| HealthBridge-Mobile | yes/no | {description} |

### Cross-Repository Coordination

{Deployment order, feature flags, coordinated changes}

</details>

<details>
<summary><strong>6. External Integrations</strong></summary>

| System | Integration Type | Impact |
|--------|------------------|--------|
| {e.g., National e-Prescription Registry} | API call / webhook / batch | {changes needed} |

_Use "N/A — No external integrations affected" if not applicable._

</details>

<details>
<summary><strong>7. Error Handling</strong></summary>

| Scenario | Error Message | Recovery |
|----------|---------------|----------|
| {error scenario} | {user-facing message} | {how to recover} |

</details>

<details>
<summary><strong>8. Data Requirements</strong></summary>

| Field | Type | Validation | Required? |
|-------|------|------------|-----------|
| {field} | {type} | {rules} | yes/no |

### Existing Code Impact

| File/Component | Purpose | Change Required | Complexity |
|----------------|---------|-----------------|------------|
| {path} | {what it does} | {what to change} | Low/Med/High |

</details>

<details>
<summary><strong>9. Missing Requirements Checklist</strong></summary>

| Requirement Area | Status | Notes |
|------------------|--------|-------|
| Happy path defined | Yes/No | |
| Error scenarios defined | Yes/No | |
| Edge cases defined | Yes/No | |
| Permissions defined | Yes/No | |
| Validation rules defined | Yes/No | |
| UI/UX defined | Yes/No | |
| Mobile impact considered | Yes/No | |
| API changes defined | Yes/No | |
| Database changes defined | Yes/No | |
| Migration plan defined | Yes/No | |
| Rollback plan defined | Yes/No | |
| Performance requirements defined | Yes/No | |
| Audit/logging requirements defined | Yes/No | |

</details>

<details>
<summary><strong>10. Risk Assessment</strong></summary>

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| {risk} | Low/Med/High | Low/Med/High | {mitigation strategy} |

</details>

<details>
<summary><strong>11. Recommendations</strong></summary>

1. {recommendation 1}
2. {recommendation 2}

</details>

<details>
<summary><strong>12. Questions for Product Owner</strong></summary>

1. {specific, actionable question}
2. {specific, actionable question}

</details>

<details>
<summary><strong>13. Questions for Developers</strong></summary>

1. {question about existing implementation}
2. {question about technical constraints}

</details>

---

_Generated by @hb-requirements-analysis | {date}_
```

---

## Section Constraints

| Section | Constraint |
|---------|-----------|
| 1. Requirements Summary | Table format. JIRA context if available. |
| 2. Gap Analysis | Must include Business + Domain/Regulatory sub-sections. |
| 3. Readiness Score | **Canonical source** for 7-dimension weighted model. All 7 criteria mandatory. |
| 4. Edge Cases | Sub-tables by category (Null, Boundary, Timing, External). |
| 5. Integration Impact | All core repos assessed. Cross-repo coordination if multi-repo. |
| 6. External Integrations | "N/A" if not applicable. |
| 7. Error Handling | Scenario + message + recovery per row. |
| 8. Data Requirements | Includes Existing Code Impact sub-section. |
| 9. Missing Requirements | 13-item checklist — all rows present. |
| 10-11. Risk + Recommendations | Specific and actionable. |
| 12-13. Questions | Specific questions only — not generic "please clarify." |

---

## Output Location

`reports/requirements-analysis/<TICKET-ID>-requirements-analysis.md`
