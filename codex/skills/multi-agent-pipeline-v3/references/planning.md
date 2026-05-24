# Planning

The planner role turns opaque requirements and material paths into a plan, task queue, and batch controls.

## Planner Input

Provide paths only:

- `PROJECT_REQUIREMENTS_PATH`
- `MATERIALS_MANIFEST_PATH`
- `BATCH_CONFIG_PATH`
- previous feedback path when revising

## Planner Output

The planner writes:

- `.agent-work/plans/plan-v<N>.md`
- `.agent-work/tasks/task-queue-v<N>.json`
- `.agent-work/state/current-batch-control.json` when asked for the next batch

The planner returns only:

```json
{
  "status": "PLAN_READY",
  "plan_path": ".agent-work/plans/plan-v1.md",
  "task_queue_path": ".agent-work/tasks/task-queue-v1.json",
  "current_batch_control_path": null,
  "questions_path": null
}
```

## Plan Review Loop

- The coordinator gives the user only the plan path and status.
- If the user requests changes, write feedback to `.agent-work/input/user-feedback-v<N>.md`.
- Resume the same planner role when possible. Otherwise use logical resume with the same paths and revision number.
- Do not proceed to execution until the user approves the plan.

## Task Queue Requirements

Every task must include:

- `task_id`
- `title`
- `task_file_path`
- `recommended_agent`
- `required_testers`
- `required_outputs`
- `acceptance_criteria`
- `dependencies`

Default `recommended_agent`: `development-agent`.

Default `required_testers`: `tester-code-quality`, `tester-runtime-effect`.

## Planner Agent Choices

Valid worker roles:

- `architect-agent`
- `development-agent`
- `frontend-developer`
- `backend-developer`
- `document-writer`
- `data-analyst`
- `toolsmith`
- `fullstack-integrator`

Valid tester roles:

- `tester-code-quality`
- `tester-runtime-effect`
- `tester-visual-aesthetic`
- `tester-security`
- `tester-performance`
- `tester-data-integrity`
- `tester-accessibility`
