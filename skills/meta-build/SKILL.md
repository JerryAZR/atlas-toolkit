---
name: meta-build
description: "Implements all features in a milestone: runs feature-implement for each feature, then milestone-wrapup. Use when user wants to implement a complete milestone. This is a meta-skill that orchestrates other skills."
---

# Meta Build

## Overview

Orchestrate the complete milestone implementation workflow. Reads the roadmap to find features under the specified milestone, then implements each one followed by milestone wrapup.

**User's Intent:** $ARGUMENTS

The argument specifies the milestone number to implement (e.g., "1" or "milestone 1").

## Why Use Subagents?

This is a **meta-skill** - it orchestrates other skills without doing the work itself. Using subagents is critical because:

- **Context preservation**: Keeps the main conversation clean and focused on project management
- **Token efficiency**: Each subagent has its own bounded context
- **Parallel execution**: Independent features can run concurrently
- **Clear boundaries**: Each subagent has a specific, limited scope

**Never run child skills directly in the main context.** Always use Task tool to spawn subagents.

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

### Step 2: Plan Execution Order

Analyze the roadmap and create an execution schedule:

- Identify dependencies between features
- Group features that can run in parallel
- Order sequential features correctly
- **If unsure about dependencies, schedule sequentially**

Example schedule:
```
Wave 1 (sequential):  1.1 User Authentication
Wave 2 (parallel):    1.2 Dashboard, 1.3 Settings, 1.4 Profile
Wave 3 (parallel):    1.5 Notifications, 1.6 Search
Wave 4 (sequential):  1.7 Export
```

### Step 3: Implement Features

Run `feature-implement` subagents according to the schedule. Use the same prompt for all cases - just pass the feature name:

**Subagent prompt:**
```
Run the feature-implement skill to implement: <feature-id> - <feature-name>

After completion, report:
1. What was implemented
2. Tests added
3. Files created/modified
```

**Parallel execution:** Spawn multiple subagents for features in the same wave.
**Sequential execution:** Wait for each subagent to complete before spawning the next.

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

### Step 5: Review for Architectural Entropy

After milestone-wrapup completes, invoke `milestone-review` in a subagent to review the changes for architectural entropy and refactor if needed.

**Subagent prompt:**
```
Run the milestone-review skill to review the latest milestone changes for architectural entropy increase.

After completion, report:
1. Review findings
2. Problems fixed during refactor (if any)
```

---

## Dependencies

1. Read ROADMAP.md → identifies features to implement
2. Plan execution order → determines parallel/sequential waves
3. feature-implement (each) → needs ROADMAP.md context
4. milestone-wrapup → needs all features implemented
5. milestone-review → runs after wrapup to check architectural entropy

---

## Success Criteria

- [ ] All features in milestone implemented
- [ ] Tests passing for implemented features
- [ ] milestone-wrapup completed successfully
- [ ] milestone-review completed with all severe entropy issues addressed

## Next Steps

After milestone is complete:
- Use `/meta-build <next-milestone-number>` for next milestone
