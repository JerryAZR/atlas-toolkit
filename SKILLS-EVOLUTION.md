# Skills Evolution: skills-old vs New Toolkit

This document tracks the planned changes from the old skill system to the new LLM coding toolkit.

---

## Phase 1: Specification & Project Setup (UNCHANGED)

| Old Skill | New Approach | Notes |
|-----------|--------------|-------|
| `spec-create` | **Keep** | Create project specifications |
| `spec-clarify` | **Keep** | Resolve ambiguities in specs |
| `project-init` | **Keep** | Bootstrap new projects with tech stack |

**Status:** No changes. This flow works well.

---

## Phase 2: Milestone Planning (CHANGING)

### Old: `milestone-plan-all`

- Single skill that creates all milestones at once
- Simple 3-5 features per milestone structure
- Limited detail per milestone

### New: 3-Phase Milestone Planning

Replace with a more detailed, phased approach:

| Phase | Description |
|-------|-------------|
| **Phase 1: Feature Discovery** | Enumerate all features from SPEC.md, gather requirements |
| **Phase 2: Milestone Structuring** | Define milestone boundaries, dependencies, priorities |
| **Phase 3: Detail Planning** | Write detailed scope, acceptance criteria per milestone |

**Rationale:** The old single-pass approach doesn't capture enough detail for complex projects. Breaking into 3 phases allows deeper thinking at each stage.

---

## Phase 3: Architecture (SIMPLIFYING)

### Old: `arch-init` → `arch-decompose`

- Recursive/loop-based decomposition
- Continues until all nodes are "atomic" or "decomposed"
- Multiple passes through the tree

### New: Simplified Non-Recursive Flow

| Step | Description |
|------|-------------|
| **1. Initialize** | Create ARCH_SUMMARY.md with root node |
| **2. One-pass Decompose** | Decompose root into immediate children only |
| **3. Define Leaves** | Mark leaf nodes as "atomic" during decomposition |
| **Done** | No loop - single decomposition pass per invocation |

**Rationale:**
- Eliminates the "keep running arch-decompose until done" pattern
- Each invocation has clear, bounded scope
- Forces clearer thinking upfront about node boundaries

---

## Phase 4: Implementation (SIMPLIFYING)

### Old: 6 Skills

```
milestone-init → node-prep → node-build → milestone-integrate → milestone-test-add → milestone-wrapup
```

| Skill | Purpose |
|-------|---------|
| `milestone-init` | Derive node capabilities for milestone |
| `node-prep` | Create skeleton + failing tests |
| `node-build` | Implement to make tests pass |
| `milestone-integrate` | Create integration tests |
| `milestone-test-add` | Add tests for modified nodes |
| `milestone-wrapup` | Manual verification + completion |

### New: 4-Phase Flow

```
plan → test-first → implement → verify
```

| Phase | Old Mapping | Description |
|-------|-------------|-------------|
| **Plan** | `milestone-init` | Define what's needed per node |
| **Test-first** | `node-prep` | Write failing tests first |
| **Implement** | `node-build` | Make tests pass |
| **Verify** | `milestone-integrate` + `milestone-wrapup` | Integration tests + manual check |

**Changes:**
- Merge `milestone-integrate` into verify phase
- Merge `milestone-test-add` into implement phase (handle as part of node-build)
- Simplify wrapup to just "verify" - less ceremony
- Remove explicit "modified" state tracking - handle inline

**Rationale:** Fewer skills to remember, smoother flow, less state management overhead.

---

## Phase 5: Issues & Quick Fixes (ADAPT)

| Old Skill | New Approach | Changes |
|-----------|--------------|---------|
| `issue-create` | **Keep + Adapt** | Same purpose, but align with new milestone structure |
| `issue-plan` | **Keep + Adapt** | Same purpose, but simpler task phases |
| `issue-resolve` | **Keep + Adapt** | Same purpose, remove batch complexity |
| `quick-patch` | **Keep** | Works independently |

**Adaptations:**
- Issue phases can map to new 4-phase flow instead of old SPC/ARC/FND/TST/IMPL/POL
- Remove or simplify "batch with review checkpoints" - maybe just run to completion
- Keep quick-patch as-is since it's already a shortcut

---

## Summary: Skill Mapping

| Old Skill | New Skill/Phase | Status |
|-----------|-----------------|--------|
| `spec-create` | `spec-create` | Keep |
| `spec-clarify` | `spec-clarify` | Keep |
| `project-init` | `project-init` | Keep |
| `milestone-plan-all` | **`roadmap-draft`** | Renamed |
| `arch-init` + `arch-decompose` | **`arch-elaborate`** | **Merged** |
| `milestone-init` | **Plan** | Keep (renamed) |
| `node-prep` | **Test-first** | Keep (renamed) |
| `node-build` | **Implement** | Keep (renamed) |
| `milestone-integrate` | **Verify** | Merge |
| `milestone-test-add` | **Implement** | Merge |
| `milestone-wrapup` | **Verify** | Merge + Simplify |
| `issue-create` | `issue-create` | Keep (adapt) |
| `issue-plan` | `issue-plan` | Keep (adapt) |
| `issue-resolve` | `issue-resolve` | Keep (adapt) |
| `quick-patch` | `quick-patch` | Keep |

---

## New Toolkit Skills (Target)

1. `spec-create`
2. `spec-clarify`
3. `project-init`
4. *milestone-plan-1* (feature discovery)
5. *milestone-plan-2* (structuring)
6. *milestone-plan-3* (detail planning)
7. `arch-init`
8. `arch-decompose`
9. `milestone-init` (or `milestone-plan`)
10. `node-prep` (or `test-first`)
11. `node-build` (or `implement`)
12. `milestone-integrate` (or `verify`)
13. `issue-create`
14. `issue-plan`
15. `issue-resolve`
16. `quick-patch`

**Net result:** ~16 skills (same count, but organized more cleanly)
