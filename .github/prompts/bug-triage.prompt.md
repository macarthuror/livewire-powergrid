---
mode: "ask"
---

# Bug Triage Prompt

Use this prompt to quickly assess an incoming bug report and determine the next action.

---

## Bug Triage Request

**Repository:** `power-components/livewire-powergrid`

### Bug Report Summary

<!-- Paste the issue title and body here, or describe the reported bug. -->

```
<paste issue content here>
```

---

### Triage Checklist

Please answer each of the following questions:

1. **Is this a confirmed bug or a usage question?**
   - If it is a usage question, draft a polite reply pointing to the [documentation](https://livewire-powergrid.com) and close as "not a bug."
   - If it may be a bug, continue.

2. **Which versions are affected?**
   - PHP version(s):
   - Laravel version(s):
   - Livewire version(s):
   - PowerGrid version(s):

3. **Can the bug be reproduced with a minimal example?**
   - Yes → paste or link to the reproduction steps.
   - No → use the `.github/prompts/minimal-reproduction.prompt.md` prompt to request one from the reporter.

4. **Is there an existing open issue or PR for this?**
   - Search issues and PRs before proceeding.

5. **What is the severity?**
   - **Critical** — data loss, security vulnerability, or complete feature failure.
   - **High** — major feature broken with no workaround.
   - **Medium** — feature partially broken; workaround exists.
   - **Low** — cosmetic issue, edge case, or minor UX problem.

6. **What is the root cause (preliminary)?**
   - Identify the `src/` file(s) most likely responsible.

### Recommended Next Action

Based on the above, recommend one of:
- `fix`: proceed immediately with `.github/instructions/bug-fix.instructions.md`.
- `repro needed`: request a minimal reproduction using `.github/prompts/minimal-reproduction.prompt.md`.
- `duplicate`: link to the existing issue.
- `not a bug`: draft a response and close.
- `needs discussion`: open a Discussion for design input.
