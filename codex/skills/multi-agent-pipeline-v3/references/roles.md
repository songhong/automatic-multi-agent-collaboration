# Codex 角色说明

Codex 版角色与 Claude 版一致：coordinator 调度，planner 计划，developer 执行，tester 验收，integrator 集成，release-packager 做最终审计。

coordinator 不读业务正文。planner、plan-reviewer、developer、tester 只在职责范围内读取被授权的业务文件。

所有角色都必须通过路径和结构化 JSON 交接，避免把长需求、代码、测试报告塞进主上下文。
