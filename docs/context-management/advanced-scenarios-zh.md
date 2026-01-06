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

## 结构
- /packages/* - 单个包
- 每个包有自己的package.json、.cursorrules
- 共享配置在/config中

## 在monorepo中工作：
1. 始终在特定包目录中工作
2. 不要加载整个monorepo上下文
3. 通过导入路径引用共享包
4. 协调跨包更改

## 包类型：
- 应用程序（web-app、mobile-app）
- 库（shared-ui、shared-utils）
- 工具（build-tools、dev-tools）

## 跨包更改：
1. 首先更新共享包
2. 版本升级并发布
3. 更新使用包
4. 测试集成

## 命令：
- `npm run dev:web` - 运行Web应用
- `npm run dev:mobile` - 运行移动应用
- `npm run build` - 构建所有包
- `npm run test` - 测试所有包
```

### 渐进式上下文加载

当需要跨包上下文时：

1. **从窄范围开始：**
   ```
   我正在/packages/web-app中工作。
   现在只关注这个包。
   ```

2. **根据需要添加依赖项：**
   ```
   现在我需要/packages/shared-ui/src/Button.tsx的上下文
   只加载这个文件。
   ```

3. **有选择地扩展：**
   ```
   同时加载/packages/shared-utils/src/formatDate.ts
   ```

**切勿：**
```
加载整个monorepo
↓ 压倒上下文，响应缓慢，质量更差
```

## 微服务上下文策略

### 挑战

多个服务具有不同的语言、框架和职责。AI需要了解服务边界和合同的上下文。

### 每个服务的上下文

**服务特定的.cursorrules：**
```markdown
# services/user-service/.cursorrules

# 用户服务

## 职责
- 用户CRUD操作
- 身份验证
- 用户资料管理

## 技术栈
- Node.js + Express
- PostgreSQL
- Redis（缓存）

## API端点
- POST /api/users - 创建用户
- GET /api/users/:id - 获取用户
- PUT /api/users/:id - 更新用户
- DELETE /api/users/:id - 删除用户
- POST /api/auth/login - 登录
- POST /api/auth/logout - 登出

## 依赖于
- 无（根服务）

## 被依赖
- 订单服务（用户信息）
- 通知服务（用户偏好）

## 数据库架构
- users表（id、email、password_hash、created_at、updated_at）
- sessions表（id、user_id、token、expires_at）

## 在此处工作时：
- 不要加载其他服务
- 使用API合同进行服务间通信
- 提交前运行测试
- 如果更改端点，更新API文档

## 常见任务：
- 添加新端点
- 更新用户架构
- 改进身份验证
```

### API合同文档

**单独维护API合同：**

```typescript
// shared/contracts/user-service.ts

/**
 * 用户服务API合同
 *
 * 其他服务应参考此进行用户服务集成。
 */

export interface User {
  id: string;
  email: string;
  name: string;
  created_at: string;
}

export interface UserServiceAPI {
  // 通过ID获取用户
  'GET /api/users/:id': {
    params: { id: string };
    response: { user: User } | { error: string };
    status: 200 | 404 | 500;
  };

  // 创建用户
  'POST /api/users': {
    body: { email: string; name: string; password: string };
    response: { user: User; token: string } | { error: string };
    status: 201 | 400 | 500;
  };
}
```

在依赖服务中引用：

```markdown
# services/order-service/.cursorrules

## 依赖于：用户服务
- 合同：/shared/contracts/user-service.ts
- 创建订单时获取用户信息
- 处理前验证用户ID存在
```

### 跨服务调试

当调试需要多个服务时：

**明确的上下文边界：**
```
我正在调试涉及用户服务的订单服务中的问题。

主要上下文：/services/order-service
次要上下文：仅用户服务API合同（/shared/contracts/user-service.ts）

不要加载用户服务实现。
专注于订单服务如何调用用户服务并处理响应。
```

**如果需要实现细节：**
```
现在加载特定文件：/services/user-service/src/controllers/userController.ts
仅此文件，不是整个用户服务。
```

## 长期项目（6个月以上）

### 挑战

上下文随时间衰减。初始决策被遗忘，模式漂移，文档过时。

### 策略：活文档

**架构决策记录（ADR）：**

```markdown
# docs/adr/0001-use-astro-for-frontend.md

