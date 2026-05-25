# Automatic Multi-Agent Collaboration（自动化多 agent 协作）

这个仓库保存同一套自动化多 agent 协作思路的两个版本：

- `claude/`：给 WSL Claude Code 使用的 skill 与 agents。
- `codex/`：给 Codex 使用的 skill、references 与 agents。

## 核心思路

长任务里最浪费 token 的地方，是反复把需求、计划、代码、日志、测试报告塞回主对话。这个 workflow 把主 agent 定义成“调度器”，而不是万能执行者。

coordinator 只推进状态、记录日志、传递路径、读取紧凑的 JSON 状态。真正需要上下文的工作交给专业角色：planner、developer、tester、integrator、release-packager。

严格规则：主 agent 只调度；业务正文留在文件里；planner 负责理解需求、追问用户、比较方案、拆任务；developer 执行任务包；tester 检查完成质量；谁开发谁修，谁测试谁复测；三轮修复仍失败就转人工。

## 架构

```text
用户需求
  -> coordinator 写入不透明需求文件
  -> planner 读取需求并写计划/任务队列
  -> plan-reviewer 审查计划质量
  -> 用户确认计划路径
  -> coordinator 派发 batch
  -> worker 写产物和 manifest
  -> tester 写 result.json 和证据
  -> 失败回到同一 worker 和同一 tester
  -> 通过后进入下一批
  -> release-packager 写最终汇总
```

`.agent-work/` 是控制面，保存 input、state、plans、tasks、results、logs、human-review、final、experience 等目录。全局经验库位于 `/home/zhuyu/.claude/agent-experience/<agent-name>.md` 或 `C:\Users\zhuyu\.codex\agent-experience\<agent-name>.md`。

## 为什么节省 token

coordinator 不加载完整需求、计划、代码或测试报告，只传路径和读取紧凑 JSON。skill 也拆成轻量 `SKILL.md` 加阶段化 `references/`，让 Claude/Codex 只加载当前阶段需要的协议。

节省来自：渐进加载、路径化 handoff、选择性测试、稳定责任链、经验蒸馏。

## 质量机制

每个任务走 `completion_quality_gate`。关键 batch 和最终交付可启用 `premium_review_gate`。计划必须先经过 `plan-reviewer`；如果计划只是模块清单、缺少方案比较、任务包不完整、没有 source anchors、该问用户却没问，就会 FAIL 并回到同一个 planner 修订。

## coordinator 材料不读边界

用户说“可以参考/阅读/查看/依据/结合材料”，授权对象是 planner、reviewer 或对应子 agent，不是 coordinator。coordinator 只记录路径和元数据，写 `content_read_by_coordinator: false`，然后把 manifest 交给 `project-planner`。

## 使用 Codex 版

复制或安装 `codex/skills/multi-agent-pipeline-v3` 到 `~/.codex/skills/multi-agent-pipeline-v3`。如果要使用完整本地项目 workflow，也把 `codex/agents` 复制到当前环境使用的 Codex agent 目录。重启 Codex 后调用 `$multi-agent-pipeline-v3`。

## 使用 Claude 版

把 `claude/agents` 和 `claude/skills/multi-agent-pipeline-v3` 复制到 Claude Code 使用的配置目录，然后在 Claude 中调用 skill。Claude 版针对 WSL Claude Code 和 `.claude/` 路径优化。

## 安全说明

不要提交 `.agent-work/` 运行产物，除非你明确想公开运行日志。不要把 API key、cookie、token、`.env` 或私人材料放进本仓库。发布生成计划和最终报告前，先人工检查。
