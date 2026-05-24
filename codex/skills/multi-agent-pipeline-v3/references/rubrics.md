# Local Rubrics

Use these rubrics when a specialized Codex skill is unavailable or when a tester needs a compact checklist.

## Code Quality

- Changes are scoped to the assigned task.
- Names, structure, and abstractions follow local patterns.
- Error paths are handled.
- Tests or verification commands cover the changed behavior.
- No unrelated formatting churn or dependency upgrades.

## Runtime

- Build/test/smoke commands were run when available.
- Generated artifacts exist and are non-empty.
- User-visible flows load without obvious errors.
- Failure output is captured in evidence files.

## Visual

- Layout fits mobile and desktop constraints.
- Text does not overlap, clip, or overflow controls.
- UI density matches the product type.
- Screenshots or rendered previews are saved when possible.

## Security

- No secrets are printed, committed, or copied.
- Inputs are validated before file, shell, database, or network use.
- Dangerous operations require explicit user approval.
- Generated docs/logs do not expose private payload bodies.

## Performance

- Avoid obvious repeated expensive work.
- Large files or datasets are streamed or chunked when practical.
- Frontend bundles and heavy assets are not inflated without reason.
- Performance claims are backed by command output or measurement paths.

## Data Integrity

- Source files and transformations are traceable.
- Calculations have sanity checks.
- Schema assumptions are documented.
- Outputs preserve expected row counts, totals, identifiers, or formats.

## Accessibility

- Controls have names and keyboard access.
- Contrast and focus states are usable.
- Semantic elements are preferred over click-only containers.
- Dynamic state changes are not visible-only when assistive labels are needed.
