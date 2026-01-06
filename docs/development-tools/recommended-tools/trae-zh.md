# TRAE — 统一IDE + SOLO自动化

## 概述
Trae是一个具有双模式（IDE + SOLO）的AI驱动开发IDE。截至2025年11月，SOLO模式对Pro用户广泛可用，实现从PRD到预览和发布的端到端自动化。Trae对于独立开发人员非常有效，归功于一次性执行和简化的工作流程。

## 主要优势
- SOLO模式广泛可用：一次性、端到端执行（PRD → 任务 → 代码 → 预览 → 发布）用于快速原型设计。
- 简化工作流程：内置代理（SOLO Coder、SOLO Builder）、集成工具、最少的协调开销。
- 最先进的模型：内置`Gemini-3-Pro-Preview`和其他高级模型；支持BYOK自定义模型。
- 卓越价值：Pro计划每月$10（首月$3），具有慷慨的配额和无限制的慢速请求。

## 模式：何时使用SOLO与IDE
- 使用SOLO进行端到端脚手架、自动规划/任务编排和一次性应用程序构建。
- 使用IDE进行日常编码、受控重构和具有手动审查的多步骤编辑。

## 定价和配额（当前）
- 计划：免费和Pro（`USD $10`/月；首月`USD $3`；提供年度折扣）。
- 配额（Pro）：每月`600次快速请求`和`无限制的慢速请求`；快速请求队列≤15秒。
- 额外包：`$3 → +100次快速`，`$7 → +300次快速`，`$12 → +600次快速`（30天有效期）。
- 模型：高级包括`Gemini-3-Pro-Preview`/`(200k)`、`Gemini-2.5-Pro`、GPT-4o/4.1、Grok-4、Kimi K2、DeepSeek V3.1；高级包括`Gemini-2.5-Flash`。
- 注意：SOLO工作流程快速消耗快速请求；Max模式将基于令牌的成本转换为快速请求使用。

## 性能和限制
- 上下文限制取决于模型（例如，Gemini 3预览变体上的200k）。
- 端到端SOLO管道自然增加快速请求消耗；为峰值计划额外包。
- 最适合中小型项目和快速迭代；非常大的monorepos仍然需要仔细的上下文管理。

## 现实世界示例
- 一次性Web应用程序：SOLO Builder生成PRD、任务、代码、预览，并支持发布/部署。
- 复杂重构：SOLO Coder与计划模式协调多步骤更改；差异视图有助于验证。
- 预算IDE工作流程：使用IDE模式和`Gemini-3-Pro-Preview`进行推理繁重的编辑；利用慢速请求进行批量操作。

## 比较优势
- 超过Kilo Code：更强的端到端SOLO自动化、内置代理、每个价格的慷慨配额。
- 超过Qwen CLI：丰富的IDE集成和高级模型；Qwen在零成本启动方面表现出色。
- 超过Gemini CLI：内聚的IDE + 自动化；Gemini CLI最适合API/CLI实验。
- 超过Octofriend：集成的规划、预览/发布管道；Octofriend在MCP优先CLI工作流程方面最强。
- 超过Copilot：更多的自动化和慷慨的慢速请求；Copilot具有固定的每月提示上限。

## 参考
- SOLO可用性：https://docs.trae.ai/ide/changelog?_lang=en
- 计费：https://docs.trae.ai/ide/billing
- 模型：https://docs.trae.ai/ide/models
- Max模式：https://www.trae.ai/changelog
- Gemini API上下文：https://ai.google.dev/gemini-api/docs/pricing

返回: [开发工具](../README-zh.md)