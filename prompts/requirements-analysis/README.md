# Requirements Analysis Workflow

Automate the complete pipeline from requirements analysis through QA test planning and development estimation for the HealthBridge health management platform.

## Files

| File | Purpose |
|------|---------|
| `requirements-analysis.md` | Phase 1: Analysis logic |
| `requirements-analysis-template.md` | Phase 1: Report structure (contains canonical 7/10 scoring model) |

## Purpose

Orchestrate the end-to-end flow from requirements gathering to delivery planning with intelligent quality gating:

1. **Phase 1:** Analyze requirements -> Calculate completeness score (0-10)
2. **Decision Point:** If score >= 7/10, proceed to Phases 2+3. If score < 7/10, STOP and clarify with PO
3. **Phase 2:** Generate acceptance tests (Given/When/Then) from requirements — no codebase analysis
4. **Phase 3:** Generate development estimates with task breakdown

**Key Innovation:** Prevents wasted effort on incomplete requirements by gating based on quality score.

## Quick Start

**VS Code Chat:**
```
Analyze this requirement using requirements-analysis workflow:

Ticket: HM-XXXXX
Title: [Feature title]
Requirement: [Paste requirement text]
```

## When to Use

| Scenario | Use This Workflow? |
|----------|-------------------|
| New feature needs analysis + planning | Yes |
| Check if requirement is complete | Yes (stops at Phase 1 if score <7) |
| Only need QA test plan | Use `../qa-test-plan/` directly |
| Only need dev estimation | Use `../dev-estimation/` directly |
| Code review (already developed) | Use `../code-review-qa/` |

## How It Works

```
Phase 1: Requirements Analysis
  - Analyze business gaps, edge cases, integration impact
  - Score completeness using 7-dimension weighted model
  - Output: <TICKET>-requirements-analysis.md
        |
    Score >= 7/10?
   YES              NO → STOP, present critical questions to PO
    |
Phase 2: Acceptance Tests (no-codebase mode)
  - Given/When/Then scenarios from requirements
  - Output: <TICKET>-acceptance-tests.md
        |
Phase 3: Development Estimation
  - Task breakdown by repository with file paths
  - Output: <TICKET>-dev-estimation.md
        |
  SUCCESS: 3 Documents Delivered
```

## The 7/10 Threshold

Based on analysis of production hotfixes, incomplete requirements were a major defect contributor. The scoring system prevents planning when requirements are ambiguous.

**Scoring model:** See `requirements-analysis-template.md` Section 3 (canonical source for all 7 dimensions and weights).

| Score | Action |
|-------|--------|
| 9-10 | Complete — Proceed with confidence |
| 7-8 | Good — Proceed, note assumptions |
| 5-6 | Incomplete — STOP, clarify with PO |
| 1-4 | Poor/Inadequate — STOP, rewrite requirements |

## Output Locations

All reports saved to `reports/requirements-analysis/`:
- `[TICKET]-requirements-analysis.md` (Phase 1, always)
- `[TICKET]-acceptance-tests.md` (Phase 2, only if score >= 7)
- `[TICKET]-dev-estimation.md` (Phase 3, only if score >= 7)

## Key Constraints

- **Decision Threshold:** 7/10 (not configurable)
- **Repository Scope:** Only impacted repos identified in Phase 1
- **Word Limits:** Requirements Analysis + Dev Estimation in `requirements-analysis.md` (prompt); Acceptance Tests per `acceptance-tests-prompt.md`

## Related Templates

| Template | Path | Purpose |
|----------|------|---------|
| Requirements Analysis | `requirements-analysis-template.md` | Phase 1 report structure |
| Acceptance Tests | `../acceptance-tests/acceptance-tests-template.md` | Phase 2 report structure |
| Dev Estimation | `../dev-estimation/dev-estimation-template.md` | Phase 3 report structure |

## FAQ

**Score is 6/10?** NO — Clarify with PO first. Threshold is 7/10 to avoid rework.

**Skip Phase 3?** Use `../acceptance-tests/` directly for standalone test scenarios.

**Requirements changed?** Significant: Re-run all. Minor: Update Phase 2/3 only.

---

**Maintainer:** HealthBridge QA Team
