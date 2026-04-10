# Context Architect Skill

## 简介（中文）
- **目的**：通过「6-Doc Protocol」将代码改动与项目上下文深度绑定，作为一个智能体的外挂记忆，在开发过程中主动维护需求、架构与实施计划，保持代码与文档同步推进。
- **技能入口**：`skills/context-architect/SKILL.md`。
- **许可**：MIT（见 `LICENSE.txt`）。

## 安装（中文）

```bash
npx skillsadd sunxiaobin89/context-architect-skill
```

## 最佳实践：如何配合 Agent 高效协同（中文）
本技能旨在提升大模型（Agent）开发复杂项目的成功率，防止出现上下文遗忘和架构腐化。为获得最佳体验，建议用户采取以下配合机制：

**在项目根目录创建 `.traerules` 或 `.cursorrules` 文件，写入以下系统级提示词，形成开发默契：**
> "本项目采用 6-Doc 协议进行开发。对于新功能开发、架构调整或复杂重构，请在编写代码的同时，主动调用 Context Architect 技能更新 `docs/context/` 目录下的相关 Markdown 文档，保持文档与代码同步推进。对于极小改动（如修复拼写错误、代码解释、格式化、局部变量重命名等），可直接执行，无需更新文档。"

## 触发与核心能力（中文）
- **常见触发短语**：
  - 初始化项目上下文
  - 校验 6-Doc 完整性
  - 对齐实现与规范
  - 开发新功能 / 更新项目文档
- **核心能力概述**：
  - `init`：初始化 `docs/context/` 目录及 6 大核心文档模板（默认先预览，确认后写入）。
  - `validate`：校验六文档的存在性、非空性及必填章节，输出诊断报告。
  - `update`：**文档协同策略**。在处理复杂开发任务时，主动起草或更新需求与实施计划（PRD、APP_FLOW、IMPLEMENTATION_PLAN），并与代码编写无缝协同。
  - `align`：自动读取技术栈（`TECH_STACK.md`）与前端规范（`FRONTEND_GUIDELINES.md`）等，为当前的代码实现提供对齐建议。

## 安全与边界约束（中文）
- 仅允许在当前仓库根目录下的 `docs/context/` 目录进行读写；拒绝任何路径穿越（`..`）。
- 默认预览模式：在进行实质性覆盖操作前，向用户展示差异与拟定修改，需确认后执行。不覆盖已有非空文件。
- 隐私合规：绝不读取或输出敏感信息（密钥、令牌、环境变量），严禁上传仓库数据。

---

## Overview (English)
- **Purpose**: Bind code changes deeply to project context via the "6-Doc Protocol". Acts as an external memory for the Agent, proactively maintaining requirements, architecture, and implementation plans during the development process to keep code and documentation in sync.
- **Entry**: `skills/context-architect/SKILL.md`.
- **License**: MIT (see `LICENSE.txt`).

## Installation (English)

```bash
npx skillsadd sunxiaobin89/context-architect-skill
```

## Best Practices: Collaborating with the Agent (English)
This skill is designed to improve the success rate of complex project development with AI agents, preventing context amnesia and architectural decay. For the best experience, we recommend setting up the following workflow:

**Create a `.traerules` or `.cursorrules` file in your project root with the following system prompt:**
> "This project uses the 6-Doc Protocol. For new features, architectural changes, or complex refactoring, please actively invoke the Context Architect skill to update the relevant Markdown files in the `docs/context/` directory WHILE writing the code. Keep documentation and code advancing synchronously. For trivial tasks (e.g., fixing typos, explaining code, formatting, renaming local variables), you may execute them directly without updating documents."

## Triggers & Core Capabilities (English)
- **Common Triggers**:
  - Initialize project context
  - Validate 6-Doc completeness
  - Align implementation with specs
  - Implement feature / Update context
- **Core Capabilities**:
  - `init`: Initialize the `docs/context/` directory and the 6 core document templates (preview by default, write on confirmation).
  - `validate`: Check the existence, non-emptiness, and required sections of the 6 documents, outputting a diagnostic report.
  - `update`: **Collaborative Doc Strategy**. When handling complex tasks, proactively draft or update requirements and implementation plans (PRD, APP_FLOW, IMPLEMENTATION_PLAN) seamlessly alongside code generation.
  - `align`: Automatically read tech stack (`TECH_STACK.md`) and frontend guidelines (`FRONTEND_GUIDELINES.md`) to provide alignment suggestions for current code implementation.

## Safety Constraints (English)
- Read/write operations are strictly restricted to the `docs/context/` directory at the repo root. Absolute paths and path traversal (`..`) are rejected.
- Preview mode by default: Shows a diff and proposed changes before any substantial overwrite operations, requiring confirmation to proceed. Never overwrites existing non-empty files without consent.
- Privacy compliance: Never reads or outputs sensitive information (keys, tokens, env vars), and strictly prohibits uploading repository data.
