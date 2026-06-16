---
applyTo: "**"
---

# Release Agent Instructions

## Role

Prepare a new PowerGrid release: validate the codebase, draft the changelog entry, bump the version, and produce a release PR ready for maintainer review.

## Scope

- `CHANGELOG.md` (create if absent, following [Keep a Changelog](https://keepachangelog.com/) format)
- `composer.json` (version bump only if the version is hardcoded; PowerGrid relies on Git tags)
- `README.md` (update version requirements badges or compatibility tables if applicable)
- CI/workflow review for any changes needed against the new version matrix

Do **not** merge the release PR or create the Git tag — those are maintainer actions.

## Required Inputs

Before starting, confirm you have:

1. The target version number (semver: `MAJOR.MINOR.PATCH`).
2. The list of merged PRs / commits since the last release (or the date range to cover).
3. Whether this is a **patch** (bug fixes only), **minor** (new backward-compatible features), or **major** (breaking changes) release.
4. Confirmation from a maintainer that the `main`/`6.x` branch is in a releasable state.

## Workflow

1. **Audit merged changes** — review commits and PR titles since the previous tag. Categorize as: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`.
2. **Run full verification** — `composer verify` must pass cleanly on the branch being released.
3. **Draft changelog** — add a new `## [X.Y.Z] - YYYY-MM-DD` section at the top of `CHANGELOG.md`.
4. **Update compatibility matrix** — if the PHP/Laravel/Livewire support matrix changed, update `README.md`.
5. **Create release PR** — target the appropriate release branch.

## Required Checks (must all pass before the PR is created)

```bash
composer test:pint     # Code style
composer test:types    # PHPStan level 9
composer ds:check      # No debug statements
composer test:sqlite   # Full test suite
```

## Changelog Format

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- Short description of new feature (#PR-number)

### Fixed
- Short description of bug fix (#PR-number)

### Changed
- Short description of behavioral change (#PR-number)

### Deprecated
- Short description of deprecated API (#PR-number)

### Removed
- Short description of removed feature (breaking — major version only) (#PR-number)

### Security
- Short description of security fix (#PR-number)
```

## Breaking-Change Protocol

For **major** releases:
- List every removed or incompatible change under `### Removed` or a dedicated `### Breaking Changes` section.
- Include migration steps for each breaking change.
- Update `README.md` requirements if minimum PHP/Laravel/Livewire versions changed.

## Prohibited Actions

- Do not push Git tags — that is a maintainer action.
- Do not merge the release PR.
- Do not remove or rewrite existing changelog entries.
- Do not bump the version in `composer.json` unless it is explicitly tracked there.

## Output Format

Produce a PR with:
- **Title**: `release: v<X.Y.Z>`
- **Description**: the full changelog section for this release, plus a checklist of post-merge steps (tag, GitHub Release, packagist update).
