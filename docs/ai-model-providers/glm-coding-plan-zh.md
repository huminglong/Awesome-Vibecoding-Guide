# GLM Coding Plan — 主要LLM 🧠

## 定价
- Lite：$3/月（首次月/季度/年预付），然后$6/月
- Pro：$15/月（首次月/季度/年预付），然后$30/月
- Max：$30/月（首次月/季度/年预付），然后$60/月
- 限制的实用性：即使在Pro和Max计划上有2-5个并行代理，也很难达到限制
- **Lite**：每5小时120次提示
- **Pro**：每5小时600次提示
- **Max**：每5小时2,400次提示
- 注意：我目前在Max计划上，但过去在Pro计划上超过一个月也没有达到限制
- **重要集成工具**：Claude Code和Droid CLI在大多数情况下在效率方面通常相当。主要区别：
  - **Claude Code**：从我的经验来看，处理和迭代代码更快
  - **Droid CLI**：对于复杂架构，整体规划模式更好
  - **提示**：可以使用[Clavix](https://clavix.dev)进行提示优化来改进两个工具的规划能力
- 可选：10%折扣推荐链接（披露）：<https://z.ai/subscribe?ic=CUEFJ9ALMX>

## 为什么选择GLM？
- 在保持质量在现代、商业LLM水平的同时成本最低
- 最先进的开源模型
- 可以在10-15分钟内自主解决错误而无需干预
- 处理复杂项目（Next.js + 复杂数据库 + 后端）
- 与现代Web开发栈配合得很好（Svelte、React、Next.js、Astro、D1数据库、Cloudflare Wrangler文件等）

## 成本比较
- Claude Code Max20：每月€200+（含欧盟增值税）
- Codex Pro：欧盟每月含增值税€229
- 考虑到GLM Coding Plan有效地在每5小时内给您大量令牌，甚至不谈每月，大多数替代方案将比GLM更昂贵。

## 推荐工作流程集成

**最适合用于：**
- [阶段1：规划](../workflow/phase-1-planning-zh.md) - PRD细化和架构讨论
- [阶段2：开发](../workflow/phase-2-development-zh.md) - 代码生成和调试
- [核心技术实施](../core-technologies-zh.md) - Astro + Tailwind开发

**预算考虑：**
- 在升级之前与[免费替代方案](./honorable-mentions/README-zh.md)进行比较
- 查看[阶段0工具选择](../workflow/phase-0-vibecoder-preparation-zh.md)以了解升级时机

**集成设置：**
- [Droid CLI](../development-tools/recommended-tools/droid-cli-zh.md) — 规划和架构的最高效集成
- [Claude Code CLI](../development-tools/recommended-tools/claude-code-cli-zh.md) — 最快的工作组合
- [Context7 MCP](../development-tools/mcp-servers/context7-mcp-zh.md) — 开发期间的文档访问

**按使用情况的工作流程阶段：**

**阶段0（准备）：**
- 与[免费替代方案](../workflow/phase-0-vibecoder-preparation-zh.md#free-option-perfect-for-getting-started)进行评估
- 当达到限制时从[预算选项](../workflow/phase-0-vibecoder-preparation-zh.md#budget-option-professional-grade)升级

**阶段1（规划）：**
- 用于PRD细化和功能分解
- 与[OpenSpec CLI](../development-tools/recommended-tools/openspec-cli-zh.md)结合进行结构化规划
- [核心技术](../core-technologies-zh.md)栈的架构讨论

**阶段2（开发）：**
- 日常编码辅助和调试
- [DevTools MCP](../development-tools/mcp-servers/devtools-mcp-zh.md)集成以进行自动测试
- Astro + Tailwind项目的组件生成

**阶段3（测试和调试）：**
- 错误分析和解决
- 代码审查和优化
- 与[测试工作流程](../workflow/phase-3-testing-debugging-zh.md)集成

返回: [AI模型提供商](./README-zh.md)