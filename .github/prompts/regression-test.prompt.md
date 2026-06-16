---
mode: "agent"
---

# Regression Test Prompt

Use this prompt to add a regression test that prevents a previously fixed bug from reappearing.

---

## Regression Test Request

**Repository:** `power-components/livewire-powergrid`

### Context

<!-- Provide the information below before the agent proceeds. -->

**Fixed issue / PR:** #<!-- number -->

**Bug description (one sentence):**

```
<describe what was broken>
```

**Root cause (one sentence):**

```
<describe what in src/ was wrong>
```

**Fix summary:**

```
<describe what was changed to fix it>
```

---

## Agent Instructions

Using the context above and following `.github/instructions/test.instructions.md`, write a regression test that:

1. **Targets the specific behavior** that was broken — not the internal implementation.
2. **Is placed in the correct test file** under `tests/Feature/` or `tests/Unit/` for the affected area. Check existing test files before creating a new one.
3. **References the issue** in the test description, e.g., `it('does not lose filter state on re-render (#123)')`.
4. **Uses the minimal Livewire test table component** that exercises the fixed code path. Reuse a component from `tests/Concerns/Components/` if one fits; create a new one in that directory only if needed.
5. **Asserts the corrected behavior**, not the previously broken one.

### Required Test Structure

```php
it('<verb> <what was fixed> (#<issue-number>)', function () {
    // Arrange

    // Act

    // Assert — this assertion would have failed before the fix
});
```

### Verification

After writing the test, confirm:

```bash
composer test:sqlite   # New test must pass
composer test:pint     # Test file must be formatted
```