# 使用Astro作为前端框架

日期：2024-01-15
状态：已接受

## 背景
我们需要一个用于营销网站和博客的前端框架。

## 决策
使用带有React岛屿的Astro。

## 理由
- 静态站点生成（快速、SEO友好）
- 岛屿架构（最少的JavaScript）
- 用于交互组件的React（团队熟悉）
- 良好的开发体验，快速构建

## 后果
- 正面：快速站点、良好SEO、低托管成本
- 负面：岛屿模式学习曲线
- 权衡：不如SPA动态，但我们不需要

## 考虑的替代方案
- Next.js：更复杂、更重的JS、对我们的需求来说过度
- 原生HTML：对动态组件太有限
- SvelteKit：团队不熟悉Svelte

## 参考
- Astro文档：https://docs.astro.build
- 岛屿架构：https://jasonformat.com/islands-architecture/
```

**每月上下文刷新：**

```markdown
# docs/context-refresh/2025-01-monthly.md

# 2025年1月上下文刷新

## 变化了什么
- 从D1迁移到PostgreSQL（性能）
- 添加了用户身份验证（电子邮件+密码）
- 重新设计了主页（新的英雄部分）

## 当前架构
- 前端：Astro + React岛屿
- 后端：Cloudflare Workers + Pages Functions
- 数据库：PostgreSQL（通过Neon）
- 身份验证：自定义JWT实现
- 托管：Cloudflare Pages

## 活动模式
- 对API路由使用服务器端点
- 对交互组件使用React岛屿
- 对关系数据使用PostgreSQL
- 对缓存使用KV
- 对文件上传使用R2

## 已弃用模式
- ❌ D1数据库（已迁移到PostgreSQL）
- ❌ 组件中的直接数据库查询（现在使用API）

## 当前优先级
1. 添加支付集成（Stripe）
2. 改进移动性能
3. 添加管理仪表板

## 团队笔记
- John：正在进行支付集成
- Sarah：处理移动优化
- Mike：构建管理仪表板
```

**每季度更新.cursorrules：**

```markdown
# .cursorrules（更新于2025-01-15）

## 当前状态（2025年第一季度）

### 技术栈（已更新）
- 前端：Astro 4.0
- 数据库：通过Neon的PostgreSQL（2024年12月从D1迁移）
- 身份验证：自定义JWT（2025年1月添加）
- 支付：Stripe（进行中）

### 最近更改
- 2024年12月：从D1迁移到PostgreSQL
- 2025年1月：添加身份验证
- 2025年1月：重新设计主页

### 活动工作
- 支付集成（John）
- 移动优化（Sarah）
- 管理仪表板（Mike）

[.cursorrules其余部分...]
```

### 定期上下文审核

**每3个月：**

1. **审查.cursorrules** - 更新为当前状态
2. **审查ADR** - 标记被取代的决策
3. **审查docs/** - 更新架构图
4. **审查prompts/** - 更新提示模板
5. **审查patterns/** - 记录新模式

**审核清单：**
```markdown
# 季度上下文审核清单

## .cursorrules
☐ 更新技术栈版本
☐ 添加新模式
☐ 删除已弃用模式
☐ 更新团队信息
☐ 更新文件结构

## ADR
☐ 审查所有ADR
☐ 标记被取代的ADR
☐ 为最近重大决策创建ADR

## 文档
☐ 更新README
☐ 更新架构图
☐ 更新API文档
☐ 更新部署文档

## 上下文文件
☐ 每月刷新文档最新
☐ 提示模板当前
☐ 模式库当前

## Git
☐ 所有分支已合并或清理
☐ 主要版本的标签
☐ 更新日志已更新
```

## 跨项目上下文共享

### 挑战

一个项目的模式、组件和知识可以使其他项目受益。但每个项目都是独特的。

### 策略：模式库

**创建共享模式库：**

```
~/dev/patterns/
├── astro-patterns/
│   ├── auth-pattern.md
│   ├── api-routes-pattern.md
│   └── islands-pattern.md
├── cloudflare-patterns/
│   ├── d1-patterns.md
│   ├── kv-patterns.md
│   └── workers-patterns.md
└── README.md
```

**模式模板：**

```markdown
# 模式：使用JWT的身份验证

