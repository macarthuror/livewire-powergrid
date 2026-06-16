---
applyTo: "**"
---

# Test Agent Instructions

## Role

Write, improve, or expand automated tests for the PowerGrid package using Pest PHP, without modifying production code.

## Scope

- Files under `tests/Feature/`, `tests/Unit/`, and `tests/Concerns/Components/`.
- Read-only access to `src/` and `resources/` to understand behavior being tested.
- Do **not** modify any file outside `tests/` unless adding a missing test helper to `tests/Helpers.php` or `tests/TestCase.php`.

## Test Stack

- **Pest PHP** — use `it()`, `describe()`, `beforeEach()`, `dataset()`, and `expect()` API.
- **Orchestra Testbench** — the `TestCase` in `tests/TestCase.php` already bootstraps Laravel + Livewire. Extend it for all Feature tests.
- **Livewire testing utilities** — use `Livewire::test(MyTable::class)` for component interaction tests.
- **In-memory SQLite** — the default test database; configured in `phpunit.xml`.

## Reusable Test Infrastructure

| Path | Purpose |
|------|---------|
| `tests/TestCase.php` | Base test case with app bootstrapping |
| `tests/Helpers.php` | Data factories and shared utilities |
| `tests/Concerns/Components/` | Reusable Livewire table components used across multiple tests |
| `tests/Datasets/` | Pest datasets for parameterized tests |
| `tests/Plugins/Autoload.php` | Auto-loaded test setup |

Reuse existing test components from `tests/Concerns/Components/` before creating new ones. Create a new component in that directory only when the feature under test requires a configuration not covered by an existing component.

## Workflow

1. **Understand the behavior** — read the relevant `src/` code and existing tests for the feature area.
2. **Identify gaps** — determine which behaviors, edge cases, or error paths lack coverage.
3. **Write tests** — follow existing test file naming (`<Feature>Test.php`) and structure.
4. **Run the suite** — confirm all tests pass with `composer test:sqlite`.
5. **Check style** — run `composer test:pint` to ensure test files are formatted correctly.

## Required Checks (must all pass before committing)

```bash
composer test:pint     # Code style (applies to test files too)
composer test:sqlite   # Full test suite
```

## Test Writing Guidelines

- Each `it()` description must clearly state what is being asserted, e.g., `it('filters rows by date range when using Eloquent datasource')`.
- Test one behavior per `it()` block.
- Use `dataset()` to cover multiple input variations without duplicating test logic.
- Assert concrete, observable outcomes (rendered HTML, Livewire property state, database changes), not internal implementation details.
- Do not use `dump()`, `dd()`, or `var_dump()` in tests.

## Prohibited Actions

- Do not remove or weaken existing test assertions.
- Do not modify production `src/` or `resources/` files.
- Do not skip tests with `->skip()` without a documented reason.

## Output Format

Produce a PR with:
- **Title**: `test: <concise description>` (e.g., `test: add coverage for custom sort with Collection datasource`)
- **Description**: which behaviors are now covered, what was missing before, and which test files were added or modified.
