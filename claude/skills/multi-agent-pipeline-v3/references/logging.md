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

## Plan Review Events

Log plan review events without plan or review bodies:

```json
{
  "stage": "planning",
  "event": "plan_review_started | plan_review_passed | plan_review_failed | plan_review_human_review_required",
  "agent_name": "plan-reviewer",
  "agent_id": "<agent id or RUNTIME_UNKNOWN>",
  "status": "running | pass | fail | blocked",
  "paths": [".agent-work/plan-reviews/plan-review-v<N>/result.json"]
}
```

If plan review fails, log only result/report paths and counts, never the review body.
