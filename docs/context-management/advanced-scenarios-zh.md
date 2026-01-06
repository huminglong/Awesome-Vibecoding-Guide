# 高级上下文管理场景

## 概述

本指南涵盖了专门开发场景的复杂上下文管理模式。这些策略有助于在具有挑战性的项目结构中保持有效的AI辅助。

## Monorepo上下文管理

### 挑战

Monorepo包含多个共享代码的包/应用程序。加载整个monorepo上下文会压倒AI并减慢响应。

### 策略：隔离的包上下文

**结构示例：**
```
monorepo/
├── packages/
│   ├── web-app/
│   ├── mobile-app/
│   ├── shared-ui/
│   └── shared-utils/
└── package.json
```

**包特定的.cursorrules：**

```markdown
# packages/web-app/.cursorrules

# Web应用程序上下文

这是我们monorepo中的Web应用程序包。

## 此包
- 位置：/packages/web-app
- 目的：面向客户的Web应用程序
- 技术：Astro、React岛屿、Tailwind

## 依赖项
- 共享UI：/packages/shared-ui（组件库）
- 共享工具：/packages/shared-utils（业务逻辑）

## 在此处工作时：
1. 从@repo/ui导入共享组件
2. 从@repo/utils导入工具
3. 永远不要重复共享包中的代码
4. 遵循Web特定模式（SSG、岛屿）

## 相关包：
- 移动应用程序使用类似模式但不同框架
- 与移动团队协调API更改

## 常见任务：
- 添加新页面（src/pages/）
- 创建岛屿组件（src/components/）
- 集成共享组件

## 不要加载：
- 移动应用程序代码（/packages/mobile-app）
- 其他包，除非明确需要
```

### 共享依赖策略

**在共享包（.cursorrules）中：**
```markdown
# packages/shared-ui/.cursorrules

# 共享UI组件库

此包由Web和移动应用程序使用。

## 用法
应用程序导入为：`import { Button } from '@repo/ui'`

## 指南
- 组件必须在Web和移动上下文中工作
- 无框架特定代码（纯React）
- 完全类型化（TypeScript严格模式）
- 每个组件都有Storybook故事
- 可访问性（WCAG 2.1 AA）

## 破坏性更改：
- 始终主要版本升级
- 在CHANGELOG中记录迁移
- 通知Web和移动团队

## 测试：
- 需要单元测试
- Storybook中的视觉回归测试
- 发布前在Web和移动应用程序中测试
```

### 工作区级别上下文

**根.cursorrules（monorepo指南）：**
```markdown
# Monorepo指南

```