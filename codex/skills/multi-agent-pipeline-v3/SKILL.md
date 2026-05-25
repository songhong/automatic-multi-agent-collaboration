---
name: multi-agent-pipeline-v3
description: Codex multi-agent pipeline for large projects. Use when the user wants a coordinator that plans, delegates implementation and testing, keeps business content in files, resumes the same responsible worker/tester across repair loops, records logs, and minimizes token usage through path-only handoffs and staged references.
---

# Multi-Agent Pipeline v3

This skill is the lightweight entrypoint for a Codex-oriented multi-agent workflow. Load only the reference file for the current stage.

## Core Rules

- The main agent coordinates only: initialize state, write logs, hand off file paths, report progress, and maintain control-plane JSON.
- The main agent must not read business payloads: requirements body, plan body, task body, code body, or full test reports.
- During plan confirmation, user dissatisfaction or requests to carefully, completely, or again read requirement/task/material files are not coordinator read permission. The coordinator must write the feedback file and route it back to the same planner role.
- Worker and tester agents read business files and write artifacts. They return only status, counts, paths, and structured JSON.
- Every handoff must include current file paths. Do not rely on conversation memory as the only state.
- Resume the same responsible agent for repair and retest whenever the runtime supports it. If true runtime resume is unavailable, use logical resume with the same role, same task id, previous manifest path, previous result path, and attempt number.
- Whoever implemented a batch fixes that batch. Whoever tested a failing batch retests it. After three failed repair attempts, stop the batch and request human review.
- Do not start every tester by default. The planner must write `required_testers`; if missing, use only `tester-code-quality` and `tester-runtime-effect`.
- Keep `.agent-work` append-only by run id. Do not delete old work; archive old runs under `.agent-work/archive/<timestamp>/` when needed.
- Use installed Codex skills only when useful. If an expected skill is unavailable, record `SKILL_UNAVAILABLE` and use the local rubric in references instead of pretending it loaded.

## Stage References

- Initialization, run ids, directories, backup, materials: `references/init.md`
- Planning, user review, task queue: `references/planning.md`
- Batch dispatch, workers, integration: `references/execution.md`
- Evidence-based testing and result JSON: `references/testing.md`
- Two-layer quality gates and premium review: `references/quality-gates.md`
- Repair loop, disagreement, human handoff: `references/repair.md`
- Logs, status machine, progress reports: `references/logging.md`
- Control-plane schemas: `references/schemas.md`
- Agent and Codex skill matrix: `references/agent-skill-matrix.md`
- Agent experience libraries, three quality rules, and global sync: `references/experience.md`
- Role prompts and quality bars: `references/roles.md`
- Local fallback rubrics when a skill is unavailable: `references/rubrics.md`

## Control Plane Files

The coordinator may read and write only these control-plane files:

```text
.agent-work/state/agent-id-cache.json
.agent-work/state/pipeline-state.json
.agent-work/state/batch-config.json
.agent-work/state/current-batch-control.json
.agent-work/state/output-check-index/<batch_id>.json
.agent-work/logs/pipeline-log.jsonl
.agent-work/logs/progress-log.md
.agent-work/input/user-feedback-v<N>.md
.agent-work/human-review/<batch_id>-questions-for-user.json
.agent-work/human-review/<batch_id>-human-review-required.md
.agent-work/final/final-pipeline-summary.md
```

These files may contain agent names, runtime ids when available, run ids, batch ids, task ids, page ids, paths, statuses, counts, attempts, required flags, and user-visible questions. They must not contain business prose, code bodies, full test report bodies, bug detail dumps, secrets, or private data.

## Required Coordinator Flow

1. Read `references/init.md` and initialize the run, logs, id cache, batch config, and materials manifest.
2. Write the user's original request to `.agent-work/input/project-requirements.md`, then stop reading that body.
3. Ask the planner role to generate a plan and return only plan paths. Share those paths with the user for confirmation.
4. If the user requests plan changes, is dissatisfied, or asks to carefully/completely/re-read requirement, task, or material files, write feedback to `.agent-work/input/user-feedback-v<N>.md` and resume the same planner role; the coordinator must not read those bodies.
5. After user approval, have the planner produce `.agent-work/state/current-batch-control.json`.
6. Dispatch the selected worker/integrator/testers from the batch control file. Read only returned JSON/status files.
7. If any required tester fails, follow `references/repair.md` with the original worker and original tester.
8. When a batch passes, request the next batch. When all batches pass, run the release packager role to write the final summary path.
9. Every task uses `completion_quality_gate`; key batches and final delivery use `premium_review_gate` according to planner/release-packager control-plane fields.

## Installed Codex Skills To Prefer

Use these only when the task needs them and they are available in the current Codex session:

```text
superpowers:brainstorming
superpowers:writing-plans
superpowers:dispatching-parallel-agents
superpowers:systematic-debugging
superpowers:test-driven-development
superpowers:verification-before-completion
vercel:react-best-practices
vercel:shadcn
vercel:nextjs
vercel:verification
vercel:agent-browser-verify
documents:documents
presentations:Presentations
spreadsheets:Spreadsheets
pdf
```

Do not assume Claude/Anthropic-only skills such as `frontend-design`, `webapp-testing`, `docx`, `pptx`, or `xlsx` exist in Codex. Use the Codex equivalents above or local references.

## Completion Criteria

- Every batch has a structured status, worker manifest, tester result JSON, and evidence paths.
- The coordinator did not read business payload bodies.
- Required output files exist and are non-empty.
- All required testers are `PASS` or approved `SKIP`; failures go through the repair loop and stop after three attempts.
- The final report path is `.agent-work/final/final-pipeline-summary.md`.