## 背景
当您需要在Astro + Cloudflare Pages项目中进行用户身份验证时。

## 问题
需要安全的、无状态的身份验证，适用于无服务器架构。

## 解决方案
使用存储在httpOnly cookie中的JWT令牌。

## 实现

### 1. 创建身份验证工具
[代码示例]

### 2. 创建登录端点
[代码示例]

### 3. 保护路由
[代码示例]

## 优点
- 无状态（适用于无服务器）
- 安全（httpOnly cookies）
- 可扩展（无会话存储）

## 缺点
- 无法使令牌失效（使用短期过期）
- 令牌大小（保持负载小）

## 替代方案
- 基于会话的身份验证（需要会话存储）
- OAuth（更复杂，更适合SSO）

## 用于
- 项目A（电子商务网站）
- 项目B（SaaS仪表板）

## 参考
- [JWT文档]
- [安全考虑]
```

### 可重用组件库

**维护复制粘贴的组件库：**

```
~/dev/components/
├── astro/
│   ├── ContactForm.astro
│   ├── Newsletter.astro
│   └── PricingTable.astro
└── react/
    ├── Modal.tsx
    ├── Tooltip.tsx
    └── Dropdown.tsx
```

**每个组件都有使用说明：**

```typescript
/**
 * 模态框组件
 *
 * 具有键盘导航和焦点管理的可重用模态框。
 *
 * 用于：
 * - 项目A（产品预览）
 * - 项目B（表单对话框）
 *
 * 在新项目中使用：
 * 1. 复制此文件
 * 2. 调整样式以匹配项目
 * 3. 测试键盘导航
 * 4. 使用屏幕阅读器测试
 *
 * 依赖项：
 * - @headlessui/react（用于可访问性）
 *
 * 自定义点：
 * - 背景颜色
 * - 边框半径
 * - 动画时间
 */

export function Modal({ children, isOpen, onClose }: ModalProps) {
  // 实现
}
```

### 项目模板

**维护项目启动模板：**

```
~/dev/templates/
├── astro-static-site/
├── astro-saas/
└── astro-ecommerce/
```

每个模板包括：
- 预配置的.cursorrules
- ADR模板
- 文件夹结构
- 示例模式
- 常用组件

## 团队协作上下文

### 挑战

多个开发人员在同一代码库上工作。AI需要了解团队约定和进行中的工作。

### 策略：共享上下文标准

**团队.cursorrules（在仓库中）：**

```markdown
# .cursorrules

## 团队上下文标准

### 团队成员
- John（负责人）：@johnsmith - 架构、数据库
- Sarah（高级）：@sarahdev - 前端、性能
- Mike（中级）：@mikedev - 功能、测试

### 进行中的工作
检查/docs/wip/中的开发功能。

### 代码风格
- 根目录中的ESLint配置
- 使用Prettier格式化
- TypeScript严格模式
- 函数式组件（React）

### 模式
- /src/pages/api/中的API路由
- /src/components/中的组件
- /src/utils/中的工具
- /src/types/中的类型

