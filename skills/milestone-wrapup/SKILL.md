---
name: milestone-wrapup
description: "Concludes a milestone by performing end-to-end verification. Use when all feature items in a milestone are implemented and verified. Follows a 2-phase check: self-evaluation first, then present to user."
---

# Milestone Wrapup

## Overview

Concludes a milestone by performing end-to-end verification with a 2-phase check:

1. **Phase 1 - Self-Evaluation**: Automated checks to verify completeness
2. **Phase 2 - User Presentation**: Present verification checklist for user confirmation

**User's Intent:** $ARGUMENTS

The argument specifies the milestone number (e.g., "1", "2"). If not provided, finds the first milestone with all features checked in ROADMAP.md.

## Prerequisites

- ROADMAP.md exists with milestone features
- All features in the target milestone are marked complete (`- [x]`)

---

## Phase 1: Self-Evaluation

Perform automated checks before presenting to user.

### Step 1: Identify Milestone

If argument provided:
- Use specified milestone (e.g., "1" for Milestone 1)

If no argument:
- Read ROADMAP.md
- Find first milestone where all features are checked (`- [x]`)
- Use that milestone

### Step 2: Verify Feature Blocks Complete

Verify all atomic feature blocks are properly implemented with mechanical checks:

| Check | Verification |
|-------|--------------|
| Feature Completeness | All features in milestone marked `- [x]` in ROADMAP.md |
| Tests Exist | Integration/unit tests exist for each feature |
| Tests Pass | Run tests to confirm they pass |
| Build Verification | Project builds successfully |
| No Regressions | Run full test suite to ensure no regressions |

**If checks fail:**
- Report what is missing
- Do NOT proceed to Step 3
- Suggest: Use feature-implement to complete missing work

### Step 3: Identify End-to-End Observable Features

Generate a list of end-to-end, observable capabilities added by this milestone — not a 1-to-1 mapping from atomic feature blocks.

**What to identify:**
- New user-facing capabilities that span multiple atomic blocks
- Key user workflows that are now possible
- Observable outcomes (UI changes, CLI outputs, API responses, logs)

**Examples:**
- "User can now initialize a new project from CLI" (instead of listing each atomic file-creation step)
- "App now shows structured help output" (instead of listing each help text block)
- "API returns JSON with pagination metadata" (instead of listing each field)

**Log acceptance:** Logs are acceptable as user-observable output in early development. Format as: "Logs show: [expected log message]"

**Group related atomic blocks** into single observable features where they form a coherent user capability.

### Step 4: Integration Review

Perform reasoning-based review for integration completeness:

**1. Code Review:**
- Are the components supporting this milestone's features wired up end-to-end?
- Check module imports, dependency injection, wiring configurations
- Verify entry points connect to implementations

**2. Test Coverage Review:**
- Is the entire data/control flow covered by tests?
- Trace the path from user action to observable result
- Identify any gaps in the test coverage

**If issues found:**
- Report what is missing
- Do NOT proceed to Phase 2
- If the underlying feature blocks already exist but are not wired correctly → use `quick-patch` skill
- If missing feature blocks → add them to ROADMAP.md and use `feature-implement` skill

---

## Phase 2: User Presentation

After self-evaluation passes, present to user for final confirmation.

### Step 5: Present Verification Checklist

Present the observable features for user verification:

```
Milestone X: [Title]

Observable Features:
1. [Feature name]: [What user does] → [What user sees]
2. [Feature name]: [What user does] → [What user sees]
...

Please verify each feature works as expected.
```

### Step 6: Wait for User Confirmation

Wait for user to respond before proceeding.

**If user reports issues:**
- Do NOT proceed to documentation
- If the issue is wiring/connection → use `quick-patch` skill
- If the issue is missing functionality → add to ROADMAP.md and use `feature-implement` skill

**If user confirms:**
- Proceed to Step 7

---

## Step 7: Document Completion

After user confirmation, update ROADMAP.md to mark the milestone complete.

Document whatever is meaningful — the agent decides what information to include.

---

## Constraints

- **Does NOT**: Implement features, fix tests, refactor
- **Does**: Verify completion, present checklist, document achievements
- Requires both automated checks pass AND user confirmation before marking complete
- Operates on milestones, not individual features

## Success Criteria

- [ ] Target milestone identified (Step 1)
- [ ] Feature blocks complete, tests pass, build verified (Step 2)
- [ ] End-to-end observable features identified (Step 3)
- [ ] Integration review passes (Step 4)
- [ ] Observable features presented to user (Step 5)
- [ ] User confirmed completion (Step 6)
- [ ] ROADMAP.md updated (Step 7)

## Next Steps

After milestone wrap up:

### If More Milestones Remain
- **Action:** Run `feature-implement` to continue with the next milestone's features

### If All Milestones Complete
- **Action:** Project is complete — no further skills needed
