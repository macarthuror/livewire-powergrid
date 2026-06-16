---
mode: "agent"
---

# Refactor Request Prompt

Use this prompt to safely refactor existing PowerGrid code without changing observable behavior.

---

## Refactor Request

**Repository:** `power-components/livewire-powergrid`

### Context

<!-- Provide the information below before the agent proceeds. -->

**Target file(s) or area:**

```
<list the src/ files or feature area to refactor>
```

**Motivation (why is the refactor needed?):**

```
<e.g., reduce duplication, improve readability, prepare for a new feature, fix technical debt>
```

**Known constraints:**

```
<e.g., public API must not change, Livewire wire:model compatibility must be preserved>
```

---

## Agent Instructions

Using the context above and following the baseline instructions in `.github/copilot-instructions.md`, perform the refactor under the following rules:

### Non-Negotiable Rules

1. **No behavior change** — every existing test must still pass after the refactor. Do not modify, remove, or weaken any test assertions.
2. **No public API changes** — do not rename, remove, or change the signature of any public method or property. Internal (private/protected) changes are fine.
3. **One concern per PR** — if the refactor scope grows, split it into smaller PRs.
4. **No new dependencies** — do not add Composer or npm packages.

### Workflow

1. Read and understand the target files fully before making any changes.
2. Run `composer test:sqlite` to establish a green baseline.
3. Make incremental changes, running tests frequently.
4. Run `composer verify` when done — all checks must pass.
5. Run `yarn build` only if `resources/` files were modified.

### Required Checks (must all pass before committing)

```bash
composer test:pint     # Code style
composer test:types    # PHPStan level 9
composer ds:check      # No debug statements
composer test:sqlite   # Full test suite — must stay green
```

### Output Format

Produce a PR with:
- **Title**: `refactor: <concise description>` (e.g., `refactor: extract column visibility logic into dedicated class`)
- **Description**: motivation, what changed structurally, and explicit confirmation that no behavior was changed.
- A statement: "All existing tests pass unchanged."
