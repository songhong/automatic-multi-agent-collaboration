# Plan Review Rubric

## Purpose

`plan-reviewer` uses this rubric to decide whether a `project-planner` output is ready for the user and downstream developer agents.

A good plan is an implementation design and task contract. A bad plan is a shallow module list.

## Automatic FAIL Conditions

Fail the plan if any of these are true:

- The plan is mostly short bullets such as "install dependencies", "modify app.py", "optimize prompt", "build UI", or "write README" without implementation design.
- The plan lacks a planning readiness check or does not explain why no user question was needed.
- The planner should have asked the user about scope, output path, target audience, quality bar, model/runtime choice, data source, privacy constraint, or tradeoff, but generated a plan directly.
- The plan has no approach comparison even though multiple routes are plausible.
- The plan lacks user-visible success criteria.
- The plan lacks material/source file mapping.
- The plan lacks architecture, data flow, module contracts, or dependency ordering for a multi-module project.
- Task packages lack goal, scope, non-goals, acceptance criteria, expected output paths, source anchors, quality acceptance criteria, or allowed source-read scope.
- Long requirements are not decomposed into task-level anchors.
- Tester selection, quality gate, or premium review settings are missing or obviously too weak.
- A developer would need to reread the whole original requirement or guess implementation intent to execute the task.

## Required PASS Qualities

A PASS plan must show:

- Clear understanding of user goal, audience, constraints, and success criteria.
- Explicit materials mapping: which files matter and how they will be used.
- When alternatives exist: 2-3 options, tradeoffs, and a recommendation.
- Implementation design: architecture, data flow, module-level steps, integration points, dependency ordering, and risks.
- Task packages detailed enough for developer agents to execute independently.
- Tester agents can tell exactly how to validate correctness and quality.
- Premium review is enabled for critical user-facing or final-delivery outputs.
- Known uncertainties are either resolved through questions or explicitly recorded for user confirmation.

## Severity Guide

- `blocking`: plan cannot be safely handed to developers; missing readiness, source anchors, task package details, or user questions.
- `major`: plan is executable but likely to produce low quality or incomplete work; missing tradeoffs, risk, testing, or material mapping.
- `minor`: plan is usable but can be clearer; wording, ordering, or evidence paths need refinement.

## Review Discipline

Do not rewrite the plan. Give specific fix directions and paths. The planner remains responsible for the revised plan.
