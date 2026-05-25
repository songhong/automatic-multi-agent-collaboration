# 计划协议

## planner 职责

`project-planner` 读取用户需求文件和材料清单，然后写入计划、任务队列和 batch control。coordinator 只接收和展示路径、状态，不读取计划正文或任务正文。

## 计划确认与不满意路由

- 初版计划返回 `STATUS: WAITING_USER_CONFIRMATION`。
- coordinator 只把 `PLAN_PATH` 和 `TASK_QUEUE_PATH` 给用户。
- 用户反馈写入 `.agent-work/input/user-feedback-v<N>.md`。
- 修订计划必须尽量 resume 同一个 planner。

计划确认阶段，用户说“不满意、没理解、漏了、不对、重新看、仔细看、完整看、阅读任务文件、阅读需求文件、看我给的文件、按原文件重做、重新理解需求”，必须转交 `project-planner`，绝不代表 coordinator 可以读取业务文件。

coordinator 只写反馈路径，然后把 `PROJECT_REQUIREMENTS_PATH`、`MATERIALS_MANIFEST_PATH`、`PREVIOUS_PLAN_PATH`、`PREVIOUS_TASK_QUEUE_PATH`、`USER_FEEDBACK_PATH` 传给同一个 planner。coordinator 不得打开、摘要、引用或检查需求正文、任务正文、计划正文或材料正文。

## planner 被驳回后的经验复盘

当用户驳回计划、说计划太简单、要求 planner 重新阅读需求，或 developer/tester 反馈任务包太模糊时，`project-planner` 必须把它视为计划质量反馈。

每次 `revise_plan` 必须重新读取需求、材料、旧计划、旧任务队列和用户反馈；用更完整的任务包、source anchors、验收标准、输出路径和 tester 选择重写计划；写 `.agent-work/plans/planner-experience-decision-v<N>.md`。

经验判断文件必须包含 `EXPERIENCE_DECISION: APPENDED | NO_TRANSFERABLE_LESSON`。planner 经验必须描述计划模式，而不是项目事实。

## 任务包完整性

planner 不得把长需求压缩成含糊任务摘要。每个 `task.md` 都必须包含：目标、范围、非目标、相关需求锚点、材料路径、验收标准、质量验收标准、期望输出路径、不确定点、developer 原始需求回看授权。

长需求不要全文复制到每个任务。使用精确锚点：

```yaml
SOURCE_REQUIREMENTS_PATH: .agent-work/input/project-requirements.md
DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: true
SOURCE_ANCHORS:
  - anchor_id: REQ-001
    must_read: true
    locator: "章节标题、关键词、页面 id、段落描述或行号提示"
    reason: "为什么本任务必须读取这段原始需求"
```

如果任务只靠 `task.md` 就能完成，设置 `DEVELOPER_MAY_READ_SOURCE_REQUIREMENTS: false`，但仍要说明相关原始需求。

## agent 选择与 batch control

planner 必须为每个任务写 `recommended_agent` 和 `required_testers`。不要默认启动全部 tester；不确定时只用 `tester-code-quality` 和 `tester-runtime-effect`。

每个活动 batch，planner 按 `schemas.md` 写 `.agent-work/state/current-batch-control.json`。该文件只包含任务路径、developer 名、tester 名、输出索引路径和 attempt，不得包含任务正文或验收标准正文。

## planner brainstorm 与用户追问协议

planner 是唯一负责理解业务需求的 agent。需求模糊、过大、被用户驳回、或容易产生浅计划时，planner 使用 brainstorming 风格流程：阅读需求和材料、找出缺失意图/约束/受众/成功标准/取舍、一次只问一个关键问题、必要时写 2-3 个方案和推荐。

需要用户输入时，planner 写 `.agent-work/human-review/planner-questions-v<N>.json`，返回 `ASK_USER_QUESTIONS_PATH` 和 `STATUS: NEEDS_USER_INPUT`。coordinator 可以转发问题路径或使用 `AskUserQuestion`，但不得自己读取需求正文并发明业务解释。

## 保守并行计划

只有无依赖、无共享必改输出、不会改同一核心配置/schema/route/生成产物、任务包完整、`conflict_risk` 为 `low` 或有明确 integrator handoff 时，planner 才能并行任务。每个任务写 `parallel_group_id`、`dependency_task_ids`、`conflict_risk`、`shared_output_paths`。

## 计划质量门

每个 `initial_plan` 和 `revise_plan` 在交给用户前必须通过 plan review。planner 写 readiness、plan-quality check、plan、task queue 后，coordinator 只把路径交给 `plan-reviewer`，并且只读 review 的 `result.json`。

如果 plan review 失败，coordinator 不把计划给用户，而是 resume 同一个 `project-planner`，传 `PLAN_REVIEW_REPORT_PATH` 和 `PLAN_REVIEW_RESULT_PATH`。最多修订 3 轮，仍失败则写 human-review 文件。

只有模块列表或短 bullet 清单的计划必须失败。

## 参考材料路由

用户说“参考、阅读、查看、使用、结合、按照这些材料文件”时，coordinator 只能按路径转交 `project-planner`，不得打开材料正文。handoff 必须包含 `PROJECT_REQUIREMENTS_PATH`、`MATERIALS_MANIFEST_PATH` 和 `USER_MATERIAL_AUTHORIZATION`。
