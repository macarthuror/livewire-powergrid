---
mode: "ask"
---

# Minimal Reproduction Prompt

Use this prompt to request a minimal, self-contained reproduction of a reported bug from the issue reporter — or to create one yourself.

---

## Minimal Reproduction Request

**Repository:** `power-components/livewire-powergrid`

### Context

<!-- Describe the bug or link to the issue. -->

**Issue:** #<!-- issue number -->

**Reported behavior:**

```
<paste the reported behavior here>
```

---

### Minimal Reproduction Requirements

A valid minimal reproduction must:

1. **Use the latest PowerGrid release** (or the specific version where the bug was observed — state the version clearly).
2. **Be runnable with a standard Laravel installation** — avoid custom configurations unless the bug is configuration-specific.
3. **Contain only the code necessary to trigger the bug** — no unrelated models, controllers, or components.
4. **Demonstrate the unexpected behavior** — include a comment or test assertion showing what was expected vs. what actually happened.
5. **Include the datasource type** — Eloquent model or in-memory Collection (the bug may affect only one).

### Template: Minimal Test Case

If writing a Pest test inside this repository:

```php
it('reproduces #<issue-number>: <short description>', function () {
    // Arrange: define the minimal table component inline or reference a
    // test component from tests/Concerns/Components/ if one fits.

    // Act: interact with the table via Livewire::test().

    // Assert: show what the expected behavior is and what actually happens.
    Livewire::test(MyMinimalTable::class)
        ->assertSee('expected value')  // adjust to actual assertion
        ->assertDontSee('unexpected value');
});
```

Run with: `composer test:sqlite -- --filter "reproduces #<issue-number>"`

### Checklist Before Sharing

- [ ] The reproduction uses the stated PowerGrid version.
- [ ] It demonstrates the bug with the fewest possible lines of code.
- [ ] It includes expected vs. actual output.
- [ ] It runs cleanly with `composer test:sqlite` (aside from the failing assertion).
- [ ] It does not include any credentials, API keys, or proprietary code.
