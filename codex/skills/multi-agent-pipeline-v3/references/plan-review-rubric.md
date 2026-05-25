# 计划审查 Rubric

`plan-reviewer` 检查 planner 的计划是否足够让用户理解、让 developer 执行、让 tester 验收。

## 必须 FAIL 的情况

- 计划只有模块标题或短 bullet，没有实施路线。
- 没有说明用户目标、成功标准、约束或交付物。
- 任务存在明显路线选择，却没有方案比较和推荐理由。
- 需求不清时没有写 `ASK_USER_QUESTIONS_PATH`，也没有说明为何不需要追问。
- 没有 readiness 判断。
- 没有材料映射，或明显漏用用户新增参考文件。
- 缺少架构、数据流、接口关系、文件/模块影响范围或执行顺序。
- task package 缺少目标、范围、验收、输出路径、source anchors 或允许回看范围。
- 长需求没有拆到 task-level anchors。
- tester、quality gate、premium review 选择明显不合理。

## PASS 条件

用户能从 plan 看出整体路线、关键取舍、风险和交付物；developer 拿任务包能直接执行；tester 知道如何判断正确性和质量；source anchors 覆盖长需求关键内容；coordinator 只需要读 `result.json` 就能调度下一步。
