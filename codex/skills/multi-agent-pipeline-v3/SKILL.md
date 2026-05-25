---
name: multi-agent-pipeline-v3
description: Codex 多 agent 流水线。适用于大型项目的主协调、计划、开发、测试、修复、经验库和路径化交接；主 agent 不读取业务正文，通过控制面 JSON 和文件路径节省 token。
---

# Multi-Agent Pipeline v3

本 skill 是 Codex 版轻量入口。详细协议按阶段读取 `references/`，避免一次加载全量流程。

## 语言规范

- 面向用户、agent 行为说明、流程规则、质量标准和错误处理说明使用中文。
- agent 名、字段名、状态值、路径变量保留英文，例如 `PROJECT_REQUIREMENTS_PATH`、`PLAN_PATH`、`PASS`、`FAIL`。
- 禁止新增乱码中文、问号乱码触发词或无说明英文长段落。

## 核心规则

- 主 agent 只协调：初始化状态、写日志、传递文件路径、报告进度、维护控制面 JSON。
- 主 agent 不得读取业务正文：需求正文、计划正文、任务正文、代码正文或完整测试报告。
- 计划确认阶段，用户不满意或要求仔细/完整/重新阅读需求、任务、材料文件，不是 coordinator 阅读授权。coordinator 必须写反馈文件并交回同一个 planner。
- worker 和 tester 读取业务文件并写产物；返回给 coordinator 的内容只允许是状态、数量、路径和结构化 JSON。
- 每次 handoff 都必须包含当前文件路径，不能只依赖对话记忆。
- 修复和复测优先 resume 同一责任 agent；运行时不支持真 resume 时，用同角色、同 task id、上一轮 manifest/result 路径和 attempt 做逻辑 resume。
- 谁开发谁修，谁测试谁复测。三轮修复仍失败，停止 batch 并转人工。
- 不默认启动全部 tester。planner 必须写 `required_testers`；缺失时只用 `tester-code-quality` 和 `tester-runtime-effect`。

## 阶段参考文件

- 初始化、run id、目录、备份、材料：`references/init.md`
- 计划、用户确认、任务队列：`references/planning.md`
- batch 调度、worker、集成：`references/execution.md`
- 证据化测试和 result JSON：`references/testing.md`
- 双层质量门和精品评审：`references/quality-gates.md`
- 修复循环、争议、人工接管：`references/repair.md`
- 日志、状态机、进展报告：`references/logging.md`
- 控制面 schema：`references/schemas.md`
- agent 与 Codex skill 矩阵：`references/agent-skill-matrix.md`
- 计划审查 rubric：`references/plan-review-rubric.md`
- agent 经验库、三原则、全局同步：`references/experience.md`
- 角色提示词与质量标准：`references/roles.md`
- skill 不可用时的本地 fallback rubric：`references/rubrics.md`

## 必须流程

1. 读取 `references/init.md` 并初始化 run、日志、id cache、batch config 和 materials manifest。
2. 把用户原始需求写入 `.agent-work/input/project-requirements.md`，随后停止读取该正文。
3. 请求 planner 生成候选计划路径；计划必须先通过 `plan-reviewer`，PASS 后才把路径给用户。
4. 用户要求改计划、不满意、或要求重新阅读需求/任务/材料时，写入 `user-feedback-v<N>.md` 并 resume 同一个 planner；coordinator 不读正文。
5. 用户确认后，planner 生成 `.agent-work/state/current-batch-control.json`。
6. 按 batch control 派发 worker/integrator/tester，只读返回的 JSON/状态文件。
7. tester 失败时，按 `references/repair.md` 回到原 worker 和原 tester。
8. batch 通过后进入下一批；全部通过后调用 release packager 写最终 summary 路径。

## Codex 中优先按需使用的 skills

`superpowers:brainstorming`、`superpowers:writing-plans`、`superpowers:dispatching-parallel-agents`、`superpowers:systematic-debugging`、`superpowers:test-driven-development`、`superpowers:verification-before-completion`、`vercel:*`、`documents:documents`、`presentations:Presentations`、`spreadsheets:Spreadsheets`、`pdf`。

不要假设 Claude-only skills 如 `frontend-design`、`webapp-testing`、`docx`、`pptx`、`xlsx` 在 Codex 中可用。

## 完成标准

每个 batch 有结构化状态、worker manifest、tester result JSON 和证据路径；coordinator 没有读取业务正文；required 输出文件存在且非空；required tester 全部 `PASS` 或批准 `SKIP`；最终报告路径是 `.agent-work/final/final-pipeline-summary.md`。

## coordinator 材料读取防线

用户说参考、阅读、查看或使用材料文件，授权对象是 planner 或子 agent，不是 coordinator。coordinator 只记录元数据，永远不读取材料正文。禁止对用户材料使用：`Read`、`cat`、`type`、`Get-Content`、`head`、`tail`、`sed`、`grep`、`rg`、`Select-String`。
