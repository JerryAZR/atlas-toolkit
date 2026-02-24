---
name: milestone-review
description: Reviews code changes for architectural entropy increase, produces a report, drafts and executes refactor plans, and verifies no regression. Use when reviewing milestone or PR changes for architectural quality.
---

# Milestone Review Skill

This skill performs architectural entropy review on code changes, produces a structured report, and optionally executes refactoring to contain entropy increase.

## Workflow

### Phase 1: Code Review

1. **Identify the scope of review**
   - Determine what files/changes to review (latest milestone, PR, or commit)
   - Use git commands to identify changed files if needed

2. **Load the review checklist**
   - Load `references/entropy_checklist.md` for the audit dimensions

3. **Conduct the review** against each dimension:
   - **Canonical State Authority**: Check for new state stores, derived state persistence, conflicting authoritative sources
   - **Writer Count Expansion**: Identify mutation sites, count writers per variable, verify atomic transitions
   - **Identity Coherence**: Look for new identifiers, conflicts with existing schemes, normalization issues
   - **Transition Atomicity**: Examine failure paths, rollback behavior, cleanup determinism
   - **Hidden Coupling Increase**: Detect cross-module reads, global state dependencies, order-dependent behavior

4. **Report findings** in a structured format (passed directly to Phase 2):
   ```
   ## Architectural Entropy Report

   ### Dimension 1: Canonical State Authority
   - [Finding 1]
   - [Finding 2]

   ### Dimension 2: Writer Count Expansion
   ...

   ```

### Phase 2: Refactor Plan Drafting

1. **Analyze findings** to determine which issues require refactoring
2. **Prioritize** based on severity and impact:
   - High: Canonical state conflicts, increased writers outside atomic blocks
   - Medium: Identity conflicts, hidden coupling
   - Low: Minor normalization issues, documentation needs
3. **Draft a refactor plan** with specific actionable items:
   - Each item should reference the specific file and line
   - Include the fix approach
   - Estimate complexity (simple/medium/complex)

### Phase 3: Execute Refactor Plan

1. **Execute each refactor item**:
   - Identify and update existing relevant tests for the touched feature/codepath
   - Write new tests only if lacking coverage of the touched feature/codepath
   - Implement the fix
   - Verify tests pass
2. **Track progress** using TodoWrite for each refactor item

### Phase 4: Verify No Regression

1. **Run the test suite** to confirm no regression
2. **Summarize any problems fixed** - report what issues were resolved during the refactor
