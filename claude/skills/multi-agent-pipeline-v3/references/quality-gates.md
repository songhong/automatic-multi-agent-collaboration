# 质量门协议

## completion_quality_gate

每个任务默认进入普通完成质量门。tester 不只检查有没有报错，还要检查正确性、需求覆盖、完整度、可运行和可维护。blocking 或 major 问题不能 PASS。

## premium_review_gate

关键 batch 和最终交付可以启用精品评审门。评审专业度、一致性、最终用户体验、文档可交付性、视觉质量、性能、无障碍和跨交付物一致性。

planner 在任务队列中写 `quality_gate`、`premium_review_required`、`premium_review_reason`、`quality_acceptance_criteria`。coordinator 只读取这些控制字段，不读取业务正文。

质量门失败后回到原 developer 修复，原 tester 复测。三轮仍失败，转人工。
