---
name: roadmap-draft
description: "Drafts implementation roadmap by ordering features into milestones. Use when a project needs to be broken down into manageable implementation phases."
---

# Roadmap Draft

## Overview

Transform a project specification into ordered implementation milestones. Start with the basic system skeleton, then progressively add features from highest to lowest priority. Limit each milestone scope to ensure manageable, testable increments.

This skill is architecture-agnostic—it plans in terms of user-facing features, not internal architecture nodes.

**User's Intent:** $ARGUMENTS

The argument (if provided) specifies priority focus (e.g., "MVP first", "API focus"). Respect this preference.

## Prerequisites

- SPEC.md exists with feature requirements

## Workflow

1. **Analyze specification** - Review SPEC.md to understand features
2. **Enumerate features** - List all features from the spec
3. **Define milestone scope** - Limit each milestone to 3-5 features maximum
4. **Order milestones** - Skeleton first, then by priority descending
5. **Document roadmap** - Create `ROADMAP.md`

---

## Step 1: Analyze Specification

Read SPEC.md to identify:
- All features and requirements
- Any explicit priorities mentioned
- Dependencies between features

---

## Step 2: Enumerate Features

Extract all features:

- Core functionality (must work for system to be useful)
- User-facing features (what users can do)
- Integration points (external services, APIs)
- Non-functional requirements (performance, security)

---

## Step 3: Define Milestone Scope

Each milestone:
- 3-5 related features maximum
- Cohesive theme (all features serve a common goal)
- Deliverable at the end (runnable, testable)

First milestone = skeleton (minimal working system)

---

## Step 4: Order Milestones

1. Skeleton first - Core infrastructure and basic flow
2. Priority descending - Highest priority features next
3. Dependency respect - Ensure prerequisites come first

---

## Step 5: Document Roadmap

Create `ROADMAP.md`:

```markdown
# Roadmap

## Milestone 1: Core Skeleton
**Target:** Minimal working system

### Features
- Feature A: Basic input handling
- Feature B: Core processing
- Feature C: Simple output

### Success Criteria
- [ ] App starts / library imports / CLI runs
- [ ] Features in scope are usable

---

## Milestone 2: Core Features
**Target:** Primary user functionality

### Features
- Feature D: User authentication
- Feature E: Main user interface

### Success Criteria
- [ ] Core features are usable
- [ ] Users can accomplish main tasks
```

---

## Constraints

- Architecture-agnostic (features, not nodes)
- No implementation details
- No exact timelines

## Success Criteria

- [ ] SPEC.md analyzed
- [ ] All features enumerated
- [ ] Each milestone has 3-5 features
- [ ] Milestone 1 is the skeleton
- [ ] ROADMAP.md created

## Next Steps

After roadmap is drafted:
- **Action:** Invoke the `roadmap-revise` skill to transform features into atomic blocks and finalize milestone scope
