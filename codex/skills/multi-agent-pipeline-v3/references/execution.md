# Execution

Execution is batch-based. The coordinator dispatches the worker selected by the planner and reads only structured return files.

## Worker Handoff

Send the worker:

- current batch control path
- task file paths
- materials manifest path
- output directory
- required output index path
- previous attempt manifest path when repairing
- previous failing test result path when repairing

## Worker Output

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
  "notes_path": ".agent-work/results/B001/dev-notes.md"
}
```

The coordinator may read the manifest fields but not the implementation artifact bodies or long notes.

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
