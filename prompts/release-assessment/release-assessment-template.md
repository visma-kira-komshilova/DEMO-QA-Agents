# Release Risk Assessment Template

Output format for release risk assessment reports. For analysis logic, see `release-assessment-prompt.md`. For orchestration, see `agents/vscode-chat-participants/release-analysis.md`.

---

## Document Structure

```markdown
# Release Risk Assessment

**Release:** Release-XX/YYYY
**Repository:** [Auto-detected]
**Analysis Date:** YYYY-MM-DD
**Total PRs:** X | **Files:** XXX (+XX,XXX/-XX,XXX)
**Overall Risk:** Low | Medium | Critical

---

<details>
<summary><strong>1. Executive Summary</strong></summary>

**Risk Level: [Level]** — [One sentence summary]

### Release Composition

| Category | Count | % of Release | Notable Items |
|----------|-------|--------------|---------------|
| **Bug Fixes** | X | XX% | [Key tickets] |
| **New Features** | X | XX% | [Key tickets] |
| **Enhancements** | X | XX% | [Key tickets] |
| **Infrastructure** | X | XX% | [Key tickets] |
| **Configuration** | X | XX% | [Key tickets] |
| **Total** | **XX** | **100%** | XX files (+X,XXX/-X,XXX lines) |

**Key Changes:**
- **HM-XXXXX:** [One line] ([X files], [test status])
- **HM-XXXXX:** [One line] ([X files], [test status])
- **HM-XXXXX:** [One line] ([X files], [test status])

**Critical Gaps:**
- [Specific gap 1]
- [Specific gap 2]

</details>

<details>
<summary><strong>2. PR Analysis Summary</strong></summary>

| Ticket | Date | Files | Test Coverage | Risk | Rationale |
|--------|------|-------|---------------|------|-----------|
| HM-XXXXX | MM-DD | 5 | Full: Unit + E2E | Low | [10 words max] |
| HM-XXXXX | MM-DD | 12 | Partial: Unit only | Medium | [10 words max] |

**Legend:** Full | Partial | None | N/A

</details>

<details>
<summary><strong>3. Test Coverage Analysis (Medium/Critical Risk PRs ONLY)</strong></summary>

### 3.1 HM-XXXXX: [Title] (Medium Risk)

**Files:** X (+XX/-XX) | **Unit:** status | **Integration:** status | **E2E:** status

**Missing Unit Tests:**
- `File.cs` -> `FileTests.cs` — TestMethod_EdgeCase(), TestMethod_Error()

**Missing E2E:**
- Playwright: [specific test needed]

**Recommendation:** [Specific action]

---

### 3.2 HM-XXXXX: [Title] (Critical Risk)

[Same concise format]

---

### 3.3 Test Coverage Summary (ONLY IF >5 PRs)

| Ticket | Unit | Integration | E2E | Status | Critical Gap |
|--------|------|-------------|-----|--------|--------------|
| HM-X | status | status | status | status | [gap or "None"] |

</details>

<details>
<summary><strong>4. Automated Regression Test Coverage</strong></summary>

### 4.1 E2E Coverage Summary

**Include EVERY ticket from Section 2.**

| Ticket | Feature Area | Selenium Coverage | Playwright Coverage | Mobile Coverage | Overall Status |
|--------|--------------|-------------------|---------------------|-----------------|----------------|
| HM-XXXXX | [area] | Covered | None | N/A | Partial |

**Coverage Statistics:**
- Full Coverage: X tickets (X%)
- Partial Coverage: X tickets (X%)
- No Coverage: X tickets (X%)
- N/A: X tickets (excluded from %)

---

### 4.2 Existing E2E Tests for This Release

#### Selenium Tests (HealthBridge-Selenium-Tests)

| Ticket | Related Test File(s) | Test Coverage | Sufficient? |
|--------|---------------------|---------------|-------------|
| HM-XXXXX | `path/to/TestFile.py` | [description] | Yes/Partial/No |

#### Playwright Tests (HealthBridge-E2E-Tests)

| Ticket | Related Test File(s) | Test Coverage | Sufficient? |
|--------|---------------------|---------------|-------------|
| HM-XXXXX | `tests/feature.spec.ts` | [description] | Yes/Partial/No |

#### Mobile Tests (HealthBridge-Mobile-Tests)

| Ticket | Related Test File(s) | Test Coverage | Sufficient? |
|--------|---------------------|---------------|-------------|
| HM-XXXXX | `test/feature/test.js` | [description] | Yes/Partial/No/N/A |

---

### 4.3 Automation Coverage Gaps

| Ticket | Change Description | Gap | Manual Testing Required? |
|--------|-------------------|-----|-------------------------|
| HM-XXXXX | [description] | [gap] | Critical/Medium/No |

---

### 4.4 Recommended E2E Test Execution Plan

**Pre-Release Must Run:**
- [ ] `path/to/TestFile` — Covers HM-XXXXX
- [ ] `path/to/TestFile` — Covers HM-XXXXX

**Smoke Tests (Critical paths):**
- [ ] [Critical path 1]
- [ ] [Critical path 2]

**Changes NOT covered by automation:**
- [ ] [Manual test scenario]

---

### 4.5 E2E Test Maintenance Action Plan

| Action | Test Case Description | Repo | Ticket | Priority | Effort |
|--------|----------------------|------|--------|----------|--------|
| CREATE | [new test needed] | [Framework] | HM-XXXXX | P0/P1/P2 | S/M/L |
| UPDATE | [test to modify] | [Framework] | HM-XXXXX | P0/P1/P2 | S/M/L |
| DELETE | [obsolete test] | [Framework] | HM-XXXXX | P2 | S |

**P0 Test Descriptions (detailed steps for critical tests):**
- [Detailed test description with steps and expected results]

</details>

<details>
<summary><strong>5. Hotfix Pattern Analysis</strong></summary>

**Apply patterns per-PR based on each PR's ticket prefix. Group by repository.**

### [Repository Name]

| Pattern (XX%) | Status | PRs Affected | Findings |
|----------------|--------|--------------|----------|
| [Pattern from repo-specific table] | pass/warn/fail | [tickets] | [findings or "No issues detected"] |

</details>

<details>
<summary><strong>6. Risk Mitigation</strong></summary>

### 6.1 Critical Priority (Blocking — Must Fix Before Release)

| Risk | Related PR | Mitigation Action |
|------|-----------|------------------|
| [risk] | #XXX | [action] |

### 6.2 High Priority (Must Test Before Release)

| Risk | Related PR | Mitigation Test |
|------|-----------|-----------------|
| [risk] | #XXX | [test] |

### 6.3 Medium Priority (Should Test)

| Risk | Related PR | Mitigation Test |
|------|-----------|-----------------|
| [risk] | #XXX | [test] |

</details>

<details>
<summary><strong>7. Release Recommendation</strong></summary>

- [ ] **GO** — All critical areas covered, automated tests pass
- [ ] **CONDITIONAL GO** — Proceed with noted manual testing of gaps
- [ ] **NO-GO** — Critical gaps must be resolved first

**Justification:** [1–2 sentences]

</details>

<details>
<summary><strong>8. Post-Release Monitoring</strong></summary>

| Metric | Baseline | Alert Threshold | Action |
|--------|----------|-----------------|--------|
| [metric] | [value] | [threshold] | [action] |

**Actions Timeline:**
- **0–4h post-release:** [specific checks]
- **Week 1:** [specific monitoring]

**Warning Signs:**
- [sign 1]
- [sign 2]

</details>

---

_Generated: YYYY-MM-DD | Release: XX-YYYY | PRs Analyzed: X_
```

---

## Section Constraints

| Section | Constraint |
|---------|-----------|
| 1. Executive Summary | Max 200 words. Release Composition Table mandatory. |
| 2. PR Analysis | Table only. Risk rationale max 10 words per row. |
| 3. Test Coverage | Medium/Critical risk PRs only. Section 3.3 only if >5 PRs. |
| 4. E2E Coverage | 4.1 includes ALL functional tickets. 4.5 is MANDATORY. |
| 5. Hotfix Patterns | Group by repository. Use repo-specific pattern tables. |
| 6. Risk Mitigation | Three tiers: Critical → High → Medium. |
| 7. Recommendation | Exactly one checkbox. Match severity of findings. |
| 8. Monitoring | Specific metrics and timelines, not generic advice. |

---

## Output Location

`reports/release-analysis/Release-<XX>-<YYYY>-Risk-Assessment.md`
