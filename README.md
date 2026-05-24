# Automatic Multi-Agent Collaboration

This repository contains two versions of the same automation idea:

- `claude/`: a Claude Code skill for a WSL-based multi-agent pipeline.
- `codex/`: a Codex skill adapted to Codex skills, references, and installed skill names.

The project name in Chinese is "自动化多agent协作".

## Core Idea

Most long agent runs waste context by repeatedly pasting requirements, plans, code, logs, and test reports into the coordinator conversation. This workflow treats the main agent as an orchestrator instead of a universal worker.

The coordinator only moves the project through states. It stores large content in files, passes paths to specialist roles, reads compact status JSON, and keeps an append-only audit trail. Specialist agents are responsible for the work that actually needs context: planning, implementation, testing, repair, integration, and final packaging.

The workflow is built around a strict coordinator pattern:

- the main agent schedules work, records logs, and reports progress
- planner, worker, tester, integrator, and release roles exchange file paths
- business content stays in files instead of being pasted into the main context
- the same responsible worker fixes its own batch
- the same tester validates the repair
- three failed repair attempts stop the batch for human review

## Architecture

```text
user request
  -> coordinator writes opaque requirements file
  -> planner writes plan and task queue
  -> user reviews plan path
  -> coordinator dispatches one batch
  -> worker writes artifacts and manifest
  -> selected testers write result JSON
  -> failures return to the same worker and same tester
  -> passing batches advance
  -> release packager writes final summary
```

The workflow uses `.agent-work/` as the control plane:

- `input/`: original request and user feedback files
- `state/`: run state, batch config, current batch control, id cache
- `plans/`: planner output
- `tasks/`: task queue and task files
- `results/`: worker manifests, tester result JSON, evidence paths
- `logs/`: machine JSONL and user-readable progress
- `human-review/`: questions and blocked batch summaries
- `final/`: final pipeline summary

## Role Model

Both platform folders include the role definitions needed to run the workflow:

```text
claude/
  agents/
  skills/multi-agent-pipeline-v3/

codex/
  agents/
  skills/multi-agent-pipeline-v3/
```

Planning roles:

- `project-planner`: creates plans, task queues, batches, and tester selection.
- `architect-agent`: handles architecture decisions and arbitration.

Worker roles:

- `development-agent`: general fallback.
- `frontend-developer`: UI and browser-facing work.
- `backend-developer`: APIs, auth, data services, and server logic.
- `document-writer`: document/PDF/report output.
- `data-analyst`: tables, charts, calculations, and data transforms.
- `toolsmith`: CLI tools and automation scripts.
- `fullstack-integrator`: joins work across surfaces.
- `release-packager`: final summary and handoff.

Tester roles:

- `tester-code-quality`
- `tester-runtime-effect`
- `tester-visual-aesthetic`
- `tester-security`
- `tester-performance`
- `tester-data-integrity`
- `tester-accessibility`

## Why This Saves Tokens

The coordinator never loads full requirements, plans, code, or test reports into its own context. It passes paths and reads compact JSON status files. The skill itself is also split into a small `SKILL.md` plus stage-specific `references/`, so Codex or Claude loads only the part of the protocol needed at that moment.

Token savings come from four design choices:

- Progressive disclosure: `SKILL.md` is an entrypoint; details live in stage references.
- Path-only handoffs: large content stays on disk.
- Selective testing: only required testers run; the default is code quality plus runtime.
- Stable repair ownership: repair loops reuse the same responsible roles instead of restarting context-heavy investigations.

## Framework

1. Initialize `.agent-work` state, logs, materials, batch config, and id cache.
2. Write the user request to a requirements file and treat it as opaque to the coordinator.
3. Ask the planner to generate a plan path and task queue path.
4. Let the user review the plan path and request revisions.
5. Execute approved work one batch at a time.
6. Test each batch with only the required testers.
7. Route failures back to the original worker and original tester.
8. Package a final summary when all batches pass.

## Advantages

- Lower context usage through path-only handoffs.
- Better accountability: one worker owns one task, and one tester owns retesting.
- Easier debugging through append-only logs and structured result files.
- Safer orchestration because the coordinator avoids reading or leaking business content.
- Flexible role selection for frontend, backend, documents, data analysis, tools, full-stack integration, and release packaging.
- Better human handoff when automation gets stuck: after three failed repair loops, the batch stops with a focused review file.

## Using The Codex Skill

Install or copy `codex/skills/multi-agent-pipeline-v3` into:

```text
~/.codex/skills/multi-agent-pipeline-v3
```

For the full local project workflow, also copy `codex/agents` into the project-level Codex agent directory used by your environment.

Restart Codex, then invoke:

```text
$multi-agent-pipeline-v3
```

The Codex version uses Codex skill names such as `superpowers:*`, `vercel:*`, `documents`, `presentations`, `spreadsheets`, and `pdf`. It does not assume Claude-only skills like `frontend-design`, `webapp-testing`, `docx`, `pptx`, or `xlsx`.

## Using The Claude Skill

Copy both `claude/agents` and `claude/skills/multi-agent-pipeline-v3` into the Claude configuration used by your Claude Code environment, then invoke the skill from Claude.

The Claude version is tuned for WSL Claude Code and its `.claude/` paths.

## Safety Notes

- Do not commit `.agent-work/` outputs unless you intentionally want to publish run logs.
- Do not put API keys, cookies, tokens, `.env` files, or private materials in this repository.
- Review generated plans and final reports before publishing them.