### 分支策略
- main - 生产
- develop - 集成
- feature/* - 新功能
- fix/* - 错误修复

### 在询问AI之前
1. 阅读/docs/adr/中的相关ADR
2. 检查/docs/patterns/中的模式
3. 审查CHANGELOG中的最近更改
4. 检查/docs/wip/中的WIP文档

### AI使用指南
- 提供有关您任务的上下文
- 引用现有模式
- 请求解释，而不仅仅是代码
- 提交前审查AI输出
```

### 进行中工作文档

**活动功能文档：**

```markdown
# docs/wip/payment-integration.md

# 支付集成（WIP）

负责人：John (@johnsmith)
状态：进行中（60%完成）
目标：2025-02-01

## 已完成
- ✅ Stripe账户设置
- ✅ API密钥配置
- ✅ 测试模式工作
- ✅ 结账流程UI

## 接下来
- ⏳ Webhook处理（进行中）
- ☐ 生产测试
- ☐ 错误处理
- ☐ 管理仪表板集成

## 更改的文件
- /src/pages/api/checkout.ts（新）
- /src/components/CheckoutButton.tsx（新）
- /src/pages/pricing.astro（修改）

## 依赖项
- stripe npm包
- 环境变量：STRIPE_SECRET_KEY、STRIPE_WEBHOOK_SECRET

## 团队笔记
- 还不要修改checkout.ts，即将进行重大重构
- Webhook端点将是/api/webhooks/stripe
- 使用测试卡：4242 4242 4242 4242

## 问题/阻碍
- 需要生产Stripe账户批准
- 等待错误状态的设计反馈
```

### 交接上下文

交接工作时：

```markdown
# docs/handoffs/authentication-to-mike.md

# 身份验证功能交接

来自：John
致：Mike
日期：2025-01-15

## 我做了什么
实现了基本的电子邮件/密码身份验证：
- 登录/登出端点（/src/pages/api/auth/）
- JWT令牌生成和验证
- 受保护路由中间件
- 用户注册流程

## 需要了解的文件
- /src/pages/api/auth/* - 身份验证端点
- /src/middleware/auth.ts - 路由保护
- /src/utils/jwt.ts - 令牌工具
- /src/components/LoginForm.tsx - 登录UI

## 工作原理
1. 用户提交登录表单
2. API验证凭据
3. 在httpOnly cookie中发出JWT令牌
4. 受保护路由在中间件中检查令牌
5. 令牌7天后过期

## 剩余工作
- 密码重置流程（优先）
- 电子邮件验证
- OAuth集成（Google、GitHub）
- 登录尝试速率限制
- 管理员用户管理

## 已知问题
- 目前无

## 测试
- /src/__tests__/auth/中的单元测试
- 运行：`npm test auth`
- 全部通过

## 如何本地测试
1. 注册：POST /api/auth/register
2. 登录：POST /api/auth/login
3. 访问受保护路由：/dashboard
4. 应该看到用户信息

## 问题？
在Slack上联系我或查看/docs/adr/0005-authentication-approach.md
```

## 不同AI模型的上下文

### 挑战

不同的AI模型具有不同的上下文窗口大小和功能。策略必须适应。

### 模型特定策略

**Claude Sonnet 4（200k令牌）：**
- 可以处理大型代码库
- 加载多个相关文件
- 全面的项目上下文
- 完整的.cursorrules无需压缩

**GPT-4（128k令牌）：**
- 更有选择性的文件加载
- 略微压缩.cursorrules
- 专注于活动文件
- 对依赖项使用摘要

**Qwen/GLM（128k令牌）：**
- 类似于GPT-4
- 优先考虑当前任务文件
- 最少的历史上下文
- 清晰、结构化的指令

**较小的模型（32k-64k令牌）：**
- 非常集中的上下文
- 仅当前任务文件
- 缩写的.cursorrules
- 通过摘要引用文档，而不是完整内容

### 自适应.cursorrules

**如果需要，创建模型特定版本：**

```
.cursorrules（完整版本，5000令牌）
.cursorrules-compact（压缩版，2000令牌）
```

**根据模型切换：**
```
使用Claude Sonnet 4？使用.cursorrules
使用GPT-3.5或小型模型？使用.cursorrules-compact
```

**压缩版专注于：**
- 仅基本模式
- 当前架构
- 关键约束
- 活动约定

## 总结：高级上下文原则

### 1. 渐进式披露
从窄范围开始，根据需要扩展。永远不要加载所有内容。

### 2. 清晰边界
定义范围内什么，范围外什么。

### 3. 活文档
保持上下文新鲜。每月审查，每季度审核。

### 4. 共享标准
全团队约定。记录并强制执行。

### 5. 上下文卫生
删除过时信息。标记已弃用模式。

### 6. 目标构建上下文
不同任务需要不同上下文。不要盲目重用。

### 7. 模型意识
使策略适应模型功能。

---

**记住：**上下文管理不是设置后忘记的。它是有效AI辅助开发的关键持续维护。