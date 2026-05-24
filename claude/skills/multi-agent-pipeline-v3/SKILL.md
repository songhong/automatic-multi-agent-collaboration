---
name: multi-agent-pipeline-v3
description: WSL Claude Code 多 agent 流水线。主 agent 只调度控制面文件，不读业务正文；planner/developer/tester 通过路径交接；真实 Resume 优先，逻辑 Resume 兜底；按需加载官方 skills，使用证据化测试和结构化 JSON 状态。
allowed-tools: Agent(project-planner, architect-agent, development-agent, frontend-developer, backend-developer, document-writer, data-analyst, toolsmith, fullstack-integrator, tester-code-quality, tester-visual-aesthetic, tester-runtime-effect, tester-security, tester-performance, tester-data-integrity, tester-accessibility, release-packager), SendMessage, AskUserQuestion, Write, Edit, Bash, Read, Grep, Glob, TaskOutput, TaskStop
---

# Multi-Agent Pipeline v3

本 skill 是轻量入口。详细协议按阶段读取 `references/`，避免每次启动加载全量流程。

## Core Rules

- 运行环境以 WSL Claude Code 为准，只使用 `.claude/agents/`、`.claude/skills/`、`.agent-work/`。
- 主 agent 只调度、写日志、汇报进度、维护控制面文件；不得读需求、计划、任务、代码、测试报告正文。
- 计划确认阶段用户任何不满意、没理解、重新看、仔细看、完整看、阅读任务文件、阅读需求文件、按原文件重做等反馈，都不是主 agent 的阅读授权；主 agent 必须写入 `user-feedback-v<N>.md` 并 resume 同一 `project-planner`。
- 子 agent 读业务文件并写产物；返回给主 agent 的内容只允许是状态、计数、路径和结构化 JSON。
- 子 agent 首次启动使用 `run_in_background: true`；继续同一职责优先 `SendMessage(to=<agent_id>)`，失败才 `Agent()` 逻辑 Resume。
- 谁开发谁修复，谁测试谁复测。三轮修复仍失败则停止该 batch，转人工。
- 默认不全量启动 tester。planner 必须写 `required_testers`；缺省只启用 `tester-code-quality` 和 `tester-runtime-effect`。
- 不直接删除旧 `.agent-work`。每次运行使用 `run_id`，旧产物归档到 `.agent-work/archive/<timestamp>/` 或新建 `.agent-work/runs/<run_id>/`。
- 官方 skills 优先使用已安装插件/全局 skill；如果 `Skill` 工具提示不可用，记录 `SKILL_UNAVAILABLE`，不得伪造已加载。

## Stage References

按当前阶段只读取对应文件：

- 初始化、目录、备份、run_id：`references/init.md`
- 计划、用户确认、任务队列：`references/planning.md`
- batch 调度、开发、集成：`references/execution.md`
- 测试、证据、result.json：`references/testing.md`
- 修复循环、DISAGREED、人工介入：`references/repair.md`
- 日志、状态机、超时：`references/logging.md`
- JSON schema 和控制面文件：`references/schemas.md`
- agent 与 skill 配备矩阵：`references/agent-skill-matrix.md`
- agent 经验库、三原则、全局同步：`references/experience.md`

## Control Plane Files

主 agent 只允许读取/写入控制面文件：

```text
.agent-work/state/agent-id-cache.json
.agent-work/state/pipeline-state.json
.agent-work/state/batch-config.json
.agent-work/state/current-batch-control.json
.agent-work/state/output-check-index/<batch_id>.json
.agent-work/logs/pipeline-log.jsonl
.agent-work/logs/progress-log.md
.agent-work/input/user-feedback-v<N>.md
.agent-work/human-review/<batch_id>-questions-for-user.json
.agent-work/human-review/<batch_id>-human-review-required.md
.agent-work/final/final-pipeline-summary.md
```

这些文件只允许包含 agent 名称、agent id、run id、batch id、task id、page id、路径、状态、计数、attempt、required 标记、用户可见问题与选项。不得包含需求正文、代码正文、测试报告正文、bug 细节或敏感信息。

## Required Coordinator Flow

1. 读取 `references/init.md`，初始化 run、日志、agent id cache、batch 配置和素材清单。
2. 将用户需求写入 `.agent-work/input/project-requirements.md`，不再阅读正文。
3. 启动或 resume `project-planner` 生成计划；把计划路径给用户确认，不读计划正文。
4. 用户要求修改计划，或要求仔细/完整/重新阅读需求、任务、素材文件时，将反馈写入控制面文件并 resume 同一 planner；主 agent 不得自行读取这些文件正文。
5. 用户确认后，planner 生成 `.agent-work/state/current-batch-control.json`。
6. 按 batch control 启动指定 developer/integrator/tester；主 agent 只读返回 JSON 和 result.json。
7. 任一 tester 失败，按 `references/repair.md` resume 原 developer 和原 tester。
8. batch 通过后进入下一 batch。
9. 所有 batch 通过后启动 `release-packager` 生成最终报告路径；主 agent 汇报路径和状态。

## Official Skills Available In This User Environment

用户已确认 WSL Claude 环境存在这些 Anthropic skills：

```text
pdf
docx
pptx
xlsx
frontend-design
webapp-testing
skill-creator
```

子 agent 可按需加载这些 skills。`code-review`、`security-review`、`verify`、`code-simplifier`、`security-guidance` 不假定为本地 skill；对应能力使用本项目 references/rubrics，除非运行时明确可用。

## Completion Criteria

- 每个 batch 有结构化状态、开发 manifest、测试 result.json 和证据路径。
- 主 agent 未读取业务正文。
- 所有 required 输出文件存在且非空。
- 所有参与 tester 均 PASS 或 SKIP；FAIL 走修复循环，三轮失败转人工。
- 最终总结报告写入 `.agent-work/final/final-pipeline-summary.md`。
