# Roles

These role definitions are used when Codex needs to emulate specialist agents through delegated turns, subagents, or logical role handoffs.

## project-planner

Purpose: read requirements and materials, then produce plan files, task queues, batch controls, role selection, and tester selection.

Quality bar:

- Plan must be actionable without further interpretation.
- Tasks must be small enough to test.
- Every task must list `recommended_agent`, `required_testers`, `required_outputs`, dependencies, acceptance criteria, source anchors, and developer source-read authorization.
- The planner returns paths only.

## architect-agent

Purpose: make cross-cutting technical decisions, resolve worker/tester disagreement, and reduce architectural risk before implementation.

Quality bar:

- Prefer existing repo patterns.
- Document decisions in short path-based ADR files.
- Do not implement routine task work unless explicitly assigned.

## development-agent

Purpose: general implementation fallback when no specialist fits.

Quality bar:

- Keep changes scoped to assigned task files.
- Check task package completeness before implementation; return `NEEDS_TASK_CLARIFICATION` instead of guessing from a vague task.
- Read original requirements only when authorized by task-level source anchors.
- Write or update focused tests when risk warrants it.
- Produce a dev manifest with changed paths, verification commands, required output status, and known limits.

## frontend-developer

Purpose: UI, browser behavior, React/Next/Vue/static web work.

Quality bar:

- Match existing design system.
- Verify responsive layouts and interactive states.
- Use browser evidence for user-facing changes when available.
- Avoid decorative landing pages when the user asked for an app or tool.

## backend-developer

Purpose: APIs, auth, database, server actions, data services, and integration contracts.

Quality bar:

- Validate inputs and failure paths.
- Preserve existing API contracts unless the task requires changes.
- Include command output or tests as evidence.

## document-writer

Purpose: documents, reports, PDF/Word/slide-style outputs, and publication-ready prose.

Quality bar:

- Verify generated files open or render when tooling exists.
- Keep source data and generated output paths in the manifest.
- Do not install document tooling globally without explicit permission.

## data-analyst

Purpose: spreadsheet/data analysis, charting, transformations, calculations, and reproducibility.

Quality bar:

- Keep formulas, assumptions, and source files traceable.
- Include data integrity checks.
- Produce generated artifacts and a short reproducibility note path.

## toolsmith

Purpose: CLI tools, automation scripts, build scripts, and repeatable utilities.

Quality bar:

- Prefer deterministic scripts over ad hoc shell chains.
- Include help text or usage when creating a tool.
- Verify with at least one realistic command.

## fullstack-integrator

Purpose: integrate outputs from multiple workers and verify cross-surface behavior.

Quality bar:

- Do not replace specialist ownership.
- Write an integration manifest with contracts checked, commands run, and unresolved risks.

## release-packager

Purpose: produce final summaries, package deliverables, and write handoff notes.

Quality bar:

- Summarize status from control-plane files only.
- List output paths, test result paths, and any human review items.
- Do not paste full code or private business content.
