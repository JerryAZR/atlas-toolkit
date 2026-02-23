---
name: issue-create
description: "Creates an off-roadmap issue document for tracking work not intended for ROADMAP.md. Use for bugs discovered during implementation, external requests, or items to be deferred."
---

# New Issue

## Overview

Create an issue document in the `issues/` directory for tracking work **not intended for the roadmap**.

**When to use this skill vs. feature-add:**
- **issue-create**: Off-roadmap items (bugs found during work, external requests, deferred items without roadmap integration)
- **feature-add**: New features intended for ROADMAP.md with atomic block decomposition

**User's Intent:** $ARGUMENTS

The issue document captures the requirements, context, and implementation notes needed to track and eventually implement the change.

## Prerequisites

- None required - can create issues at any time

## Workflow

Follow this process in order:

1. **Parse Arguments** - Extract issue description
2. **Determine Index** - Find next available issue number
3. **Create Document** - Write the issue file
4. **Confirm** - Report completion

---

## Step 1: Parse Arguments

The user provides a description of the feature or bug. Extract the core concept for the filename.

**Filename format:** `<index>-<slug>.md`

- Convert to kebab-case
- Use descriptive but concise names
- Examples:
  - "material theme support" → `002-material-theme.md`
  - "fix login redirect bug" → `003-login-redirect-bug.md`

---

## Step 2: Determine Index

Find the next available issue number:

1. Check existing files in `issues/` directory
2. Parse numeric prefixes
3. Use next available number (001, 002, etc.)

If no issues exist, start with 001.

---

## Step 3: Create Document

Create `issues/<index>-<slug>.md` with the following template:

```markdown
# <Title>

## Expected Behavior

Describe what should happen. Include criteria of success.

## Current Behavior

Describe what currently happens (for bugs) or that it doesn't exist yet (for features).

## Architecture Impact

Which architecture nodes does this affect?
- List nodes from `arch/ARCH_SUMMARY.md` if available

## Priority

- [ ] Address now
- [ ] Defer to later milestone

## Testing

What new tests are needed?
- Unit tests
- Integration tests
- Test scenarios

## Notes

Additional context, links, or considerations.
```

Fill in each section based on user input and available context.

---

## Step 4: Confirm

Report the created file path to the user.

---

## Constraints

- Do NOT implement the feature or fix
- Do NOT write tests
- Only create the issue document

---

## Success Criteria

- [ ] Issue document created at `issues/<index>-<slug>.md`
- [ ] All sections populated from user input
- [ ] Filename follows `<index>-<slug>.md` format
- [ ] Priority decision recorded

## Next Steps

After issue creation, determine the appropriate next action:

### If Issue Statement is Unclear
- Requirements are ambiguous or missing critical details
- **Action:** Run `spec-clarify` skill to resolve ambiguities

### If Issue is Clear
- Requirements are well-defined and actionable
- **Action:** Run `issue-plan` skill to break down the issue into tasks
