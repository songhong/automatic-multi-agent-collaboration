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
