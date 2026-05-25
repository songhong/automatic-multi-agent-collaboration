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

## Completion Quality Gate

Every scheduled tester performs a completion quality check for its own scope. Testing is not limited to detecting crashes or syntax errors. The tester must verify that the implemented output satisfies the task goal, acceptance criteria, source anchors, material requirements, and `quality_acceptance_criteria`.

Reports must separate:

- `CORRECTNESS_ISSUES`: wrong, missing, broken, unauthorized, unsafe, or unverifiable behavior.
- `QUALITY_ISSUES`: incomplete execution quality, weak UX, poor maintainability, unclear documentation, inconsistent output, or work that technically exists but is not user-ready.

A PASS requires zero blocking/major correctness issues and zero blocking/major quality issues for the tester's assigned scope.

## Premium Review Gate

When `premium_review_required: true`, testers raise the bar from task completion to professional deliverability. Premium review focuses on coherence, polish, consistency, final-user usability, and whether the result feels ready to hand over.

Premium review must stay scoped. Do not request unrelated rewrites. Fail only issues that materially reduce final quality or contradict the user's stated success criteria.
