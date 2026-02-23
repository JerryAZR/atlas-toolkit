---
name: meta-build
description: "Implements all features in a milestone: runs feature-implement for each feature, then milestone-wrapup. Use when user wants to implement a complete milestone. This is a meta-skill that orchestrates other skills."
---

# Meta Build

## Overview

Orchestrate the complete milestone implementation workflow. Reads the roadmap to find features under the specified milestone, then implements each one followed by milestone wrapup.

**User's Intent:** $ARGUMENTS

The argument specifies the milestone number to implement (e.g., "1" or "milestone 1").

## Prerequisites

- ROADMAP.md exists with milestone definitions
- Specified milestone has features listed

## Workflow

This skill acts as a project manager, coordinating subagents for implementation.

### Step 1: Read Roadmap

First, read ROADMAP.md to identify features under the target milestone.

**Action:** Read ROADMAP.md and extract:
- Features listed under milestone $ARGUMENTS
- Dependencies between features (if any noted)
- Completion criteria for the milestone

### Step 2: Analyze Dependencies

Based on the roadmap analysis, determine feature implementation order:

- **Sequential:** Features with dependencies must run in order
- **Parallelizable:** Independent features can run in parallel

If unsure about dependencies, schedule sequentially.

### Step 3: Implement Features

For each feature in the milestone:

**If features are independent (parallelizable):**
Run multiple `feature-implement` subagents in parallel:

**Subagent prompt (for each parallel feature):**
```
Run the feature-implement skill to implement: <feature-name>

After completion, report:
1. Atomic blocks implemented
2. Tests added
3. Files created/modified
```

**If features have dependencies (sequential):**
Run each `feature-implement` sequentially:

**Subagent prompt:**
```
Run the feature-implement skill to implement the next feature in milestone $ARGUMENTS.

After completion, report:
1. What was implemented
2. Tests added
3. Files created/modified
```

Repeat for each feature in order.

### Step 4: Wrap Up Milestone

Once all features are implemented, invoke `milestone-wrapup` in a subagent.

**Subagent prompt:**
```
Run the milestone-wrapup skill to verify and document milestone $ARGUMENTS completion.

After completion, report:
1. Self-evaluation results
2. Milestone summary
3. Next milestone (if any)
```

---

## Dependencies

1. Read ROADMAP.md → identifies features to implement
2. feature-implement (each) → needs ROADMAP.md context
3. milestone-wrapup → needs all features implemented

Features with clear dependencies run sequentially. Independent features can run in parallel (use parallel subagents for efficiency).

---

## Success Criteria

- [ ] All features in milestone implemented
- [ ] Tests passing for implemented features
- [ ] milestone-wrapup completed successfully

## Next Steps

After milestone is complete:
- Use `/milestone-implement` for next milestone
- Use `/project-setup` for new project
