# 交叉参考指南

在文档中导航相关主题。本指南显示概念如何连接以及在哪里找到相关信息。

## 主要概念图

### AI辅助开发工作流程

```
上下文管理
├─→ 基本设置（context-management/README-zh.md）
├─→ 高级场景（context-management/advanced-scenarios-zh.md）
└─→ 故障排除（troubleshooting/recovery-patterns-zh.md）

提示和迭代
├─→ 基本提示（prompting/README-zh.md）
├─→ 常见提示速查表（development-tools/cheat-sheets/common-prompts-zh.md）
└─→ 工作流程集成（workflow/README-zh.md）

质量保证
├─→ 可访问性（quality-standards/accessibility-zh.md）
├─→ SEO（quality-standards/seo-zh.md）
├─→ 性能（quality-standards/performance-zh.md）
└─→ 测试（workflow/phase-3-testing-debugging-zh.md）
```

### 业务和项目管理

```
项目设置
├─→ 哲学（introduction/philosophy-zh.md）
├─→ 核心技术（core-technologies-zh.md）
├─→ 技术栈决策（tech-stacks/README-zh.md）
└─→ 工具选择（development-tools/decision-matrix-zh.md）

客户工作
├─→ 客户管理（business-model/client-management-zh.md）
├─→ 维护策略（business-model/maintenance-strategy-zh.md）
├─→ ROI跟踪（business-model/roi-tracking-zh.md）
└─→ 质量标准（quality-standards/README-zh.md）
```

## 主题索引

### 按开发阶段

**规划和设置：**
- [哲学和原则](./introduction/philosophy-zh.md)
- [核心技术概述](./core-technologies-zh.md)
- [工具选择矩阵](./development-tools/decision-matrix-zh.md)
- [上下文管理设置](./context-management/README-zh.md)

**实施：**
- [阶段1：规划](./workflow/phase-1-planning-zh.md)
- [阶段2：实施](./workflow/phase-2-implementation-zh.md)
- [常见提示](./development-tools/cheat-sheets/common-prompts-zh.md)
- [Git策略](./workflow/git-strategies-zh.md)

**测试和部署：**
- [阶段3：测试和调试](./workflow/phase-3-testing-debugging-zh.md)
- [性能标准](./quality-standards/performance-zh.md)
- [可访问性测试](./quality-standards/accessibility-zh.md)
- [SEO验证](./quality-standards/seo-zh.md)

**维护：**
- [维护策略](./business-model/maintenance-strategy-zh.md)
- [恢复模式](./troubleshooting/recovery-patterns-zh.md)
- [ROI跟踪](./business-model/roi-tracking-zh.md)

### 按技术

**Astro：**
- [核心技术](./core-technologies-zh.md)
- [性能优化](./quality-standards/performance-zh.md)（Astro特定部分）
- [可访问性模式](./quality-standards/accessibility-zh.md)（Astro组件）
- [SEO设置](./quality-standards/seo-zh.md)（Astro模式）
- [国际化](./tech-stacks/internationalization-zh.md)

**Cloudflare：**
- [Cloudflare Pages](./hosting-tools/cloudflare-pages-zh.md)
- [Cloudflare CLI速查表](./development-tools/cheat-sheets/cloudflare-cli-zh.md)
- [核心技术](./core-technologies-zh.md)（D1、KV、R2部分）

**Git：**
- [Git策略](./workflow/git-strategies-zh.md)
- [Git命令速查表](./development-tools/cheat-sheets/git-commands-zh.md)
- [恢复模式](./troubleshooting/recovery-patterns-zh.md)

**AI工具：**
- [工具决策矩阵](./development-tools/decision-matrix-zh.md)
- [上下文管理](./context-management/README-zh.md)
- [高级场景](./context-management/advanced-scenarios-zh.md)

### 按问题类型

**性能问题：**
1. [性能标准](./quality-standards/performance-zh.md) - Core Web Vitals优化
2. [测试和调试](./workflow/phase-3-testing-debugging-zh.md) - 性能选项卡使用
3. [调试命令](./development-tools/cheat-sheets/debugging-commands-zh.md) - 性能分析