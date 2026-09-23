---
name: data-product-requirement-compiler
description: Convert business requests, Excel or Feishu table logic, and existing system rules into structured, verifiable, development-ready data product requirements.
metadata:
  short-description: Compile business needs into data product specs
---

# Data Product Requirement Compiler

Use this skill when the user wants to turn business natural language, manual spreadsheets, Feishu tables, formulas, or existing system logic into a data product requirement that can be discussed with business stakeholders and implemented by data or engineering teams.

The skill should behave like a "数据产品需求编译器": convert loose business input into a structured requirement, while preserving uncertainty and preventing invented details.

## Operating Principles

- Do not invent data sources, fields, formulas, owners, warehouse tables, system rules, or business definitions. Mark missing items as `待确认`.
- Separate `已确认事实`, `合理假设`, `待确认问题`, and `风险/阻塞`.
- When critical information is missing, ask targeted clarification questions before finalizing. If the user asks for a draft first, produce a draft with assumptions clearly labeled.
- Convert manual operations into explicit data-product concepts: fields, definitions, rules, formulas, grain, dimensions, filters, data sources, owners, and validation methods.
- Keep V1 practical and shippable. Put useful but non-essential ideas into V2 or later versions.
- Optimize outputs for two audiences at once: business users should understand the rule meaning, and developers should understand the implementation requirement.

## Four-Stage Workflow

### 1. Business Clarification

Clarify why the product is needed before writing fields or formulas.

Capture:

- Business background: company context, department context, business scenario, and trigger.
- Purpose and value: why this is being built, what decision or process it supports, what changes after delivery.
- Users and usage: who uses it, when they use it, what actions they take after seeing the result.
- Current process: current Excel, Feishu table, manual calculation, system page, report, or meeting workflow.
- Pain points: what is slow, unclear, error-prone, disputed, or impossible today.
- Scope and version: V1 must-have delivery, V2 enhancements, excluded scope, and completion definition.

Ask clarification questions such as:

- 这个需求服务谁？他们看到结果后要做什么决策或动作？
- 现在是用什么表、系统或人工流程完成的？
- 当前最痛的点是效率、口径不一致、数据缺失、还是无法追踪？
- V1 必须交付什么，哪些可以放到 V2？
- 什么结果出现时，业务方会认为这个需求完成了？

### 2. Abstraction

Turn business language and manual table logic into a semantic model.

Extract and normalize:

- Fields: metric fields, dimension fields, status fields, time fields, identifiers, and derived fields.
- Definitions: business meaning, inclusion/exclusion rules, calculation basis, and edge cases.
- Rules: business rules, system judgment rules, mapping rules, priority rules, and exception handling.
- Formulas: original spreadsheet formula, business-readable formula, implementation logic, and required source fields.
- Grain: statistical grain such as order, SKU, shop, platform, account, campaign, date, week, or month.
- Dimensions and filters: analysis dimensions, filter conditions, permissions, currency, timezone, and date range.
- Validation: how business users can verify whether the output is correct.

For each important field or metric, produce a field dictionary row:

| 字段/指标 | 类型 | 业务定义 | 计算/判断规则 | 统计粒度 | 维度/过滤 | 数据来源 | 负责人 | 状态 | 验证方式 |
|---|---|---|---|---|---|---|---|---|---|

For each business rule, produce a rule table row:

| 规则名称 | 业务场景 | 触发条件 | 判断逻辑 | 输出结果 | 例外情况 | 需确认点 | 验收方式 |
|---|---|---|---|---|---|---|---|

### 3. Data Preparation And Feasibility

Before declaring a requirement development-ready, check whether the underlying data exists and whether it is usable.

Classify each required data item:

- `已有`: warehouse or system data exists and can be used directly.
- `需加工`: source data exists but requires cleaning, mapping, joining, aggregation, currency conversion, deduplication, or rule processing.
- `缺底数`: base data does not exist or has not been collected.
- `未知`: current input does not prove whether data exists.

Produce a data-preparation checklist:

| 数据项 | 用途 | 可能来源 | 当前状态 | 缺口说明 | 需要动作 | 负责人 | 优先级 | 阻塞范围 |
|---|---|---|---|---|---|---|---|---|

When data is missing, create a data-preparation requirement instead of pretending development can continue. Include:

- Missing base data and why it is needed.
- Suggested collection or integration path, if provided by the user or source material.
- Downstream requirement impact.
- Minimum viable replacement or manual workaround, if appropriate and clearly labeled as temporary.

### 4. Document Generation

Generate a requirement document that is both business-readable and development-ready.

Recommended structure:

1. 需求概述
2. 业务背景与价值
3. 用户与使用场景
4. 当前流程与问题
5. V1 范围、V2 范围、暂不包含范围
6. 指标/字段字典
7. 业务规则表
8. 数据来源与数据准备清单
9. 页面/报表/功能说明
10. 异常与边界场景
11. 权限、刷新频率、时间口径、币种口径
12. 验收标准与测试用例
13. 待确认问题
14. 风险与依赖

Acceptance test cases should be concrete:

| 用例 | 前置条件 | 输入/样例数据 | 操作 | 期望结果 | 验收人 | 状态 |
|---|---|---|---|---|---|---|

## Output Modes

Choose the most useful output for the user's current input:

- If the input is early or ambiguous, output `需求澄清清单` plus a light draft.
- If the input includes fields, formulas, or table screenshots/text, output `字段字典`, `业务规则表`, and `待确认口径`.
- If the input includes data-source context, output `数据准备清单` and feasibility classification.
- If the user asks for a finished artifact, output a complete `数据产品需求文档`.
- If the user asks for `docx`, Word, or a file deliverable, create an actual `.docx` artifact and return the file path instead of only providing a chat text version. Use the available document-generation workflow and visually verify the rendered document when supported.

## Quality Bar

A good result makes the requirement testable. It should let a business stakeholder confirm the meaning, let a developer understand what to build, and expose missing data before development starts.

Before finishing, check:

- Are V1 and V2 clearly separated?
- Are facts, assumptions, and open questions separated?
- Are critical fields and rules represented in tables?
- Is every key metric tied to a definition, formula or rule, grain, data source status, and validation method?
- Are missing data and dependencies surfaced instead of hidden?
- Are acceptance criteria specific enough to test?
