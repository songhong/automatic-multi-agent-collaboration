# Repair

Repairs preserve responsibility.

## Routing

- The worker that created the failing output repairs it.
- The tester that found the failure retests it.
- Keep the same role and runtime id when available.
- If runtime resume is unavailable, logical resume must include all current file paths and previous result paths.

## Attempt Limit

- Attempt 1: initial repair.
- Attempt 2: second repair with failing result path.
- Attempt 3: final repair.
- After three failed repair attempts, stop the batch and write `.agent-work/human-review/<batch_id>-human-review-required.md`.

## Worker Repair Input

Provide:

- batch control path
- dev manifest path
- failing tester result paths
- attempt number
- output index path

Do not paste long failure bodies into the coordinator context. Pass paths.

## Disagreement

If the worker claims fixed but the original tester still fails twice on the same issue class, ask `architect-agent` to write a short arbitration file under `.agent-work/human-review/`. The coordinator reads only the arbitration status and paths.
