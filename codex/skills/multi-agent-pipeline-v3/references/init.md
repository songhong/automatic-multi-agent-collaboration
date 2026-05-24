# Initialization

Initialize before planning or implementation.

## Run Setup

- Create `.agent-work/` if missing.
- Create:
  - `.agent-work/runs/<run_id>/`
  - `.agent-work/input/`
  - `.agent-work/state/`
  - `.agent-work/logs/`
  - `.agent-work/plans/`
  - `.agent-work/tasks/`
  - `.agent-work/results/`
  - `.agent-work/human-review/`
  - `.agent-work/final/`
- Use a stable `run_id`, for example `YYYYMMDD-HHMMSS`.
- Do not delete old runs. If cleanup is needed, move old generated pipeline artifacts to `.agent-work/archive/<timestamp>/`.

## Required Initial Files

- `.agent-work/input/project-requirements.md`: user's original request. The coordinator writes it and then treats it as opaque.
- `.agent-work/state/materials-manifest.json`: paths to user-provided materials, with existence and file size only.
- `.agent-work/state/batch-config.json`: batch size, output directory, run id, max repair attempts.
- `.agent-work/state/agent-id-cache.json`: role-to-runtime-id cache when the runtime exposes reusable ids.
- `.agent-work/state/pipeline-state.json`: current stage, batch id, status, and counters.
- `.agent-work/logs/pipeline-log.jsonl`: append-only machine log.
- `.agent-work/logs/progress-log.md`: user-readable progress notes.

## Coordinator Limits

The coordinator may inspect existence, size, extension, and paths. It must not open business payloads, generated code, plans, task bodies, or full test reports.

## Initial User Check

Confirm, without reading payload bodies:

- material paths exist or are intentionally absent
- output directory is known or marked `TO_BE_DECIDED_BY_PLANNER`
- batch size is explicit or defaults to `1`
- max repair attempts defaults to `3`
