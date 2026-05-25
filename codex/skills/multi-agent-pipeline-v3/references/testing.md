# 测试协议

tester 检查正确性和完成质量，不只是检查有没有错误。tester 必须读取任务包、developer manifest、输出路径、证据和授权 source anchors。

测试报告必须分成 `CORRECTNESS_ISSUES` 与 `QUALITY_ISSUES`。任一类存在 blocking 或 major 问题都不能 PASS。

## 必查项

- developer 是否按任务包执行。
- 是否覆盖验收标准和质量验收标准。
- 是否越权读取无关原始需求。
- 产物是否可运行、完整、可维护。
- 如果任务包过简，返回 `BLOCKED_TASK_PACKAGE_INCOMPLETE` 或 FAIL。

## 经验 summary 验收

不是每个成功任务都必须写经验。只有失败修复闭环、计划/审查失败修订、或发现可迁移错误模式时才需要经验 summary。

如果本轮是 tester FAIL 后的修复复测，tester 必须检查 developer 的 `<OUTPUT_DIR>/experience-append-summary.md`：

- 缺少 summary：不能 PASS，返回 `BLOCKED_EXPERIENCE_SUMMARY_MISSING`。
- `EXPERIENCE_DECISION: NO_TRANSFERABLE_LESSON` 且理由合理：可以 PASS。
- `EXPERIENCE_DECISION: APPENDED` 但 `GLOBAL_EXPERIENCE_SYNC` 不是 `OK`：不能普通 PASS，返回 `BLOCKED_EXPERIENCE_SYNC` 或 FAIL。
- 经验过于具体、不可迁移：不能 PASS，要求改写为原则级、模式级、可迁移表达。
- 触发 LRU 蒸馏时，必须看到 `LRU_DISTILLATION_RUN` 和 `DISTILLED_ENTRY_COUNT`。

tester 自己如果发现可迁移的验收/质量检查经验，也按条件式经验协议写入项目和全局经验库；没有新教训时写 `NO_TRANSFERABLE_LESSON`，不要强行编经验。

## result.json

coordinator 只读取 result.json 的状态、数量、路径和是否需要修复。测试报告正文不进入 coordinator 上下文。
