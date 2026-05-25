# 执行协议

coordinator 读取 `.agent-work/state/current-batch-control.json` 的控制字段，按 planner 指定的 developer、tester、并行组和依赖执行。不得读取任务正文。

developer 接收任务路径、输出目录、材料清单路径、经验库路径和 attempt。developer 先读 `task.md`，再按任务包授权读取 source anchors 指定的原始需求片段。

如果任务包缺少目标、范围、验收标准、输出路径或 source anchors，developer 返回 `NEEDS_TASK_CLARIFICATION`，不得靠猜测开发。

只有 `parallel_group_id` 相同、无依赖、无共享输出、低冲突风险的任务可以并行。不确定就串行。需要合并时，由 `fullstack-integrator` 读取各 worker 的 manifest 和输出路径，完成集成验证。

worker 必须写 manifest、summary、证据路径和状态，只向 coordinator 返回路径和结构化状态。
