# Testing

Testing must be evidence-based. Testers write result files and return only paths and status.

## Tester Handoff

Send each tester:

- batch control path
- dev manifest path
- output index path
- relevant artifact paths
- prior result path when retesting
- attempt number

## Tester Result

Each tester writes:

```json
{
  "status": "PASS",
  "tester_role": "tester-runtime-effect",
  "batch_id": "B001",
  "attempt": 1,
  "issue_count": 0,
  "blocking_issue_count": 0,
  "evidence": [
    {"kind": "command", "path": ".agent-work/results/B001/runtime-output.txt"}
  ],
  "report_path": ".agent-work/results/B001/tester-runtime-effect-result.json"
}
```

Allowed status values:

- `PASS`
- `FAIL`
- `SKIP`
- `BLOCKED`

`SKIP` requires a reason and must not hide required coverage.

## Tester Responsibilities

- `tester-code-quality`: structure, maintainability, lint/type checks when available, local rubrics.
- `tester-runtime-effect`: build, smoke test, command output, artifact openability.
- `tester-visual-aesthetic`: layout, visual polish, screenshots, responsive checks.
- `tester-security`: injection risk, secret leakage, unsafe file/network operations.
- `tester-performance`: speed, bundle size, memory, obvious bottlenecks.
- `tester-data-integrity`: calculations, schemas, file transforms, reproducibility.
- `tester-accessibility`: labels, keyboard flow, contrast, semantic structure.

## Coordinator Aggregation

The coordinator reads only status, counts, evidence paths, and report paths. Any required tester with `FAIL` or `BLOCKED` sends the batch to repair.
