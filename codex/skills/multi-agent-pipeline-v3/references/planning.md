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

## Planner Rejection Learning

When the user rejects a plan, says it is too simple, asks the planner to reread the requirements, or when a developer/tester reports that task packages are too vague, the planner must treat the event as a planning feedback signal.

For every such revision, planner must:

1. Reread `PROJECT_REQUIREMENTS_PATH`, `MATERIALS_MANIFEST_PATH`, `PREVIOUS_PLAN_PATH`, `PREVIOUS_TASK_QUEUE_PATH`, and `USER_FEEDBACK_PATH`.
2. Rewrite the plan and task queue with stronger task packages, source anchors, acceptance criteria, output paths, and tester selection.
3. Write `.agent-work/plans/planner-experience-decision-v<N>.md`.
4. Return `planner_experience_decision_path` together with `plan_path` and `task_queue_path`.

The decision file must include `EXPERIENCE_DECISION: APPENDED | NO_TRANSFERABLE_LESSON`. If the old plan failed because of a transferable planning pattern, append a principle-level lesson to `.agent-work/experience/project-planner.md` and to the writable global `project-planner.md` experience file.

Planner experience must be about planning patterns, not project facts. For example, write `Long requirements should be decomposed into task packages with goal, scope, acceptance criteria, source anchors, and output paths rather than only deliverable titles`, not `task-016 was too short`.

## Task Queue Requirements

Every task must include:

- `task_id`
- `title`
- `task_file_path`
- `recommended_agent`
- `required_testers`
- `required_outputs`
- `acceptance_criteria`
- `source_requirements_path`
- `source_anchors`
- `developer_may_read_source_requirements`
- `uncertainties`
- `dependencies`

Default `recommended_agent`: `development-agent`.

Default `required_testers`: `tester-code-quality`, `tester-runtime-effect`.

## Task Package Completeness

Planner must not compress long requirements into vague task summaries. Every generated `task.md` must be a self-contained task package containing:

- goal and scope
- relevant requirement anchors
- material paths
- acceptance criteria
- expected output paths
- uncertainty list
- developer source-read authorization

For long requirements, do not copy the full requirement into every task. Use anchors:

```yaml
SOURCE_REQUIREMENTS_PATH: .agent-work/input/project-requirements.md
DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: true
SOURCE_ANCHORS:
  - anchor_id: REQ-001
    must_read: true
    locator: "section title, keyword, page id, paragraph description, or line hint"
    reason: "why this source text is needed for this task"
```

If a task can be completed from `task.md` alone, set `DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: false` and still include the relevant source requirement details in the task package.

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

## Planner Brainstorm And User Question Protocol

Planner is the only role responsible for understanding business requirements. Coordinator must not read requirement, plan, or task bodies.

Use a brainstorming-style flow when requirements are ambiguous, too large, rejected by the user, or likely to produce a shallow plan:

1. Explore the provided requirement and material paths.
2. Identify missing intent, constraints, audience, success criteria, and tradeoffs.
3. Ask one focused question at a time when the answer materially changes the plan.
4. When there are meaningful alternatives, write 2-3 approaches with tradeoffs and a recommendation.
5. Only after the user-facing direction is clear, write the execution plan and task queue.

When user input is needed, planner writes `.agent-work/human-review/planner-questions-v<N>.json` and returns `ASK_USER_QUESTIONS_PATH` with `STATUS: NEEDS_USER_INPUT`. Coordinator may pass this path to the user or ask the planner-authored question; it must not invent business interpretation.

## Conservative Parallel Planning

Planner may group tasks for parallel execution only when tasks have no unresolved dependency, no shared required output path, no planned edits to the same core config/schema/route/generated artifact, and complete task packages. If uncertain, set `conflict_risk: high` and keep the task serial.

## Plan Quality Gate

Every initial or revised plan must pass `plan-reviewer` before it is shown to the user. Planner writes `planning-readiness-v<N>.json`, `plan-quality-check-v<N>.json`, `plan-v<N>.md`, and `task-queue-v<N>.json`; coordinator then calls `plan-reviewer` with paths only.

Coordinator reads only `.agent-work/plan-reviews/plan-review-v<N>/result.json`. If review FAILS, coordinator resumes the same `project-planner` and passes `PLAN_REVIEW_REPORT_PATH` and `PLAN_REVIEW_RESULT_PATH`. Maximum 3 plan review repair attempts; then stop for human review.

A plan that is only a module list or short bullet checklist must fail plan review.



## Reference Material Routing

When the user says to reference, read, view, use, combine, or follow material files, coordinator routes the instruction to `project-planner` by path and must not open the material body. Handoff includes `PROJECT_REQUIREMENTS_PATH`, `MATERIALS_MANIFEST_PATH`, and `USER_MATERIAL_AUTHORIZATION: user allowed planner to reference listed material contents`.
