# 本地 fallback rubrics

当专项 skill 不可用时，agent 使用本地 rubric：正确性、完整度、可运行、可维护、安全、性能、无障碍。

rubric 结果必须写到 tester 报告和 result.json；coordinator 只读 result.json。
