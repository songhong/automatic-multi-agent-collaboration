# Execution Protocol

## Developer Dispatch

Coordinator reads `.agent-work/state/current-batch-control.json` and starts the specified developer agent for each task group.

Developer agents read:

- `TASK_PATH`
- relevant project files
- source requirement anchors from `TASK_PATH` only when explicitly authorized
- `.agent-work/experience/shared-principles.md`
- their own experience file
- `references/experience.md` when reading or writing experience entries

Developer agents write:

- implementation manifest
- development summary
- evidence files for commands run

They return only structured status and paths.

## Experience Update

After any successful repair, the responsible developer must append a transferable lesson to:

- `.agent-work/experience/<developer-agent>.md`
- the matching global experience file when writable
- `shared-principles.md` only when the lesson is cross-role

The entry must pass the three rules in `references/experience.md`: principle over number, pattern over page, and transferable over copyable. The developer must also write:

```text
<OUTPUT_DIR>/experience-append-summary.md
```

The summary lists project and global paths touched. It must not contain code bodies, secrets, or long business text.

## Task Package Gate

Before implementation, every developer must inspect `TASK_PATH` for these fields:

- `GOAL`
- `SCOPE`
- `ACCEPTANCE_CRITERIA`
- `EXPECTED_OUTPUT_PATHS`
- `SOURCE_REQUIREMENTS_PATH`
- `SOURCE_ANCHORS`
- `DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS`

If required fields are missing, vague, or insufficient to implement safely, do not start development. Write a clarification request:

```text
.agent-work/handoffs/<batch_id>/<developer-agent>/attempt-<N>/NEEDS_TASK_CLARIFICATION.md
```

Return `STATUS: NEEDS_TASK_CLARIFICATION` with that path. The coordinator routes it to `project-planner`.

Developers may read the original requirement file only when `DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: true`, and only for the listed `SOURCE_ANCHORS`. Do not freely read unrelated requirement sections.

## Evidence Requirement

Every implementation manifest must include:

```text
VERIFICATION_EVIDENCE:
- command: <command or N/A>
  status: pass | fail | skipped
  evidence_path: <path or N/A>
TASK_PACKAGE_USED:
  task_path: <TASK_PATH>
  source_requirements_read: true | false
  source_anchor_ids_read:
    - <anchor_id or N/A>
```

If a verification command cannot run, record `skipped` with a reason in the manifest. Do not claim it passed.

## Integration

Use `fullstack-integrator` when a batch has multiple developers, frontend/backend contracts, shared config, routes, env, or startup scripts.

Integrator writes:

```text
.agent-work/handoffs/<batch_id>/fullstack-integrator/attempt-<N>/integration-summary.md
```

The coordinator forwards the integration summary path to testers but does not read正文.

## Required Output Check

Before tester dispatch, coordinator reads `.agent-work/state/output-check-index/<batch_id>.json` and checks only that each `required: true` path exists and size is greater than zero.

If missing or empty, resume the same developer/integrator for output repair. This does not count as a tester repair attempt.

## Conservative Parallel Dispatch

Coordinator may dispatch tasks in the same `parallel_group_id` concurrently only when `current-batch-control.json` shows:

- `conflict_risk` is `low`, or `medium` with `integration_required: true`,
- `dependency_task_ids` are empty or already completed,
- `shared_output_paths` is empty across concurrently dispatched tasks,
- no two tasks target the same core config, schema, route, generated file, or required output.

If any condition is unclear, dispatch serially. Parallel speed must not break ownership: the developer who created a failing output repairs it, and the tester who found the issue retests it.

After parallel developers finish, run `fullstack-integrator` before testers when the batch includes multiple developers, shared contracts, routes, startup scripts, or cross-output consistency requirements.
