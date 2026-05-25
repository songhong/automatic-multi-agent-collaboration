# Agent Skill Matrix

Use installed Codex skills sparingly and on demand. Do not load every skill at startup.

## Planning

| Role | Use When | Optional Codex Skills |
| --- | --- | --- |
| project-planner | plan, task queue, batch selection | `superpowers:brainstorming`, `superpowers:writing-plans`, `superpowers:dispatching-parallel-agents` |
| architect-agent | architecture, conflict arbitration, cross-cutting decisions | `superpowers:brainstorming`, `superpowers:writing-plans` |

## Workers

| Role | Use When | Optional Codex Skills |
| --- | --- | --- |
| development-agent | general coding fallback | `superpowers:test-driven-development`, `superpowers:systematic-debugging` |
| frontend-developer | UI, React, Next.js, browser behavior | `vercel:react-best-practices`, `vercel:shadcn`, `vercel:nextjs`, `vercel:agent-browser-verify` |
| backend-developer | APIs, auth, data services | `superpowers:systematic-debugging`, `vercel:nextjs`, `vercel:vercel-functions`, `vercel:vercel-storage` |
| document-writer | Word/PDF/document artifacts | `documents:documents`, `pdf` |
| data-analyst | CSV/XLSX/data transforms and charts | `spreadsheets:Spreadsheets`, `superpowers:systematic-debugging` |
| toolsmith | CLI tools, scripts, automation | `superpowers:test-driven-development`, `superpowers:systematic-debugging` |
| fullstack-integrator | multi-surface integration | relevant worker skills only |
| release-packager | final report, packaging, handoff | `superpowers:verification-before-completion` |

## Testers

| Role | Use When | Optional Codex Skills |
| --- | --- | --- |
| tester-code-quality | every batch unless pure non-code output | `superpowers:verification-before-completion` |
| tester-runtime-effect | every executable or renderable batch | `vercel:verification`, `vercel:agent-browser-verify` |
| tester-visual-aesthetic | UI, slides, docs, reports | `vercel:agent-browser-verify`, `documents:documents`, `presentations:Presentations` |
| tester-security | external input, auth, network, file writes | local security rubric; use official security skill only if installed |
| tester-performance | heavy UI, data, or runtime work | `vercel:verification` |
| tester-data-integrity | spreadsheets, reports, analytics, transforms | `spreadsheets:Spreadsheets` |
| tester-accessibility | user-facing UI | `vercel:react-best-practices`, browser verification |

## Missing Skills

If a skill listed here is unavailable:

1. Record `SKILL_UNAVAILABLE` in the worker/tester manifest.
2. Continue with the local rubric and available commands.
3. Do not install packages or global tools unless the user has approved installation policy for the task.

## On-Demand Skill Loading Rules

Agents load specialist skills only when the current task needs them. Loading every possible skill wastes tokens.

- Planner may use brainstorming-style planning for unclear, large, rejected, or tradeoff-heavy requirements.
- Frontend agents use UI/design/browser testing skills only for UI or runnable web surfaces.
- Document/data agents use document, PDF, presentation, spreadsheet, and data skills only when the deliverable requires them.
- Backend/tooling/integration agents use official docs, debugging, testing, deployment, or browser skills only when the task depends on that capability.
- Tester agents load visual, runtime, security, performance, data, or accessibility skills only for their assigned scope.
- Release packager uses document/PDF/README-related skills only for final delivery packaging or audit.

If a skill is unavailable, record `SKILL_UNAVAILABLE` and continue with local rubrics where safe.
