# Logging And State

## JSONL Events

Append one JSON object per key event to `.agent-work/logs/pipeline-log.jsonl`.

Required fields:

```json
{
  "time": "ISO8601",
  "run_id": "YYYYMMDD-HHMMSS",
  "stage": "planning",
  "event": "planner_started",
  "agent_name": "project-planner",
  "agent_id": "RUNTIME_UNKNOWN",
  "batch_id": "batch-001",
  "attempt": 1,
  "status": "running",
  "paths": []
}
```

Do not log business正文, code正文, bug details, secrets, tokens, cookies, or private file contents.

## User Progress

`.agent-work/logs/progress-log.md` is user-facing and concise:

```text
- batch-001: frontend-developer completed attempt 1; runtime tester PASS; visual tester FAIL; repair attempt 2 started.
```

Report to the user after each page/batch or when blocked.

## Timeouts

Use `TaskOutput` to poll child agents. If a child task exceeds the configured timeout:

1. Use `TaskStop` only on child tasks started by this coordinator.
2. Log `agent_timeout`.
3. Restart with `Agent()` logical Resume and full file paths.
4. If the second attempt times out, turn the batch over to human review.
