# agent skill 使用矩阵

只按任务需要加载 skill，禁止为了保险一次性加载所有 skill。缺失 skill 时记录 `SKILL_UNAVAILABLE`，再使用本项目 reference/rubric。

- planner：可用 brainstorming 方法逐问澄清、比较方案、提炼成功标准。
- `frontend-developer`：按需使用 `frontend-design`、`webapp-testing` 或 Codex 中的 `vercel:*`、浏览器验证相关 skill。
- `document-writer`：按需使用 `docx`、`pdf`、`pptx`、`xlsx`、`doc-coauthoring` 或 Codex 文档/演示/表格 skill。
- `data-analyst`：按需使用表格、数据校验和可视化工具。
- `backend-developer`、`toolsmith`、`fullstack-integrator`：按需使用官方文档、调试、测试、部署相关 skill。
- tester：按测试类型加载视觉、运行、无障碍、安全、性能、数据完整性相关 skill。
- `release-packager`：按需使用文档、PDF、README、交付打包相关 skill，并承担最终交付审计。
