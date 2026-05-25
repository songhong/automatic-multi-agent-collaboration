# agent 经验库协议

每个子 agent 都有自己的经验库，但经验不是每次任务都必须写。经验库只保存可迁移的原则，不保存项目正文、失败日志全文、隐私信息或一次性细节。

## 路径

```text
.agent-work/experience/shared-principles.md
.agent-work/experience/<agent-name>.md
/home/zhuyu/.claude/agent-experience/<agent-name>.md
C:/Users/zhuyu/.codex/agent-experience/<agent-name>.md
```

## 条件式写经验

成功且没有新教训的普通任务不写经验，避免经验库膨胀。

只有这些情况才写经验：

- tester FAIL 后，developer 修复并复测通过。
- plan-reviewer FAIL 后，planner 修订并通过。
- 用户驳回计划，planner 重新规划后发现可迁移教训。
- tester 自己发现可迁移的验收/质量检查模式。
- architect、integrator、release-packager 发现跨模块、架构或最终交付层面的可迁移问题。

如果没有可迁移经验，写 `EXPERIENCE_DECISION: NO_TRANSFERABLE_LESSON` 和简短理由，不强行编经验。

## 三条经验写入原则

1. 原则性 > 数值性：写“为什么错”，不要写“改了什么值”。
2. 模式级 > 页面级：写“哪类布局、架构、流程容易犯错”，不要写某个页面特例。
3. 可迁移 > 可复制：换一个完全不同的项目后，这条经验仍然能指导决策。

判断方法：去掉具体数值、页面名、文件名、项目名后，如果这句话还能指导未来决策，才允许入库。

## 同步硬规则

一旦 `EXPERIENCE_DECISION: APPENDED`，必须同步：

- 项目经验库：`.agent-work/experience/<agent-name>.md`
- 对应全局经验库：Claude 或 Codex 的 `<agent-name>.md`
- 如跨角色通用，同时同步 `shared-principles.md`

`experience-append-summary.md` 必须包含：

```text
EXPERIENCE_DECISION: APPENDED | NO_TRANSFERABLE_LESSON
PROJECT_EXPERIENCE_SYNC: OK | N/A
GLOBAL_EXPERIENCE_SYNC: OK | FAILED | N/A
GLOBAL_EXPERIENCE_PATHS:
- <path or N/A>
SYNC_FAILURE_REASON: <reason or N/A>
LRU_DISTILLATION_RUN: YES | NO
DISTILLED_ENTRY_COUNT: <number>
```

如果 `APPENDED` 但全局同步失败，不能当作普通通过；必须返回 `BLOCKED_EXPERIENCE_SYNC` 或让 tester FAIL。

## LRU 蒸馏

每个经验文件分为：

```markdown
## Distilled Principles
## Full Entries
```

触发阈值：超过 80 条完整经验，或文件超过 60KB。

蒸馏顺序：

- 优先蒸馏 `LAST_REFERENCED` 最久未使用的条目。
- 同等时间下，优先蒸馏 `REFERENCE_COUNT` 更低的条目。
- 保留最近 30 条完整经验。
- 保留 `REFERENCE_COUNT >= 3` 的高频经验。

蒸馏是把旧条目合并成可迁移原则，追加到 `Distilled Principles`，不是简单删除。蒸馏结果必须写入 `experience-append-summary.md`。

## 引用更新

agent 工作前读取 `Distilled Principles` 和相关 `Full Entries`。如果某条经验被本次任务采用，更新该条的 `LAST_REFERENCED` 和 `REFERENCE_COUNT`。如果无法可靠更新，summary 写 `REFERENCE_UPDATE: SKIPPED_REASON`。

## coordinator 限制

coordinator 只负责创建、复制、合并、传递经验库路径，并读取 summary 的状态字段。不得读取经验正文、总结经验正文或把经验内容写入控制面日志。
