# E2E Test Recommendations Template

Output format for E2E test recommendations report (generated alongside RCA report). For analysis logic, see `bugfix-rca-prompt.md`.

---

## Document Structure

```markdown
# E2E Test Recommendations: <TICKET-ID>

**Related RCA:** `reports/bugfix-rca/<TICKET-ID>-rca.md`
**Date:** {date}

---

<details>
<summary><strong>1. Summary</strong></summary>

{What E2E tests are needed and why, linked to the RCA findings.}

</details>

<details>
<summary><strong>2. Existing Coverage Analysis</strong></summary>

**If E2E automation is configured:**

| Framework | Existing Tests | Coverage Status |
|-----------|---------------|-----------------|
| Selenium UI | {tests found or "None"} | Full / Partial / Gap / N/A |
| Selenium Integration | {tests found or "None"} | Full / Partial / Gap / N/A |
| Playwright | {tests found or "None"} | Full / Partial / Gap / N/A |
| Mobile | {tests found or "None"} | Full / Partial / Gap / N/A |

**If NO E2E automation (Q4.1 = "none"):**
> N/A — No E2E test automation configured for this project.

</details>

<details>
<summary><strong>3. Recommended Test Scenarios</strong></summary>

### Scenario 1: {Name}
- **Priority:** P0 / P1 / P2
- **Repository:** {Selenium / Playwright / Mobile}
- **Preconditions:** {setup required}
- **Steps:**
  1. {step}
  2. {step}
- **Expected Result:** {outcome}

### Scenario 2: {Name}
...

</details>

<details>
<summary><strong>4. Implementation Code</strong></summary>

### For Playwright (TypeScript)
\`\`\`typescript
{implementable test code}
\`\`\`

### For Selenium (Python)
\`\`\`python
{implementable test code}
\`\`\`

</details>

<details>
<summary><strong>5. Regression Suite Integration</strong></summary>

{How to integrate these tests into existing suites — file locations, naming conventions, CI pipeline considerations.}

</details>
```

---

## Constraints

- No word limit
- If E2E configured: Coverage table has N rows (one per framework)
- If no E2E: E2E section shows N/A note
- Status uses defined thresholds: Full / Partial / Gap / N/A
- Each scenario requires: Priority, Repository, Preconditions, Steps, Expected Result
- Implementation code must be runnable, not pseudo-code
- Always generate alongside the RCA report

## Output Location

`reports/bugfix-rca/<TICKET-ID>-e2e-test-recommendations.md`
