# Code Quality Rubric

Check only files listed in manifests unless shared files are clearly affected.

Blocking:
- build/typecheck/lint fails
- runtime entrypoint cannot import/compile
- missing required dependency declaration

Major:
- unclear module boundaries
- duplicated logic that risks inconsistent behavior
- fragile hardcoded paths/config
- poor error handling in user-facing flows

Minor:
- naming, small duplication, local organization improvements

Record evidence paths for commands. Do not return code snippets to the coordinator.
