---
name: feature-implement
description: "Implements a feature from the roadmap using test-first development. Use when you need to add new functionality following a disciplined implementation workflow."
---

# Feature Implement

## Overview

Implements a roadmap feature using test-first development with a strict workflow:
1. Analyze impact
2. Plan with foundational work first
3. Write tests that fail
4. Implement to pass tests
5. Verify integration

**User's Intent:** $ARGUMENTS

The argument specifies the feature to implement (e.g., "1.1 User Login"). If not provided, picks the first unchecked feature from ROADMAP.md.

## Prerequisites

- ROADMAP.md exists with features
- ARCHITECTURE.md exists (if feature touches architecture)
- Project is initialized with test framework

---

## Step 1: Identify Feature

If argument provided:
- Use specified feature (e.g., "1.1 User Login")

If no argument:
- Read ROADMAP.md
- Find first unchecked feature `- [ ] X.Y`
- Use that feature

Extract the atomic transition block for the selected feature.

---

## Step 2: Analyze Impact

Before planning, analyze the feature scope:

**Questions to answer:**
- Which ARCHITECTURE.md nodes does this feature touch?
- What existing implementation files need modification?
- Does it require new arch nodes?
- What are the dependencies?

**Output:** Create a brief impact summary:
```
Feature: X.Y <Name>
Touches: <list nodes>
Modifies: <list files>
New nodes: <list new nodes or "None">
Dependencies: <list dependencies>
```

---

## Step 3: Plan Implementation

Break the feature into fine-grained todo items following this order:

### 3.1 Foundational
- Data models / types needed
- Dependencies to add
- Test hooks (functions that throw NotImplementedError)

### 3.2 Write Tests
- Unit tests for new functions
- Integration tests for the feature
- Add more NotImplemented hooks if needed

### 3.3 Validate Test Failure
- Run tests
- Confirm they fail as expected
- If tests pass unexpectedly, fix tests

### 3.4 Implement
- Write implementation to make tests pass
- Fix tests if needed, but DO NOT weaken tests

### 3.5 Integration Review
- Verify feature integrates with system
- Add integration tests if gaps found
- Fix any integration issues

---

## Step 4: Execute Plan

Execute each todo item. Use subagents for clean context when appropriate.

### For each foundational item:
1. Create data models
2. Add dependencies
3. Add test hooks with NotImplementedError

### For each test item:
1. Write test file
2. Run and confirm failure

### For implementation:
1. Implement feature
2. Run tests
3. Fix until all pass

---

## Step 5: Mark Complete

After implementation succeeds:

1. Update ROADMAP.md: change `- [ ] X.Y` to `- [x] X.Y`
2. Document any new files created in the feature's atomic block (optional)
3. Summary: What was implemented, what tests added, what files modified

---

## Constraints

- Tests must fail before implementation begins
- Do not weaken tests to make them pass
- Keep implementation scoped to the feature
- Update ROADMAP.md when complete

## Success Criteria

- [ ] Feature identified (from argument or ROADMAP.md)
- [ ] Impact analysis completed
- [ ] Foundational work done (models, hooks)
- [ ] Tests written and confirmed failing
- [ ] Implementation passes tests
- [ ] Integration verified
- [ ] ROADMAP.md updated
