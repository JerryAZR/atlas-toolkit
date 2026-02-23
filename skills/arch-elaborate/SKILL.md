---
name: arch-elaborate
description: "Elaborates the architecture tree for a project. Use when the user wants to create a new architecture from SPEC.md."
---

# Architecture Elaborate

## Overview

Elaborate an architecture tree for this project. Recursively decompose each subsystem/module until all leaf nodes in the architecture tree are atomic (single, well-defined responsibility).

**User's Intent:** $ARGUMENTS

The argument (if provided) specifies the user's architectural preferences - naming conventions, structural choices, or specific decomposition patterns. Respect these preferences when elaborating the architecture.

## Prerequisites

- SPEC.md exists with project requirements

## Workflow

1. **Create ARCHITECTURE.md** - Start with root node
2. **Decompose recursively** - Expand each node into 2-6 children
3. **Mark leaves as atomic** - Leaf nodes get "(atomic)" label in heading
4. **Define responsibility** - Brief 1-3 sentence description per node

---

## Step 1: Create ARCHITECTURE.md

Create `ARCHITECTURE.md` (replaces old ARCH_SUMMARY.md):

```markdown
---
project: <Project Name>
version: 0.1.0

tech_stack:
  language: <Language or "TBD">
  framework: <Framework or "TBD">
  test_framework: <Test Framework or "TBD">
  runtime: <Runtime Model or "TBD">
---

# <RootNode>

Responsibility: <1-3 sentence high-level ownership>

Dataflow:
- Input: <external sources>
- Output: <external consumers>
- Side-effects: <if any>

## <ChildNode> (atomic | subdivided)

Responsibility: <what this child owns>

---

## <ChildNode> (atomic | subdivided)

Responsibility: <what this child owns>

### <GrandChildNode> (atomic | subdivided)

...
```

---

## Step 2: Decompose Recursively

For each node that needs decomposition:

1. **Identify children** - 2-6 orthogonal children that fulfill parent's responsibility
2. **Define each child** - Brief responsibility + dataflow
3. **Recurse** - Continue until each leaf has atomic responsibility

**Atomic indicator**: A node is atomic when further decomposition adds no value:
- Single, clear responsibility
- Implementable as one unit
- Doesn't need children to fulfill its purpose

---

## Step 3: Mark Leaves

Add "(atomic)" after leaf node headings:

```markdown
## Authentication (atomic)

Responsibility: Handles user login/logout and token management
```

For nodes with children, use "(subdivided)" or omit any marker:

```markdown
## Core Services (subdivided)

...
```

---

## Constraints

- Keep all documentation in ARCHITECTURE.md - no separate node files
- No state tracking needed - just the tree structure
- Respect user's architectural preferences from the argument

## Success Criteria

- [ ] ARCHITECTURE.md created
- [ ] All leaf nodes marked as atomic
- [ ] Each node has brief responsibility description

## Next Steps

After architecture is elaborated:
- **Action:** Invoke the `roadmap-draft` skill to create the implementation roadmap
