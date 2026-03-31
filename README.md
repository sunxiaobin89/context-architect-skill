# Context Architect Skill

## 简介（中文）
- 目的：通过「6-Doc Protocol」将代码改动与项目上下文强绑定，确保实现前先补齐文档，编码过程与规范对齐。
- 技能入口：仓库根目录的 `SKILL.md`（已含元数据、触发短语、操作流程、语言策略与安全边界）。
- 许可：MIT（见 `LICENSE.txt`）。

## 安装（中文）
- 推荐方式（示例，替换 `<owner/repo>` 为你的公开仓库地址）：

```bash
npx skillsadd <owner/repo>
```

## 触发与能力（中文）
- 触发短语（中文）：
  - 初始化项目上下文
  - 校验 6-Doc 完整性
  - 对齐实现与规范
- 触发短语（英文）：
  - Initialize project context
  - Validate 6-Doc completeness
  - Align implementation with specs
- 能力概述：
  - init：创建 `docs/context/` 与六个文档模板（默认干运行，确认后写入）
  - validate：检查六文档的存在性/非空性/必填章节，输出结构化报告（支持 `--json`）
  - align：读取 `TECH_STACK.md` 与 `FRONTEND_GUIDELINES.md` 等，对齐实现并输出偏差与建议（只读）

## 语言策略（中文）
- 输出语言默认跟随用户最新输入语言：中文请求输出中文；英文请求输出英文。
- 混合语言时可在请求中显式指定（例如“请用英文输出”或 `--lang en`）。
- 在英文模式下，结构化报告使用英文字段名：`file` / `issue` / `suggestion` / `OK` / `warning` / `fail`。

## 安全边界（中文）
- 仅允许在仓库根下的 `docs/context/` 目录进行读写；拒绝绝对路径与路径穿越（`..`）。
- 默认干运行：先输出差异与创建清单，需确认后执行；不覆盖非空文件；必要时先备份再写入。
- 不读取或输出敏感信息（密钥、令牌、环境变量）；不上传或外传仓库数据。
- 异常与返回：权限不足/磁盘空间不足/非法路径将标记为失败并给出修复建议；支持 `--json` 报告。

## 自测清单（中文）
1. 中文触发：使用“初始化项目上下文”进行干运行，确认输出为中文且不写入文件。
2. 英文触发：使用“Initialize project context”进行干运行，确认输出为英文且术语为英文字段名。
3. validate：在 `docs/context/` 仅存在部分文档时运行“校验 6-Doc 完整性”，报告应列出缺失/缺段并给出建议。
4. align：在项目存在 `package.json` 与前端代码时运行“对齐实现与规范”，确认只读输出建议，不直接修改代码。
5. JSON 输出：在英文触发下使用 `--json`，确认字段为 `file/issue/suggestion` 等，便于工具链解析。
6. 安全边界：尝试仓库外路径（应拒绝并报告原因）；尝试覆盖非空文件（应拒绝并提示备份策略）。

## JSON 输出示例（中文/英文）

### validate 报告（中文）

```json
{
  "reportType": "validate",
  "lang": "zh",
  "timestamp": "2026-03-31T10:25:00Z",
  "files": [
    {
      "file": "docs/context/TECH_STACK.md",
      "status": "fail",
      "issues": [
        {
          "id": "tech.version.missing",
          "section": "Versions",
          "severity": "high",
          "message": "缺少版本约束章节",
          "suggestion": "为核心依赖补充明确的 semver 版本与兼容策略"
        }
      ]
    },
    {
      "file": "docs/context/APP_FLOW.md",
      "status": "warning",
      "issues": [
        {
          "id": "flow.journey.incomplete",
          "section": "User Journeys",
          "severity": "medium",
          "message": "用户旅程描述不完整",
          "suggestion": "补充关键步骤与异常分支的状态迁移"
        }
      ]
    },
    {
      "file": "docs/context/PRD.md",
      "status": "ok",
      "issues": []
    }
  ],
  "summary": {
    "total": 6,
    "ok": 3,
    "warning": 2,
    "fail": 1
  }
}
```

