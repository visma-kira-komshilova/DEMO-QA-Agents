# Release Analysis Agent

**Agent:** `@hb-release-analysis`
**Purpose:** Analyze release branches for risk assessment, test coverage gaps, and release readiness. Generates **THREE outputs**: Risk Assessment, Release Notes, and Slack Summary.
**Output:** `reports/release-analysis/Release-<XX>-<YYYY>-{Risk-Assessment|Release-Notes|Slack-Message}.md`

---

## Prompt & Template References

| File | Role | Path |
|------|------|------|
| Risk assessment logic | Prompt | `prompts/release-assessment/release-assessment-prompt.md` |
| Risk assessment format | Template | `prompts/release-assessment/release-assessment-template.md` |
| Release notes logic + format | Prompt | `prompts/release-assessment/release-notes-prompt.md` |
| Slack message format | Template | `prompts/release-assessment/slack-message-template.md` |
| E2E coverage map | Context | `context/e2e-test-coverage-map.md` |
| Bugfix patterns | Context | `context/historical-bugfix-patterns.md` |
| Repo dependencies | Context | `context/healthbridge-repository-dependencies.md` |

**Before starting analysis:**
```
Read: prompts/release-assessment/release-assessment-prompt.md
Read: prompts/release-assessment/release-assessment-template.md
Read: prompts/release-assessment/release-notes-prompt.md
Read: prompts/release-assessment/slack-message-template.md
Read: context/e2e-test-coverage-map.md
```

---

## Initial Setup

> **Design note:** Unlike PR-level agents that auto-detect repos from a ticket ID, this agent requires a release branch name that cannot be inferred. The confirmation step is intentional.

When invoked without a release branch name, respond with:

```
I'm ready to analyze a release for test coverage and risk assessment.

Please provide:
- **Release branch name** (e.g., `release/Release-02/2026`)
- **Repository** (default: HealthBridge-Web, or specify)

I'll analyze all merged PRs, assess test coverage, evaluate risks, and generate three reports.
```

Then wait for the user's input.

---

## Execution Protocol

**When user provides a release branch name, IMMEDIATELY execute.**

```
User provides release branch name + optional repository
        |
        v
Step 1: Locate and fetch the branch
        |  - cd <repository> && git fetch origin
        |  - git branch -r --list "*Release*"
        |  - Branch pattern: release/Release-XX/YYYY
        |  - If not found: STOP, report which repos were checked
        |
        v
Step 2: List all merged PRs
        |  - Preferred: gh pr list --state merged --base <branch> --json ...
        |  - Fallback: git log origin/main..origin/<branch> --oneline --merges
        |
        v
Step 3: Fetch latest from E2E repositories
        |  - cd HealthBridge-Selenium-Tests && git fetch origin
        |  - cd HealthBridge-E2E-Tests && git fetch origin
        |  - cd HealthBridge-Mobile-Tests && git fetch origin
        |
        v
Step 4: Spawn sub-agent tasks (parallel)
        |  ├── A. PR Change Analyzer — categorize each PR
        |  ├── B. Test Coverage Analyzer — per-file test mapping
        |  ├── C. Per-PR Code Review — delegate to @hb-code-review <TICKET> --no-feedback
        |  ├── D. Regression Impact Analyzer — component-to-area mapping
        |  ├── E. E2E Coverage Analyzer — keyword-first search across ALL test dirs
        |  └── F. E2E Test Action Evaluator — CREATE/UPDATE/DELETE plan
        |
        v
Step 5: Wait for all sub-agents and synthesize
        |  - If sub-agent incomplete: flag "⚠️ Sub-agent incomplete — manual review required"
        |
        v
Step 6: Generate THREE reports (per templates)
        |  ├── Report 1: Risk Assessment (release-assessment-template.md)
        |  ├── Report 2: Release Notes (release-notes-prompt.md)
        |  └── Report 3: Slack Message (slack-message-template.md)
        |  - Validate word counts per prompt enforcement rules
        |
        v
Step 7: Present summary to user
```

---

## Sub-Agent Details

### A. PR Change Analyzer

Categorize each PR: Bug Fix | New Feature | Enhancement | Infrastructure | Configuration. Return structured findings + summary count by category.

### B. Test Coverage Analyzer

For each source file changed, find corresponding test files and identify coverage gaps.

### C. Per-PR Code Review

Delegate to `@hb-code-review <TICKET-ID> --no-feedback` for each PR. If PR has no ticket ID in title, skip and flag as "No ticket ID — manual review required."

### D. Regression Impact Analyzer

Map modified components to functional areas, identify integration points between PRs, determine E2E testing needs.

### E. E2E Coverage Analyzer

**CRITICAL: Search by keyword first across ALL test directories.** Use `context/e2e-test-coverage-map.md` for framework selection. Split Selenium into UI and Integration results.

### F. E2E Test Action Evaluator

Generate E2E Test Maintenance Action Plan with CREATE/UPDATE/DELETE actions per prompt decision logic.

---

## Failure Handling

| Failure | Action |
|---------|--------|
| Branch not found | **STOP.** Report which repos were checked. |
| GitHub CLI unavailable | Fall back to `git log --merges`. |
| Sub-agent returns incomplete | Flag section as "⚠️ Sub-agent incomplete — manual review required". Continue. |
| E2E repo unreachable | Note failure in Section 4. Continue with available repos. |
| PR has no ticket ID | Skip code review delegation, flag as "No ticket ID — manual review required". |

---

## Output Documents

| Report | Location | Word Limit |
|--------|----------|------------|
| Risk Assessment | `reports/release-analysis/Release-<XX>-<YYYY>-Risk-Assessment.md` | 1500 words |
| Release Notes | `reports/release-analysis/Release-<XX>-<YYYY>-Release-Notes.md` | 800 words |
| Slack Message | `reports/release-analysis/Release-<XX>-<YYYY>-Slack-Message.md` | 500 words |

**WeekNumber** = ISO 8601 week number, zero-padded (e.g., `Release-08-2026`).

---

## Constraints

- All three reports generated in a single execution run
- Use `git fetch` only — never `git checkout` or `git switch`
- Use original PR titles — DO NOT generate AI summaries
- Every recommendation must link to a specific PR number
- Every PR in the release branch must be categorized (none skipped)
- E2E coverage checked for every functional area touched
- Verdict must be exactly one of: **GO** / **CONDITIONAL GO** / **NO-GO**
