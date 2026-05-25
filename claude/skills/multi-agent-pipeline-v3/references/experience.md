# Experience Library Protocol

## Paths

Each agent has a project cache and a global library:

```text
PROJECT_SHARED_EXPERIENCE: .agent-work/experience/shared-principles.md
PROJECT_AGENT_EXPERIENCE: .agent-work/experience/<agent-name>.md
CLAUDE_GLOBAL_EXPERIENCE: /home/zhuyu/.claude/agent-experience/<agent-name>.md
CODEX_GLOBAL_EXPERIENCE: C:\Users\zhuyu\.codex\agent-experience\<agent-name>.md
```

The coordinator may create, copy, and pass these paths, but must not read experience bodies. Subagents read their own project cache plus `shared-principles.md`.

## Initialization

During init:

1. Create `.agent-work/experience/`.
2. For every configured agent, ensure a UTF-8 project cache file exists.
3. If a global experience file exists, copy or merge it into the project cache.
4. If a global experience file is missing, create it from the template below.
5. Preserve existing project experience; do not delete it during run archive.

## Entry Template

Every agent experience file uses this shape:

```markdown
# <Agent Name> Experience Library

## Distilled Principles

<!-- Add compact merged principles here. -->

## Full Entries

### Experience - YYYY-MM-DDTHH:mm:ssZ

SOURCE_AGENT: <agent-name>
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
```

## Three Quality Rules

An entry is allowed only when it passes all three rules:

1. Principle over number: write why the decision was wrong, not which literal value changed.
2. Pattern over page: write which layout, architecture, data, document, or workflow pattern tends to cause the mistake.
3. Transferable over copyable: the lesson must still guide a different project with different content.

Decision test: remove concrete numbers, page names, filenames, and project-specific nouns. If the sentence no longer guides a future decision, rewrite it before appending.

Bad examples:

- `The focus card shimmer should use rgba(217,119,87,0.12).`
- `Page 14 stair diagram CSS class should be common.`

Good examples:

- `Custom shimmer on accent cards should stay in the same color family as the border treatment.`
- `Reusable visual components should use role-based CSS class names rather than page-specific names.`

## Write Policy

- Developers must append an experience entry after a repair succeeds.
- `project-planner` must write a planner experience decision after any plan rejection, "plan too simple" feedback, requirement reread request, developer `NEEDS_TASK_CLARIFICATION`, or tester report that task packages/anchors/tester choices were insufficient.
- Testers must check whether the developer wrote a qualifying entry after a repair. If the entry is missing or too concrete, fail or block with a path to the required correction.
- Any subagent may append a qualifying entry when it discovers a reusable lesson in its own role.
- If the lesson is cross-role, append it to `shared-principles.md` as well as the agent-specific file.
- Each completion must write `<OUTPUT_DIR>/experience-append-summary.md` with paths touched and entries added.

## Global Sync

When a subagent appends a valid entry, it must write the same entry to:

1. `.agent-work/experience/<agent-name>.md`
2. The matching global file for the current runtime, if writable.
3. `.agent-work/experience/shared-principles.md` and the global shared file only when the lesson is cross-role.

If the global path is not writable, write `GLOBAL_EXPERIENCE_SYNC: SKIPPED_PERMISSION` in the experience append summary and continue. Do not request elevated permission from inside a subagent.

## Capacity

- Agent-specific libraries keep at most 30 full entries.
- Shared principles keep at most 10 full entries.
- When over capacity, merge the least recently referenced entries into a compact distilled principle, then remove the merged full entries.
- When an existing entry is useful, update `LAST_REFERENCED` and increment `REFERENCE_COUNT`.
