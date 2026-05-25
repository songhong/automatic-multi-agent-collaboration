
# Quality Gates Protocol

## Two-Layer Quality Model

The pipeline uses two separate quality gates:

1. `completion_quality_gate` runs for every task. It verifies correctness, requirement coverage, completeness, runnable output, maintainability, and whether the task package was sufficient.
2. `premium_review_gate` runs only for key batches and final delivery. It reviews professional polish, consistency, user experience, documentation readiness, visual/aesthetic quality, performance, accessibility, and cross-deliverable coherence.

Do not run premium review on every small task by default. It is a quality amplifier, not the normal bug check.

## Planner Responsibilities

For every task in the queue, planner must write `quality_gate`, `premium_review_required`, `premium_review_reason`, `quality_acceptance_criteria`, `parallel_group_id`, `dependency_task_ids`, `conflict_risk`, and `shared_output_paths`.

Set `premium_review_required: true` for homepages, final reports, README or delivery documents, public demos, core interactions, high-polish user requests, and final delivery candidates.

## Tester Responsibilities

Every tester report must separate `CORRECTNESS_ISSUES` and `QUALITY_ISSUES`. A tester may PASS only when both categories have no blocking or major items for that tester's scope.

## Premium Review Routing

For key batches, coordinator dispatches only the premium-relevant testers named by planner. Final delivery premium review is performed by `release-packager`, which may route failures to planner, developer, integrator, or tester by responsibility.
