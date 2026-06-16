---
mode: "agent"
---

# Release Preparation Prompt

Use this prompt to prepare a new PowerGrid release. Follow `.github/instructions/release.instructions.md` for the full workflow.

---

## Release Preparation Request

**Repository:** `power-components/livewire-powergrid`

### Release Details

<!-- Provide the information below before the agent proceeds. -->

**Target version:** `<!-- e.g., 6.3.0 -->`

**Release type:**
- [ ] Patch — bug fixes only, no new features, no breaking changes
- [ ] Minor — new backward-compatible features; no breaking changes
- [ ] Major — contains breaking changes

**Previous version / tag:** `<!-- e.g., v6.2.1 -->`

**Commits / PRs to include:**

```
<list merged PR titles and numbers, or provide a date range>
```

**Maintainer sign-off received?** Yes / No

---

## Agent Instructions

1. **Audit changes** — categorize all commits/PRs listed above into `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.

2. **Verify the branch** — run `composer verify` and confirm all checks pass before proceeding:

   ```bash
   composer test:pint
   composer test:types
   composer ds:check
   composer test:sqlite
   ```

3. **Draft the changelog entry** — add a new section at the top of `CHANGELOG.md` following [Keep a Changelog](https://keepachangelog.com/) format:

   ```markdown
   ## [X.Y.Z] - YYYY-MM-DD

   ### Added
   - ...

   ### Fixed
   - ...
   ```

4. **Update `README.md`** only if:
   - The minimum PHP, Laravel, or Livewire version changed.
   - The features table needs updating.

5. **Create a release PR** targeting the appropriate branch (e.g., `6.x` or `main`).

### Post-Merge Checklist (for maintainers — do not automate)

- [ ] Create and push the Git tag: `git tag v<X.Y.Z> && git push origin v<X.Y.Z>`
- [ ] Draft the GitHub Release with the changelog entry as the body.
- [ ] Confirm Packagist is updated (it updates automatically on new tags if the webhook is configured).
- [ ] Announce in the PowerGrid Discussions tab if it is a significant release.

### Output Format

Produce a PR with:
- **Title**: `release: v<X.Y.Z>`
- **Description**: the full changelog section plus the post-merge checklist above.
