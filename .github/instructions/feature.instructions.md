---
applyTo: "**"
---

# Feature Agent Instructions

## Role

Implement well-scoped new features or enhancements to the PowerGrid package, following its fluent-API patterns and ensuring backward compatibility.

## Scope

- New or modified files under `src/`, `resources/`, and `tests/`.
- Public-facing APIs must follow the existing fluent, chainable builder style (see `Column::make()`, `Button::make()`).
- All user-visible configuration must integrate with the existing `setUp()` array and `PowerGridComponent` lifecycle.

## Required Inputs

Before starting, confirm you have:

1. A clear description of the desired behavior and the use case it solves.
2. Confirmation that no existing feature already covers the use case.
3. Agreement on the public API shape (method names, parameters, return types).
4. Whether documentation needs to be updated in [powergrid-doc](https://github.com/Power-Components/powergrid-doc).

If any input is missing, ask the user before writing code.

## Workflow

1. **Design the API** — sketch the public interface and confirm it matches PowerGrid's fluent conventions. Present it to the user if unsure.
2. **Implement** — add the feature in `src/`. New Livewire component properties must be `#[Locked]` or server-side validated.
3. **Blade/Views** — add or update templates in `resources/views/` for all supported frameworks (Tailwind, Bootstrap 5, DaisyUI) as applicable.
4. **Write tests** — add Feature tests in `tests/Feature/` covering happy path, edge cases, and interactions with other features (e.g., pagination, sorting, filters).
5. **Verify** — run `composer verify` and confirm all checks pass.
6. **Rebuild assets** — run `yarn build` if `resources/` files were modified.

## Required Checks (must all pass before committing)

```bash
composer test:pint     # Code style
composer test:types    # PHPStan level 9
composer ds:check      # No debug statements
composer test:sqlite   # Full test suite
```

## Public API Compatibility Rules

- New public methods must have PHPDoc `@since` tags noting the version they were introduced.
- Optional new parameters must default to values that preserve existing behavior.
- Deprecate (not remove) any public method or property being superseded. Use `@deprecated` and trigger a PHP deprecation notice.
- Breaking changes require a major version bump; flag them explicitly in the PR description.

## Prohibited Actions

- Do not break existing tests or weaken their assertions.
- Do not introduce new dependencies without prior approval and an advisory-database check.
- Do not add application-specific or demo code inside `src/`.
- Do not add `dump()`, `dd()`, or `var_dump()` to production code.

## Output Format

Produce a PR with:
- **Title**: `feat: <concise description>` (e.g., `feat: add sticky columns support`)
- **Description**: motivation, API design decisions, affected files, and whether documentation needs updating.
- **Tests**: list the new Feature/Unit test files added.
- **Breaking changes**: explicitly call out any, or state "No breaking changes."
