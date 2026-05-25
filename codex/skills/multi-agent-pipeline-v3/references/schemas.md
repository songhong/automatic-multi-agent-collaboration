# 控制面 Schema

## current-batch-control.json

```json
{
  "run_id": "<run id>",
  "batch_id": "<batch id>",
  "attempt": 1,
  "tasks": [
    {
      "task_id": "task-001",
      "task_path": ".agent-work/tasks/task-001/task.md",
      "recommended_agent": "frontend-developer",
      "required_testers": ["tester-code-quality", "tester-runtime-effect"],
      "quality_gate": "completion_quality_gate",
      "premium_review_required": false,
      "premium_review_reason": "N/A",
      "quality_acceptance_criteria": [],
      "parallel_group_id": "serial-001",
      "dependency_task_ids": [],
      "conflict_risk": "low",
      "shared_output_paths": []
    }
  ]
}
```

该文件只能包含控制字段和路径，不得包含需求、计划、任务或测试报告正文。

## task.md 必备字段

`TASK_ID`、`GOAL`、`SCOPE`、`NON_GOALS`、`SOURCE_REQUIREMENTS_PATH`、`DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS`、`SOURCE_ANCHORS`、`MATERIAL_PATHS`、`ACCEPTANCE_CRITERIA`、`QUALITY_ACCEPTANCE_CRITERIA`、`OUTPUT_PATHS`、`UNCERTAINTIES`。

## result.json 通用字段

result JSON 包含 agent 名、状态、问题数量、blocking/major 数量、报告路径和是否需要修复。coordinator 只读这些结构化字段。
