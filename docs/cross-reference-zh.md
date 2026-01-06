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

**可访问性问题：**
1. [可访问性标准](./quality-standards/accessibility-zh.md) - WCAG合规指南
2. [测试和调试](./workflow/phase-3-testing-debugging-zh.md) - 可访问性测试
3. [SEO](./quality-standards/seo-zh.md) - 可访问性/SEO重叠

**SEO问题：**
1. [SEO标准](./quality-standards/seo-zh.md) - 完整SEO指南
2. [性能标准](./quality-standards/performance-zh.md) - Core Web Vitals影响SEO
3. [国际化](./tech-stacks/internationalization-zh.md) - 多语言SEO

**上下文/AI问题：**
1. [上下文管理](./context-management/README-zh.md) - 基本设置
2. [高级场景](./context-management/advanced-scenarios-zh.md) - 复杂项目
3. [恢复模式](./troubleshooting/recovery-patterns-zh.md) - 上下文丢失恢复
4. [常见提示](./development-tools/cheat-sheets/common-prompts-zh.md) - 更好的提示

**Git问题：**
1. [恢复模式](./troubleshooting/recovery-patterns-zh.md) - Git恢复策略
2. [Git命令速查表](./development-tools/cheat-sheets/git-commands-zh.md) - 命令参考
3. [Git策略](./workflow/git-strategies-zh.md) - 最佳实践

**部署问题：**
1. [Cloudflare CLI速查表](./development-tools/cheat-sheets/cloudflare-cli-zh.md) - Wrangler命令
2. [Cloudflare Pages](./hosting-tools/cloudflare-pages-zh.md) - 部署指南
3. [恢复模式](./troubleshooting/recovery-patterns-zh.md) - 紧急回滚

### 按用户类型

**初学者：**
1. [哲学](./introduction/philosophy-zh.md) - 从这里开始
2. [核心技术](./core-technologies-zh.md) - 技术栈概述
3. [工具决策矩阵](./development-tools/decision-matrix-zh.md) - 选择免费工具
4. [上下文管理](./context-management/README-zh.md) - 基本设置
5. [工作流程概述](./workflow/README-zh.md) - 开发过程

**自由职业者：**
1. [客户管理](./business-model/client-management-zh.md) - 客户关系
2. [ROI跟踪](./business-model/roi-tracking-zh.md) - 跟踪盈利能力
3. [维护策略](./business-model/maintenance-strategy-zh.md) - 持续收入
4. [质量标准](./quality-standards/README-zh.md) - 交付专业工作
5. [工具决策矩阵](./development-tools/decision-matrix-zh.md) - 性价比高的工具

**代理机构：**
1. [高级上下文场景](./context-management/advanced-scenarios-zh.md) - 团队协作
2. [工具决策矩阵](./development-tools/decision-matrix-zh.md) - 团队工具选择
3. [质量标准](./quality-standards/README-zh.md) - 客户交付
4. [工作流程](./workflow/README-zh.md) - 团队流程

## 经常需要的组合

### "设置新的Astro + Cloudflare项目"
1. [核心技术](./core-technologies-zh.md)
2. [上下文管理](./context-management/README-zh.md)
3. [Cloudflare Pages](./hosting-tools/cloudflare-pages-zh.md)
4. [Git策略](./workflow/git-strategies-zh.md)

### "改进网站性能"
1. [性能标准](./quality-standards/performance-zh.md)
2. [SEO标准](./quality-standards/seo-zh.md)（Core Web Vitals部分）
3. [测试和调试](./workflow/phase-3-testing-debugging-zh.md)
4. [调试命令](./development-tools/cheat-sheets/debugging-commands-zh.md)

### "使网站可访问"
1. [可访问性标准](./quality-standards/accessibility-zh.md)
2. [测试和调试](./workflow/phase-3-testing-debugging-zh.md)（可访问性测试）
3. [客户管理](./business-model/client-management-zh.md)（解释价值）

### "为搜索引擎优化"
1. [SEO标准](./quality-standards/seo-zh.md)
2. [性能标准](./quality-standards/performance-zh.md)（Core Web Vitals）
3. [可访问性标准](./quality-standards/accessibility-zh.md)（SEO重叠）
4. [国际化](./tech-stacks/internationalization-zh.md)（如果是多语言）

