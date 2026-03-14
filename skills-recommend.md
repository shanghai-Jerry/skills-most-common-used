# 高级 Agent 开发工程师常用 Skills 推荐

> 本文档整理了 Cursor / Claude Code / Codex 等 AI Agent 开发中最常用、最受欢迎的 Skills，涵盖内置技能、官方团队技能和社区精选技能。
>
> 更新日期：2026-03-14

---

## 目录

- [一、什么是 Agent Skills](#一什么是-agent-skills)
- [二、Cursor 内置技能（6 个）](#二cursor-内置技能6-个)
- [三、高级 Agent 工程师必备技能分类](#三高级-agent-工程师必备技能分类)
  - [3.1 工程效率类](#31-工程效率类)
  - [3.2 代码质量与安全类](#32-代码质量与安全类)
  - [3.3 MCP 与 Agent 架构类](#33-mcp-与-agent-架构类)
  - [3.4 前端与全栈开发类](#34-前端与全栈开发类)
  - [3.5 云平台与基础设施类](#35-云平台与基础设施类)
  - [3.6 数据与 AI/ML 类](#36-数据与-aiml-类)
  - [3.7 文档与办公自动化类](#37-文档与办公自动化类)
- [四、官方团队精选 Skills（Top 推荐）](#四官方团队精选-skillstop-推荐)
- [五、社区 Awesome 资源](#五社区-awesome-资源)
- [六、Skill 开发最佳实践](#六skill-开发最佳实践)

---

## 一、什么是 Agent Skills

Agent Skills 是一种开放标准，用于扩展 AI Agent 的专业能力。与 Rules（短小的编码规范指引）不同，Skills 是**模块化、可复用的工作流包**，包含：

- `SKILL.md` — 核心指令文件（YAML 前置元数据 + Markdown 正文）
- 可选的参考文档、示例、脚本等辅助文件

**核心特性：**

| 特性 | 说明 |
|------|------|
| 动态上下文发现 | Agent 根据当前任务自动决定加载哪些 Skill，不浪费 Token |
| 跨平台兼容 | 一次编写，可在 Cursor、Claude Code、Codex、Gemini CLI、GitHub Copilot 等 16+ 工具中使用 |
| 渐进式加载 | 安装数十个 Skill 也不会影响性能 |
| 版本控制友好 | 存放在项目 `.cursor/skills/` 中可随代码库提交共享 |

**存储位置：**

| 级别 | 路径 | 作用域 |
|------|------|--------|
| 个人 | `~/.cursor/skills/skill-name/` | 所有项目可用 |
| 项目 | `.cursor/skills/skill-name/` | 仅当前项目，可与团队共享 |

---

## 二、Cursor 内置技能（6 个）

Cursor 自带 6 个内置技能，位于 `~/.cursor/skills-cursor/`，覆盖了 Agent 开发的基础工作流。

### 1. create-rule — 创建 Cursor 规则

| 项目 | 说明 |
|------|------|
| **作用** | 在 `.cursor/rules/` 中创建 `.mdc` 规则文件，为 AI Agent 提供持久化的项目编码规范和上下文指引 |
| **使用场景** | 设定编码标准、项目约定、文件特定模式（如 TypeScript 错误处理规范、React 组件模式） |
| **核心要点** | 规则文件支持 `alwaysApply`（全局生效）和 `globs`（按文件模式匹配生效）两种模式 |
| **最佳实践** | 每个规则保持在 50 行以内，一个规则只关注一个关切点，提供具体的正反对比示例 |

**典型用法：** 当你希望 Agent 在所有 `.ts` 文件中遵循统一的错误处理模式时，创建一条 globs 为 `**/*.ts` 的规则。

### 2. create-skill — 创建 Agent 技能

| 项目 | 说明 |
|------|------|
| **作用** | 引导用户创建自定义 Agent Skill，包括 SKILL.md 的结构化编写、描述优化、目录组织 |
| **使用场景** | 将重复的工作流封装为可复用的技能（如 PR 审查流程、Git 提交消息生成、数据库 Schema 查询） |
| **核心要点** | 描述用第三人称、包含"做什么"和"何时用"、SKILL.md 控制在 500 行以内 |
| **设计模式** | 支持模板模式、示例模式、工作流模式、条件工作流模式、反馈循环模式 |

**典型用法：** 输入 `/create-skill` 在对话中交互式地创建新技能。

### 3. create-subagent — 创建自定义子 Agent

| 项目 | 说明 |
|------|------|
| **作用** | 创建专用子 Agent（SubAgent），它们在隔离上下文中运行，拥有自定义系统提示词 |
| **使用场景** | 需要专业化角色时：代码审查员、调试专家、数据科学家、安全审计员等 |
| **核心要点** | 子 Agent 文件为 `.md` 格式，放在 `.cursor/agents/`（项目级）或 `~/.cursor/agents/`（用户级） |
| **关键技巧** | 在描述中加入"use proactively"可让 Agent 自动委派任务给子 Agent |

**典型用法：** 创建一个 `code-reviewer` 子 Agent，每次修改代码后自动触发代码审查。

### 4. migrate-to-skills — 迁移规则/命令为技能

| 项目 | 说明 |
|------|------|
| **作用** | 将旧的"智能应用"规则（`.mdc`）和斜杠命令（`.md`）转换为新的 Agent Skills 格式 |
| **使用场景** | 项目升级、统一管理现有的规则和命令、从旧格式迁移到标准 Skills 格式 |
| **核心要点** | 保持原始内容不变（逐字复制），仅调整元数据格式；支持批量并行迁移 |
| **安全保障** | 支持撤销迁移，恢复原始文件 |

**典型用法：** 当积累了大量 `.cursor/rules/` 中的 `.mdc` 规则时，一键迁移为可跨平台使用的标准 Skill。

### 5. shell — 直接执行 Shell 命令

| 项目 | 说明 |
|------|------|
| **作用** | 将 `/shell` 后的文本作为字面 Shell 命令直接执行 |
| **使用场景** | 快速执行终端命令，无需 Agent 解释或"优化"命令 |
| **核心要点** | 不会重写、解释或"改进"命令；先执行后报告状态 |

**典型用法：** `/shell docker compose up -d` 直接启动 Docker 服务。

### 6. update-cursor-settings — 修改编辑器设置

| 项目 | 说明 |
|------|------|
| **作用** | 读取并修改 Cursor/VSCode 的 `settings.json` 用户设置 |
| **使用场景** | 调整字体大小、主题、Tab 大小、自动保存、格式化等编辑器偏好设置 |
| **核心要点** | 先读取现有配置再修改，保留所有未变更设置，支持 JSON 注释格式 |

**典型用法：** "帮我把字体调大一点" → 自动修改 `editor.fontSize`。

---

## 三、高级 Agent 工程师必备技能分类

### 3.1 工程效率类

| 技能名称 | 来源 | 作用 | 推荐指数 |
|----------|------|------|----------|
| **code-review** | 自建/社区 | 自动化代码审查，按优先级（Critical/Warning/Suggestion）分层反馈 | ⭐⭐⭐⭐⭐ |
| **git-commit-helper** | 自建 | 分析 git diff 生成结构化 Commit 消息（Conventional Commits 格式） | ⭐⭐⭐⭐⭐ |
| **debugger** | 子 Agent | 系统化调试：捕获错误→定位根因→最小修复→验证方案 | ⭐⭐⭐⭐⭐ |
| **webapp-testing** | Anthropic 官方 | 使用 Playwright 测试本地 Web 应用 | ⭐⭐⭐⭐ |
| **github** | CallStack | GitHub 工作流：PR 管理、代码审查、分支策略 | ⭐⭐⭐⭐ |

### 3.2 代码质量与安全类

| 技能名称 | 来源 | 作用 | 推荐指数 |
|----------|------|------|----------|
| **audit-context-building** | Trail of Bits | 超细粒度代码分析，构建深度架构上下文 | ⭐⭐⭐⭐⭐ |
| **building-secure-contracts** | Trail of Bits | 智能合约安全工具包，支持 6 条区块链的漏洞扫描 | ⭐⭐⭐⭐ |
| **constant-time-analysis** | Trail of Bits | 检测密码学代码中编译器引入的时序侧信道 | ⭐⭐⭐⭐ |
| **ask-questions-if-underspecified** | Trail of Bits | 需求模糊时主动追问澄清，避免错误假设 | ⭐⭐⭐⭐⭐ |

### 3.3 MCP 与 Agent 架构类

| 技能名称 | 来源 | 作用 | 推荐指数 |
|----------|------|------|----------|
| **mcp-builder** | Anthropic 官方 | 创建 MCP 服务器，集成外部 API 和服务 | ⭐⭐⭐⭐⭐ |
| **building-mcp-server-on-cloudflare** | Cloudflare | 在 Cloudflare 上构建远程 MCP 服务器（含 OAuth） | ⭐⭐⭐⭐⭐ |
| **agents-sdk** | Cloudflare | 构建有状态 AI Agent（调度、RPC、MCP 服务器） | ⭐⭐⭐⭐⭐ |
| **building-ai-agent-on-cloudflare** | Cloudflare | 在 Cloudflare 上构建具有状态和 WebSocket 的 AI Agent | ⭐⭐⭐⭐ |
| **skill-creator** | Anthropic 官方 | 创建扩展 Claude 能力的 Skill 指南 | ⭐⭐⭐⭐ |
| **composio-skills** | Composio | 连接 AI Agent 到 1000+ 外部应用，含管理认证 | ⭐⭐⭐⭐ |

### 3.4 前端与全栈开发类

| 技能名称 | 来源 | 作用 | 推荐指数 |
|----------|------|------|----------|
| **react-best-practices** | Vercel | React 最佳实践与模式 | ⭐⭐⭐⭐⭐ |
| **next-best-practices** | Vercel | Next.js 最佳实践与推荐模式 | ⭐⭐⭐⭐⭐ |
| **web-design-guidelines** | Vercel | Web 设计指南与标准 | ⭐⭐⭐⭐ |
| **composition-patterns** | Vercel | React 组件组合与复用模式 | ⭐⭐⭐⭐ |
| **frontend-design** | Anthropic 官方 | 前端设计和 UI/UX 开发工具 | ⭐⭐⭐⭐ |
| **react-native-best-practices** | CallStack | React Native 性能优化 | ⭐⭐⭐⭐ |
| **shadcn-ui** | Google Labs | 使用 shadcn/ui 构建 UI 组件 | ⭐⭐⭐⭐ |

### 3.5 云平台与基础设施类

| 技能名称 | 来源 | 作用 | 推荐指数 |
|----------|------|------|----------|
| **terraform-code-generation** | HashiCorp | 生成和验证 Terraform HCL 代码 | ⭐⭐⭐⭐⭐ |
| **terraform-module-generation** | HashiCorp | 创建和重构 Terraform 模块 | ⭐⭐⭐⭐ |
| **wrangler** | Cloudflare | 部署和管理 Workers、KV、R2、D1 等 | ⭐⭐⭐⭐⭐ |
| **durable-objects** | Cloudflare | 有状态协调：RPC、SQLite、WebSocket | ⭐⭐⭐⭐ |
| **web-perf** | Cloudflare | 审计 Core Web Vitals 和渲染阻塞资源 | ⭐⭐⭐⭐ |
| **netlify-functions** | Netlify | 构建 Serverless API 端点和后台任务 | ⭐⭐⭐⭐ |
| **stripe-best-practices** | Stripe | 构建 Stripe 集成的最佳实践 | ⭐⭐⭐⭐ |
| **supabase-postgres-best-practices** | Supabase | Supabase PostgreSQL 最佳实践 | ⭐⭐⭐⭐ |
| **neon-postgres** | Neon | Neon 无服务器 Postgres 最佳实践 | ⭐⭐⭐⭐ |

### 3.6 数据与 AI/ML 类

| 技能名称 | 来源 | 作用 | 推荐指数 |
|----------|------|------|----------|
| **hugging-face-model-trainer** | Hugging Face | 模型训练（SFT、DPO、GRPO、GGUF 转换） | ⭐⭐⭐⭐⭐ |
| **hugging-face-datasets** | Hugging Face | 创建和管理数据集（含配置与 SQL 查询） | ⭐⭐⭐⭐⭐ |
| **hugging-face-evaluation** | Hugging Face | 模型评估（vLLM/lighteval） | ⭐⭐⭐⭐ |
| **hugging-face-trackio** | Hugging Face | 实时 ML 实验跟踪仪表盘 | ⭐⭐⭐⭐ |
| **hugging-face-cli** | Hugging Face | HF Hub CLI：模型、数据集、仓库管理 | ⭐⭐⭐⭐ |
| **clickhouse-agent-skills** | ClickHouse | ClickHouse 最佳实践 | ⭐⭐⭐⭐ |
| **replicate** | Replicate | 发现、比较和运行 AI 模型 | ⭐⭐⭐⭐ |
| **data-scientist** | 子 Agent | SQL 查询、BigQuery 操作、数据分析洞察 | ⭐⭐⭐⭐ |

### 3.7 文档与办公自动化类

| 技能名称 | 来源 | 作用 | 推荐指数 |
|----------|------|------|----------|
| **docx** | Anthropic 官方 | 创建、编辑、分析 Word 文档 | ⭐⭐⭐⭐ |
| **xlsx** | Anthropic 官方 | 创建、编辑、分析 Excel 电子表格 | ⭐⭐⭐⭐ |
| **pdf** | Anthropic 官方 | 提取文本、创建 PDF、处理表单 | ⭐⭐⭐⭐ |
| **pptx** | Anthropic 官方 | 创建、编辑、分析 PowerPoint 演示文稿 | ⭐⭐⭐⭐ |
| **doc-coauthoring** | Anthropic 官方 | 协作文档编辑和共同编写 | ⭐⭐⭐⭐ |
| **firecrawl-cli** | Firecrawl | 通过 CLI 抓取、爬取、搜索和映射网页 | ⭐⭐⭐⭐ |
| **google-workspace** | Google | 管理 Drive、Sheets、Gmail、Calendar 等 | ⭐⭐⭐⭐ |

---

## 四、官方团队精选 Skills（Top 推荐）

以下是各大厂商官方发布的 Agent Skills，质量有保障，强烈推荐安装使用：

### Anthropic 官方（17 个 Skills）

```
anthropics/mcp-builder          # 创建 MCP 服务器 ⭐ 最推荐
anthropics/webapp-testing       # Playwright Web 测试
anthropics/skill-creator        # Skill 创建指南
anthropics/pdf                  # PDF 处理
anthropics/xlsx                 # Excel 处理
anthropics/docx                 # Word 处理
anthropics/frontend-design      # 前端设计
anthropics/canvas-design        # 视觉艺术设计
anthropics/algorithmic-art      # 生成艺术（p5.js）
```

### Vercel 工程团队（8 个 Skills）

```
vercel-labs/react-best-practices    # React 最佳实践 ⭐ 最推荐
vercel-labs/next-best-practices     # Next.js 最佳实践 ⭐ 最推荐
vercel-labs/web-design-guidelines   # Web 设计指南
vercel-labs/composition-patterns    # React 组合模式
vercel-labs/next-upgrade            # Next.js 升级
vercel-labs/next-cache-components   # Next.js 缓存策略
```

### Cloudflare 团队（7 个 Skills）

```
cloudflare/agents-sdk                        # AI Agent SDK ⭐ 最推荐
cloudflare/building-mcp-server-on-cloudflare # MCP 服务器构建 ⭐ 最推荐
cloudflare/wrangler                          # Workers 部署管理
cloudflare/durable-objects                   # 有状态协调
cloudflare/web-perf                          # 性能优化
```

### Trail of Bits 安全团队

```
trailofbits/audit-context-building           # 安全审计上下文 ⭐ 最推荐
trailofbits/ask-questions-if-underspecified   # 主动追问
trailofbits/building-secure-contracts        # 智能合约安全
trailofbits/constant-time-analysis           # 时序分析
```

### Hugging Face 团队（8 个 Skills）

```
huggingface/hugging-face-model-trainer   # 模型训练 ⭐ 最推荐
huggingface/hugging-face-datasets        # 数据集管理
huggingface/hugging-face-evaluation      # 模型评估
huggingface/hugging-face-trackio         # 实验跟踪
huggingface/hugging-face-cli             # Hub CLI
```

### HashiCorp Terraform 团队

```
hashicorp/terraform-code-generation      # Terraform 代码生成 ⭐ 最推荐
hashicorp/terraform-module-generation    # 模块生成
hashicorp/terraform-provider-development # Provider 开发
```

---

## 五、社区 Awesome 资源

以下是最活跃的 Agent Skills 社区仓库，适合发现更多 Skills：

| 仓库 | Stars | 说明 |
|------|-------|------|
| [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | 11,200+ | 最大的集合，549+ Skills，含所有主要官方团队 |
| [JackyST0/awesome-agent-skills](https://github.com/JackyST0/awesome-agent-skills) | 340+ | 提供一键安装脚本（macOS/Linux/Windows） |
| [skillmatic-ai/awesome-agent-skills](https://github.com/skillmatic-ai/awesome-agent-skills) | 270+ | 定位为权威资源，含基础知识和构建指南 |
| [philipbankier/awesome-agent-skills](https://github.com/philipbankier/awesome-agent-skills) | — | 跨平台目录，覆盖 Skills、MCP、Rules 等 |

**安装 Skill 的方式：**

```bash
# 方式一：克隆到项目级目录
mkdir -p .cursor/skills && cd .cursor/skills
git clone https://github.com/some-org/some-skill.git

# 方式二：克隆到用户级目录（所有项目可用）
mkdir -p ~/.cursor/skills && cd ~/.cursor/skills
git clone https://github.com/some-org/some-skill.git

# 方式三：使用 Cursor 内置命令
# 在 Agent 对话中输入 /create-skill 交互式创建
```

---

## 六、Skill 开发最佳实践

### 1. SKILL.md 编写原则

```yaml
---
name: my-skill-name          # 小写字母 + 连字符，最多 64 字符
description: >-               # 最多 1024 字符，用第三人称
  Analyze git diffs and generate descriptive commit messages
  following Conventional Commits format. Use when the user asks
  for help writing commit messages or reviewing staged changes.
---
```

### 2. 描述（Description）是发现的关键

Agent 根据描述决定何时激活 Skill，因此描述必须包含：

- **做什么（WHAT）**：具体能力描述
- **何时用（WHEN）**：触发场景和关键词

```yaml
# ❌ 太模糊
description: Helps with documents

# ✅ 具体且有触发词
description: >-
  Extract text and tables from PDF files, fill forms, merge documents.
  Use when working with PDF files or when the user mentions PDFs,
  forms, or document extraction.
```

### 3. 内容精简原则

- SKILL.md 控制在 **500 行以内**
- Agent 本身已很聪明，只提供它不知道的领域知识
- 详细参考资料放在单独文件中（渐进式加载）
- 参考文件保持**一层深度**，避免嵌套引用

### 4. 自由度匹配任务脆弱性

| 自由度 | 适用场景 | 示例 |
|--------|----------|------|
| 高（文本指令） | 多种有效方案，依赖上下文 | 代码审查指南 |
| 中（伪代码/模板） | 有首选模式但允许变通 | 报告生成 |
| 低（具体脚本） | 脆弱操作，一致性至关重要 | 数据库迁移 |

### 5. 常用设计模式

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| **模板模式** | 提供输出格式模板 | 报告、文档生成 |
| **示例模式** | 提供输入/输出示例对 | Commit 消息、代码风格 |
| **工作流模式** | 分步骤带检查清单 | 复杂多步操作 |
| **条件工作流** | 决策点引导 | 多分支操作流程 |
| **反馈循环** | 验证→修复→再验证 | 质量关键型任务 |

### 6. 反模式避免

| 反模式 | 正确做法 |
|--------|----------|
| Windows 风格路径 `scripts\helper.py` | 使用 `scripts/helper.py` |
| 提供太多选项让 Agent 困惑 | 给出默认选择 + 特殊情况的替代方案 |
| 时效性信息（"2025年8月前用旧API"） | 用"当前方法"和"旧方法（已弃用）"分区 |
| 术语不统一（混用 URL/route/path） | 全文统一使用一个术语 |
| 模糊命名（`helper`、`utils`） | 使用描述性名称（`processing-pdfs`） |

---

## 附：Agent 开发核心知识图谱

```
Agent 开发工程师技能树
├── 基础能力
│   ├── Prompt Engineering（提示词工程）
│   ├── LLM API 调用与参数调优
│   └── Token 管理与上下文窗口优化
├── 协议与标准
│   ├── MCP（Model Context Protocol）— Agent 连接工具的标准
│   ├── A2A（Agent-to-Agent）— Agent 间通信协议
│   └── Agent Skills 开放标准
├── 架构模式
│   ├── 并行 / Map-Reduce 模式
│   ├── 路由器（Router）模式
│   ├── 意图分类器（Intent Classifier）
│   ├── 编排器 / 规划器（Orchestrator / Planner）
│   └── 评估器-优化器（Evaluator-Optimizer）循环
├── 工具生态
│   ├── Cursor / Claude Code / Codex 等 IDE Agent
│   ├── MCP Server 开发（JSON-RPC 2.0 + Streamable HTTP）
│   ├── Playwright / Puppeteer 浏览器自动化
│   └── Docker / Terraform / Kubernetes 基础设施
└── 质量保障
    ├── Agent 输出验证与评估
    ├── 安全审计（提示注入、工具投毒检测）
    └── 可观测性与追踪
```

---

> **提示：** 在 Cursor 中输入 `/create-skill` 可以交互式创建新技能，输入 `/` 可以查看和调用所有已安装的技能。推荐从安装官方团队的 Skills 开始，逐步根据自己的工作流定制专属技能。
