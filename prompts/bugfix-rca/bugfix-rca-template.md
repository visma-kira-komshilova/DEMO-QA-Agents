# Bugfix RCA Template

Output format for RCA report. For analysis logic, see `bugfix-rca-prompt.md`. For E2E report format, see `bugfix-rca-e2e-template.md`.

---

## Document Structure

```markdown
# Root Cause Analysis: <TICKET-ID>

**Branch:** `<branch-name>`
**Repository:** `<repo-name>` _(selected because: [reason])_
**Mode:** Hotfix / Investigation
**Release:** `<release-branch>` _(Hotfix mode only)_
**Date:** {date}

---

<details>
<summary><strong>1. Executive Summary</strong></summary>

| Field | Details |
|-------|---------|
| **Repository** | {repo name — how it was selected} |
| **Bug Description** | {what went wrong} |
| **Causative PR/Commit** | {PR # or commit hash, or "Unknown — not identifiable from git history"} |
| **Root Cause Category** | {Edge Case / NULL Handling / Logic Error / etc.} |
| **Preventability Verdict** | Preventable / Partially Preventable / Not Preventable |
| **Severity** | Critical / High / Medium / Low |
| **Environment** | Production / Staging / Testing |

</details>

<details>
<summary><strong>2. Bugfix Pattern Match</strong></summary>

> **MANDATORY SECTION — Do not skip.**
> Use the pattern table matching the **analyzed repository**.
> See `context/historical-bugfix-patterns.md` for all pattern tables.

| Pattern | % of Historical Bugs | Match? | Evidence |
|---------|---------------------|--------|----------|
| {Pattern 1} | XX% | EXACT/PARTIAL/No Match | {evidence} |
| {Pattern 2} | XX% | EXACT/PARTIAL/No Match | {evidence} |
| {Pattern 3} | XX% | EXACT/PARTIAL/No Match | {evidence} |
| {Pattern 4} | XX% | EXACT/PARTIAL/No Match | {evidence} |
| {Pattern 5} | XX% | EXACT/PARTIAL/No Match | {evidence} |
| {Pattern 6+} | XX% | EXACT/PARTIAL/No Match | {evidence} |

**Primary Pattern Match:** {pattern name} ({percentage}%)
**Secondary Pattern:** {pattern name, if any}
**Combined Score:** {primary %}% of historical hotfixes match this primary pattern. {Secondary noted as secondary factor, if applicable.}

> Combined Score = Primary pattern % only. Do not sum primary + secondary.

**Why This Matters:** {How this pattern typically occurs and how to prevent it}

</details>

<details>
<summary><strong>3. Timeline</strong></summary>

| Event | Date | Details |
|-------|------|---------|
| Bug Introduced | YYYY-MM-DD | In PR #XXX / Release-XX (or "Unknown") |
| Bug Discovered | YYYY-MM-DD | {How discovered} |
| Bugfix Deployed | YYYY-MM-DD | In bugfix/<TICKET_ID> |

_(If approaching 1500-word limit, abbreviate to a single sentence.)_

</details>

<details>
<summary><strong>4. Technical Root Cause</strong></summary>

### Original Code (Buggy)

\`\`\`
{code snippet, 5-10 lines}
\`\`\`

**File:** `{file}:{line}`

### Fixed Code

\`\`\`
{code snippet, 5-10 lines}
\`\`\`

### Analysis

{Why the original code was incorrect. Reference file:line locations.}

</details>

<details>
<summary><strong>5. 5 Whys Analysis</strong></summary>

| # | Why? | Answer |
|---|------|--------|
| 1 | Why did {the bug} happen? | {direct cause} |
| 2 | Why did {direct cause} happen? | {deeper cause} |
| 3 | Why did {deeper cause} happen? | {underlying cause} |
| 4 | Why did {underlying cause} happen? | {systemic cause} |
| 5 | Why did {systemic cause} happen? | **{root cause}** |

**Root Cause:** {1-2 sentence summary}

</details>

<details>
<summary><strong>6. Preventability Assessment</strong></summary>

| Prevention Layer | Could it have caught this? | Gap |
|-----------------|---------------------------|-----|
| Requirements Analysis | Yes/No | {details} |
| Code Review | Yes/No | {details} |
| Unit Tests | Yes/No | {details} |
| Integration Tests | Yes/No | {details} |
| E2E Automated Tests | Yes/No | {details} |
| Manual Acceptance Testing | Yes/No | {details} |

**Most effective prevention:** {which layer would have been most effective}

</details>

<details>
<summary><strong>7. Recommendations</strong></summary>

1. **Immediate:** {action to prevent recurrence}
2. **Short-term:** {process or test improvement}
3. **Long-term:** {systemic fix or pattern prevention}

_(If approaching 1500-word limit, abbreviate to 2 items.)_

</details>
```

---

## Constraints

- Max 1500 words
- Bugfix Pattern Match (Section 2) is MANDATORY
- Combined Score = primary % only — do NOT sum
- Repository selection stated in header
- file:line references for all code analysis
- 5 Whys must reach systemic root cause
- If causative commit unknown, state "Unknown" — do not fabricate
- Sections 3 and 7 may be abbreviated if tight; Sections 1, 2, 4, 5, 6 must not

## Output Location

`reports/bugfix-rca/<TICKET-ID>-rca.md`
