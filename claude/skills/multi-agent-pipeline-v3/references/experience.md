# agent 经验库协议

每个 agent 都有自己的经验库。经验库只保存可迁移的原则，不保存项目正文、失败日志全文、隐私信息或一次性细节。

路径：`.agent-work/experience/shared-principles.md`、`.agent-work/experience/<agent-name>.md`、`/home/zhuyu/.claude/agent-experience/<agent-name>.md`、`C:/Users/zhuyu/.codex/agent-experience/<agent-name>.md`。

## 三条经验写入原则

1. 原则性 > 数值性：写“为什么错”，不要写“改了什么值”。
2. 模式级 > 页面级：写“哪类布局、架构、流程容易犯错”，不要写某个页面特例。
3. 可迁移 > 可复制：换一个完全不同的项目后，这条经验仍然能指导决策。

判断方法：去掉具体数值、页面名、文件名、项目名后，如果这句话还能指导未来决策，才允许入库。

## 写入规则

开发 agent 开始前读取共享经验和自己的经验。修复成功后必须写 `<OUTPUT_DIR>/experience-append-summary.md`，并把合格经验追加到项目经验库和可写的全局经验库。跨 agent 通用经验同时追加到 `shared-principles.md`。

计划被用户驳回、计划太简单、没有追问关键问题、任务包太粗、或 plan-reviewer 失败时，`project-planner` 必须判断是否产生可迁移经验。

coordinator 只负责创建、复制、合并、传递经验库路径。不得读取经验正文、总结经验正文或把经验写入控制面日志。
