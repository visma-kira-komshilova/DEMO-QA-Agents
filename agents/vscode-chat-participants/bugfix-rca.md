# Bugfix Root Cause Analysis Agent

**Agent:** `@hb-bugfix-rca`
**Purpose:** Root cause analysis for production bugfixes. Supports both hotfix (known release) and investigation (unknown origin) modes. Generates TWO reports: RCA Analysis + E2E Test Recommendations.
**Output:** `reports/bugfix-rca/<TICKET-ID>-rca.md` and `reports/bugfix-rca/<TICKET-ID>-e2e-test-recommendations.md`

---

## Prompt & Template References

| File | Role | Path |
|------|------|------|
| Analysis logic | Prompt | `prompts/bugfix-rca/bugfix-rca-prompt.md` |
| RCA report format | Template | `prompts/bugfix-rca/bugfix-rca-template.md` |
| E2E report format | Template | `prompts/bugfix-rca/bugfix-rca-e2e-template.md` |
| Bugfix patterns | Context | `context/historical-bugfix-patterns.md` |
| E2E coverage map | Context | `context/e2e-test-coverage-map.md` |
| Repo dependencies | Context | `context/healthbridge-repository-dependencies.md` |

**Before starting analysis:**
```
Read: prompts/bugfix-rca/bugfix-rca-prompt.md
Read: prompts/bugfix-rca/bugfix-rca-template.md
Read: prompts/bugfix-rca/bugfix-rca-e2e-template.md
Read: context/historical-bugfix-patterns.md
```

---

## Execution Protocol

**No initial prompt.** Begin analysis immediately when the user provides a ticket ID, per CLAUDE.md execution protocol.

---

## Analysis Modes (Auto-Detected)

Mode is **always auto-detected** from input. The user does NOT need to specify it.

| Input Pattern | Detected Mode | What Happens |
|---------------|---------------|-------------|
| `HM-14200 Release-3/2026` | Hotfix Mode | Compares bugfix branch vs release branch |
| `HM-14200` (ticket ID only) | Investigation Mode | Searches git history for the fix |
| `hotfix HM-14200 Release-3/2026` | Hotfix Mode | Explicit hotfix + release provided |
| `hotfix HM-14200` (no release) | Investigation Mode | No release specified, fall back |
| `investigate HM-14200` | Investigation Mode | Explicit investigation request |

---

## HM-* Multi-Repository Disambiguation (CRITICAL)

When the ticket prefix is `HM-*`, the branch may exist in **any of 4 repositories**. The agent MUST search all of them.

```bash
for repo in HealthBridge-Web HealthBridge-Api HealthBridge-Claims-Processing HealthBridge-Prescriptions-Api; do
  cd "$repo" && git fetch origin && git branch -r --list "*<TICKET_ID>*" && cd ..
done
```

| Result | Action |
|--------|--------|
| Branch in exactly 1 repo | Use that repo. Report: "Branch found in `<repo>`" |
| Branch in multiple repos | Analyze ALL. Report: "Branch found in X repos: `<list>`. Analyzing all." |
| Branch in 0 repos | **STOP.** Report which repos were checked. |

**Mandatory:** Every RCA report MUST state which repository was analyzed and how it was selected.

---

## Workflow

```
User provides ticket ID (+ optional release)
        |
        v
Step 1: Auto-detect mode (Hotfix or Investigation)
        |
        v
Step 2: Locate branch (HM-* disambiguation if needed)
        |  - git fetch origin, search all candidate repos
        |  - Filter commits by ticket ID (per CLAUDE.md)
        |
        v
Step 3: Get bugfix changes
        |  - Hotfix: compare bugfix vs release branch
        |  - Investigation: compare bugfix vs main
        |
        v
Step 4: Trace causative commit
        |  - Hotfix: search release for causative PR
        |  - Investigation: git blame, git log -S, history search
        |
        v
Step 5: Analyze root cause (see prompt for framework)
        |  - Pattern matching against historical-bugfix-patterns.md
        |  - 5 Whys analysis
        |  - Preventability assessment
        |
        v
Step 6: E2E coverage analysis
        |  - Fetch all E2E repos (per CLAUDE.md)
        |  - Keyword-first search strategy
        |
        v
Step 7: Generate BOTH reports
        |  - RCA: prompts/bugfix-rca/bugfix-rca-template.md
        |  - E2E: prompts/bugfix-rca/bugfix-rca-e2e-template.md
        |
        v
Present Summary to User
```

---

## Failure Handling

| Failure | Action |
|---------|--------|
| Branch not found in any repo | **STOP.** Report which repos were checked. Suggest: verify ticket ID, check if pushed. |
| Causative commit not found | Mark Section 3 "Bug Introduced" as "Unknown". Continue with fix diff as evidence. Add recommendation for manual investigation. |
| Git blame inconclusive (merge/formatting commit) | Follow merge to source PR. If still inconclusive, use `git log -p -S`. If all fail, use "Unknown" procedure. |
| Release branch not found (Hotfix Mode) | Report: "Release branch not found. Falling back to Investigation Mode." Switch mode. |
| 0 ticket-specific commits | Analyze all commits with warning. |
| E2E repo fetch fails | Note failure in E2E report. Continue with available repos. |

---

## Constraints

- **RCA Report:** Maximum 1500 words
- **E2E Report:** No word limit
- Always generate BOTH reports
- Bugfix Pattern Match section is MANDATORY in every RCA report
- file:line references for all code analysis
- Use remote refs only — never `git checkout` for analysis
- Filter commits by ticket ID with `--no-merges`
- Repository selection MUST be reported in Executive Summary
- Use correct pattern table for the repository (not just branch prefix)
- Every recommendation must link to specific analysis findings
- Generate implementable test code, not just descriptions

---

**Generated reports location:** `reports/bugfix-rca/`
