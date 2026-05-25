# project-planner Experience Library

UTF-8 experience library. Keep full entries under 30 items; merge older low-reference items into distilled principles.

## Three Quality Rules

1. Principle over number: write why the decision was wrong, not what literal value changed.
2. Pattern over page: write which reusable pattern tends to cause the mistake, not a page-specific fix.
3. Transferable over copyable: after removing concrete values, page names, file names, and project nouns, the lesson must still guide a future project.

Bad: focus card shimmer uses rgba(217,119,87,0.12)
Good: Custom shimmer on accent cards should stay in the same color family as the border treatment.

Bad: page14 stair diagram CSS class should be common
Good: Reusable visual components should use role-based CSS class names rather than page-specific names.

## Distilled Principles
### Distilled Principle - Plan Rejection Learning

LAST_REFERENCED: 2026-05-25T00:00:00Z
REFERENCE_COUNT: 1
SOURCE_AGENT: project-planner

PRINCIPLE:
When a user rejects a plan as too simple or incomplete, the planner should identify the planning pattern that failed, such as over-compressed requirements, missing task-package fields, weak source anchors, or incomplete tester selection, then revise the task packages rather than merely expanding the plan prose.

WHY:
Planner mistakes propagate to every downstream developer and tester. A rejected plan is useful training signal only when it is abstracted into a reusable planning rule.

DO_NOT_REPEAT:
- Do not record page-specific misses such as `task-016 was too short`; rewrite them as task decomposition, requirement anchoring, or tester selection principles.
<!-- Add compact merged principles here. -->

## Full Entries

<!--
### Experience - YYYY-MM-DDTHH:mm:ssZ

SOURCE_AGENT: project-planner
TASK_ID: <task-id or N/A>
FAILURE_TYPE: implementation | test | planning | integration | packaging | process
LAST_REFERENCED: YYYY-MM-DDTHH:mm:ssZ
REFERENCE_COUNT: 1

PRINCIPLE:
<one transferable principle>

WHY:
<why this was wrong and why the principle generalizes>

DO_NOT_REPEAT:
- <pattern-level anti-pattern>
-->