### align 报告（英文）

```json
{
  "reportType": "align",
  "lang": "en",
  "timestamp": "2026-03-31T10:30:00Z",
  "checks": [
    {
      "category": "tech_stack",
      "item": "React version",
      "spec": ">=18.3.0",
      "current": "18.2.0",
      "status": "fail",
      "suggestion": "Upgrade react to 18.3+ and run compatibility checks"
    },
    {
      "category": "frontend_guidelines",
      "item": "Component naming",
      "spec": "PascalCase components",
      "current": "some_component",
      "status": "warning",
      "suggestion": "Rename to SomeComponent and update imports"
    },
    {
      "category": "backend_structure",
      "item": "Idempotency",
      "spec": "PUT/POST must be idempotent for retries",
      "current": "not defined",
      "status": "warning",
      "suggestion": "Document idempotency rules and add deduplication on server"
    }
  ],
  "summary": {
    "ok": 5,
    "warning": 2,
    "fail": 1
  }
}
```

### init 干运行报告（中文）

```json
{
  "reportType": "init-dry-run",
  "lang": "zh",
  "timestamp": "2026-03-31T10:20:00Z",
  "plannedCreates": [
    "docs/context/",
    "docs/context/PRD.md",
    "docs/context/APP_FLOW.md",
    "docs/context/TECH_STACK.md",
    "docs/context/FRONTEND_GUIDELINES.md",
    "docs/context/BACKEND_STRUCTURE.md",
    "docs/context/IMPLEMENTATION_PLAN.md"
  ],
  "conflicts": [
    {
      "file": "docs/context/PRD.md",
      "reason": "already_exists_non_empty",
      "action": "skip"
    }
  ],
  "requireConfirmation": true
}
```

---

## Overview (English)
- Purpose: Enforce the “6-Doc Protocol” to bind code changes to project context, ensuring documentation completeness before implementation and alignment during coding.
- Entry: Root `SKILL.md` with metadata, triggers, workflows, language policy, and safety constraints.
- License: MIT (`LICENSE.txt`).

## Installation (English)
- Recommended (replace `<owner/repo>` with your public repo):

```bash
npx skillsadd <owner/repo>
```

## Triggers & Capabilities (English)
- Triggers:
  - Initialize project context
  - Validate 6-Doc completeness
  - Align implementation with specs
- Capabilities:
  - init: Create `docs/context/` and six doc templates (dry-run by default, write on confirmation)
  - validate: Check existence/non-empty/required sections; produce structured report (`--json` supported)
  - align: Read `TECH_STACK.md` / `FRONTEND_GUIDELINES.md`, verify alignment; output deviations and suggestions (read-only)

## Language Policy (English)
- Output language mirrors user’s latest input: Chinese → Chinese output; English → English output.
- For mixed contexts, explicitly request language (e.g., “use English output” or `--lang en`).
- In English mode, structured report uses English fields: `file` / `issue` / `suggestion` / `OK` / `warning` / `fail`.

## Safety Constraints (English)
- File operations restricted to `docs/context/` at repo root; reject absolute paths and path traversal (`..`).
- Dry-run first: show diff and creation list; write on confirmation; never overwrite non-empty files; backup before replacement.
- No sensitive data reading/output; no external data upload.
- Errors return structured diagnostics and support `--json`.

## Self-Test Checklist (English)
1. Chinese trigger: “初始化项目上下文” dry-run produces Chinese output without writes.
2. English trigger: “Initialize project context” dry-run produces English output with English field names.
3. validate: With partial docs under `docs/context/`, report missing/sections and suggestions.
4. align: With `package.json` and frontend code, report alignment suggestions only; do not modify code.
5. JSON output: With `--json`, confirm fields `file/issue/suggestion` etc. for tooling.
6. Safety: Attempt outside-repo path (should reject); attempt overwriting non-empty files (should reject and suggest backup).
