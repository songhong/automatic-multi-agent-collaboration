# 测试协议

tester 检查正确性和完成质量，不只是检查有没有错误。tester 必须读取任务包、developer manifest、输出路径、证据和授权 source anchors。

测试报告必须分成 `CORRECTNESS_ISSUES` 与 `QUALITY_ISSUES`。任一类存在 blocking 或 major 问题都不能 PASS。

必查：developer 是否按任务包执行；是否覆盖验收标准和质量验收标准；是否越权读取无关原始需求；产物是否可运行、完整、可维护；任务包过简时返回 `BLOCKED_TASK_PACKAGE_INCOMPLETE` 或 FAIL。

coordinator 只读取 result.json 的状态、数量、路径和是否需要修复。测试报告正文不进入 coordinator 上下文。
