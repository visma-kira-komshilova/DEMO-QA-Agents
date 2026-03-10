# Bugfix RCA Analysis Prompt

You are performing root cause analysis for a production bugfix. Follow this analysis framework for both Hotfix and Investigation modes.

---

## Mode-Specific Git Commands

### Hotfix Mode (Known Release)

**Step 1: Locate both branches**
```bash
cd "<repository-path>" && git fetch origin && git branch -r --list "*<TICKET_ID>*"
git branch -r --list "*Release-<WEEK>/<YEAR>*"
```

**Step 2: Filter commits by ticket ID**
```bash
git rev-list --count --no-merges origin/main..origin/bugfix/<TICKET_ID>
git log --no-merges origin/main..origin/bugfix/<TICKET_ID> --oneline --grep="<TICKET_ID>"
git rev-list --count --no-merges origin/main..origin/bugfix/<TICKET_ID> --grep="<TICKET_ID>"
```

Report: "Branch contains X total commits (excluding merges), analyzing Y commits specific to <TICKET_ID>"

**Step 3: Get bugfix changes**
- All commits match ticket → standard `git diff`
- Mixed commits → ticket-specific commits only

**Step 4: Search release for causative PR**
```bash
git log origin/release/Release-<WEEK>/<YEAR> --oneline -- "<affected-file-path>"
git log origin/release/Release-<WEEK>/<YEAR> -p -- "<affected-file-path>"
```

**Step 5: Compare before/after**
```bash
git show origin/release/Release-<PREV_WEEK>/<YEAR>:<file-path>
git show origin/release/Release-<WEEK>/<YEAR>:<file-path>
git show origin/bugfix/<TICKET_ID>:<file-path>
```

### Investigation Mode (Unknown Origin)

**Step 1: Get bugfix changes**
```bash
cd "<repository-path>" && git fetch origin && git branch -r --list "*<TICKET_ID>*"
git diff origin/main..origin/bugfix/<TICKET_ID> --stat
```

Filter by ticket ID if branch contains mixed commits.

**Step 2: Search git history for origin**
```bash
git log -p --all -S '<problematic-code-snippet>' -- "<file-path>"
git log --oneline --all -- "<file-path>"
git blame origin/main -- "<file-path>"
```

**Step 3: Identify causative commit**
```bash
git show <commit-hash>
git log --oneline --merges --ancestry-path <commit-hash>..origin/main
git branch -r --contains <commit-hash> --list "*Release*"
```

**Step 4: Trace full history** (dynamic 12-month lookback)
```bash
git log --oneline --since="$(date -v-12m +%Y-%m-%d 2>/dev/null || date -d '12 months ago' +%Y-%m-%d)" -- "<file-path>"
```

Note: `date -v-12m` is macOS, `date -d '12 months ago'` is Linux. The `||` handles both.

---

## Root Cause Analysis Framework

### Pattern Matching (CRITICAL)

Read `context/historical-bugfix-patterns.md` for the repository-to-pattern routing table. Use the correct table based on the analyzed repository.

**Combined Score Calculation:**

- **Primary Pattern:** EXACT MATCH. Report its historical percentage.
- **Secondary Pattern:** PARTIAL match. Note separately.
- **Combined Score = Primary pattern % only.** Do NOT sum percentages.

Example: Edge Cases (26%) EXACT + NULL Handling PARTIAL → Combined Score: 26%. "26% of historical hotfixes match this primary pattern. NULL Handling noted as secondary factor."

The percentages represent independent category frequencies, not additive probabilities.

### 5 Whys Analysis

1. **Why** did the bug occur? → [Technical cause]
2. **Why** did [1] happen? → [Design/implementation issue]
3. **Why** did [2] happen? → [Process gap]
4. **Why** did [3] happen? → [Knowledge/resource gap]
5. **Why** did [4] happen? → [Systemic root cause]

Must reach a systemic root cause. Do not stop at the surface.

### Preventability Assessment

| Testing Layer | Could Prevent? | Specific Gap |
|---------------|---------------|--------------|
| **Unit Tests** | Yes/No | [Missing test case] |
| **Integration Tests** | Yes/No | [Missing scenario] |
| **E2E Automated Tests** | Yes/No | [Missing workflow] |
| **Manual Acceptance** | Yes/No | [Missing test case] |
| **Code Review** | Yes/No | [What reviewer should catch] |
| **Requirements** | Yes/No | [Specification gap] |

---

## E2E Coverage Analysis

Fetch latest from E2E repos per CLAUDE.md protocol. Use keyword-first search strategy across all test directories.

### How to Parse the Coverage Map

1. Identify the **functional area** affected by the bug
2. Read `context/e2e-test-coverage-map.md` — find the matching row
3. For each framework column:
   - "Yes" → search for existing tests using the Search Keywords from the Detailed section
   - "No" → report "N/A — Outside scope"
4. Use the Search Keywords across ALL test directories (keyword-first, not folder-first)

### Coverage Status Definitions

| Status | Definition |
|--------|-----------|
| **Full** | Tests cover happy path AND at least one edge case relevant to the bug |
| **Partial** | Tests exist but only happy path, or don't cover the specific bug scenario |
| **Gap** | Framework covers this area (per map) but no tests exist for this feature |
| **N/A** | Functional area outside scope of this framework (per map) |

---

## Pre-Submission Checklist

### RCA Report
- [ ] All 7 sections completed per template
- [ ] Repository identified and selection method stated (Section 1)
- [ ] Correct pattern table used — based on repository, not just branch prefix (Section 2)
- [ ] Combined Score = primary % only, not summed (Section 2)
- [ ] Timeline dates populated or explicitly "Unknown" (Section 3)
- [ ] Both buggy and fixed code snippets included (Section 4)
- [ ] 5 Whys reaches systemic root cause (Section 5)
- [ ] All 6 prevention layers assessed (Section 6)
- [ ] Recommendations link to specific findings (Section 7)
- [ ] Word count ≤ 1500

### E2E Report
- [ ] All 5 sections completed per template
- [ ] If E2E configured: Coverage table has N rows (one per framework)
- [ ] If no E2E: E2E section shows N/A note
- [ ] Status uses defined thresholds (Full/Partial/Gap/N/A)
- [ ] Each scenario has Priority, Repository, Preconditions, Steps, Expected Result
- [ ] Implementation code provided for Playwright + Selenium
- [ ] Regression suite integration guidance included

---

**File Location:** `prompts/bugfix-rca/bugfix-rca-prompt.md`
