# Planning Protocol

## Planner Responsibilities

`project-planner` reads the user requirement file and material manifest, then writes:

```text
.agent-work/plans/plan-v<N>.md
.agent-work/tasks/task-queue-v<N>.json
.agent-work/state/current-batch-control.json
```

The coordinator only receives and shows paths/status. It does not read plan or task正文.

## Plan Confirmation

- Initial plan returns `STATUS: WAITING_USER_CONFIRMATION`.
- Coordinator sends only `PLAN_PATH` and `TASK_QUEUE_PATH` to the user.
- User feedback is written to `.agent-work/input/user-feedback-v<N>.md`.
- Revisions must resume the same planner when possible.

## Agent Selection

Planner must assign each task:

```json
{
  "recommended_agent": "frontend-developer",
  "required_testers": ["tester-code-quality", "tester-runtime-effect"]
}
```

Do not default to all testers. If uncertain, use:

```json
["tester-code-quality", "tester-runtime-effect"]
```

Add specialized testers only when the task requires them:

- UI appearance: `tester-visual-aesthetic`
- Security-sensitive code: `tester-security`
- Performance-sensitive flow: `tester-performance`
- Data/math/report correctness: `tester-data-integrity`
- Web accessibility: `tester-accessibility`

## Batch Control

For each active batch, planner writes `.agent-work/state/current-batch-control.json` using `schemas.md`. This is the coordinator's source of truth for dispatch.

The file must contain task paths, developer agent names, tester names, output index path, and attempt. It must not contain task正文 or acceptance正文.
