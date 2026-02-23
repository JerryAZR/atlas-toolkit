---
name: roadmap-revise
description: "Transforms milestone features into atomic primitives and reorganizes milestones to limit scope. Use after roadmap-draft to expose hidden complexity and restructure milestones for manageable implementation."
---

# Roadmap Revise

## Overview

This skill does two things:

1. **Normalize**: Transform milestone features into atomic behavioral units
2. **Revise**: Reorganize milestones to limit scope and complexity

**User's Intent:** $ARGUMENTS

The argument (if provided) specifies focus. Default: all milestones.

## Prerequisites

- ROADMAP.md exists
- SPEC.md exists

---

## Part 1: Normalize Features

### Step 1: Load Milestones

Read `ROADMAP.md`. List each milestone's features verbatim.

### Step 2: Decompose Each Feature

Convert each feature into atomic transition blocks:

```
Trigger: (required)
Precondition: (required)
State Mutation:
Persistence Mutation:
Observable Response:
```

**Required Fields:**

| Field | Description |
|-------|-------------|
| Trigger | The earliest event in a causal chain that initiates a new evaluation cycle of the app |
| Precondition | If false, the trigger does not execute. System rejects or disables the action. |

**Optional Fields (answer questions below):**

Answer these questions for each block:

1. Would the app behave differently after restart? (Y/N)
   → If Yes, include **Persistence Mutation**

2. Does this change future decision logic in this runtime? (Y/N)
   → If Yes, include **State Mutation**

4. Does this affect UI/API output or generate outgoing traffic? (Y/N)
   → If Yes, include **Observable Response**

**At least one of the optional fields must be present** — otherwise the block has no effect.

**Rules:**
- One trigger per block
- One dominant state mutation per block
- Split if multiple state mutations

### Step 3: Enforce Atomicity

Reject a block if:
- Trigger is missing
- All optional fields are empty (block has no effect)
- Multiple unrelated state variables
- More than one invariant

If violated → split further.

### Step 4: Complexity Summary

Compute per milestone:
- Triggers: X
- State Variables: Y
- Persistence Mutations: Z
- New Data Structures: M

---

## Part 2: Revise Milestones

### Step 5: Reorganize Milestones

Reorganize to limit scope and complexity:

**Target per milestone:**
- Max 5 features
- Max 3 state variables mutated
- Max 1 persistence mutation
- Keep tightly-coupled features together

**Coupling criteria:**
- Same trigger
- Same state variable
- Sequential dependency (A must complete before B)
- Same data structure

**If a milestone exceeds targets:**
1. Move low-coupling features to next milestone
2. Split if no features can be moved
3. Prioritize: tightly-coupled features stay together

### Step 6: Label Features

Label each atomic feature as checklist items `X.Y` where:
- X = milestone index (1, 2, 3...)
- Y = feature index within milestone (1, 2, 3...)

Example:
```
## Milestone 1 Features
- [ ] 1.1 User Login
- [ ] 1.2 User Logout

## Milestone 2 Features
- [ ] 2.1 Create Post
- [ ] 2.2 Edit Post
```

---

## Output

Overwrite `ROADMAP.md`:

```markdown
# Roadmap

## Milestone 1: <Title>
**Scope:** X features, Y state mutations

**Features:**
- [ ] 1.1 <Feature>

  Atomic transition block...

- [ ] 1.2 <Feature>

  Atomic transition block...

---

## Milestone 2: <Title>
...

### Complexity Summary
- Triggers: X
- State Variables: Y
- Persistence Mutations: Z
```

---

## Constraints

- Do not make-up features not in `SPEC.md`
- Do not remove essential functionality
- Keep tightly-coupled features together

## Success Criteria

- [ ] All features decomposed into atomic blocks
- [ ] Features labeled as X.Y checklists
- [ ] Each milestone has max 5 features
- [ ] Tightly-coupled features kept together
- [ ] ROADMAP.md updated

## Next Steps

After roadmap is revised:
- **Action:** Invoke the `feature-implement` skill to start implementing the first feature from the roadmap
