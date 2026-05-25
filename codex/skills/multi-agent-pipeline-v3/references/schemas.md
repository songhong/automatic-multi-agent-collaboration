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

## Plan Review Schemas

Planner readiness path: `.agent-work/plans/planning-readiness-v<N>.json`.
Planner self-check path: `.agent-work/plans/plan-quality-check-v<N>.json`.
Plan review result path: `.agent-work/plan-reviews/plan-review-v<N>/result.json`.

Plan review `result.json` must include `agent_name`, `agent_id`, `plan_version`, `review_attempt`, `status`, `issue_count`, `blocking_issue_count`, `major_issue_count`, `needs_planner_rewrite`, `review_report_path`, `failed_sections`, `planner_agent_to_resume`, and `confidence`.

Coordinator may read the result JSON only, not the review report body.



## Materials Manifest Schema

Materials manifest entries must be metadata-only: `path`, `filename`, `extension`, `size`, `last_modified`, `user_instruction`, `content_read_by_coordinator: false`, and `authorized_reader: project-planner`. Do not include summaries, excerpts, keywords, inferred topics, or body text.