### "从错误中恢复"
1. [恢复模式](./troubleshooting/recovery-patterns-zh.md)
2. [Git命令速查表](./development-tools/cheat-sheets/git-commands-zh.md)
3. [调试命令](./development-tools/cheat-sheets/debugging-commands-zh.md)

### "添加多语言支持"
1. [国际化](./tech-stacks/internationalization-zh.md)
2. [SEO标准](./quality-standards/seo-zh.md)（Hreflang部分）
3. [客户管理](./business-model/client-management-zh.md)（定价i18n）

### "选择性价比高的工具"
1. [工具决策矩阵](./development-tools/decision-matrix-zh.md)
2. [哲学](./introduction/philosophy-zh.md)
3. [ROI跟踪](./business-model/roi-tracking-zh.md)

### "处理复杂项目结构"
1. [高级上下文场景](./context-management/advanced-scenarios-zh.md)
2. [上下文管理](./context-management/README-zh.md)
3. [Git策略](./workflow/git-strategies-zh.md)

## 快速参考链接

### 速查表（快速复制粘贴）
- [Git命令](./development-tools/cheat-sheets/git-commands-zh.md)
- [Cloudflare CLI](./development-tools/cheat-sheets/cloudflare-cli-zh.md)
- [常见提示](./development-tools/cheat-sheets/common-prompts-zh.md)
- [调试命令](./development-tools/cheat-sheets/debugging-commands-zh.md)

### 标准（质量检查清单）
- [可访问性](./quality-standards/accessibility-zh.md)
- [SEO](./quality-standards/seo-zh.md)
- [性能](./quality-standards/performance-zh.md)

### 工作流程（流程指南）
- [阶段1：规划](./workflow/phase-1-planning-zh.md)
- [阶段2：实施](./workflow/phase-2-implementation-zh.md)
- [阶段3：测试和调试](./workflow/phase-3-testing-debugging-zh.md)
- [Git策略](./workflow/git-strategies-zh.md)

### 业务（客户和收入）
- [客户管理](./business-model/client-management-zh.md)
- [维护策略](./business-model/maintenance-strategy-zh.md)
- [ROI跟踪](./business-model/roi-tracking-zh.md)

## 学习路径

### 路径1：完全初学者
```
1. 介绍 → 哲学
2. 核心技术
3. 工具决策矩阵（选择免费工具）
4. 上下文管理
5. 工作流程概述
6. 阶段1：规划（从小项目开始）
7. 阶段2：实施
8. 阶段3：测试
9. 速查表（作为参考）
```

### 路径2：有经验的开发者（AI新手）
```
1. 哲学（理解方法）
2. 上下文管理（关键！）
3. 工具决策矩阵（根据预算选择）
4. 工作流程概述
5. 常见提示（有效AI使用）
6. 质量标准（保持专业性）
7. 高级场景（需要时）
```

### 路径3：开始客户工作的自由职业者
```
1. 哲学
2. 业务模型 → 客户管理
3. 质量标准（全部三个）
4. 工作流程（所有阶段）
5. 维护策略
6. ROI跟踪
7. 工具决策矩阵（性价比选择）
```

### 路径4：代理机构/团队
```
1. 工具决策矩阵（团队工具）
2. 高级上下文场景（团队协作）
3. 质量标准（交付内容）
4. 工作流程（团队流程）
5. Git策略（团队约定）
6. 业务模型（所有文档）
```

## 接下来去哪里

**刚完成一个部分？以下是相关的：**

**阅读哲学？** → 下一步：核心技术，工具决策矩阵

**阅读核心技术？** → 下一步：上下文管理，工作流程概述

**阅读上下文管理？** → 下一步：工作流程阶段1，常见提示

**阅读质量标准？** → 下一步：测试和调试，客户管理

**阅读工作流程？** → 下一步：Git策略，测试和调试

**阅读业务模型？** → 下一步：质量标准，ROI跟踪

**阅读速查表？** → 工作时保持打开作为参考

**阅读故障排除？** → 问题出现时收藏

**阅读高级场景？** → 项目变得复杂时回来

---

**记住：** 您不需要线性阅读所有内容。使用本指南在需要时跳转到相关主题。