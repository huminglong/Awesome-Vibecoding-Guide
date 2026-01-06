# 高级提示技术 🚀

**复杂开发场景的专家级策略**

---

## 目录

1. [多步骤工作流程](#多步骤工作流程)
2. [提示链接](#提示链接)
3. [错误恢复提示](#错误恢复提示)
4. [上下文感知提示](#上下文感知提示)
5. [提示优化](#提示优化)
6. [元提示](#元提示)
7. [并行提示](#并行提示)
8. [自我纠正提示](#自我纠正提示)

---

## 多步骤工作流程

### 概述

**何时使用**：对于单次提示来说太大的复杂功能。

**核心原则**：分解为阶段，增量构建，在每一步验证。

---

### 策略1：线性多步骤

**模式**：每一步都依赖于前一步。

```
步骤1 → 验证 → 步骤2 → 验证 → 步骤3 → 验证 → 完成
```

**示例：构建身份验证系统**

**步骤1：数据库架构**
```
"为身份验证系统创建数据库架构。

文件：src/models/User.ts

要求：
- id：UUID主键
- email：字符串，唯一，索引
- password：字符串（将存储bcrypt哈希）
- name：字符串
- role：枚举（'user'、'admin'）
- emailVerified：布尔值，默认false
- createdAt：时间戳
- updatedAt：时间戳

还在src/database/migrations/中创建迁移文件

成功标准：
- 定义了TypeScript类型
- 数据库迁移创建表
- 在email上建立索引以提高性能"
```

↓ *验证：运行迁移，检查架构*

**步骤2：用户存储库**
```
"为数据库操作创建UserRepository。

文件：src/repositories/UserRepository.ts

使用步骤1中的User模型（src/models/User.ts）。

方法：
- findById(id: string): Promise<User | null>
- findByEmail(email: string): Promise<User | null>
- create(data: CreateUserDTO): Promise<User>
- update(id: string, data: UpdateUserDTO): Promise<User>
- updatePassword(id: string, hashedPassword: string): Promise<void>
- markEmailVerified(id: string): Promise<void>

遵循src/repositories/PostRepository.ts中的存储库模式。

包括以下错误处理：
- 未找到
- 唯一约束违规
- 数据库连接错误"
```

↓ *验证：编写单元测试，测试每个方法*

**步骤3：身份验证服务**
```
"创建具有业务逻辑的身份验证服务。

文件：src/services/AuthService.ts

使用步骤2中的UserRepository。

方法：
- register(email, password, name): 创建用户，哈希密码，发送验证邮件
- login(email, password): 验证凭证，返回JWT令牌
- verifyEmail(token): 标记电子邮件为已验证
- requestPasswordReset(email): 生成重置令牌，发送邮件
- resetPassword(token, newPassword): 验证令牌，更新密码

安全要求：
- 使用bcrypt哈希密码（10轮）
- 生成安全随机令牌（32字节）
- 令牌1小时后过期
- JWT令牌24小时后过期

使用我们现有的EmailService（src/services/EmailService.ts）发送电子邮件。

错误处理：
- 无效凭证 → 返回清晰错误（不透露哪个字段）
- 用户已存在 → 返回ConflictError
- 无效令牌 → 返回UnauthorizedError"
```

↓ *验证：单元测试，模拟依赖项*

**步骤4：API路由**
```
"创建身份验证API路由。

文件：src/api/auth.ts

使用步骤3中的AuthService。

路由：
- POST /api/auth/register
- POST /api/auth/login
- POST /api/auth/verify-email
- POST /api/auth/request-reset
- POST /api/auth/reset-password

每个路由：
- 验证输入（express-validator）
- 调用适当的AuthService方法
- 返回适当的HTTP状态码
- 以一致格式处理错误

遵循src/api/posts.ts中的模式（我们现有的路由）。

速率限制：
- 登录：每15分钟每个IP 5次尝试
- 注册：每小时每个IP 3次尝试
- 密码重置：每小时每个电子邮件3次尝试

使用我们现有的rateLimitMiddleware（src/middleware/rateLimit.ts）。"
```

↓ *验证：集成测试，测试所有端点*

**步骤5：前端集成**
```
"创建身份验证UI组件。

文件：
- src/components/auth/LoginForm.tsx
- src/components/auth/RegisterForm.tsx
- src/components/auth/PasswordResetForm.tsx

连接到步骤4中的API路由。

使用React Hook Form进行表单处理。
使用我们现有的FormError组件显示错误。

状态管理：
- 在localStorage中存储JWT
- 在Zustand存储中存储用户数据（src/stores/authStore.ts - 创建这个）
- 处理令牌过期（重定向到登录）

遵循src/components/forms/PostForm.tsx中的表单模式。"
```

↓ *验证：手动测试，E2E测试*

**为什么这有效：**
- ✅ 每一步都有明确范围
- ✅ 验证点防止错误累积
- ✅ 可以暂停/恢复步骤之间
- ✅ 调试更容易（知道故障发生在哪里）
- ✅ 可以并行一些步骤（前端+后端）

---

### 策略2：分支多步骤

**模式**：多个并行路径汇聚。

```
           ┌─ 步骤2a ─┐
步骤1 ────┼─ 步骤2b ─┼──── 步骤3（合并）
           └─ 步骤2c ─┘
```

**示例：构建具有多个小部件的仪表板**

**步骤1：仪表板外壳**
```
"创建仪表板布局外壳。

文件：src/pages/Dashboard.tsx

布局：
- 页眉（用户信息，登出）
- 侧边栏（导航）
- 主内容区域（2x2网格中的4个小部件槽）
- 页脚

没有小部件，只是占位符：
<WidgetPlaceholder name='Analytics' />
<WidgetPlaceholder name='Recent Activity' />
<WidgetPlaceholder name='User Stats' />
<WidgetPlaceholder name='Quick Actions' />

使用Tailwind网格进行响应式布局。
遵循我们的应用布局模式（src/layouts/AppLayout.tsx）。"
```

↓ *验证：布局正确渲染*

**然后并行化：**

**步骤2a：分析小部件**
```
"为仪表板创建分析小部件。

文件：src/components/widgets/AnalyticsWidget.tsx

显示：
- 总浏览量（过去30天）
- 总用户数
- 收入趋势（折线图）

数据来自：GET /api/analytics/summary
图表库：Chart.js（已安装）

小部件框架：使用src/components/widgets/WidgetContainer.tsx中的WidgetContainer
加载状态：使用我们的Skeleton组件"
```

**步骤2b：最近活动小部件（并行）**
```
"为仪表板创建最近活动小部件。

文件：src/components/widgets/RecentActivityWidget.tsx

显示：
- 最近10个用户活动
- 活动类型图标+描述+时间戳
- '查看全部'链接到活动页面

数据来自：GET /api/activities/recent
实时更新：使用WebSocket连接（ws://localhost:8080/activities）

小部件框架：使用src/components/widgets/WidgetContainer.tsx中的WidgetContainer
加载状态：使用我们的Skeleton组件"
```

**步骤2c：用户统计小部件（并行）**
```
"创建用户统计小部件用于仪表板。

文件：src/components/widgets/UserStatsWidget.tsx

显示：
- 活跃用户（今天）
- 新注册（本周）
- 流失率
- 用户满意度评分

数据来自：GET /api/users/stats
刷新：每5分钟（useInterval钩子）

小部件框架：使用src/components/widgets/WidgetContainer.tsx中的WidgetContainer
加载状态：使用我们的Skeleton组件"
```

↓ *所有三个可以同时开发*

**步骤3：集成**
```
"将所有小部件集成到仪表板中。

更新src/pages/Dashboard.tsx：
- 用实际小部件替换占位符
- 添加加载状态（小部件加载时显示骨架屏）
- 添加错误边界（如果一个小部件失败，其他仍能工作）
- 添加刷新按钮（刷新所有小部件）

使用Promise.all并行获取所有小部件数据（加载更快）。

错误处理：
- 小部件加载失败 → 在小部件框架中显示错误消息，不崩溃仪表板
- 网络错误 → 显示重试按钮
- 无数据 → 显示空状态

遵循管理员面板中的仪表板模式（src/pages/admin/Dashboard.tsx）。"
```

**为什么这有效：**
- ✅ 并行开发（小部件可以同时构建）
- ✅ 隔离故障（一个小部件错误不会破坏其他小部件）
- ✅ 每个小部件都是独立可测试的
- ✅ 易于添加/删除小部件
- ✅ 清晰的最终集成点

---

## 提示链接

### 概述

**概念：**一个提示的输出成为下一个的输入。

**使用场景：**
- 需要多个专门任务的复杂工作流程
- 渐进式优化
- 管道处理

---

### 模式1：顺序优化链

**何时使用：**需要渐进式改进或转换输出。

**示例：代码生成 → 优化 → 文档化链**

**提示1：生成初始代码**
```
"生成一个计算阶乘的函数。

语言：TypeScript
处理：负数（返回错误）、零（返回1）、大数（使用BigInt）
包括：输入验证

只返回代码，不解释。"
```

↓ *AI生成代码*

**提示2：优化（链接）**
```
"优化这个阶乘函数以提高性能：

[粘贴提示1的代码]

要应用的优化：
- 重复计算的记忆化
- 迭代代替递归（避免堆栈溢出）
- 0和1的提前返回

只返回优化后的代码。"
```

↓ *AI优化*

**提示3：添加测试（链接）**
```
"为这个阶乘函数编写全面测试：

[粘贴提示2的优化代码]

测试框架：Jest

覆盖：
- 快乐路径（0、1、5、10）
- 边缘情况（负数、大数）
- 性能（验证记忆化工作）

返回完整测试文件。"
```

↓ *AI编写测试*

**提示4：生成文档（链接）**
```
"为这个阶乘函数生成JSDoc文档：

[粘贴提示2的最终代码]

包括：
- 描述
- @param文档
- @returns文档
- @throws文档
- @example带3个使用示例
- @complexity注释（时间和空间）

返回带内联文档的代码。"
```

**最终结果：**优化、测试、文档化的函数。

**为什么链接有效：**
- 每一步都有单一焦点
- 质量在每个阶段提高
- 可以在每个阶段验证/调整
- 管道可重用

---

### 模式2：数据转换链

**何时使用：**通过多个转换处理数据。

**示例：API响应 → 转换 → 过滤 → 格式化 → 显示**

**提示1：转换原始API数据**
```
"将原始API响应转换为标准化格式：

原始数据：
[粘贴API响应]

转换为：
{
  id: string,
  name: string,
  email: string,
  role: 'user' | 'admin',
  active: boolean,
  lastLogin: Date
}

返回转换函数的TypeScript代码。"
```

**提示2：过滤数据（链接）**
```
"向这个转换函数添加过滤：

[粘贴提示1的代码]

要添加的过滤器：
- 仅活跃用户
- 30天内最后登录
- 角色为'user'（排除管理员）

返回应用过滤器的更新函数。"
```

**提示3：排序和分页（链接）**
```
"向这个函数添加排序和分页：

[粘贴提示2的代码]

排序：
- 按lastLogin排序（最新优先）

分页：
- 页面大小：20
- 接受页码参数
- 返回{ data: User[], totalPages: number, currentPage: number }

返回最终函数。"
```

**结果：**完整的数据处理管道。

---

## 错误恢复提示

### 概述

**当AI生成错误代码时，如何有效纠正。**

**反模式：**
```
"那是错的，再试一次"
[AI犯不同错误]
```

**更好模式：**明确纠正并解释。

---

### 策略1：识别和纠正

**模式：**

```
"您生成的代码有[特定问题]。

具体：
- [什么错了]
- [为什么错了]
- [期望行为]

请通过以下方式修复：
1. [步骤1]
2. [步骤2]

保持其他所有相同。"
```

**示例：**

**AI生成带错误的代码：**
```typescript
function calculateTotal(items) {
  let total = 0;
  items.forEach(item => {
    total += item.price; // 错误：不考虑数量
  });
  return total;
}
```

**糟糕的纠正：**
```
"这错了，修复它"
```

**好的纠正：**
```
"calculateTotal函数有错误。

问题：
第4行：total += item.price不乘以item.quantity

这意味着：
- 2个$10的物品 → 计算为$10而不是$20

修复：
将第4行改为：total += item.price * item.quantity

保持其他所有相同（函数签名、循环结构、返回）。"
```

**结果：**精确修复，无其他更改。

---

### 策略2：提供正确示例

**模式：**

```
"输出不符合要求。

期望输出：
[确切示例]

实际输出：
[AI生成的内容]

差异：
[什么不同]

请更新以完全匹配期望输出。"
```

**示例：**

**要求：**将货币格式化为"$1,234.56"

**AI生成：**
```typescript
function formatCurrency(amount) {
  return `$${amount.toFixed(2)}`; // 输出：$1234.56（无逗号）
}
```

**纠正：**
```
"formatCurrency函数不添加千位分隔符。

期望输出：$1,234.56
实际输出：$1234.56

缺少：逗号千位分隔符

更新使用Intl.NumberFormat：
const formatter = new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'USD'
});

这将格式化1234.56 → $1,234.56正确。"
```

---

### 策略3：参考正确模式

**模式：**

```
"实现不遵循我们的项目模式。

当前实现：
[AI生成的内容]

应遵循来自的模式：
[参考文件:函数]

具体：
- [模式元素1]
- [模式元素2]

请更新以匹配我们的模式。"
```

**示例：**

**AI生成自定义验证：**
```typescript
function validateEmail(email) {
  const regex = /^[^@]+@[^@]+\.[^@]+$/;
  if (!regex.test(email)) {
    throw new Error('Invalid email');
  }
}
```

**纠正：**
```
"validateEmail函数不遵循我们的错误处理模式。

当前实现：
- 抛出通用错误
- 使用简单正则表达式

我们的模式（来自src/utils/validation.ts）：
- 抛出ValidationError（自定义类）
- 使用我们标准化的电子邮件正则表达式
- 返回验证结果对象

来自src/utils/validation.ts的示例：

```typescript
function validateEmail(email): ValidationResult {
  const isValid = EMAIL_REGEX.test(email);
  return {
    isValid,
    error: isValid ? null : 'Please enter a valid email address'
  };
}
```

请更新您的实现以匹配此模式。"
```

**结果：**AI生成与项目一致的代码。

---

## 上下文感知提示

### 概述

**利用项目特定上下文获得更好结果。**

---

### 策略1：参考架构文档

**模式：**

```
"在实施之前，阅读docs/architecture/[主题].md

确认您理解：
- [关键原则1]
- [关键原则2]

然后按照该架构实施。"
```

**示例：**

```
"在实施新API端点之前，阅读docs/architecture/api-design.md

确认您理解：
- 我们的REST约定（资源命名、HTTP方法）
- 错误响应格式
- 分页模式
- 身份验证要求

然后按照该架构实施POST /api/articles。

要求：[...]"
```

**为什么这有效：**
- AI首先加载架构
- 确认理解后再编码
- 实施遵循既定模式

---

### 策略2：有效使用.cursorrules

**创建项目特定说明：**

**.cursorrules：**
```
# 项目约定

## 代码风格
- 使用函数组件（无类组件）
- 使用TypeScript严格模式
- 使用箭头函数
- Async/await（无.then()）

## 错误处理
- 始终使用我们的自定义错误类（ValidationError、NotFoundError等）
- 永不抛出通用错误
- 始终包含错误代码

## 测试
- 每个功能必须有测试
- 使用Jest + React Testing Library
- 测试文件命名：ComponentName.test.tsx
- 覆盖率目标：80%

## API设计
- REST约定：GET /resource, POST /resource, PUT /resource/:id, DELETE /resource/:id
- 始终返回JSON
- 使用我们的标准响应格式：
  成功：{ data: {...} }
  错误：{ error: { code: string, message: string } }

## 数据库
- 使用Prisma ORM
- 所有查询在存储库层（不在路由或服务中）
- 始终对多步骤操作使用事务

## 组件
- 一个组件一个文件
- Props接口始终定义
- 使用我们的组件模板：docs/templates/component.tsx
```

**提示现在可以更短：**

**没有.cursorrules：**
```
"创建一个按钮组件。使用TypeScript，函数组件，箭头函数，
props接口，一个组件一个文件，使用Tailwind进行样式设计，包括
变体（primary、secondary、danger），包括大小（sm、md、lg），包括
禁用状态，包括加载状态，遵循我们的命名约定..."
```

**有.cursorrules：**
```
"创建一个按钮组件，带变体（primary、secondary、danger）
和大小（sm、md、lg）。包括禁用和加载状态。"
```

AI自动遵循.cursorrules中的约定。

---

### 策略3：项目记忆模式

**维护项目知识库：**

**docs/context/project-knowledge.md：**
```markdown
# 项目知识库

## 已做决策
- 2024-01-15：从bcrypt切换到argon2以获得更好安全性
- 2024-01-10：使用PostgreSQL代替MongoDB（更适合关系数据）
- 2024-01-05：使用Cloudflare Pages进行托管（免费、快速、边缘计算）

## 技术栈
- 前端：React + TypeScript + Tailwind + Vite
- 后端：Node.js + Express + TypeScript
- 数据库：PostgreSQL + Prisma ORM
- 托管：Cloudflare Pages + Workers
- 身份验证：JWT令牌
- 电子邮件：SendGrid
- 文件存储：Cloudflare R2

## 约定
- 文件命名：kebab-case（user-profile.tsx）
- 组件命名：PascalCase（UserProfile）
- 函数命名：camelCase（getUserById）
- CSS：仅Tailwind实用类（除非绝对必要，否则不使用自定义CSS）

## 常见模式
- 身份验证：src/middleware/auth.ts中的JWT中间件
- 验证：路由中的express-validator，服务中的详细验证
- 错误处理：自定义错误类，集中错误处理程序
- 数据库：存储库模式，Prisma用于所有查询
- 表单：React Hook Form + Zod验证

## 已知问题
- 电子邮件发送在开发中很慢（使用mailtrap）
- 数据库迁移必须手动运行（不是自动的）
- 文件上传限制为10MB（Cloudflare R2限制）
```

**提示可以引用此内容：**

```
"阅读docs/context/project-knowledge.md了解我们的技术栈和约定。

然后按照我们既定的模式实施用户注册。"
```

AI从一个文件获得完整项目上下文。

---

## 提示优化

### 概述

**减少令牌使用同时保持质量。**

---

### 策略1：模板+变量

**代替：**
```
"创建一个带有电子邮件、密码、姓名字段的用户注册表单。
验证电子邮件格式，密码最少8个字符，姓名必需。
在字段下方显示错误。提交到POST /api/users。显示成功消息。
使用React Hook Form。使用我们的FormError组件。"

[每次类似表单都发送此完整提示]
```

**更好：**

**创建模板（docs/templates/prompt-form.md）：**
```markdown
# 表单创建模板

创建一个{{FORM_NAME}}表单，带{{FIELDS}}字段。

验证：
{{VALIDATION_RULES}}

提交到：{{API_ENDPOINT}}
成功消息：{{SUCCESS_MESSAGE}}

使用React Hook Form。
使用我们的FormError组件（src/components/common/FormError.tsx）。
遵循src/components/forms/{{REFERENCE_FORM}}.tsx中的表单模式。
```

**提示变成：**
```
"使用模板docs/templates/prompt-form.md：

FORM_NAME: 用户注册
FIELDS: 电子邮件，密码，姓名
VALIDATION_RULES: 电子邮件格式，密码最少8个字符，姓名必需
API_ENDPOINT: POST /api/users
SUCCESS_MESSAGE: 账户创建！检查电子邮件以验证。
REFERENCE_FORM: LoginForm"
```

**好处：**
- 更短提示
- 一致结果
- 可重用模式
- 令牌节省

---

### 策略2：引用代替重复

**代替：**
```
[每次粘贴整个架构文档]

"现在按照此架构实现功能X..."
```

**更好：**
```
"参考docs/architecture/api-design.md（您在本会话中早些时候读过它）。

按照该架构实现功能X。"
```

如果在同一会话中，AI有架构在上下文中。无需重复。

---

### 策略3：渐进式披露

**不要一次性转储所有信息。按需提供。**

**代替：**
```
"这是我的整个用户模型，数据库架构，验证规则，业务
逻辑约束，UI要求，API文档，测试策略，
部署配置和错误处理模式。现在添加一个字段。"
```

**更好：**

**提示1：**
```
"在用户模型中添加'phoneNumber'字段（src/models/User.ts）。
类型：字符串，可选，格式：E.164国际格式。"
```

**提示2（如果需要）：**
```
"为phoneNumber字段添加验证（src/services/UserService.ts）。
使用libphonenumber-js进行验证。"
```

**提示3（如果需要）：**
```
"将phoneNumber输入添加到注册表单（src/components/RegisterForm.tsx）。
使用react-phone-number-input组件。"
```

渐进式加载上下文，按需提供。

---

## 元提示

### 概述

**让AI帮助编写更好的提示。**

---

### 模式：提示改进

**在使用前要求AI改进您的提示：**

```
"我想要求AI实施一个功能。这是我的提示草稿：

[您的提示]

请改进此提示以：
- 更具体
- 包含必要上下文
- 明确定义成功标准
- 遵循AI提示的最佳实践

返回改进后的提示。"
```

**示例：**

**提示草稿：**
```
"创建一个表单供用户提交反馈"
```

**元提示：**
```
"改进AI编码助手的这个提示：

'创建一个表单供用户提交反馈'

使其更具体并包含必要上下文。"
```

**AI改进为：**
```
"创建一个用户反馈表单组件。

文件：src/components/FeedbackForm.tsx

字段：
- 类别：下拉菜单（错误报告，功能请求，一般反馈）
- 主题：文本输入（必需，最多100个字符）
- 消息：文本区域（必需，最多500个字符）
- 电子邮件：文本输入（可选，用于后续跟进）

验证：
- 类别必需
- 主题必需，最大长度
- 消息必需，最大长度
- 提供时验证电子邮件格式

提交到：POST /api/feedback
成功：显示提示'感谢您的反馈！'，清除表单
错误：在表单下方显示错误消息

使用React Hook Form进行表单处理。
使用我们的Toast组件（src/components/Toast.tsx）。
遵循ContactForm中的表单模式（src/components/ContactForm.tsx）。"
```

好多了！

---

## 并行提示

### 概述

**同时执行多个独立提示。**

**何时使用：**多个不相互依赖的任务。

---

### 策略：并行开发

**示例：构建用户资料页面**

**顺序（慢）：**
```
1. 创建用户数据获取钩子 → 等待
2. 创建资料页眉组件 → 等待
3. 创建资料统计组件 → 等待
4. 创建资料活动组件 → 等待
5. 组装页面 → 等待

总计：5个顺序等待
```

**并行（快）：**

**会话1（后端）：**
```
"创建用户资料API端点。

GET /api/users/:id/profile

返回：
- 用户基本信息
- 统计（帖子数，关注者，正在关注）
- 最近活动

[详细要求]"
```

**会话2（数据钩子 - 依赖会话1完成）：**
```
"创建useUserProfile钩子以获取资料数据。

使用GET /api/users/:id/profile
返回{ profile, loading, error }

[详细要求]"
```

**会话3（组件1 - 可以与会话2并行）：**
```
"创建ProfileHeader组件。

现在使用模拟数据（API稍后会来）。

显示：头像，姓名，简介，编辑按钮

[详细要求]"
```

**会话4（组件2 - 与会话2、3并行）：**
```
"创建ProfileStats组件。

模拟数据现在。

显示：帖子数，关注者，正在关注

[详细要求]"
```

**会话5（组件3 - 与会话2、3、4并行）：**
```
"创建ProfileActivity组件。

模拟数据现在。

显示：最近帖子，评论

[详细要求]"
```

**会话6（集成 - 在所有完成后）：**
```
"将所有资料组件集成到UserProfile页面中。

使用会话2中的useUserProfile钩子。
布局：ProfileHeader + ProfileStats + ProfileActivity

用真实数据替换模拟。"
```

**结果：**3-4个并行会话代替6个顺序 = 更快开发。

---

## 自我纠正提示

### 概述

**在提示中构建验证，使AI检查自己的工作。**

---

### 模式：内置验证

**模板：**

```
[任务描述]

实施后，验证：
- [检查1]
- [检查2]
- [检查3]

如果任何检查失败，请修复后再响应。

返回：代码+验证结果。
```

**示例：**

```
"创建一个将两个排序数组合并为一个排序数组的函数。

语言：TypeScript
时间复杂度：O(n + m)
空间复杂度：O(n + m)

实施后，验证：
1. 函数处理空数组（[]，[1,2] → [1,2]）
2. 函数处理一个空数组（[1,2]，[] → [1,2]）
3. 函数处理相同元素（[1,1]，[1,1] → [1,1,1,1]）
4. 函数保持排序顺序（用测试用例验证）
5. 时间复杂度是O(n + m)（无嵌套循环）
6. 空间复杂度是O(n + m)（一个结果数组）

如果任何验证失败，请修复实现。

返回：
1. 函数
2. 验证结果（所有检查通过）
3. 演示正确性的测试用例"
```

**AI响应包括：**
```typescript
function mergeSortedArrays(arr1: number[], arr2: number[]): number[] {
  // 实现
  [...]
}

// 验证结果：
✓ 空数组处理：通过
✓ 一个空数组：通过
✓ 相同元素：通过
✓ 排序顺序保持：通过
✓ 时间复杂度O(n+m)：通过（遍历两个数组的单次通过）
✓ 空间复杂度O(n+m)：通过（一个大小为n+m的结果数组）

// 测试用例：
[演示所有检查的测试用例]
```

**好处：**
- AI在响应前自我检查
- 减少来回迭代
- 更高质量的首次输出

---

## 总结

### 何时使用每种技术

| 技术               | 使用场景       | 好处               |
| ------------------ | -------------- | ------------------ |
| **多步骤工作流程** | 复杂功能       | 可管理范围，验证点 |
| **提示链接**       | 渐进式优化     | 每个阶段输出改进   |
| **错误恢复**       | AI生成错误代码 | 精确纠正，避免挫折 |
| **上下文感知**     | 项目特定工作   | 与约定一致         |
| **优化**           | 高令牌使用     | 成本节省，更快提示 |
| **元提示**         | 不确定如何提示 | AI帮助编写更好提示 |
| **并行提示**       | 独立任务       | 更快开发           |
| **自我纠正**       | 需要高质量     | 内置验证           |

---

### 高级提示模式参考

```
┌─────────────────────────────────────────────┐
│  高级提示模式选择          │
├─────────────────────────────────────────────┤
│  简单功能       → 单个提示       │
│  复杂功能      → 多步骤工作流程 │
│  多个组件  → 分支工作流程  │
│  管道处理     → 提示链接     │
│  AI犯错误      → 错误恢复      │
│  项目特定     → 上下文感知       │
│  高令牌成本      → 优化        │
│  不明确要求 → 元提示      │
│  速度关键       → 并行提示  │
│  质量关键     → 自我纠正     │
└─────────────────────────────────────────────┘
```

---

**下一步：**
- [模板库](./template-library-zh.md) → 现成的提示模板
- [任务特定模式](./task-specific-patterns-zh.md) → 专门提示
- [基础](./foundations-zh.md) → 核心原则回顾

**返回：** [提示指南主页](./README-zh.md)