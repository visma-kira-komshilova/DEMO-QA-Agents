# Release Assessment Prompt

Analysis logic for the Release Analysis Agent. For orchestration, see `agents/vscode-chat-participants/release-analysis.md`. For report format, see `release-assessment-template.md`.

**Related Documents:**
- `release-assessment-template.md` — Risk assessment report structure
- `release-notes-prompt.md` — Customer-facing release notes
- `slack-message-template.md` — Slack notification format
- `context/e2e-test-coverage-map.md` — Framework-to-functional-area mapping
- `context/historical-bugfix-patterns.md` — Repo-specific pattern tables

---

## Context

HealthBridge is a health management platform with weekly release cycles, multiple interconnected services, and strict requirements for patient data accuracy and clinical safety.

All inputs are derived by the release analysis agent via git commands and sub-agent results. This prompt receives pre-analyzed PR data.

---

## PR Categorization

| Category | Definition |
|----------|-----------|
| Bug Fix | Fixes for reported issues or incorrect behavior |
| New Feature | Completely new functionality |
| Enhancement | Improvements to existing features (performance, UX) |
| Infrastructure | Database scripts, data fixes, system maintenance |
| Configuration | Settings changes, integration config |

---

## Risk Level Definitions

### Per-PR Risk

| Level | Criteria |
|-------|----------|
| **Low** | Minor changes, well-tested, no core areas affected |
| **Medium** | Moderate complexity, some test gaps, affects important features |
| **Critical** | High complexity, major test gaps, security issues, affects core clinical logic |

### Combined Release Risk

Takes into account: max individual PR risk, PR clustering, integration complexity, test coverage %, pattern concentration, functional area overlap. NOT just an average of individual PR risks.

---

## E2E Coverage Calculation (Single Source of Truth)

### Formula

```
E2E Regression Coverage = (Fully covered + 0.5 × Partially covered) / Total impacted areas
```

**N/A entries excluded from calculation.**

### Coverage Status Legend

| Status | Definition |
|--------|-----------|
| **Full** | Covered in 2+ test repositories — can rely on automation |
| **Partial** | Covered in 1 repository only — supplement with manual testing |
| **None** | No E2E tests exist — manual regression testing required |
| **N/A** | Not applicable (backend-only, admin tools, config, docs) |

### Coverage Thresholds

| Range | Assessment |
|-------|-----------|
| ≥ 70% | Good coverage |
| 50–69% | Acceptable with caution |
| < 50% | High risk — significant manual testing required |

---

## Recommendation Decision Criteria

| Condition | Recommendation |
|-----------|---------------|
| 0 Critical PRs AND E2E coverage ≥ 70% | **GO** |
| 0 Critical PRs AND (coverage 50–69% OR 1–2 unresolved Medium PRs) | **CONDITIONAL GO** |
| 1+ Critical PRs OR coverage < 50% with patient-safety impact | **NO-GO** |

---

## E2E Test Action Evaluator Logic

### Action Decisions

| Action | When |
|--------|------|
| **CREATE** | New feature with no coverage, or critical bug fix exposed a gap |
| **UPDATE** | PR changes feature behavior, new validation rules, UI changes |
| **DELETE** | Feature removed, test made obsolete, duplicate coverage |

### Priority

| Priority | Scope |
|----------|-------|
| **P0 (Critical)** | Patient safety, security, clinical calculations, data corruption risks |
| **P1 (High)** | New patient-facing features, breaking changes, core workflow modifications |
| **P2 (Medium)** | Bug fixes, enhancements, non-critical flows |

### Effort

| Size | Hours |
|------|-------|
| **S** | 1–2h |
| **M** | 3–6h |
| **L** | 7–12h |

**Framework Selection:** Use `context/e2e-test-coverage-map.md` Quick Reference Table. Do not hardcode framework-to-area mappings.

---

## Analysis Sections

### 1. Executive Summary
Max 200 words. Overall risk level with justification, release composition table, key changes (top 3–5), critical gaps.

### 2. PR Analysis Summary
High-level table of ALL PRs. Table only — no verbose explanations. Risk rationale max 10 words per row.

