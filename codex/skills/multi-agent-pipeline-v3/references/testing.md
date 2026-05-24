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

- `tester-code-quality`: structure, maintainability, lint/type checks when available, local rubrics, task package completeness, and post-repair experience entry quality.
- `tester-runtime-effect`: build, smoke test, command output, artifact openability.
- `tester-visual-aesthetic`: layout, visual polish, screenshots, responsive checks.
- `tester-security`: injection risk, secret leakage, unsafe file/network operations.
- `tester-performance`: speed, bundle size, memory, obvious bottlenecks.
- `tester-data-integrity`: calculations, schemas, file transforms, reproducibility.
- `tester-accessibility`: labels, keyboard flow, contrast, semantic structure.

## Coordinator Aggregation

The coordinator reads only status, counts, evidence paths, and report paths. Any required tester with `FAIL` or `BLOCKED` sends the batch to repair.

## Task Package Check

At least `tester-code-quality` must verify:

- worker manifest includes `task_package_used`
- task file includes goal, scope, acceptance criteria, expected outputs, source requirement path, source anchors, and source-read authorization
- worker did not read source requirements outside authorized anchors

If the task package is too vague, acceptance criteria are missing, or source-read authorization was exceeded, return `FAIL` or `BLOCKED`.

## Experience Check

After a repair attempt succeeds, at least `tester-code-quality` must verify:

- `<OUTPUT_DIR>/experience-append-summary.md` exists
- the summary points to `.agent-work/experience/<worker-role>.md`
- the appended entry passes `references/experience.md`
- the lesson is principle-level, pattern-level, and transferable rather than a literal page/value/file tweak

If the entry is missing or too concrete, return `FAIL` or `BLOCKED` with the correction path.
