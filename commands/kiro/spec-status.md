---
description: Show specification status and progress
allowed-tools: Bash, Read, Glob, Write, Edit, MultiEdit, Update
---

# Specification Status

Show current status and progress for feature: **$ARGUMENTS**

## Spec Context

### Check if feature is specified

!`if [ -z "$ARGUMENTS" ]; then echo "📊 No specific feature provided - showing all specs overview"; else echo "📋 Showing status for feature: $ARGUMENTS"; fi`

### Spec Files (when feature is specified)

!`if [ -n "$ARGUMENTS" ]; then if [ -d ".kiro/specs/$ARGUMENTS" ]; then echo "Feature directory found:"; ls -la .kiro/specs/$ARGUMENTS/; else echo "⚠️ Feature '$ARGUMENTS' not found in .kiro/specs/"; fi; fi`

!`if [ -n "$ARGUMENTS" ] && [ -f ".kiro/specs/$ARGUMENTS/spec.json" ]; then echo "Spec metadata:"; cat .kiro/specs/$ARGUMENTS/spec.json; fi`

### All Specs Overview

- Available specs:
  !`if [ -d ".kiro/specs" ]; then ls -1 .kiro/specs/ | grep -v "^$" || echo "No specs found"; else echo "⚠️ No .kiro/specs directory found"; fi`

- Active specs:
  !`if [ -d ".kiro/specs" ]; then find .kiro/specs/ -name "spec.json" -exec grep -l "implementation_ready.*true" {} \; 2>/dev/null | sed 's|.*/specs/||' | sed 's|/spec.json||' || echo "No active implementations"; fi`

## Task: Generate Status Report

Generate appropriate status report based on whether a specific feature is
provided.

### If $ARGUMENTS is empty (show all specs overview)

List all available specifications with their current status:

1. Show each spec name and phase
2. Display completion percentage for each
3. Highlight active implementations
4. Show next recommended spec to work on

### If $ARGUMENTS contains a feature name (show detailed status)

Create comprehensive status report for the specification.

First, check if the spec exists:

- If spec exists: Parse @.kiro/specs/$ARGUMENTS/spec.json for language and
  generate detailed report
- If spec doesn't exist: Suggest running `/kiro:spec-init $ARGUMENTS` to create
  it

### 1. Specification Overview

Display:

- Feature name and description
- Creation date and last update
- Current phase (requirements/design/tasks/implementation)
- Overall completion percentage

### 2. Phase Status

For each phase, show:

- ✅ **Requirements Phase**: [completion %]

  - Requirements count: [number]
  - Acceptance criteria defined: [yes/no]
  - Requirements coverage: [complete/partial/missing]

- ✅ **Design Phase**: [completion %]

  - Architecture documented: [yes/no]
  - Components defined: [yes/no]
  - Diagrams created: [yes/no]
  - Integration planned: [yes/no]

- ✅ **Tasks Phase**: [completion %]
  - Total tasks: [number]
  - Completed tasks: [number]
  - Remaining tasks: [number]
  - Blocked tasks: [number]

### 3. Implementation Progress

If in implementation phase:

- Task completion breakdown
- Current blockers or issues
- Estimated time to completion
- Next actions needed

#### Task Completion Tracking

- Parse tasks.md checkbox status: `- [x]` (completed) vs `- [ ]` (pending)
- Count completed vs total tasks
- Show completion percentage
- Identify next uncompleted task

### 4. Quality Metrics

Show:

- Requirements coverage: [percentage]
- Design completeness: [percentage]
- Task granularity: [appropriate/too large/too small]
- Dependencies resolved: [yes/no]

### 5. Recommendations

Based on status, provide:

- Next steps to take
- Potential issues to address
- Suggested improvements
- Missing elements to complete

### 6. Steering Alignment

Check alignment with steering documents:

- Architecture consistency: [aligned/misaligned]
- Technology stack compliance: [compliant/non-compliant]
- Product requirements alignment: [aligned/misaligned]

## Instructions

1. **Check if $ARGUMENTS is provided**

   - If empty: Show overview of all specs
   - If provided: Show detailed status for specific spec

2. **For specific feature ($ARGUMENTS provided)**:

   - Verify spec exists in .kiro/specs/$ARGUMENTS/
   - Parse spec.json for language preference
   - Read all spec files (requirements.md, design.md, tasks.md)
   - Calculate detailed completion metrics
   - Show phase-by-phase progress

3. **For all specs overview (no $ARGUMENTS)**:

   - List all directories in .kiro/specs/
   - Show summary status for each spec
   - Identify which specs are actively being implemented
   - Recommend next actions

4. **Calculate completion percentages** based on:

   - Checkbox status in tasks.md
   - Approval status in spec.json
   - File existence for each phase

5. **Provide actionable recommendations**:
   - Next task to implement
   - Missing phases to complete
   - Blocked items to resolve

Generate status report that provides clear visibility into spec progress and
next steps.
