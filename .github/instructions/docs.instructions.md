---
applyTo: "**"
---

# Docs Agent Instructions

## Role

Create, update, or improve documentation for the PowerGrid package. Documentation changes should be accurate, concise, and consistent with the current codebase behavior.

## Scope

| Location | Content |
|----------|---------|
| `README.md` | Top-level overview, features table, requirements, quick-start |
| `CONTRIBUTING.md` | Contribution guide, code contribution workflow |
| `.github/copilot-instructions.md` | Repository-wide AI agent baseline instructions |
| `.github/instructions/` | Role-based agent instruction files |
| `.github/prompts/` | Reusable AI prompt templates |
| [powergrid-doc repo](https://github.com/Power-Components/powergrid-doc) | Full user documentation (out of scope for this repository) |

Do **not** modify `src/`, `tests/`, or `resources/` files when working on documentation only.

## Required Inputs

Before starting, confirm you have:

1. The specific section or file to update.
2. The current behavior or API being documented (verified against `src/`).
3. Whether the change is for end users, contributors, or AI agents.

## Workflow

1. **Verify accuracy** — read the relevant `src/` files to confirm the documented behavior matches the current implementation.
2. **Draft changes** — edit the target Markdown file, matching the existing writing style (imperative mood, second person, short sentences).
3. **Cross-check** — ensure no contradictions exist between `README.md`, `CONTRIBUTING.md`, and the instruction files.
4. **Review links** — check that all internal and external links are valid.

## Writing Style

- Use second person ("you can", "run", "add").
- Use imperative mood for instructions ("Run the tests", "Extend `PowerGridComponent`").
- Use fenced code blocks with language tags for all code examples.
- Keep paragraphs short; use bullet lists for multi-item descriptions.
- Match the tone and formatting of the surrounding content in the file being edited.
- Do not add emojis unless matching an existing pattern in the file.

## Prohibited Actions

- Do not document behaviors that do not yet exist in `src/`.
- Do not remove warnings or notes about breaking changes or deprecations.
- Do not change code in `src/` to match documentation — update the documentation to match code, or flag the discrepancy.

## Output Format

Produce a PR with:
- **Title**: `docs: <concise description>` (e.g., `docs: document AI agent files in CONTRIBUTING.md`)
- **Description**: what was added or changed and why. If updating instructions, explain what changed in the codebase that prompted the update.
