---
name: feature-add
description: "Breaks down a user-requested feature into atomic feature blocks and adds them to appropriate future milestones. Use when a user requests a new feature to be added to the roadmap."
---

# Feature Add

## Overview

Adds a new feature to the roadmap by:
1. Breaking down the feature into atomic transition blocks
2. Assigning blocks to appropriate milestones (or creating new ones)

**User's Intent:** $ARGUMENTS

The argument specifies the feature to add (e.g., "user authentication", "dark mode").

## Prerequisites

- ROADMAP.md exists with existing milestones
- SPEC.md exists for understanding feature context

---

## Step 1: Analyze Feature

Understand the requested feature by reading SPEC.md and existing ROADMAP.md.

**Questions to answer:**
- What does the feature do?
- How does it interact with existing functionality?
- What are the user-facing behaviors?

**If ambiguities exist:**
- Ask user clarifying questions to better understand intent
- Present 2-3 options per question with clear tradeoff and recommendation for each

---

## Step 2: Decompose into Atomic Blocks

Break down the feature into atomic transition blocks using the same format as roadmap-revise:

```
Trigger: (required)
Precondition: (required)
State Mutation:
Persistence Mutation:
Observable Response:
Invariant Introduced:
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

2. Does this introduce a rule that must always hold? (Y/N)
   → If Yes, include **Invariant Introduced**

3. Does this change future decision logic in this runtime? (Y/N)
   → If Yes, include **State Mutation**

4. Does this affect UI/API output or generate outgoing traffic? (Y/N)
   → If Yes, include **Observable Response**

**At least one of the optional fields must be present** — otherwise the block has no effect.

**Rules:**
- One trigger per block
- One dominant state mutation per block
- Split if multiple state mutations

---

## Step 3: Enforce Atomicity

Validate each block:

Reject a block if:
- Trigger is missing
- All optional fields are empty (block has no effect)
- Multiple unrelated state variables
- More than one invariant

If violated → split further.

---

## Step 4: Assign to Milestones

Determine where each atomic block belongs:

### Option A: Add to Existing Future Milestone

If blocks are related to an existing milestone's scope:
- Add blocks to that milestone in ROADMAP.md
- Maintain proper numbering (e.g., if last feature is 2.3, add as 2.4, 2.5...)

### Option B: Create New Milestone

If blocks form a distinct, coherent feature set:
- Create a new milestone section in ROADMAP.md
- Number starting from next milestone (e.g., if last is Milestone 2, create Milestone 3)
- Add blocks as 3.1, 3.2, etc.

### Coupling Criteria for Assignment

Group blocks together if they share:
- Same trigger
- Same state variable
- Sequential dependency (A must complete before B)
- Same data structure

---

## Step 5: Update ROADMAP.md

Add the feature blocks to ROADMAP.md:

```markdown
## Milestone X: <Title>

**Features:**
- [ ] X.Y <Feature Name>

  Atomic transition block...
```

---

## Constraints

- Do not modify existing completed or in-progress milestones
- Only add to future milestones or create new ones
- Maintain atomic block format from roadmap-revise
- Keep coupled features together

## Success Criteria

- [ ] Feature analyzed and understood
- [ ] Decomposed into atomic transition blocks
- [ ] Atomicity enforced (all blocks valid)
- [ ] Blocks assigned to appropriate milestones
- [ ] ROADMAP.md updated with new feature blocks
