# Schemas

These are minimal control-plane shapes. Agents may add fields if they do not place business bodies in coordinator-readable files.

## batch-config.json

```json
{
  "run_id": "20260524-190000",
  "batch_size": 1,
  "max_repair_attempts": 3,
  "output_dir": "TO_BE_DECIDED_BY_PLANNER"
}
```

## agent-id-cache.json

```json
{
  "project-planner": {
    "runtime_id": "RUNTIME_UNKNOWN",
    "role": "planner",
    "last_used_at": "2026-05-24T19:00:00+08:00",
    "resume_mode": "logical"
  }
}
```

## current-batch-control.json

```json
{
  "batch_id": "B001",
  "status": "READY",
  "tasks": [
    {
      "task_id": "T001",
      "task_file_path": ".agent-work/tasks/T001.md",
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
      "recommended_agent": "development-agent",
      "required_testers": ["tester-code-quality", "tester-runtime-effect"],
      "required_outputs": [
        {"path": "dist/index.html", "required": true}
      ]
    }
  ]
}
```

## pipeline-state.json

```json
{
  "run_id": "20260524-190000",
  "stage": "execution",
  "current_batch_id": "B001",
  "status": "IN_PROGRESS",
  "completed_batches": [],
  "blocked_batches": []
}
```

## Task Queue Quality And Parallel Fields

Every task item must include:

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

Every tester `result.json` must include `correctness_issue_count`, `quality_issue_count`, `premium_review`, and `quality_gate`.
