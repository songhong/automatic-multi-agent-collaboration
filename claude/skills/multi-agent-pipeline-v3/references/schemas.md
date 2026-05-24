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
