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

## Plan Dissatisfaction Routing

During plan review, user feedback such as "not satisfied", "you missed it", "read my task file carefully", "read the requirement file completely", "look again", or equivalent wording must be routed to the planner. It never authorizes the coordinator to read business payloads.

Coordinator steps:

1. Write the user's exact feedback to `.agent-work/input/user-feedback-v<N>.md`.
2. Resume the same planner role when possible.
3. Send only paths:
   - `PROJECT_REQUIREMENTS_PATH`
   - `MATERIALS_MANIFEST_PATH`
   - `PREVIOUS_PLAN_PATH`
   - `PREVIOUS_TASK_QUEUE_PATH`
   - `USER_FEEDBACK_PATH`
4. The planner reads those files and writes a revised plan and task queue.
5. The coordinator returns only revised paths and status.

The coordinator must not open, summarize, quote, or inspect the requirement body, task body, plan body, or material body while handling dissatisfaction.

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
