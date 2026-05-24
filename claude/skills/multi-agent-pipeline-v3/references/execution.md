# Execution Protocol

## Developer Dispatch

Coordinator reads `.agent-work/state/current-batch-control.json` and starts the specified developer agent for each task group.

Developer agents read:

- `TASK_PATH`
- relevant project files
- `.agent-work/experience/shared-principles.md`
- their own experience file

Developer agents write:

- implementation manifest
- development summary
- evidence files for commands run

They return only structured status and paths.

## Evidence Requirement

Every implementation manifest must include:

```text
VERIFICATION_EVIDENCE:
- command: <command or N/A>
  status: pass | fail | skipped
  evidence_path: <path or N/A>
```

If a verification command cannot run, record `skipped` with a reason in the manifest. Do not claim it passed.

## Integration

Use `fullstack-integrator` when a batch has multiple developers, frontend/backend contracts, shared config, routes, env, or startup scripts.

Integrator writes:

```text
.agent-work/handoffs/<batch_id>/fullstack-integrator/attempt-<N>/integration-summary.md
```

The coordinator forwards the integration summary path to testers but does not read正文.

## Required Output Check

Before tester dispatch, coordinator reads `.agent-work/state/output-check-index/<batch_id>.json` and checks only that each `required: true` path exists and size is greater than zero.

If missing or empty, resume the same developer/integrator for output repair. This does not count as a tester repair attempt.
