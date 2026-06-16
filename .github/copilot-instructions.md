# PowerGrid — Repository-wide Copilot Instructions

## Overview

Livewire PowerGrid is a Laravel package that generates advanced, highly-configurable data tables powered by Livewire. The core class users extend is `PowerGridComponent` (`src/PowerGridComponent.php`). Public API surfaces include `Column`, `Button`, `PowerGridFields`, and the trait-based `Concerns/` system.

## Language & Stack

- **PHP 8.2+** — use typed properties, match expressions, named arguments, and constructor promotion where idiomatic.
- **Laravel 10–13 / Livewire 3–4** — follow their conventions (service providers, facades, Livewire components, Blade views).
- **Pest PHP** for all tests. Write expressive, describe-style tests that mirror existing tests under `tests/Feature/` and `tests/Unit/`.
- **PHPStan level 9** (`phpstan.neon`) — all new code must pass static analysis.
- **Laravel Pint** for code style (`pint.json`). Always format changed files.
- **Vite + Yarn** for JS/CSS assets. Run `npm run build` (or `yarn build`) if any file under `resources/` is modified.

## Directory Layout (key paths)

```
src/
  PowerGridComponent.php   # Base Livewire component users extend
  Column.php               # Column definition fluent API
  Button.php               # Action button fluent API
  Concerns/                # Trait-based feature modules (Filter, Sorting, Checkbox…)
  Components/              # SetUp sub-components (Header, Footer, Detail…)
  DataSource/              # Eloquent / Collection query builders
  Commands/                # Artisan commands (create, publish, update)
  Themes/                  # Tailwind / Bootstrap5 / DaisyUI theme adapters
  Testing/                 # Test helpers exposed to end users
resources/
  views/                   # Blade templates per theme framework
tests/
  Feature/                 # Full integration tests
  Unit/                    # Unit tests
  Concerns/Components/     # Reusable test table components
.github/
  instructions/            # Role-based agent instruction files
  prompts/                 # Reusable prompt templates
  workflows/               # CI workflows + copilot-setup-steps.yml
```

## Validation Commands (always run before committing)

```bash
# Code style check (Pint)
composer test:pint

# PHPStan static analysis (level 9)
composer test:types

# LaraDumps debug-statement scan
composer ds:check

# Run full test suite (SQLite, in-memory)
composer test:sqlite

# Run all checks together (preferred)
composer verify

# Rebuild JS/CSS assets (only when resources/ files changed)
yarn build
```

## Coding Standards

- Follow PSR-12 and the existing Pint configuration in `pint.json`.
- Prefer fluent, chainable builder APIs consistent with `Column::make()` and `Button::make()`.
- Do **not** add `dump()`, `dd()`, or `var_dump()` calls — the `ds:check` script will catch and fail on these.
- Keep `src/` focused on the package; never add application-specific or demo code there.
- Match the existing namespace `PowerComponents\LivewirePowerGrid\`.
- Add PHPDoc blocks only where they add meaning beyond the type signature.

## Public API Compatibility

- Treat every `public` method and property in `src/` as public API.
- **Do not rename, remove, or change the signature of any existing public method** without a deprecation cycle.
- New optional parameters must have defaults that preserve existing behavior.
- Breaking changes require a major version bump — flag them explicitly in the PR description.

## Security & Safety

- Never commit secrets, credentials, tokens, or environment values.
- Validate and sanitize all user-facing input that reaches Blade or database queries.
- Use parameter binding for all raw DB queries; never concatenate user input into SQL.
- New Livewire component properties exposed to the browser must be marked `#[Locked]` or validated server-side.

## Test Requirements

- Every bug fix must include a regression test that fails before the fix and passes after.
- Every new public feature must include Feature tests covering the happy path and at least one edge case.
- Tests live in `tests/Feature/` or `tests/Unit/`; reusable Livewire test components live in `tests/Concerns/Components/`.
- Use the existing `TestCase` (`tests/TestCase.php`) and `Helpers` (`tests/Helpers.php`) patterns.
- Do not remove or weaken existing tests.

## Task-Routing Guide

| Task type             | Use instruction file                         | Use prompt template                          |
| --------------------- | -------------------------------------------- | -------------------------------------------- |
| Bug fix               | `.github/instructions/bug-fix.instructions.md`   | `.github/prompts/bug-triage.prompt.md`       |
| New feature           | `.github/instructions/feature.instructions.md`   | `.github/prompts/refactor.prompt.md`         |
| Writing/adding tests  | `.github/instructions/test.instructions.md`      | `.github/prompts/regression-test.prompt.md`  |
| Documentation update  | `.github/instructions/docs.instructions.md`      | —                                            |
| Release / changelog   | `.github/instructions/release.instructions.md`   | `.github/prompts/release-prep.prompt.md`     |
| Minimal reproduction  | —                                            | `.github/prompts/minimal-reproduction.prompt.md` |

When the task scope is ambiguous, default to the **bug-fix** instruction file and open a question to the user before making changes.
