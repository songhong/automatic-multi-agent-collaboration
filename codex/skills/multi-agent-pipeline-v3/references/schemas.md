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
