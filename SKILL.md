---
name: "context-architect"
description: "Manages project documentation and context alignment. MUST be invoked for ALL feature development, refactoring, or coding tasks to enforce the Doc-First policy."
license: MIT
metadata:
  author: daydream
  version: "1.0.0"
allowed-tools: "Host-managed safe file operations only; no external shell commands"
triggers:
  - "初始化项目上下文"
  - "校验 6-Doc 完整性"
  - "对齐实现与规范"
  - "开发新功能"
  - "修改现有功能"
  - "更新项目文档"
  - "写代码"
  - "重构"
  - "实现"
  - "Initialize project context"
  - "Validate 6-Doc completeness"
  - "Align implementation with specs"
  - "Implement feature"
  - "Update context"
  - "build"
  - "create"
  - "refactor"
---

# Context Architect Skill

This skill enforces the "6-Doc Protocol" for AI-assisted development, ensuring all code changes are driven by clear, structured documentation.

## Core Capabilities

1.  **Initialize Context (`init`)**:
    - Creates `docs/context/` directory structure.
    - Generates 6 core templates in `docs/context/`: `PRD.md`, `APP_FLOW.md`, `TECH_STACK.md`, `FRONTEND_GUIDELINES.md`, `BACKEND_STRUCTURE.md`, `IMPLEMENTATION_PLAN.md`.
    - Prompts user to fill in key details before coding starts.

2.  **Validate Context (`validate`)**:
    - Checks if all 6 documents exist in `docs/context/` and are non-empty.
    - Warns if critical sections (e.g., "Tech Stack versions") are missing.

3.  **Update Context (`update` - DOC-FIRST ENFORCEMENT)**:
    - Triggered when the user requests a feature, refactor, or modification.
    - **TRIVIAL TASK BYPASS**: If the request is extremely simple (e.g., fixing a typo, code explanation, formatting, adding comments, renaming a variable within a single file), SKIP the doc update and proceed directly to answering or fixing.
    - For non-trivial tasks, analyzes the request and UPDATES the relevant documents (`PRD.md`, `APP_FLOW.md`, `IMPLEMENTATION_PLAN.md`) FIRST.
    - For non-trivial tasks, NEVER write or modify source code before the context documents are updated and the implementation plan is clear.

4.  **Align Context (`align`)**:
    - Reads relevant docs from `docs/context/` into context before major coding tasks.
    - Ensures implementation matches `TECH_STACK.md` and `FRONTEND_GUIDELINES.md`.

## Usage Instructions

### 1. Initialization
When starting a new project or feature:
> "Initialize the project context."
> "Set up the documentation structure."

**Action**: The skill will create the `docs/context/` folder and populate it with templates.

### 2. Validation
Before writing code:
> "Check if my documentation is ready."
> "Validate the specs."

**Action**: The skill will scan `docs/context/` and report missing or incomplete files.

### 3. Update & Implement (Doc-First)
When asking for a new feature:
> "Implement the login page."
> "开发登录页"

**Action**: The skill will FIRST update `PRD.md`, `APP_FLOW.md`, and `IMPLEMENTATION_PLAN.md` based on the request. Only after these docs are updated will it start writing source code.

### 4. Alignment (Automatic)
When user asks for code:
> "Implement the login page."

**Action**: The skill implicitly checks `docs/context/` to ensure the login page follows `APP_FLOW.md` and uses components from `FRONTEND_GUIDELINES.md`.

## The 6-Doc Protocol

1.  **PRD.md**: What are we building? (Scope, Users, Success Metrics)
2.  **APP_FLOW.md**: How does it work? (User Journeys, Routing)
3.  **TECH_STACK.md**: What tools do we use? (Exact versions, Libraries)
4.  **FRONTEND_GUIDELINES.md**: What does it look like? (Design System, UI Components)
5.  **BACKEND_STRUCTURE.md**: How is data handled? (Schema, API Contracts)
6.  **IMPLEMENTATION_PLAN.md**: What is the step-by-step plan? (Checklist)

## 触发与输出约定

- 触发短语：当用户表达“初始化项目上下文”“校验 6-Doc 完整性”“对齐实现与规范”“开发新功能”等时启用本技能。
- 强制文档先行：无论何时被唤醒进行功能开发，必须先更新 `docs/context/` 中的相关文档（特别是 `PRD.md`、`APP_FLOW.md` 和 `IMPLEMENTATION_PLAN.md`），再编写源代码。
- 输出风格：所有检查与建议均以结构化条目形式给出（文件 → 问题 → 建议），支持可选的 `--json` 机器可读输出。
- 干运行模式：所有创建/修改操作默认先生成差异报告，需显式确认后再执行。

