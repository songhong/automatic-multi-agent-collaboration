# Repair Protocol

## Ownership

- The developer that created the defect repairs it.
- The tester that found the issue validates the repair.
- Prefer `SendMessage` to the recorded agent id. Use `Agent()` fallback only when real resume fails.

## Repair Attempt

Before editing, the developer writes a repair plan in the new manifest:

```text
REPAIR_PLAN:
- ISSUE_ID: <id>
  SUSPECTED_ROOT_CAUSE: <short reason>
  FILES_TO_TOUCH:
  - <path>
  VERIFICATION_COMMAND: <command or N/A>
```

Then the developer applies minimal fixes and writes `ITEM_RESPONSES`:

```text
ITEM_RESPONSES:
- TESTER: <tester-name>
  ISSUE_ID: <id>
  RESPONSE: FIXED | DISAGREED
  DETAIL: <brief technical reason>
```

## Retest

Only failed testers need to retest unless the repair touches shared integration or high-risk code. Retest uses the same tester instance when possible.

Tester handles `DISAGREED`:

- Accept: `RESOLVED_BY_DISAGREED`, does not count as issue.
- Reject: `ESCALATED`, remains failing.
- Security blocking issues cannot be waived without equivalent mitigation.

## Stop Condition

After `max_repair_attempts` failed attempts:

- write `.agent-work/human-review/<batch_id>-human-review-required.md`,
- update pipeline state to blocked,
- report only path/status to user.
