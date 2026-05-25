# Agent Skill Matrix

## Official Skills Available

The user confirmed these Anthropic skills exist in WSL Claude plugin/global paths:

```text
pdf
docx
pptx
xlsx
frontend-design
webapp-testing
skill-creator
```

Use them through the `Skill` tool when relevant. If unavailable at runtime, record `SKILL_UNAVAILABLE`.

## Optional Or Non-Local Capabilities

Do not assume these are installed local skills:

```text
code-review
code-simplifier
security-review
security-guidance
verify
vercel:*
superpowers:*
```

If available as plugins in the running Claude environment, they may be used. Otherwise use the local instructions and rubrics in this project.

## Developer Agents

- `project-planner`: planning references, agent selection guide, schemas.
- `architect-agent`: schemas, agent selection guide, architecture/interface checklist.
- `frontend-developer`: `frontend-design` for UI-heavy work; `webapp-testing` only for self-check if needed.
- `backend-developer`: local backend checklist; use external plugin skills only if explicitly available.
- `document-writer`: `pdf`, `docx`, `pptx` according to output type.
- `data-analyst`: `xlsx` for spreadsheet work.
- `toolsmith`: local CLI checklist and evidence protocol.
- `fullstack-integrator`: schemas, execution protocol, webapp-testing for integration verification.
- `release-packager`: logging, schemas, output check index.

## Tester Agents

- `tester-code-quality`: local code quality rubric plus build/lint/typecheck evidence.
- `tester-runtime-effect`: `webapp-testing` for web apps; otherwise command/log evidence.
- `tester-visual-aesthetic`: `frontend-design` plus screenshot evidence.
- `tester-security`: local security rubric plus available audit/secret-scan evidence.
- `tester-performance`: metrics-focused local checklist.
- `tester-data-integrity`: independent recomputation/check fixtures.
- `tester-accessibility`: local accessibility checklist plus browser/axe evidence if available.

## On-Demand Skill Loading Rules

Agents should load specialist skills only when the current task needs them. Loading every possible skill wastes tokens and can dilute instructions.

- `project-planner`: may use brainstorming-style planning when requirements are unclear, rejected, large, or need tradeoff discussion. Use only the relevant planning references and agent selection guide.
- `frontend-developer`: use `frontend-design` for UI quality/design tasks; use `webapp-testing` for local interaction checks when a runnable web surface exists; use framework-specific skills only when the repo clearly uses that framework.
- `backend-developer`: use official docs or platform/framework skills only when the task depends on current API behavior; otherwise use local evidence and tests.
- `document-writer`: use `docx`, `pdf`, `pptx`, `xlsx`, or `doc-coauthoring` only when the deliverable type requires it.
- `data-analyst`: use `xlsx` for spreadsheet inputs/outputs and independent data checks for numerical claims.
- `toolsmith`: use debugging/testing/platform docs only when the command behavior or runtime is uncertain.
- `fullstack-integrator`: use `webapp-testing` when validating integrated web flows; otherwise rely on manifests and runtime evidence.
- `tester-visual-aesthetic`: use `frontend-design` and screenshot evidence for visual quality.
- `tester-runtime-effect`: use `webapp-testing` for browser flows; otherwise command/log evidence.
- `tester-security`: use available security tools or rubrics; do not install global scanners without user permission.
- `tester-performance`: use measurable local metrics and record limitations when tooling is unavailable.
- `tester-data-integrity`: use independent recomputation or fixture checks.
- `tester-accessibility`: use browser/axe evidence when available and record limitations otherwise.
- `release-packager`: use document/PDF/README-related skills only when producing or auditing those deliverables.

If a named skill is unavailable at runtime, write `SKILL_UNAVAILABLE` in the evidence or report and continue with local rubrics where safe.
