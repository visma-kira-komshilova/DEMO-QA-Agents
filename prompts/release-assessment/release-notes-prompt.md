# Release Notes Generation Prompt

Analysis logic and output format for customer-facing release notes. For orchestration, see `agents/vscode-chat-participants/release-analysis.md`.

---

## Purpose

Transform technical PR information into clear, non-technical release notes that help customers understand what's new, improved, and fixed.

---

## Document Structure

```markdown
# HealthBridge Release Notes — Release XX/YYYY

**Release Date:** [Date]
**Version:** Release-XX/YYYY

---

## Highlights

- **[Feature Name]** — One sentence benefit description
- **[Feature Name]** — One sentence benefit description
- **[Feature Name]** — One sentence benefit description

---

## What's New and Improved

### [Functional Area]
- **[PR Title]** (PR #XXX) — Customer benefit explanation

### [Functional Area]
- **[PR Title]** (PR #XXX) — Customer benefit explanation

---

## User Interface Changes

### New UI Elements
- [Description of new buttons, screens, or features]

### Improved UI Elements
- [Description of enhanced existing elements]

---

## Bug Fixes

- **[Area]**: Fixed issue where [problem description] (PR #XXX)

---

**Need Help?** Contact HealthBridge Support
**Documentation:** [Link to relevant docs]

_Release notes generated for Release-XX/YYYY_
```

---

## Content Rules

### INCLUDE

| Type | Examples |
|------|----------|
| New features | New buttons, screens, calculations, reports |
| Feature improvements | Enhanced workflows, better performance visible to users |
| UI changes | New layouts, redesigned screens, new icons |
| Customer-impacting bug fixes | Issues users could encounter |
| Integration changes | New or improved external connections |

### EXCLUDE

| Type | Examples |
|------|----------|
| Infrastructure | CI/CD, Docker, deployment scripts |
| Internal tooling | Developer tools, test utilities |
| Refactoring | Code cleanup with no visible impact |
| Test-only changes | Unit tests, test fixtures |
| Dependency updates | NuGet/npm updates (unless security-related) |
| Documentation | README updates, code comments |

### PR Filtering Heuristics

Skip PRs where:
- Title contains: `refactor`, `cleanup`, `ci:`, `chore:`, `test:`, `docs:`, `bump`, `upgrade`, `dependency`
- Only files changed are in: `Tests/`, `.github/`, `docs/`

**Uncertain PRs:** Include under most relevant area with italicized note: *(internal change — included for completeness)*.

**No customer-visible changes:** Generate only Header and Footer with: *"This release contains internal improvements and infrastructure updates only."*

---

## Highlight Selection

Prioritize by: (1) highest customer visibility, (2) new features over improvements, (3) patient safety or compliance relevance. Exclude internal improvements even if large in scope. Max 5 items.

---

## Area Categorization

Map PR content to customer-facing areas. Align with `context/e2e-test-coverage-map.md`.

| Code Indicators | Area |
|-----------------|------|
| `Prescription`, `Medication`, `Dosage`, `Pharmacy` | Prescriptions & Medications |
| `Patient`, `Record`, `Chart`, `Admission`, `Discharge` | Patient Records & Charts |
| `Appointment`, `Schedule`, `Calendar`, `Booking` | Appointments & Scheduling |
| `Insurance`, `Claim`, `Billing`, `Payment` | Insurance & Billing |
| `Lab`, `Result`, `Diagnostic`, `TestOrder` | Lab Results & Diagnostics |
| `Report`, `Analytics`, `Dashboard`, `Export` | Reporting & Analytics |
| `Integration`, `API`, `Webhook`, `External` | Integrations |
| `UI`, `Button`, `Dialog`, `Form`, `Layout` | User Interface |

If unmappable, include under `[Other Improvements]`.

---

## Writing Guidelines

### Tone
- **Benefit-focused** — "You can now..." not "Added feature X"
- **Professional but friendly** — "We've improved..." not "The system now..."
- **Action-oriented** — "Easily export..." not "Export functionality added"

### Language

| Avoid | Use Instead |
|-------|-------------|
| "Implemented new endpoint" | "You can now connect to..." |
| "Fixed null reference exception" | "Fixed an issue that could cause errors when..." |
| "Refactored calculation logic" | "Improved accuracy of calculations for..." |
| "Updated database schema" | [Skip — internal change] |

### Formatting
- **Bold** for feature names
- Bullet points for lists
- 1–2 sentences per PR
- Include PR numbers for traceability

---

## Constraints

| Constraint | Value |
|------------|-------|
| Target length | 400–600 words |
| Maximum length | 800 words |
| Highlights | Max 5 items |
| Changes by Area | Max 2 sentences per PR |
| Bug Fixes | Max 5 items |

**Trim priority (if over limit):** (1) low-impact bug fixes, (2) minor UI changes, (3) improvement PRs. Never trim Highlights or patient-safety items.

---

## Output Location

`reports/release-analysis/Release-<XX>-<YYYY>-Release-Notes.md`
