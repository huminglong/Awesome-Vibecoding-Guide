# 综合Vibecoding指南

## 来自实践者，为实践者而作

本汇编源自真实的商业项目和数十万行AI辅助代码。可以通读全文，或将此仓库提供给您的AI代理进行摘要和问答。欢迎点赞和关注！⭐

另外，本指南基本上是我个人思想的集合——您可能不同意这些观点 :)

主要观点——只要遵循某些步骤——**用于vibecoding本身的LLM并不像宣传的那么重要**——因为通过正确的技术和整体设置，即使是比前沿模型"更差"的LLM也能交付您所需的一切（至少在Web开发方面）。

特别是在前沿模型**昂贵**的世界中——通常仅仅为了一个比开源（甚至免费）模型好一点点的模型而支付大量费用是没有意义的

---

## 从这里开始
- [简介 ✨](docs/introduction/README-zh.md)
- [快速入门 🚀](docs/quickstart/README-zh.md)

## 目录
- [开发工具 🛠️](docs/development-tools/README-zh.md)
  - **推荐工具**: Claude Code CLI • Droid CLI • Zed • Windsurf • Clavix (PRD生成器) • Warp • TRAE
  - **MCP服务器**: Context7 • DevTools • Sequential Thinking • Task Manager • Shadcn
  - **其他**: 兼容性指南
- [荣誉提及 🏆](docs/development-tools/honorable-mentions/README-zh.md)
  - **IDE原生**: Kilo Code (VSCode + CLI)
  - **免费且高性价比**: Qwen Coder • Gemini CLI • AmpCode • Octofriend
  - **原生集成**: GitHub Copilot
  - **我放弃的工具**: Traycer • GitHub Speckit • OpenSpec • Cline • Roo Code (VSCode插件)
- [AI模型提供商 🤖](docs/ai-model-providers/README-zh.md)
  - **主要提供商**: GLM Coding Plan • Synthetic.new
  - **荣誉提及**: 预算平台 (Chutes.ai • OpenRouter • nanoGPT • Factory AI) • 过于昂贵的选项 (Claude Subscription)
- [核心技术 🧰](docs/core-technologies-zh.md)
  - Astro • Tailwind CSS • Cloudflare Pages
- [托管工具 🌐](docs/hosting-tools/README-zh.md)
  - **免费且可扩展的托管**: Cloudflare Pages • Workers • R2存储 • D1数据库 • KV
  - 用于构建和部署生产应用的完整边缘平台
- [商业模式 💼](docs/business-model/README-zh.md)
  - **真实收入策略**: 为本地企业构建网站
  - 现实检验 • 模式 • 定价与经济学 • 价值主张
- [质量标准 ⭐](docs/quality-standards/README-zh.md)
  - **专业质量**: 可访问性 • SEO • 性能 • 设计一致性
  - **发布前检查清单**: 客户项目的质量门控
  - 防止"vibe coded"外观 • 证明溢价定价的合理性
- [上下文管理 🧠](docs/context-management/README-zh.md)
- [工作流程与流程 🔄](docs/workflow/README-zh.md)
  - **Git安全**: 保护您的工作免受AI编码灾难
  - **Git策略**: 用于开发和维护的两阶段工作流程
  - Phase 0 (Vibecoder准备) + Phase 1–4 深入探讨
- [掌握AI提示 🎯](docs/prompting/README-zh.md)
  - **基础**: 优秀提示的剖析 • 通用原则 • 反模式
  - **任务特定**: 功能开发 • 调试 • 重构 • 代码审查 • 测试
  - **高级**: 多步骤工作流程 • 提示链 • 错误恢复 • 优化
  - **质量导向**: 防止"vibe coded"输出 • 专业标准 • 客户就绪提示
  - **模板**: 17个常见场景的即用型提示模板
- [故障排除指南 🔧](docs/troubleshooting/README-zh.md)
  - **人类上下文问题**: 为什么95%的调试问题是上下文失败
  - **快速参考**: 40+ 症状 → 解决方案查找表
  - **紧急流程图**: AI产生垃圾 • 无法调试 • 高成本 • 破坏性变更
  - **常见问题**: AI行为 • 代码质量 • 工作流程瓶颈 • 工具问题
  - **集成**: 工作流程文档中的特定阶段故障排除
- [术语表 📚](docs/glossary-zh.md)
- [贡献 🤝](docs/contributing-zh.md)

---

## 🧭 学习路径

**Vibecoding新手:**

1. [简介](docs/introduction/README-zh.md) → 了解方法
2. [快速入门](docs/quickstart/README-zh.md) → Git和基本设置入门
3. [Phase 0](docs/workflow/phase-0-vibecoder-preparation-zh.md) → 工具选择和心态

**构建您的第一个项目:**

4. [Phase 1: 规划](docs/workflow/phase-1-planning-zh.md) → 项目结构和规范
5. [Phase 2: 开发](docs/workflow/phase-2-development-zh.md) → 实现工作流程
6. [核心技术](docs/core-technologies-zh.md) → 推荐技术栈

**扩展和优化:**

7. [开发工具](docs/development-tools/README-zh.md) → 高级工具配置
8. [AI模型提供商](docs/ai-model-providers/README-zh.md) → 优化AI辅助
9. [Phase 3-4](docs/workflow/) → 测试、调试和部署

**提升您的技能:**

10. [掌握AI提示](docs/prompting/README-zh.md) → 与AI有效沟通
11. [故障排除指南](docs/troubleshooting/README-zh.md) → 解决常见问题

**业务重点:**

12. [商业模式](docs/business-model/README-zh.md) → 通过vibecoding赚取真实收入
13. [质量标准](docs/quality-standards/README-zh.md) → 专业质量证明溢价定价的合理性
14. [托管工具](docs/hosting-tools/README-zh.md) → 高性价比基础设施
15. [上下文管理](docs/context-management/README-zh.md) → 高效工作流程

**快速参考:**
- [交叉参考指南](docs/cross-reference-zh.md) → 在相关主题之间导航并找到连接
- [术语表](docs/glossary-zh.md) → 关键术语和定义