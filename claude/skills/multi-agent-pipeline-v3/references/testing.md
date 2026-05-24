# Testing Protocol

## Evidence-Based Testing

Each tester must write:

```text
.agent-work/test-reports/<batch_id>/<tester-name>/attempt-<N>/result.json
.agent-work/test-reports/<batch_id>/<tester-name>/attempt-<N>/<PASS_or_FAIL>__<scope>-test-report.md
```

`result.json` is coordinator-readable. Full report is for the developer/tester only.

## Common Result Fields

Use `schemas.md` for the full shape. Required fields:

```json
{
  "status": "PASS",
  "issue_count": 0,
  "blocking_issue_count": 0,
  "test_report_path": "...",
  "evidence_paths": [],
  "metrics": {},
  "limitations": [],
  "confidence": "high"
}
```

## Tester-Specific Evidence

- `tester-code-quality`: build/lint/typecheck logs when available; code review report path; task package completeness check; after a repair, verify `experience-append-summary.md` exists and the appended entry passes `references/experience.md`.
- `tester-runtime-effect`: server log, browser console log, screenshot or interaction trace. For web apps, prefer installed `webapp-testing`.
- `tester-visual-aesthetic`: desktop screenshot and mobile screenshot. Prefer installed `frontend-design`. If no screenshot can be produced, report limitation and do not PASS visual quality solely from CSS inspection.
- `tester-security`: dependency audit output, secret scan output, security review notes. If tools are unavailable, record `TOOL_UNAVAILABLE`; do not install global tools without explicit permission.
- `tester-performance`: build time, load time, bundle size, API latency, memory/time where applicable.
- `tester-data-integrity`: independent recomputation, fixture check, or sample calculation evidence.
- `tester-accessibility`: keyboard path, contrast/ARIA notes, axe/browser evidence if available.

## PASS Rules

A tester can PASS only when:

- required evidence exists or limitations are non-blocking,
- task package fields were sufficient for the implemented scope,
- developer did not read source requirements outside authorized anchors,
- blocking issue count is zero,
- task-relevant acceptance has been checked,
- required post-repair experience entries exist and are principle-level rather than page/value-level,
- result JSON is complete.

`SKIP` is allowed only when the tester is not relevant to the task type. Planner should avoid scheduling irrelevant testers.
