# Agent Selection Reference Guide

本指南帮助 project-planner 在分析项目需求后，**系统化地**为每个任务分配正确的开发 agent 和测试 agent。

## 一、决策树：项目特征 → Agent 类型

```
收到项目需求
  │
  ├── 主要是 Web 前端 (React/Vue/HTML/CSS/UI组件)？
  │     ├── 是 → 主要用 frontend-developer
  │     └── 否 → 继续
  │
  ├── 主要是后端/API (数据库/认证/业务逻辑/微服务)？
  │     ├── 是 → 主要用 backend-developer
  │     └── 否 → 继续
  │
  ├── 主要是文档产出 (Word/PPT/PDF/实验报告/论文)？
  │     ├── 是 → 主要用 document-writer
  │     └── 否 → 继续
  │
  ├── 主要是数据分析 (数据处理/可视化/科学计算/统计)？
  │     ├── 是 → 主要用 data-analyst
  │     └── 否 → 继续
  │
  ├── 主要是CLI工具/自动化 (命令行/脚本/系统工具)？
  │     ├── 是 → 主要用 toolsmith
  │     └── 否 → 继续
  │
  ├── 主要是架构、接口契约、模块边界审查？
  │     ├── 是 → architect-agent
  │     └── 否 → 继续
  │
  ├── 主要是前后端联调、产物合并、启动脚本、环境配置？
  │     ├── 是 → fullstack-integrator
  │     └── 否 → 继续
  │
  ├── 主要是最终交付、README、运行说明、产物清单？
  │     ├── 是 → release-packager
  │     └── 否 → 继续
  │
  └── 不确定/混合类型/简单修改？
        └── 是 → development-agent（通用）
```

## 二、各开发 Agent 能力清单

### frontend-developer（前端专攻）
- React/Vue/Svelte 组件开发
- HTML/CSS/JS 页面构建
- UI 交互、动画、状态管理
- 响应式布局、跨浏览器兼容
- 前端路由、表单处理
- **配备技能**：优先使用已安装 `frontend-design`，需要浏览器证据时使用 `webapp-testing`；React/shadcn 按项目已有模式执行
- **不可用于**：后端 API、数据库、CLI、文档生成

### backend-developer（后端专攻）
- RESTful API / GraphQL 设计
- 数据库设计（SQL/NoSQL）
- 认证授权（JWT/OAuth/Session）
- 业务逻辑、数据校验
- 中间件、缓存、消息队列
- **配备技能**：brainstorming
- **不可用于**：前端 UI、文档排版、数据分析可视化

### document-writer（文档写作）
- Word 文档（python-docx）
- PowerPoint 演示文稿（python-pptx）
- PDF 生成（fpdf2/LaTeX）
- 物理/化学实验报告（含科学制图）
- 数学/化学公式（LaTeX mhchem）
- 科学图表（matplotlib/seaborn）
- **配备技能**：pdf, brainstorming
- **不可用于**：Web 前端、后端 API、CLI 工具、纯数据管道

### data-analyst（数据分析）
- 数据分析（pandas/numpy）
- 科学计算（scipy）
- 可视化（matplotlib/seaborn/plotly）
- 统计检验、回归分析
- 数据清洗、特征工程
- **配备技能**：brainstorming
- **不可用于**：Web 前端、后端 API、文档排版

### toolsmith（CLI 工具）
- 命令行工具开发
- 自动化脚本（shell/python/node）
- 系统管理工具
- 文件处理批处理
- CI/CD 流水线脚本
- **配备技能**：brainstorming
- **设计原则**：Unix 哲学、管道兼容、--dry-run、合理退出码
- **不可用于**：Web 前端、文档排版、数据分析

### architect-agent（架构与接口契约）
- 架构一致性、模块边界、接口契约
- 审查任务拆分是否会造成跨 agent 冲突
- 不写业务代码

### fullstack-integrator（集成）
- 前后端接口对齐
- 启动脚本、路由、环境变量、联调
- 多 agent 产物合并

### release-packager（交付）
- 最终产物清单
- README/运行说明
- 最终总结报告

### development-agent（通用）
- 全栈项目
- 简单修改、bug 修复
- 当一个任务包含多种类型（前后端混合）
- 不确定用什么 agent 时的安全默认
- **配备技能**：brainstorming
- **适用场景**：任务类型不明确、或同时涉及前后端

## 三、测试 Agent 参与矩阵

