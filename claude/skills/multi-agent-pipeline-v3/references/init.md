# Init Protocol

## Run Setup

- Generate `run_id` as `YYYYMMDD-HHMMSS`.
- Do not delete previous `.agent-work` content.
- If `.agent-work/input`, `plans`, `tasks`, `handoffs`, `test-reports`, `state`, `human-review`, or `final` already contain a previous run, move them to `.agent-work/archive/<timestamp>/`.
- Preserve `.agent-work/logs/` and `.agent-work/experience/`.

## Required Directories

```text
.agent-work/input
.agent-work/materials
.agent-work/plans
.agent-work/tasks
.agent-work/handoffs
.agent-work/test-reports
.agent-work/state/agent-ids
.agent-work/state/output-check-index
.agent-work/logs
.agent-work/human-review
.agent-work/final
.agent-work/evidence
.agent-work/experience
```

## Experience Library Initialization

Follow `references/experience.md`.

- Create `.agent-work/experience/`.
- Ensure UTF-8 files exist for `shared-principles.md` and every configured agent.
- Copy or merge global Claude experience from `/home/zhuyu/.claude/agent-experience/<agent-name>.md` into the matching project cache file when available.
- If global files are missing, create them from the template in `references/experience.md`.
- Preserve existing project experience files; do not overwrite them with empty templates.

## Initial Control Files

- `.agent-work/state/agent-id-cache.json`
- `.agent-work/state/pipeline-state.json`
- `.agent-work/state/batch-config.json`
- `.agent-work/logs/pipeline-log.jsonl`
- `.agent-work/logs/progress-log.md`
- `.agent-work/materials/materials-manifest.md`

`batch-config.json` defaults:

```json
{
  "batch_size": 3,
  "planner_pool_size": 1,
  "developer_pool_size": 1,
  "tester_pool_size": 3,
  "max_repair_attempts": 3,
  "default_testers": ["tester-code-quality", "tester-runtime-effect"],
  "output_dir": "TO_BE_DECIDED"
}
```

## Agent Availability Check

Check only `.claude/agents/*.md`. Do not check `.codex`, `.Codex`, or `.agents` for Claude execution.

Required agents:

```text
pipeline-coordinator
project-planner
architect-agent
development-agent
frontend-developer
backend-developer
document-writer
data-analyst
toolsmith
fullstack-integrator
tester-code-quality
tester-visual-aesthetic
tester-runtime-effect
tester-security
tester-performance
tester-data-integrity
tester-accessibility
release-packager
```

If an agent file is missing, report the missing path and stop before starting the pipeline.
