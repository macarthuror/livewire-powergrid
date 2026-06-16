---
applyTo: "**"
---

# Bug-Fix Agent Instructions

## Role

Diagnose and resolve confirmed bugs in the PowerGrid package with minimal, targeted changes. Never refactor unrelated code in the same PR.

## Scope

- Files under `src/` and `resources/` directly responsible for the reported behavior.
- The test suite (`tests/`) for the affected feature area.
- Do **not** touch unrelated files, change code style in unmodified lines, or alter public API signatures.

## Required Inputs

Before starting, confirm you have:

1. A clear description of the unexpected behavior.
2. The PHP / Laravel / Livewire versions where the bug was observed.
3. Steps or an existing test that reproduces the issue.
4. The expected vs. actual output.

If any input is missing, ask the user before proceeding.

## Workflow

1. **Reproduce** — run or write a failing test that demonstrates the bug (`composer test:sqlite`).
2. **Locate root cause** — trace the call path from the user-facing API into `src/`; identify the exact line(s) causing the issue.
3. **Fix** — apply the smallest possible change that resolves the issue without breaking other behavior.
4. **Regression test** — ensure the failing test now passes. Add a new test if one did not exist.
5. **Verify** — run `composer verify` and confirm all checks pass.
6. **Rebuild assets** — run `yarn build` only if files under `resources/` were modified.

## Required Checks (must all pass before committing)

```bash
composer test:pint     # Code style
composer test:types    # PHPStan level 9
composer ds:check      # No debug statements
composer test:sqlite   # Full test suite
```

## Prohibited Actions

- Do not rename or remove any public method or property.
- Do not add `dump()`, `dd()`, or `var_dump()` to production code.
- Do not introduce new Composer or npm dependencies without approval.
- Do not change behavior in code paths unrelated to the bug.

## Output Format

Produce a PR with:
- **Title**: `fix: <concise description>` (e.g., `fix: column filter not applied when using Collection datasource`)
- **Description**: what was broken, root cause, how it was fixed, link to issue if applicable.
- **Tests**: reference the new or updated test that covers the fix.
