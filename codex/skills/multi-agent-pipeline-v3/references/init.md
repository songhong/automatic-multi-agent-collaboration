# 初始化协议

coordinator 初始化 `.agent-work/`，建立控制面文件、日志、材料清单、agent id 缓存、批量配置和经验库缓存。初始化阶段仍然不得读取业务正文。

## 允许读取的内容

coordinator 只允许读取 pipeline reference 文件和控制面文件。对用户材料只允许读取元数据：path、filename、extension、size、last_modified、existence。

允许命令：`ls`、`stat`、`Get-Item`、`Get-ChildItem`、`Test-Path`。禁止对用户需求或材料使用：`Read`、`cat`、`type`、`Get-Content`、`head`、`tail`、`sed`、`grep`、`rg`、`Select-String`。

## 目录与材料清单

确保存在 `.agent-work/input/`、`materials/`、`state/`、`logs/`、`plans/`、`tasks/`、`results/`、`test-reports/`、`evidence/`、`handoffs/`、`human-review/`、`final/`、`archive/`、`experience/`。

材料清单只写 `path`、`filename`、`extension`、`size`、`last_modified`、`user_instruction`、`content_read_by_coordinator: false`、`authorized_reader: project-planner`。不要写摘要、关键词、正文摘录或判断。

## 需求文件与 agent id

把用户原始要求写入 `.agent-work/input/project-requirements.md`。写入后 coordinator 不再读取该文件正文，只把路径交给 planner。

创建 `.agent-work/state/agent-id-cache.json`。修复循环、计划修订、复测必须优先使用缓存里的同一 agent id。只有 id 不可用、过期或 SendMessage 失败时，才允许逻辑 resume。

## 经验库初始化

创建 `.agent-work/experience/`，为每个配置的 agent 准备项目缓存经验文件，并从全局经验库复制或合并。coordinator 只创建、复制、合并和传递经验库路径，不读取经验正文、不摘要经验、不把经验内容写入日志。

## 参考文件触发词

用户说“参考、参考内容、可以参考、看这个文件、阅读这个文件、按照文件内容、依据材料、用这几个文件、结合这些文档”，都只代表授权 `project-planner` 或后续授权子 agent 阅读正文，不代表 coordinator 可以读。

如果 coordinator 误读了材料正文，必须立即停止流程，写 `.agent-work/human-review/coordinator-read-violation.md`，只记录路径和工具名，不记录正文内容，也不得基于已读内容继续规划。
