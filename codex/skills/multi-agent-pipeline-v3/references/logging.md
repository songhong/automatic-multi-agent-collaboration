# 日志协议

关键事件追加到 `.agent-work/logs/pipeline-log.jsonl`。日志只写 agent 名、任务 id、batch id、状态、路径、attempt、时间和结果计数，不写业务正文。

每完成一页、一个任务或一个 batch，更新 `.agent-work/logs/progress-log.md` 并向用户报告简短进展。

全部完成后写 `.agent-work/final/final-pipeline-summary.md`，包含执行概览、通过的 batch、失败/人工介入项、最终交付路径和回滚提示。
