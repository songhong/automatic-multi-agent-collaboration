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

## Plan Dissatisfaction Routing

During plan confirmation, the following user intents must be routed to `project-planner`; they never authorize the coordinator to read business files:

- "不满意", "没理解", "漏了", "不对"
- "重新看", "仔细看", "完整看"
- "阅读任务文件", "阅读需求文件", "看我给的文件"
- "按原文件重做", "重新理解需求"

Coordinator steps:

1. Write the user's exact feedback to `.agent-work/input/user-feedback-v<N>.md`.
2. Resume the same planner when possible.
3. Send the planner these paths only:
   - `PROJECT_REQUIREMENTS_PATH`
   - `MATERIALS_MANIFEST_PATH`
   - `PREVIOUS_PLAN_PATH`
   - `PREVIOUS_TASK_QUEUE_PATH`
   - `USER_FEEDBACK_PATH`
4. The planner reads the requirement/material/previous plan files and writes a revised plan.
5. The coordinator returns only revised paths and status to the user.

The coordinator must not open, summarize, quote, or inspect the requirement body, task body, plan body, or material body while handling dissatisfaction.

## Task Package Completeness

Planner must not compress long requirements into vague task summaries. Every generated `task.md` must be a self-contained task package that lets the assigned developer work without guessing.

Each task package must contain:

- goal and scope
- relevant requirement anchors
- material paths
- acceptance criteria
- expected output paths
- uncertainty list
- developer source-read authorization

For long requirements, planner should not copy the full requirement into every task. Instead, write precise anchors:

```yaml
SOURCE_REQUIREMENTS_PATH: .agent-work/input/project-requirements.md
DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: true
SOURCE_ANCHORS:
  - anchor_id: REQ-001
    must_read: true
    locator: "section title, keyword, page id, paragraph description, or line hint"
    reason: "why this source text is needed for this task"
```

If a task can be completed from `task.md` alone, set `DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: false` and still explain the relevant source requirement in the task package.

If planner cannot locate exact line numbers, use section titles, unique phrases, page names, material filenames, or paragraph descriptions as locators.

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
