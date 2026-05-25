# Control Plane Schemas

## current-batch-control.json

```json
{
  "run_id": "20260524-183647",
  "batch_id": "batch-001",
  "attempt": 1,
  "status": "ready_for_development",
  "tasks": [
    {
      "task_id": "task-001",
      "page_id": "page-001",
      "task_path": ".agent-work/tasks/task-001/task.md",
      "source_requirements_path": ".agent-work/input/project-requirements.md",
      "source_anchors": [
        {
          "anchor_id": "REQ-001",
          "must_read": true,
          "locator": "section title, keyword, paragraph description, or line hint",
          "reason": "why this source text is needed"
        }
      ],
      "developer_may_read_source_requirements": true,
      "developer_agent": "frontend-developer",
      "tester_agents": ["tester-code-quality", "tester-runtime-effect"],
      "output_check_index_path": ".agent-work/state/output-check-index/batch-001.json"
    }
  ],
  "integrator_agent": "fullstack-integrator",
  "integration_required": false
}
```

## Agent Return JSON

Every child agent final response must include a fenced JSON block:

```json
{
  "agent_name": "frontend-developer",
  "agent_id": "RUNTIME_UNKNOWN",
  "run_id": "20260524-183647",
  "batch_id": "batch-001",
  "attempt": 1,
  "status": "READY_FOR_TEST",
  "paths": {
    "development_summary_path": ".agent-work/handoffs/...",
    "clarification_request_path": null
  },
  "task_package_used": {
    "task_path": ".agent-work/tasks/task-001/task.md",
    "source_requirements_read": true,
    "source_anchor_ids_read": ["REQ-001"]
  }
}
```

## test result.json

```json
{
  "run_id": "20260524-183647",
  "batch_id": "batch-001",
  "tester": "tester-runtime-effect",
  "tester_agent_id": "RUNTIME_UNKNOWN",
  "attempt": 1,
  "status": "PASS",
  "issue_count": 0,
  "blocking_issue_count": 0,
  "bug_owner_executor": "frontend-developer",
  "test_report_path": ".agent-work/test-reports/...",
  "failed_task_ids": [],
  "failed_page_ids": [],
  "evidence_paths": [],
  "metrics": {},
  "limitations": [],
  "confidence": "high"
}
```

## output-check-index

```json
{
  "run_id": "20260524-183647",
  "batch_id": "batch-001",
  "outputs": [
    {
      "task_id": "task-001",
      "page_id": "page-001",
      "path": "output/page-001.html",
      "required": true,
      "type_hint": "html"
    }
  ]
}
```

## Task Queue Quality And Parallel Fields

Every task item in `task-queue-v<N>.json` must include these fields in addition to the base task fields:

```json
{
  "quality_gate": "completion_quality_gate",
  "premium_review_required": false,
  "premium_review_reason": "N/A",
  "quality_acceptance_criteria": [],
  "parallel_group_id": "parallel-group-001",
  "dependency_task_ids": [],
  "conflict_risk": "low",
  "shared_output_paths": []
}
```

`quality_acceptance_criteria` must describe user-visible completion quality. Do not fill it with generic phrases only; tie it to the task's goal and source anchors.

## current-batch-control Quality Fields

Each task in `current-batch-control.json` must carry these coordinator-readable quality fields:

```json
{
  "quality_gate": "completion_quality_gate",
  "premium_review_required": false,
  "premium_review_reason": "N/A",
  "parallel_group_id": "parallel-group-001",
  "dependency_task_ids": [],
  "conflict_risk": "low",
  "shared_output_paths": []
}
```

These fields are control-plane metadata and may be read by coordinator. They must not contain requirement body text.

## Test Result Quality Fields

Every tester `result.json` must include issue category counts:

```json
{
  "correctness_issue_count": 0,
  "quality_issue_count": 0,
  "premium_review": false,
  "quality_gate": "completion_quality_gate"
}
```

## Plan Review Schemas

`planning-readiness-v<N>.json`:

```json
{
  "plan_version": 1,
  "status": "READY_FOR_PLAN_REVIEW",
  "requirements_sufficient": true,
  "user_questions_needed": false,
  "ask_user_questions_path": null,
  "approach_options_considered": 2,
  "missing_information": [],
  "reason_no_questions_needed": "The requirements define target output, data sources, quality bar, and deployment constraints."
}
```

`plan-quality-check-v<N>.json`:

```json
{
  "plan_version": 1,
  "status": "READY_FOR_PLAN_REVIEW",
  "has_success_criteria": true,
  "has_material_mapping": true,
  "has_approach_comparison": true,
  "has_architecture_or_data_flow": true,
  "has_module_implementation_details": true,
  "has_task_dependencies": true,
  "has_testing_strategy": true,
  "has_quality_gates": true,
  "has_deliverables": true,
  "shallow_module_list_risk": false
}
```

Plan review `result.json`:

```json
{
  "agent_name": "plan-reviewer",
  "agent_id": "RUNTIME_UNKNOWN",
  "plan_version": 1,
  "review_attempt": 1,
  "status": "PASS",
  "issue_count": 0,
  "blocking_issue_count": 0,
  "major_issue_count": 0,
  "needs_planner_rewrite": false,
  "review_report_path": ".agent-work/plan-reviews/plan-review-v1/PLAN_REVIEW_PASS.md",
  "failed_sections": [],
  "planner_agent_to_resume": "project-planner",
  "confidence": "high"
}
```

Coordinator may read plan review `result.json`, but must not read `PLAN_REVIEW_FAIL.md` or `PLAN_REVIEW_PASS.md` bodies. Those report paths are for planner and plan-reviewer.
