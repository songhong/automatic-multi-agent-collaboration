# Logging

Logging is part of the workflow, not an afterthought.

## JSONL Event

Append one JSON object per important event:

```json
{
  "ts": "2026-05-24T19:00:00+08:00",
  "run_id": "20260524-190000",
  "stage": "testing",
  "batch_id": "B001",
  "agent_role": "tester-runtime-effect",
  "event": "TEST_COMPLETE",
  "status": "PASS",
  "paths": [".agent-work/results/B001/tester-runtime-effect-result.json"]
}
```

## Required Events

- `RUN_INITIALIZED`
- `PLAN_REQUESTED`
- `PLAN_READY`
- `PLAN_REVISION_REQUESTED`
- `PLAN_APPROVED`
- `BATCH_STARTED`
- `DEV_STARTED`
- `DEV_COMPLETE`
- `TEST_STARTED`
- `TEST_COMPLETE`
- `REPAIR_STARTED`
- `REPAIR_COMPLETE`
- `BATCH_PASSED`
- `BATCH_FAILED_NEEDS_HUMAN`
- `PIPELINE_COMPLETE`

## User Progress

Progress messages should be concise and path-based:

- what completed
- current status
- next action
- relevant report path

Do not paste code, test report bodies, or private content.