### 3. Test Coverage Analysis (Medium/Critical Risk PRs ONLY)
Skip Low risk and N/A. Per-PR unit/integration/E2E gaps with specific file paths and test case names. Synthesize `@hb-code-review` Section 4 findings.

### 4. Automated Regression Test Coverage
Sections 4.1–4.5 per template. Include ALL functional tickets in 4.1 (not filtered by risk). Use keyword-first search across ALL test directories.

### 5. Hotfix Pattern Analysis
Apply patterns per-PR based on each PR's ticket prefix. Group findings by repository. Use repo-specific tables from `context/historical-bugfix-patterns.md`.

### 6. Risk Mitigation
Three-tier priorities: Critical (blocking) → High (must test) → Medium (should test).

### 7. Go/No-Go Recommendation
Exactly one of: GO / CONDITIONAL GO / NO-GO. Apply decision criteria above. 1–2 sentence justification.

### 8. Post-Release Monitoring
Critical metrics (24h), actions timeline (0–4h, Week 1), warning signs.

---

## Slack Message: Test Coverage Format

**CRITICAL:** Use LISTS, not tables — Slack doesn't render markdown tables.

```
## TEST COVERAGE

**E2E Regression Coverage:** XX%

- **E2E Coverage:** XX% (calculated per formula above)
- **PRs with Unit Tests:** X/Y
```

Unit test coverage reported as ratio (not percentage) — per-PR unit test depth varies too much for meaningful aggregate %.

---

## Word Count Enforcement

| Report | Limit | Enforcement |
|--------|-------|-------------|
| Risk Assessment | 1500 words | HARD FAIL |
| Release Notes | 800 words | HARD FAIL |
| Slack Message | 500 words (target 250–350) | HARD FAIL |

**If exceeded:**
1. STOP — do not save report
2. Display word count breakdown by section
3. Apply content filtering: exclude low-risk PRs from Section 2, summarize Section 3 by count only
4. Wait for user instruction — do NOT auto-regenerate

**Small releases (<5 PRs):** Omit Section 3.3, condense Section 4 subsections. Under 1500 words is acceptable.

**Per-section targets (Risk Assessment):**

| Section | Target |
|---------|--------|
| 1. Executive Summary | ~200 words |
| 2. PR Table | ~150 words |
| 3. Test Coverage | ~400 words |
| 4. E2E Coverage | ~400 words |
| 5. Hotfix Patterns | ~150 words |
| 6. Risk Mitigation | ~200 words |
| 7–8. Recommendation + Monitoring | ~400 words combined |

---

## Quality Rules

### AVOID
- Generic statements not tied to specific PR findings
- Vague terms like "improve testing" without file paths and test cases
- Duplicate content across sections
- AI-generated PR names — use original PR titles only

### REQUIRE
- Every recommendation linked to a specific PR number
- Exact file paths for missing tests
- Specific test scenarios with clear acceptance criteria
- Original PR titles from GitHub
- E2E test repository search results for each functional area

---

## Pre-Submission Checklists

### Report 1: Risk Assessment

- [ ] All 8 sections present
- [ ] Section 1: Release Composition Table with category counts
- [ ] Section 2: ALL tickets in PR summary table
- [ ] Section 3: Only Medium/Critical risk PRs analyzed
- [ ] Section 4.1: ALL functional tickets included
- [ ] Section 4.5: E2E Test Maintenance Action Plan (MANDATORY)
- [ ] Section 5: Repo-specific pattern tables applied per-PR
- [ ] Section 7: Exactly one verdict (GO/CONDITIONAL GO/NO-GO)
- [ ] Word count ≤ 1500
- [ ] Original PR titles used (no AI summaries)

### Report 2: Release Notes

- [ ] Header, Highlights, Changes by Area, Bug Fixes, Footer present
- [ ] Only customer-visible changes included
- [ ] Infrastructure/refactoring/test-only PRs excluded
- [ ] Word count ≤ 800

### Report 3: Slack Message

- [ ] All 7 sections present (metadata, highlights, risks, coverage, manual testing, recommendation, link)
- [ ] Starts with positive highlights
- [ ] Uses lists, NOT tables
- [ ] E2E coverage formula applied correctly
- [ ] N/A PRs excluded from coverage calculation
- [ ] Word count ≤ 500