| 项目类型 | code-quality | visual | runtime | security | performance | data-integrity | accessibility |
|---------|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| Web 前端 | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| 后端 API | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 文档 | ✅ | ✅ | ✅ | — | — | ✅ | — |
| 数据分析 | ✅ | — | ✅ | — | ✅ | ✅ | — |
| CLI 工具 | ✅ | — | ✅ | ✅ | ✅ | — | — |
| 全栈/通用 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

✅ = 参与测试，— = 返回 SKIP。

### 各测试 Agent 触发条件

- **tester-code-quality**：总是参与（所有项目类型）
- **tester-visual-aesthetic**：仅 Web 前端 + 文档
- **tester-runtime-effect**：总是参与
- **tester-security**：Web 前端 + 后端 + CLI + 全栈
- **tester-performance**：Web 前端 + 后端 + 数据分析 + CLI + 全栈
- **tester-data-integrity**：后端 + 文档 + 数据分析 + 全栈
- **tester-accessibility**：仅 Web 前端

## 四、任务级别的 Agent 分配

### 单一类型任务 → 直接匹配
```
任务："构建用户登录页面" → frontend-developer
任务："设计数据库Schema" → backend-developer
任务："生成实验报告Word" → document-writer
任务："分析销售数据趋势" → data-analyst
任务："写部署脚本" → toolsmith
```

### 混合类型项目 → 按任务拆分
```
项目："全栈电商网站"
├── task-001: "商品列表前端页面" → frontend-developer
├── task-002: "商品API+库存管理" → backend-developer
├── task-003: "用户登录/注册页面" → frontend-developer
├── task-004: "认证API" → backend-developer
├── task-005: "购物车前端+后端" → development-agent（全栈）
```

### 任务分配到测试 Agent

每个任务根据其 `type` 决定需要哪些测试，写入 `required_testers`:

| task type | required_testers |
|-----------|-----------------|
| frontend | code-quality, visual, runtime, security, performance, accessibility |
| backend | code-quality, runtime, security, performance, data-integrity |
| frontend+backend (fullstack) | code-quality, visual, runtime, security, performance, data-integrity, accessibility |
| document | code-quality, visual, runtime, data-integrity |
| data-analysis | code-quality, runtime, performance, data-integrity |
| cli-tool | code-quality, runtime, security, performance |
| general | code-quality, runtime |

## 五、复杂项目分解示例

### 示例 1：物理实验报告
```
项目类型：文档 (document)
├── task-001: "数据处理与误差分析" → data-analyst
│   required_testers: code-quality, runtime, data-integrity
├── task-002: "生成实验报告Word(含图表公式)" → document-writer
│   required_testers: code-quality, visual, runtime, data-integrity
└── task-003: "制作汇报PPT" → document-writer
    required_testers: code-quality, visual, runtime
```

### 示例 2：前端仪表盘
```
项目类型：Web 前端 (frontend)
├── task-001: "侧边栏导航组件" → frontend-developer
├── task-002: "数据图表面板" → frontend-developer
├── task-003: "用户设置表单页面" → frontend-developer
└── task-004: "仪表盘首页(汇总卡片+图表)" → frontend-developer
所有任务 required_testers: code-quality, visual, runtime, security, performance, accessibility
```

### 示例 3：数据分析脚本
```
项目类型：数据分析 (data-analysis)
├── task-001: "数据清洗与预处理" → data-analyst
│   required_testers: code-quality, runtime
├── task-002: "统计分析与可视化" → data-analyst
│   required_testers: code-quality, runtime, performance, data-integrity
└── task-003: "生成分析报告" → data-analyst (或 document-writer 如需 Word/PDF)
    required_testers: code-quality, runtime, data-integrity
```

## 六、不确定时的处理原则

1. **如果无法确定项目类型** → 使用 `development-agent` + `tester-code-quality` + `tester-runtime-effect`
2. **如果一个任务跨越多个类型** → 使用 `development-agent`，`type` 设为 `fullstack`
3. **如果用户只说"帮我做一个XX"** → 用 brainstorming skill 和用户澄清后决定
4. **如果项目很简单但用户给了复杂需求** → 优先选择最匹配的专用 agent，不用通用
5. **如果确定不了测试是否要参与** → 先启用 code-quality/runtime，只有任务证据显示需要时再加入专项 tester，避免无效 SKIP 消耗 token
