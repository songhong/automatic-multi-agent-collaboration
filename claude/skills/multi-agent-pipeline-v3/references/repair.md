# 修复循环协议

谁开发谁修复，谁测试谁复测。coordinator 不换人、不读正文，只把失败报告路径传回原责任链。

原 developer 接收失败 tester 的 `TEST_REPORT_PATH`、`RESULT_JSON_PATH`、上一轮 manifest、任务路径和 attempt。

同一 batch 最多修复 3 轮。仍失败时写 human-review 文件并停止该 batch。

developer 可以不同意 tester 的问题，但必须写技术理由。原 tester 负责复核是否接受该反驳。
