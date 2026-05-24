# Execution

Execution is batch-based. The coordinator dispatches the worker selected by the planner and reads only structured return files.

## Worker Handoff

Send the worker:

- current batch control path
- task file paths
- source requirement anchors from task files
- materials manifest path
- output directory
- required output index path
- previous attempt manifest path when repairing
- previous failing test result path when repairing
- project and global experience library paths

## Worker Output

## Task Package Gate

Before implementation, every worker must inspect each task file for:

- `GOAL`
- `SCOPE`
- `ACCEPTANCE_CRITERIA`
- `EXPECTED_OUTPUT_PATHS`
- `SOURCE_REQUIREMENTS_PATH`
- `SOURCE_ANCHORS`
- `DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS`

If required fields are missing, vague, or insufficient to implement safely, do not start work. Write `.agent-work/handoffs/<batch_id>/<worker-role>/attempt-<N>/NEEDS_TASK_CLARIFICATION.md` and return `STATUS: NEEDS_TASK_CLARIFICATION`.

Workers may read the original requirement file only when `DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: true`, and only for the listed `SOURCE_ANCHORS`. Do not freely read unrelated requirement sections.

Workers write implementation artifacts and a manifest:

```json
{
  "status": "DEV_COMPLETE",
  "agent_role": "frontend-developer",
  "task_ids": ["T001"],
  "attempt": 1,
  "changed_files_manifest_path": ".agent-work/results/B001/dev-manifest.json",
  "required_outputs": [
    {"path": "dist/index.html", "required": true, "exists": true, "non_empty": true}
  ],
  "task_package_used": {
    "task_path": ".agent-work/tasks/T001.md",
    "source_requirements_read": true,
    "source_anchor_ids_read": ["REQ-001"]
  },
  "notes_path": ".agent-work/results/B001/dev-notes.md"
}
```

The coordinator may read the manifest fields but not the implementation artifact bodies or long notes.

## Experience Update

After any successful repair, the responsible worker must append a transferable lesson to:

- `.agent-work/experience/<worker-role>.md`
- the matching global experience file when writable
- `shared-principles.md` only when the lesson is cross-role

The entry must pass the three rules in `references/experience.md`: principle over number, pattern over page, and transferable over copyable. The worker must also write:

```text
<OUTPUT_DIR>/experience-append-summary.md
```

The summary lists project and global paths touched. It must not contain code bodies, secrets, or long business text.

## Integration Role

Use `fullstack-integrator` when a batch crosses front-end/back-end, document/data, or tool/runtime boundaries. It produces an integration manifest and does not replace specialist workers.

## Output Existence Check

After development, the coordinator checks only required output existence and non-empty size using the output index. It does not inspect content.

## Progress Feedback

Report user-visible progress after each page, batch, or major artifact:

- current batch id
- status
- worker role
- tester roles
- result path
- next action
