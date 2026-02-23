---
name: quick-patch
description: "Resolves trivial bugs or implements small features with TDD workflow. Use when user requests a quick fix or simple feature that can be completed in a single session without full issue tracking."
---

# Quick Patch

## Overview

Resolve trivial bugs or implement small features using TDD workflow: write failing tests first, then implement to make them pass.

This is a shortcut for quick work. For changes that involve new feature blocks not yet on the roadmap, use the feature-add flow instead.

**User's Intent:** $ARGUMENTS

## Prerequisites

- Project has existing test structure (`tests/` directory)
- Target file exists in `src/` or appropriate location

## Workflow

Follow this process in order:

1. **Assess Complexity** - Determine if patch is trivial or involves missing feature blocks
2. **Route Appropriately** - Quick-patch for trivial, feature-add for new features
3. **Create Tests** - Write failing tests covering the bug/feature
4. **Run Tests** - Verify tests fail (red state)
5. **Implement Fix** - Write code to make tests pass
6. **Verify** - Run tests to confirm green state

---

## Step 1: Assess Complexity

Quickly evaluate if the patch is **trivial** or involves **missing feature blocks**:

### Trivial Indicators

- Single file change
- Self-contained logic
- No new feature blocks needed
- Can be tested in isolation
- Estimated work: < 30 minutes

### Missing Feature Blocks

- Requires atomic transition blocks not yet in ROADMAP.md
- New user-facing functionality
- Needs roadmap integration

---

## Step 2: Route Appropriately

Determine the appropriate path:

1. **Trivial** → Proceed with quick-patch (Step 3)
2. **Missing feature blocks** → Suggest feature-add flow

### If Missing Feature Blocks

If the change involves atomic transition blocks not yet implemented in ROADMAP.md:

```
This appears to involve feature blocks not yet on the roadmap. The requested functionality
requires atomic transition blocks that don't exist in ROADMAP.md yet.

Consider using the roadmap flow:

  /feature-add <feature-description>
  /feature-implement

This will properly decompose the feature into atomic blocks and add them to the roadmap.

Should I proceed with quick-patch anyway, or use the roadmap flow?
```

**If user insists**: Proceed with quick-patch (respect their choice).

**If user agrees**: Use feature-add → feature-implement

---

## Step 3: Create Tests

Find or create the appropriate test file:

1. **Find existing test file**: Look in `tests/` for relevant test file
2. **If no test file**: Create new test file following project conventions
3. **Add failing test**: Write test that defines expected behavior

### Test Requirements

Write **functional tests** that:

- Test public interface only
- Define expected behavior (not structure)
- Cover the bug scenario or feature use case
- Will FAIL because the fix doesn't exist yet

```
CORRECT (functional test):
assert validate_email("user@example.com") == True
assert process_input("") raises ValueError

WRONG (structural test):
assert MyClass is not None
assert my_method is callable
```

### Test Placement

- **Existing tests**: Add new test to existing test file
- **New tests**: Create `tests/test_<module>.py` or follow project conventions

---

## Step 4: Run Tests

Execute the tests to verify they fail:

```bash
pytest tests/ -v
# or
python -m pytest tests/<test_file>.py -v
```

**Expected result**: Tests FAIL (red state) because fix is not implemented.

If tests PASS unexpectedly:
- Review test - may be testing wrong behavior
- Review implementation - may already work

---

## Step 5: Implement Fix

Implement the fix to make tests pass:

1. **Find target file**: Locate the file needing modification
2. **Implement fix**: Add/fix the code
3. **Run tests**: Verify tests pass

**Focus**: Make tests pass only. Don't add unrelated changes.

---

## Step 6: Verify

Run tests one final time to confirm green state:

```bash
pytest tests/ -v
```

**Expected result**: All tests PASS (green state).

If tests still fail:
- Debug the implementation
- Review test expectations
- Fix until passing

---

## Constraints

### What This Skill Does

- Creates failing tests first (TDD)
- Implements trivial fixes/features
- Provides quick turnaround for small changes

### What This Skill Does NOT Do

- Handle complex, multi-file architectural changes
- Create issue documents
- Provide task roadmaps
- Handle changes requiring > 1 hour of work

### When NOT to Use

- Large feature implementations
- Architectural refactoring
- Multi-session work
- Changes requiring review checkpoints

---

## Success Criteria

- [ ] Complexity assessed
- [ ] Feature-add suggested if missing feature blocks (if applicable)
- [ ] User choice respected
- [ ] Failing tests created
- [ ] Tests verified to fail (red state)
- [ ] Fix implemented
- [ ] Tests pass (green state)

## Next Steps

After quick-patch completes:
- **More changes**: Run quick-patch again for additional fixes
- **Missing features**: Use feature-add → feature-implement
- **Testing**: Run full test suite to ensure no regressions