## 安全约束与边界

- 文件操作范围仅限于当前仓库 `docs/context/` 目录，不允许写入仓库外路径；
- 禁止覆盖已有非空文件；提供备份与差异说明；
- 不读取或输出任何敏感信息（密钥、令牌、环境变量）；
- 不使用外部 shell 命令，所有写入由宿主安全接口完成；
- IO 失败、权限不足、磁盘空间不足等异常需明确报告并可恢复。

## 操作流程（精简版）

### init：初始化项目上下文

- 目标：创建 `docs/context/` 与六个模板文件
- 默认：干运行，先输出将创建/冲突清单
- 写入：仅在用户确认后执行；不覆盖非空文件；对空文件可填充模板
- 返回：成功/部分成功/失败（含明确原因与修复建议）

### validate：校验 6-Doc 完整性

- 检查：存在性、非空性、必填章节齐全
- 输出：结构化报告（file → issue → suggestion），支持 `--json`
- 失败：缺失关键文档或关键章节时拒绝进入实现阶段

### update：更新上下文（文档先行强制策略）

- 目标：确保任何实质性代码变更前，需求与计划已被文档化。
- 触发：用户请求开发新功能、重构或修改现有逻辑。
- 豁免（Bypass）：如果请求是极小改动（如修复拼写、代码解释、格式化、单文件内变量重命名），则跳过文档更新，直接执行。
- 动作：对于非豁免任务，首先更新 `docs/context/` 中的相关文档（如 `PRD.md`, `APP_FLOW.md`, `IMPLEMENTATION_PLAN.md`）。
- 约束：对于非豁免任务，在 `IMPLEMENTATION_PLAN.md` 明确具体的实施步骤前，**严禁编写或修改任何源代码**。

### align：对齐实现与规范

- 读取：`TECH_STACK.md`、`FRONTEND_GUIDELINES.md`（必要时包含 `BACKEND_STRUCTURE.md`）
- 对齐：依赖版本/禁止项/构建与测试脚本、设计系统与无障碍规范、API 契约与幂等策略
- 只读：输出偏差与建议；不直接修改代码

## 6-Doc 必填章节与判定标准

### PRD.md
- 范围：明确功能边界与非目标
- 用户：主要用户画像与场景
- 成功指标：可量化的验收标准

### APP_FLOW.md
- 用户旅程：关键步骤与异常分支
- 路由结构：主/次级路由与守卫
- 状态迁移：页面/组件状态图或描述

### TECH_STACK.md
- 语言/框架与版本：精确到次版本
- 依赖清单：核心库、可选库与禁止项
- 约束与不允许：性能、安全、合规要求

### FRONTEND_GUIDELINES.md
- 设计系统：色彩、字体、间距、阴影
- 组件规范：命名、属性、交互约束
- 无障碍与动效：A11y 要求与动效节奏

### BACKEND_STRUCTURE.md
- 数据模型：主表、关联、索引、约束
- API 契约：请求/响应、错误语义、重试策略
- 一致性：事务/补偿、幂等、去重

### IMPLEMENTATION_PLAN.md
- 分阶段任务：清单与里程碑
- 验收标准：每项任务的完成判定
- 风险与缓解：已知风险与预案

## 安全边界与失败语义（补充）

### 操作范围
- 仅允许在仓库根下的 `docs/context/` 目录进行读写
- 禁止绝对路径与路径穿越（`..`）

### 写入策略
- 干运行优先：先输出差异与创建清单，需确认后执行
- 不覆盖非空文件：对空文件可填充模板
- 需要替换时先生成备份，再写入

### 隐私与合规
- 不读取、不输出任何敏感信息（密钥、令牌、环境变量）
- 不上传或外传仓库数据

### 异常与返回
- 权限不足/磁盘空间不足/非法路径：标记为失败并给出修复建议
- 输出结构化报告，支持 `--json` 模式用于管道处理

## 语言策略

- 输出语言默认与用户最新输入语言保持一致：中文请求输出中文，英文请求输出英文；
- 当输入为混合语言时，优先匹配用户主要语言；必要时可在请求中显式注明语言（例如 “--lang en” 或 “请用英文输出”）；
- 结构化报告的字段名与关键词在英文模式下使用英文术语（file/issue/suggestion/OK/warning/fail）以便国际化工具链解析。
