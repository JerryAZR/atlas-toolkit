---
name: meta-setup
description: "Complete project setup workflow: project-init → arch-elaborate → roadmap-draft → roadmap-revise. Use when user wants to set up a complete new project. This is a meta-skill that orchestrates other skills."
---

# Meta Setup

## Overview

Orchestrate the complete project setup workflow in sequence. Each skill runs in a subagent to keep the main context clean.

**User's Intent:** $ARGUMENTS

The argument specifies the project description or name.

## Prerequisites

- User has a project idea/concept

## Workflow

This skill acts as a project manager, delegating each step to subagents and tracking progress.

### Step 1: Initialize Project

Invoke `project-init` in a subagent with the user's project description.

**Subagent prompt:**
```
Run the project-init skill to bootstrap a new project.

Project description: $ARGUMENTS

After completion, report:
1. What SPEC.md contains (key points)
2. What files were created
3. Any decisions made about tech stack
```

### Step 2: Elaborate Architecture

Once project-init is complete, invoke `arch-elaborate` in a subagent.

**Subagent prompt:**
```
Run the arch-elaborate skill to create the architecture tree.

After completion, report:
1. Root node and its children
2. Key architectural decisions
3. How many atomic leaf nodes were identified
```

### Step 3: Draft Roadmap

Once architecture is elaborated, invoke `roadmap-draft` in a subagent.

**Subagent prompt:**
```
Run the roadmap-draft skill to create the implementation roadmap.

After completion, report:
1. Number of milestones created
2. Key features per milestone
3. Estimated complexity
```

### Step 4: Revise Roadmap

Once roadmap is drafted, invoke `roadmap-revise` in a subagent to refine into atomic blocks.

**Subagent prompt:**
```
Run the roadmap-revise skill to refine the roadmap into atomic transition blocks.

After completion, report:
1. Number of atomic blocks created
2. Any blocks that were split
3. Dependencies identified
```

---

## Dependencies

All steps are sequential - each requires output from the previous:
1. project-init → creates SPEC.md
2. arch-elaborate → needs SPEC.md → creates ARCHITECTURE.md
3. roadmap-draft → needs ARCHITECTURE.md → creates ROADMAP.md
4. roadmap-revise → needs ROADMAP.md → refines ROADMAP.md

---

## Success Criteria

- [ ] project-init completed with SPEC.md
- [ ] arch-elaborate completed with ARCHITECTURE.md
- [ ] roadmap-draft completed with initial ROADMAP.md
- [ ] roadmap-revise completed with atomic blocks

## Next Steps

After project setup is complete:
- Use `/feature-add` to add features
- Use `/milestone-implement` to implement a milestone
