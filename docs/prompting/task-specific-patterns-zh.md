# 任务特定提示模式 🎨

**掌握不同开发场景的专门提示策略**

---

## 目录

1. [功能实施](#功能实施)
2. [调试和错误解决](#调试和错误解决)
3. [重构](#重构)
4. [代码审查](#代码审查)
5. [文档](#文档)
6. [测试](#测试)
7. [性能优化](#性能优化)
8. [安全加固](#安全加固)

---

## 功能实施

### 概述

**目标**：让AI构建与现有代码无缝集成的新功能。

**关键挑战：**
- 理解现有模式
- 保持一致性
- 处理集成点
- 预测边缘情况

**成功模式：**

```
清晰要求 + 架构上下文 + 模式参考 + 示例
= 第一次尝试的高质量实施
```

---

### 模式1：简单功能添加

**何时使用**：向现有模块添加独立功能

**模板：**

```
在[FILE_PATH]中实施[FEATURE_NAME]。

要求：
- [要求1]
- [要求2]
- [要求3]

遵循[REFERENCE_FILE]中[REFERENCE_FEATURE]使用的模式。

成功标准：
- [标准1]
- [标准2]
```

**真实示例1：添加电子邮件验证**

**糟糕的提示：**
```
"添加电子邮件验证"
```

**好的提示：**
```
"向src/components/RegisterForm.tsx中的用户注册表单添加电子邮件验证。

要求：
- 在模糊时验证（不是每次击键）
- 在输入字段下方显示错误消息
- 检查格式：标准电子邮件正则表达式
- 必填字段（空 = 错误）

遵循同一文件中密码验证使用的模式（第45-67行）。

成功标准：
- 无效电子邮件 → 红色边框 + 错误消息'Please enter a valid email'
- 有效电子邮件 → 绿色边框，无错误消息
- 模糊时为空字段 → 错误消息'Email is required'
- 如果电子邮件无效，提交禁用

使用我们现有的FormError组件，来自src/components/common/FormError.tsx。"
```

**为什么好的提示有效：**
- ✅ 特定文件和位置
- ✅ 清晰的验证规则
- ✅ 定义了UX行为
- ✅ 引用现有模式
- ✅ 使用现有组件
- ✅ 可测量的成功标准

**结果**：AI生成与现有代码风格匹配的验证，处理所有情况，与FormError组件集成。

---

**真实示例2：添加排序功能**

**糟糕的提示：**
```
"向表格添加排序"
```

**好的提示：**
```
"向src/components/tables/UserTable.tsx中的UserTable组件添加排序功能。

要求：
- 排序按：姓名（字母顺序）、电子邮件（字母顺序）、创建日期（时间顺序）
- 点击列标题排序
- 默认排序：姓名升序
- 第二次点击：反转排序顺序
- 第三次点击：移除排序（回到默认）

视觉指示器：
- 未排序：无图标
- 升序排序：标题旁边的↑图标
- 降序排序：标题旁边的↓图标

状态管理：
- 使用现有Zustand存储模式（参见src/stores/tableStore.ts）
- 在URL查询参数中持久化排序状态（?sort=name&order=asc）

遵循src/components/tables/PostTable.tsx中PostTable的模式（第89-145行）。

成功标准：
- 点击标题 → 数据正确重新排序
- 排序更改时URL更新
- 带排序参数的页面加载 → 自动应用排序
- 排序与分页一起工作（对整个数据集排序，不仅仅是当前页）
- 图标正确更新

使用Heroicons作为箭头图标（我们在整个应用程序中使用它们）。"
```

**为什么这有效：**
- ✅ 详细的交互行为
- ✅ 指定了视觉反馈
- ✅ 定义了状态管理方法
- ✅ 与URL路由集成
- ✅ 处理分页交互
- ✅ 引用特定模式
- ✅ 一致的图标库

**结果：**完全按照指定工作的排序实现，保持项目模式，无需后续修复。

---

### 模式2：具有多个组件的复杂功能

**何时使用**：功能需要多个文件/组件协同工作

**模板：**

```
在多个文件中实施[FEATURE_NAME]。

架构：
[高级组件分解]

实施顺序：
1. [步骤1]
2. [步骤2]
3. [步骤3]

对于此提示：实施[STEP_N]

[此步骤的详细要求]

集成点：
- [接口1]
- [接口2]

参考：[文件]中的[类似功能]
```

**真实示例3：实时通知系统**

**糟糕的提示：**
```
"创建通知系统"
```

**好的提示（步骤1）：**
```
"实施实时通知系统。这是4步中的第1步 - 后端WebSocket设置。

架构概述：
├─ 后端：WebSocket服务器（src/websocket/notifications.ts）
├─ 状态：Zustand存储（src/stores/notificationStore.ts）
├─ UI：NotificationCenter组件（src/components/NotificationCenter.tsx）
└─ 后端API：通知CRUD（src/api/notifications.ts）

对于此步骤：创建WebSocket服务器。

要求：
- 文件：src/websocket/notifications.ts
- 库：使用'ws'（已安装）
- 端口：8080（与主应用程序3000分开）
- 身份验证：在连接时验证JWT令牌
- 要处理的事件：
  - 'notification:new' → 广播给特定用户
  - 'notification:read' → 标记为已读
 - 'notification:delete' → 删除通知

消息格式：
{
  type: 'notification:new' | 'notification:read' | 'notification:delete',
  payload: {
    id: string,
    userId: string,
    message: string,
    type: 'info' | 'success' | 'warning' | 'error',
    timestamp: number
  }
}

遵循src/websocket/chat.ts中我们现有聊天功能的WebSocket模式。

错误处理：
- 无效JWT → 使用代码4001关闭连接
- 格式错误的消息 → 记录错误，发送错误响应，不关闭连接
- 连接错误 → 在客户端实现指数退避重连（稍后步骤）

成功标准：
- 服务器在端口8080上启动
- 接受WebSocket连接
- 连接时验证JWT
- 向正确用户广播消息
- 优雅处理断开连接"
```

**好的提示（步骤2）：**
```
"通知系统4步中的第2步 - 前端状态管理。

在src/stores/notificationStore.ts中创建Zustand存储。

存储形状：
{
  notifications: Notification[],
  unreadCount: number,
 isConnected: boolean,
  socket: WebSocket | null
}

操作：
- connect(token: string) → 建立WebSocket连接
- disconnect() → 关闭连接
- addNotification(notification: Notification) → 添加到数组，增加未读
- markAsRead(id: string) → 更新通知，减少未读
- deleteNotification(id: string) → 从数组中删除
- clearAll() → 清空通知数组

WebSocket集成：
- connect()建立到ws://localhost:8080的连接
- 监听'notification:new' → 调用addNotification
- 在markAsRead上发送'notification:read'
- 在deleteNotification上发送'notification:delete'
- 断开连接时处理重连（指数退避：1s、2s、4s、8s，最大30s）

遵循src/stores/userStore.ts和src/stores/chatStore.ts中的Zustand模式。

成功标准：
- 存储连接到WebSocket服务器
- 操作正确更新状态
- WebSocket消息触发状态更新
- 断开连接后重连工作
- 卸载时无内存泄漏"
```

**为什么这有效：**
- ✅ 将复杂功能分解为可管理的步骤
- ✅ 每个步骤都有清晰、专注的范围
- ✅ 显示架构概述以提供上下文
- ✅ 逐步实施防止压倒
- ✅ 每个步骤可以独立验证
- ✅ 引用现有模式

**结果：**每一步都有干净、工作的实现。易于调试。可维护的代码结构。

---

### 模式3：功能集成

**何时使用**：添加与多个现有系统集成的功能

**模板：**

```
实施与[SYSTEMS]集成的[FEATURE]。

功能要求：
[核心功能]

集成点：
1. [系统1]：[如何集成]
2. [系统2]：[如何集成]
3. [系统3]：[如何集成]

保持与以下兼容：
- [约束1]
- [约束2]

测试方法：
- [测试场景1]
- [测试场景2]
```

**真实示例4：双因素身份验证**

**糟糕的提示：**
```
"添加2FA"
```

**好的提示：**
```
"实施与我们现有身份验证系统集成的双因素身份验证（2FA）。

功能要求：
- 基于TOTP的2FA（基于时间的一次性密码）
- 用户可以在账户设置中启用/禁用2FA
- 密码验证后在登录时需要
- 启用2FA时生成备份代码（10个）
- 用于身份验证器应用轻松设置的QR码

集成点：

1. 用户模型（src/models/User.ts）：
   - 添加字段：twoFactorEnabled: boolean, twoFactorSecret: string | null, backupCodes: string[]
   - 迁移：在src/database/migrations/中创建新迁移文件

2. 身份验证API（src/api/auth.ts）：
   - 修改POST /api/auth/login：
     - 密码验证后，检查twoFactorEnabled
     - 如果为true：返回{ requiresTwoFactor: true, tempToken: string }而不是JWT
     - 如果为false：继续正常流程（返回JWT）
   - 添加POST /api/auth/verify-2fa：
     - 接受：tempToken, code（6位数字）
     - 验证TOTP代码或备份代码
     - 成功时返回：JWT
   - 不要破坏现有OAuth流程（src/api/auth/oauth.ts）

3. 设置页面（src/pages/Settings.tsx）：
   - 添加2FA部分
   - 启用2FA按钮 → 显示QR码 + 密钥 + 备份代码
   - 禁用2FA按钮 → 需要密码确认
   - 重新生成备份代码按钮

库：
- 使用'otplib'进行TOTP生成/验证（添加到package.json）
- 使用'qrcode'进行QR码生成（添加到package.json）

安全要求：
- 加密存储twoFactorSecret（使用src/utils/crypto.ts中现有的加密/解密工具）
- 存储前哈希备份代码（像密码一样使用bcrypt）
- 2FA验证速率限制：每15分钟每用户5次尝试
- 临时令牌5分钟后过期

UX要求：
- 显示剩余备份代码数量
- 当<3个备份代码剩余时警告
- 成功/错误消息匹配我们现有的toast模式（src/components/Toast.tsx）
- 验证期间加载状态

边缘情况：
- 用户失去身份验证器访问权限 → 可以使用备份代码
- 用户失去备份代码 → 必须通过电子邮件恢复禁用2FA（单独功能，不在此范围内）
- 时钟漂移 → 允许1步窗口容差（otplib处理此问题）

测试：
- 手动测试：启用2FA → 使用2FA登录 → 使用备份代码 → 禁用2FA
- 测试速率限制工作
- 测试临时令牌过期

成功标准：
- 2FA可以无错误启用/禁用
- 登录流程在有和没有2FA的情况下都工作
- 备份代码正确工作
- 所有现有身份验证流程仍然工作（OAuth、电子邮件/密码）
- 无安全漏洞（密钥加密，代码哈希）
- UI匹配现有设计模式"
```

**为什么这有效：**
- ✅ 全面映射集成点
- ✅ 明确安全要求
- ✅ 定义UX行为
- ✅ 考虑边缘情况
- ✅ 保持兼容性
- ✅ 指定库
- ✅ 包含测试方法

**结果：**完全的2FA实现，无缝集成，保持安全标准，不破坏现有功能。

---

## 调试和错误解决

### 概述

**目标**：让AI快速识别和修复错误，最少来回。

**关键原则**：更多数据 = 更快解决

**成功模式：**

```
错误消息 + 上下文 + 期望 vs 实际 + 已尝试
= 快速、准确的修复
```

---

### 模式0：上下文优先调试（从这里开始！）

**何时使用**：任何东西坏了，在尝试任何其他调试模式之前

**关键原则**：95%的调试失败是上下文失败，不是AI失败

**问题：**
```
❌ 糟糕："它坏了，修复它"
❌ 糟糕："登录不工作"
❌ 糟糕："得到错误"

✅ 良好：[参见下面的完整上下文模板]
```

**在要求AI调试之前，您必须：**

1. **打开DevTools**（F12或Cmd+Option+I）
2. **检查控制台选项卡** - 复制所有错误消息
3. **检查网络选项卡** - 找到失败的请求（红色状态）
4. **重现问题** - 写下确切步骤
5. **记录期望 vs 实际** - 具体说明

**模板：**

```
[我试图做什么]：
[用户的目标 - 他们期望完成什么]

[实际发生了什么]：
[观察到的实际行为]

[我收集的上下文]：

控制台错误：
[从浏览器控制台粘贴完整错误消息 + 堆栈跟踪]

网络请求：
[失败的请求URL、方法、状态代码、响应]
示例：POST /api/login → 401未授权 → {"error": "无效令牌"}]

重现步骤：
1. [确切步骤1]
2. [确切步骤2]
3. [确切步骤3]
→ 错误出现在步骤[X]

环境：
- 浏览器：[Chrome 120、Safari 17等]
- 设备：[桌面/移动/平板]
- 操作系统：[macOS、Windows、iOS、Android]

代码位置（如果知道）：
文件：[path/to/file.js:line]

[期望 vs 实际]：
- 期望：[特定行为]
- 实际：[特定行为]

[已尝试]（可选）：
- [尝试] → [结果]

现在，基于这个完整上下文，您能识别根本原因吗？
```

**真实示例：**

```
[我试图做什么]：
提交包含用户姓名、电子邮件和消息的联系表单

[实际发生了什么]：
表单不提交，不向用户显示错误消息

[我收集的上下文]：

控制台错误：
TypeError: Cannot read property 'value' of null
    at submitForm (contact.js:45)
    at HTMLButtonElement.onclick (contact.html:89)

网络请求：
没有发出网络请求（表单从未到达提交）

重现步骤：
1. 填写姓名字段："John Doe"
2. 填写电子邮件字段："john@example.com"
3. 填写消息字段："测试消息"
4. 点击"提交"按钮
→ 控制台出现错误，视觉上没有任何反应

环境：
- 浏览器：Chrome 122
- 设备：桌面
- 操作系统：macOS Sonoma 14.2

代码位置：
文件：src/contact.js:45

[期望 vs 实际]：
- 期望：表单提交，显示成功消息
- 实际：没有任何反应，控制台中有静默错误

[已尝试]：
- 检查表单HTML - 所有字段都有正确的ID
- 验证按钮onclick处理程序已连接

现在，基于这个完整上下文，您能识别根本原因吗？
```

**为什么这有效：**

没有上下文：
- AI做20个猜测
- 您尝试20个随机修复
- 浪费几小时
- 没有任何效果

有上下文：
- AI立即识别根本原因
- 一个有针对性的修复
- 几分钟内解决问题

**⚠️ 如果您跳过上下文收集，AI将浪费您的时间猜测。始终首先使用此模式。**

**📖 相关：** [AI调试中的人类上下文问题](../../troubleshooting/human-context-debugging.md)

---

### 模式1：运行时错误调试

**何时使用**：应用程序在执行期间崩溃或抛出错误

**模板：**

```
[问题]：简要描述

[错误消息]：
[确切错误消息]

[堆栈跟踪]：
[相关堆栈跟踪行]

[上下文]：
- 文件：[错误发生的路径:行]
- 操作：[用户在做什么]
- 环境：[开发/暂存/生产]
- 开始发生：[何时]

[期望 vs 实际]：
- 期望：[行为]
- 实际：[行为]

[已尝试]：
- [尝试1] → [结果]
- [尝试2] → [结果]

[相关代码]：
[粘贴或引用]
```

**真实示例1：空指针错误**

**糟糕的提示：**
```
"修复登录中的错误"
```

**好的提示：**
```
"修复登录流程中的空指针错误。

错误消息：
TypeError: Cannot read property 'id' of undefined
 at handleLogin (src/api/auth.ts:45:31)

堆栈跟踪：
  at handleLogin (src/api/auth.ts:45:31)
  at validateAndLogin (src/api/auth.ts:23:12)
  at /src/middleware/validation.ts:15:5

上下文：
- 文件：src/api/auth.ts第45行
- 操作：用户提交了有效电子邮件/密码的登录表单
- 环境：开发
- 开始发生：昨天我们添加电子邮件验证功能后
- 只影响emailVerified: false的用户
- emailVerified: true的用户可以正常登录

代码（src/api/auth.ts第40-50行）：
```typescript
async function handleLogin(req, res) {
  const { email, password } = req.body;

  const user = await User.findByEmail(email);
  const isValid = await bcrypt.compare(password, user.password);

  if (!isValid) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  const token = jwt.sign({ id: user.id }, SECRET); // 第45行 - 此处错误
 return res.json({ token });
}
```

期望 vs 实际：
- 期望：为未验证用户返回401或允许登录并提示验证
- 实际：500错误崩溃

已尝试：
- 在第45行前添加console.log → 未验证电子邮件的用户是undefined
- 检查User.findByEmail → 对于未验证用户返回null（正确）
- 查看最近更改 → 电子邮件验证PR修改了User.findByEmail

假设：
User.findByEmail现在对于未验证用户返回null，但我们在访问user.id之前没有检查null。

请修复此问题并正确处理未验证用户（返回清晰的错误消息）。"
```

**为什么这有效：**
- ✅ 提供确切错误消息
- ✅ 堆栈跟踪显示错误位置
- ✅ 上下文缩小原因范围
- ✅ 包含代码片段
- ✅ 已尝试显示已完成的调查
- ✅ 假设指导AI

**AI响应：**
```
"您的假设正确。User.findByEmail对于未验证用户返回null。

修复：
```typescript
async function handleLogin(req, res) {
  const { email, password } = req.body;

  const user = await User.findByEmail(email);

  // 修复：检查用户是否存在
  if (!user) {
    return res.status(401).json({
      error: 'Email not verified or invalid credentials'
    });
  }

  const isValid = await bcrypt.compare(password, user.password);

  if (!isValid) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  const token = jwt.sign({ id: user.id }, SECRET);
  return res.json({ token });
}
```
```

**结果：**一次迭代中修复错误，有清晰解释。

---

**真实示例2：异步/竞争条件错误**

**糟糕的提示：**
```
"数据有时加载不正确"
```

**好的提示：**
```
"修复导致用户配置文件中陈旧数据的竞争条件。

问题：
用户配置文件在更新后有时显示过时信息。约30%的时间发生。

错误消息：
没有抛出错误，但数据不正确。

上下文：
- 文件：src/pages/UserProfile.tsx
- 操作：用户更新配置文件（姓名/电子邮件）→ 导航到配置文件页面
- 环境：生产
- 开始发生：我们从REST切换到React Query的乐观更新后

期望 vs 实际：
- 期望：配置文件页面立即显示更新的姓名/电子邮件
- 实际：有时显示旧数据，刷新显示新数据

重现：
1. 更新配置文件姓名
2. 点击保存
3. 立即导航到配置文件页面（保存后<100ms）
4. 约30%时间：仍显示旧姓名

相关代码：

src/pages/UserProfile.tsx：
```typescript
function UserProfile() {
  const { data: user } = useQuery(['user', userId], fetchUser);
  return <div>{user?.name}</div>;
}
```

src/api/userApi.ts：
```typescript
export async function updateUser(userId, data) {
  const response = await fetch(`/api/users/${userId}`, {
    method: 'PUT',
    body: JSON.stringify(data)
  });
  return response.json();
}
```

src/hooks/useUpdateUser.ts：
```typescript
export function useUpdateUser() {
  const queryClient = useQueryClient();

  return useMutation(updateUser, {
    onSuccess: (data, variables) => {
      // 乐观更新
      queryClient.setQueryData(['user', variables.userId], data);
    }
  });
}
```

已尝试：
- 添加console.logs → 确认API返回新数据
- React Query开发工具 → 有时在缓存中显示旧数据
- 延迟导航500ms → 问题消失（但UX差）

假设：
竞争条件：配置文件页面查询在变更更新缓存之前运行，从服务器获取旧数据，用陈旧数据覆盖缓存。

我们如何确保变更在配置文件页面查询运行之前更新缓存？"
```

**为什么这有效：**
- ✅ 清楚描述间歇性错误
- ✅ 提供重现步骤
- ✅ 来自多个文件的相关代码
- ✅ 包含调查结果
- ✅ 清楚假设
- ✅ 特定问题

**AI响应：**
```
"您的假设正确。典型的React Query竞争条件。

问题：配置文件页面查询独立运行并覆盖乐观更新。

解决方案：使查询无效而不是手动设置数据：

```typescript
export function useUpdateUser() {
  const queryClient = useQueryClient();

  return useMutation(updateUser, {
    onSuccess: (data, variables) => {
      // 而不是setQueryData，使无效以触发重新获取
      queryClient.invalidateQueries(['user', variables.userId]);
    }
  });
}
```

这确保：
1. 变更完成
2. 缓存标记为陈旧
3. 任何活动查询重新获取新数据
4. 无竞争条件

替代方案（如果重新获取太慢）：
```typescript
onMutate: async (variables) => {
  // 取消传出查询
  await queryClient.cancelQueries(['user', variables.userId]);

  // 快照当前数据
  const snapshot = queryClient.getQueryData(['user', variables.userId]);

  // 乐观更新
  queryClient.setQueryData(['user', variables.userId], (old) => ({
    ...old,
    ...variables.data
  }));

  return { snapshot };
},
onError: (err, variables, context) => {
  // 错误时回滚
  queryClient.setQueryData(['user', variables.userId], context.snapshot);
},
onSettled: (data, error, variables) => {
  // 重新获取以确保一致性
  queryClient.invalidateQueries(['user', variables.userId]);
}
```

这取消竞争查询并处理回滚。"
```

**结果：**提供两个解决方案 - 简单修复和健壮的乐观更新模式。竞争条件消除。

---

### 模式2：逻辑错误调试

**何时使用**：代码运行但产生错误结果

**模板：**

```
[错误]：什么错了

[期望行为]：
[详细描述]

[实际行为]：
[详细描述]

[测试用例]：
输入：[数据]
期望输出：[结果]
实际输出：[结果]

[代码]：
[相关函数]

[分析]：
[您发现了什么]
```

**真实示例3：计算错误**

**糟糕的提示：**
```
"总计错了"
```

**好的提示：**
```
"修复购物车中不正确的总计计算。

错误：
应用折扣代码时，购物车总计与商品价格总和不匹配。

期望行为：
1. 计算小计：(item.price × item.quantity)的总和
2. 应用折扣：小计 × (discount.percentage / 100)
3. 计算总计：小计 - 折扣
4. 添加税：总计 × 1.08（8%税）
5. 最终总计：总计 + 税

实际行为：
应用折扣代码时，税在小计上计算（折扣前）而不是折扣后的总计上。

测试用例：
输入：
- 商品：2 × $10产品 = $20小计
- 折扣代码：50%折扣
- 期望折扣：$10
- 期望税前总计：$10
- 期望税（$10的8%）：$0.80
- 期望最终总计：$10.80

实际：
- 小计：$20 ✓
- 折扣：$10 ✓
- 税前总计：$10 ✓
- 税：$1.60（$20的8% - 错误，应该是$10的8%）
- 最终总计：$11.60（错误，应该是$10.80）

代码（src/utils/cartCalculations.ts）：
```typescript
export function calculateCartTotal(items, discountCode) {
  const subtotal = items.reduce((sum, item) => {
    return sum + (item.price * item.quantity);
  }, 0);

  let discount = 0;
  if (discountCode) {
    discount = subtotal * (discountCode.percentage / 100);
  }

  const total = subtotal - discount;
  const tax = subtotal * 0.08; // 错误：应该是total * 0.08

  return {
    subtotal,
    discount,
    total,
    tax,
    finalTotal: total + tax
  };
}
```

分析：
第16行：`const tax = subtotal * 0.08`应该是`const tax = total * 0.08`。

税应该在折扣后的总计上计算，而不是原始小计。

请修复此错误并添加测试用例以防止回归。"
```

**为什么这有效：**
- ✅ 清楚的期望 vs 实际行为
- ✅ 带数字的具体测试用例
- ✅ 包含错误位置的代码
- ✅ 识别根本原因
- ✅ 请求测试以防止回归

**结果：**立即修复错误，添加测试，无未来回归。

---

## 重构

### 概述

**目标**：在不改变行为的情况下改进代码结构。

**关键原则**：清楚说明什么改变，什么保持不变。

**成功模式：**

```
当前问题 + 期望状态 + 迁移路径 + 行为保持
= 安全、有效的重构
```

---

### 模式1：组件重构

**何时使用**：简化或重构组件代码

**模板：**

```
将[COMPONENT]从[OLD_APPROACH]重构为[NEW_APPROACH]。

当前问题：
- [问题1]
- [问题2]

期望状态：
- [目标1]
- [目标2]

必须保持：
- [行为1]
- [行为2]

允许的更改：
- [更改1]
- [更改2]

成功标准：
- 测试仍然通过
- [特定要求]
```

**真实示例1：类到钩子**

**糟糕的提示：**
```
"将此组件转换为使用钩子"
```

**好的提示：**
```
"将UserProfile组件从类组件重构为带有钩子的函数组件。

文件：src/components/UserProfile.tsx

当前问题：
- 类组件冗长（200行）
- 生命周期方法分散（componentDidMount、componentDidUpdate、componentWillUnmount）
- 生命周期方法之间逻辑重复
- 难以提取可重用逻辑
- setState调用难以跟踪

期望状态：
- 带钩子的函数组件
- useEffect用于生命周期逻辑（合并）
- 可重用逻辑的自定义钩子（用户获取、订阅）
- 更干净、更可读的代码
- 重构后约100行

必须保持：
- 所有功能完全相同
- Props接口不变（其他组件依赖它）
- 卸载时订阅清理（关键 - 无内存泄漏）
- 加载/错误状态工作相同
- 组件仍默认导出UserProfile

重构方法：
1. 将状态转换为useState钩子
2. 转换componentDidMount + componentDidUpdate → useEffect
3. 转换componentWillUnmount清理 → useEffect清理
4. 提取用户获取 → 自定义useUser钩子
5. 提取订阅 → 自定义useUserSubscription钩子

当前代码：
```typescript
class UserProfile extends React.Component<Props, State> {
  subscription: any = null;

  state = {
    user: null,
    loading: true,
    error: null
 };

  componentDidMount() {
    this.fetchUser();
    this.setupSubscription();
  }

  componentDidUpdate(prevProps) {
    if (prevProps.userId !== this.props.userId) {
      this.fetchUser();
      this.teardownSubscription();
      this.setupSubscription();
    }
  }

  componentWillUnmount() {
    this.teardownSubscription();
  }

  fetchUser = async () => {
    this.setState({ loading: true, error: null });
    try {
      const user = await api.getUser(this.props.userId);
      this.setState({ user, loading: false });
    } catch (error) {
      this.setState({ error, loading: false });
    }
  };

  setupSubscription = () => {
    this.subscription = userUpdates.subscribe(
      this.props.userId,
      (update) => {
        this.setState({ user: update });
      }
    );
  };

  teardownSubscription = () => {
    if (this.subscription) {
      this.subscription.unsubscribe();
      this.subscription = null;
    }
  };

  render() {
    const { user, loading, error } = this.state;

    if (loading) return <Loading />;
    if (error) return <Error error={error} />;
    if (!user) return null;

    return (
      <div className="profile">
        <Avatar src={user.avatar} />
        <h1>{user.name}</h1>
        <p>{user.email}</p>
      </div>
    );
  }
}
```

要创建的自定义钩子：
1. useUser(userId) → { user, loading, error }
2. useUserSubscription(userId, onUpdate) → 处理订阅生命周期

成功标准：
- 组件工作相同
- 测试无需修改通过
- 订阅清理验证（使用React DevTools检查）
- 代码<100行
- 逻辑提取到可重用自定义钩子
- ESLint通过（无警告）"
```

**为什么这有效：**
- ✅ 识别清楚问题
- ✅ 描述期望结果
- ✅ 列出关键行为保持
- ✅ 概述重构方法
- ✅ 提供当前代码
- ✅ 成功标准可测量

**结果：**干净重构，保持所有功能，改进代码质量，提取可重用钩子。

---

### 模式2：架构重构

**何时使用**：重构多个文件或模块

**模板：**

```
将[SYSTEM]架构从[OLD]重构为[NEW]。

基本原理：
[为什么需要重构]

范围：
受影响文件：[列表]
不更改文件：[列表]

迁移计划：
阶段1：[步骤]
阶段2：[步骤]
阶段3：[步骤]

对于此提示：实施阶段[N]

[详细阶段要求]

向后兼容性：
[迁移期间如何维护]
```

**真实示例2：服务层引入**

**糟糕的提示：**
```
"添加服务层模式"
```

**好的提示：**
```
"将API路由重构为使用服务层模式。3阶段中的第1阶段。

基本原理：
当前问题：
- 业务逻辑混合在路由处理程序中
- 无代码重用 between 路由
- 难以测试（需要HTTP模拟）
- 难以添加新API版本
- 事务管理分散

期望状态：
- 路由 → 薄控制器（验证、响应格式化）
- 服务 → 业务逻辑（可重用、可测试）
- 仓库 → 数据访问
- 清晰的关注点分离

架构：

当前：
```
routes/userRoutes.ts → 直接数据库访问
```

新：
```
routes/userRoutes.ts → services/UserService.ts → repositories/UserRepository.ts
```

范围：

阶段1（此提示）：用户模块
- 重构src/api/users.ts路由
- 创建src/services/UserService.ts
- 创建src/repositories/UserRepository.ts

阶段2（稍后）：帖子模块
阶段3（稍后）：评论模块

受影响文件（阶段1）：
- src/api/users.ts（路由 - 修改）
- src/services/UserService.ts（新）
- src/repositories/UserRepository.ts（新）
- src/models/User.ts（不变，仅引用）

尚未更改的文件：
- 所有其他路由（帖子、评论等）
- 数据库连接（src/database/connection.ts）
- 中间件（验证、身份验证）

阶段1要求：

1. 创建src/repositories/UserRepository.ts：
```typescript
export class UserRepository {
  async findById(id: string): Promise<User | null>
  async findByEmail(email: string): Promise<User | null>
  async create(data: CreateUserDTO): Promise<User>
 async update(id: string, data: UpdateUserDTO): Promise<User>
  async delete(id: string): Promise<void>
  async findMany(filters: UserFilters): Promise<User[]>
}
```

2. 创建src/services/UserService.ts：
```typescript
export class UserService {
  constructor(private userRepo: UserRepository) {}

 async createUser(data: CreateUserDTO): Promise<User>
  async updateUser(id: string, data: UpdateUserDTO): Promise<User>
 async deleteUser(id: string): Promise<void>
  async getUser(id: string): Promise<User>
  async listUsers(filters: UserFilters): Promise<User[]>
}
```

业务逻辑放在这里：
- 电子邮件唯一性检查
- 密码哈希
- 超出架构验证的验证
- 欢迎电子邮件发送

3. 重构src/api/users.ts：
更新路由以使用UserService而不是直接数据库调用。

保留在路由中：
- 请求验证（express-validator）
- 响应格式化
- HTTP状态代码
- 错误处理（try-catch）

移动到服务：
- 数据库查询
- 业务逻辑
- 事务管理

示例转换：

之前（src/api/users.ts）：
```typescript
router.post('/users', async (req, res) => {
  try {
    const { email, password, name } = req.body;

    // 业务逻辑混合在路由中
    const existingUser = await db.users.findOne({ email });
    if (existingUser) {
      return res.status(400).json({ error: 'Email already exists' });
    }

    const hashedPassword = await bcrypt.hash(password, 10);

    const user = await db.users.create({
      email,
      password: hashedPassword,
      name
    });

    await emailService.sendWelcomeEmail(user.email);

    return res.status(201).json(user);
  } catch (error) {
    return res.status(500).json({ error: 'Internal server error' });
 }
});
```

之后（src/api/users.ts）：
```typescript
router.post('/users', async (req, res) => {
  try {
    const { email, password, name } = req.body;

    // 路由只是编排
    const user = await userService.createUser({ email, password, name });

    return res.status(201).json(user);
  } catch (error) {
    if (error instanceof ValidationError) {
      return res.status(400).json({ error: error.message });
    }
    return res.status(500).json({ error: 'Internal server error' });
 }
});
```

UserService.createUser：
```typescript
async createUser(data: CreateUserDTO): Promise<User> {
 // 业务逻辑现在在服务中
  const existingUser = await this.userRepo.findByEmail(data.email);
  if (existingUser) {
    throw new ValidationError('Email already exists');
  }

  const hashedPassword = await bcrypt.hash(data.password, 10);

  const user = await this.userRepo.create({
    ...data,
    password: hashedPassword
  });

  await emailService.sendWelcomeEmail(user.email);

  return user;
}
```

向后兼容性：
- 路由保持相同API契约（无破坏性更改）
- 响应格式不变
- 状态代码不变
- 错误消息不变

测试：
- 现有路由测试应仍通过
- 添加新服务单元测试
- 添加新仓库单元测试

成功标准：
- 所有用户路由重构为使用UserService
- 业务逻辑移动到服务层
- 数据访问移动到仓库层
- 现有API测试无需修改通过
- 添加新服务/仓库测试
- 代码更可维护和可测试"
```

**为什么这有效：**
- ✅ 清楚重构基本原理
- ✅ 分阶段方法防止压倒
- ✅ 范围明确定义
- ✅ 提供前后示例
- ✅ 考虑向后兼容性
- ✅ 包含测试策略

**结果：**系统化重构，改进架构同时保持功能。可以分阶段应用于其他模块。

---

## 代码审查

### 概述

**目标**：让AI根据特定标准彻底审查代码。

**关键原则**：提供上下文和清单以进行专注审查。

**成功模式：**

```
要审查的代码 + 审查标准 + 上下文 + 特定关注点
= 彻底、可操作的审查
```

---

### 模式1：拉取请求审查

**何时使用**：合并前审查代码更改

**模板：**

```
审查PR #[NUMBER]：[TITLE]

更改的文件：
[列表]

审查标准：
- [标准1]
- [标准2]

上下文：
[这是什么功能/修复]

特定关注点：
[要关注的区域]

输出格式：
- 发现的问题（按严重性分类）
- 安全关注点
- 建议改进
```

**真实示例1：功能PR审查**

**糟糕的提示：**
```
"审查此PR"
```

**好的提示：**
```
"审查PR #142：添加密码重置功能

更改的文件：
- src/api/auth/passwordReset.ts（新，150行）
- src/models/User.ts（修改，添加resetToken字段）
- src/services/EmailService.ts（修改，添加resetEmail方法）
- src/middleware/rateLimit.ts（修改，添加重置端点限制）
- tests/auth/passwordReset.test.ts（新，80行）

审查标准：

安全（高优先级）：
- [ ] 重置令牌是加密随机的
- [ ] 令牌适当过期
- [ ] 无时序攻击（比较时序安全）
- [ ] 速率限制正确配置
- [ ] 电子邮件通过安全通道发送
- [ ] 令牌是一次性使用
- [ ] 新请求时旧令牌无效

代码质量：
- [ ] 遵循我们的API模式（参见src/api/auth/login.ts）
- [ ] 错误处理全面
- [ ] 验证适当（电子邮件格式等）
- [ ] 测试覆盖边缘情况
- [ ] 日志中无敏感数据

业务逻辑：
- [ ] 电子邮件包含安全重置链接
- [ ] 链接只工作一次
- [ ] 过期链接显示适当消息
- [ ] 成功/失败不透露电子邮件是否存在（安全）

性能：
- [ ] 数据库查询优化（无n+1）
- [ ] 昂贵操作（令牌生成）高效完成

上下文：
这实现用户请求的通过电子邮件的密码重置。用户输入电子邮件 → 收到链接 → 点击链接 → 设置新密码。

特定关注点：
1. 安全在这里至关重要 - 这是身份验证相关的
2. 确保我们不泄露系统中电子邮件是否存在的信息
3. 速率限制必须防止滥用
4. 令牌生成必须安全（不可预测）
5. 检查测试是否覆盖安全边缘情况

输出格式：
请提供：

1. 严重问题（合并前必须修复）
2. 安全关注点（即使是小的）
3. 代码质量问题（值得修复）
4. 测试覆盖缺口
5. 总体评估（批准/请求更改）"
```

**为什么这有效：**
- ✅ 特定文件列出
- ✅ 详细审查清单
- ✅ 对功能的安全关注适当
- ✅ 上下文解释功能做什么
- ✅ 特定关注点突出
- ✅ 输出格式结构化

**结果：**彻底审查，识别安全问题、代码质量问题和测试缺口，人类审查者可能错过的。

---

## 性能优化

### 概述

**目标**：让AI识别和修复性能瓶颈。

**关键原则**：提供性能数据，而不是仅代码。

---

### 模式1：性能分析

**真实示例：**

**好的提示：**
```
"优化慢速用户列表页面性能。

问题：
页面加载1000个用户需要3秒。目标：<500ms。

性能数据：
- 网络标签：GET /api/users需要2800ms
- React DevTools：UserList渲染需要400ms
- 数据库查询时间：2500ms（来自日志）

文件：
- src/api/users.ts（API端点）
- src/components/UserList.tsx（前端组件）
- src/database/queries/users.ts（数据库查询）

当前实现：

API（src/api/users.ts）：
```typescript
router.get('/api/users', async (req, res) => {
  const users = await User.findAll({
    include: [Profile, Posts, Comments] // 急切加载
  });
  return res.json(users);
});
```

组件（src/components/UserList.tsx）：
```typescript
function UserList() {
  const { data: users } = useQuery('users', fetchUsers);

  return (
    <div>
      {users.map(user => (
        <UserCard
          key={user.id}
          user={user}
          posts={user.posts}
          profile={user.profile}
        />
      ))}
    </div>
  );
}
```

数据库查询日志显示：
- 1个查询用于用户：100ms
- 1000个查询用于配置文件：200ms（n+1问题！）
- 1000个查询用于帖子：400ms（n+1问题！）

约束：
- 必须仍显示用户配置文件数据
- 必须显示每个用户的帖子计数
- 不需要单个帖子详细信息
- 分页是可以接受的（如果需要）
- 虚拟滚动是可以接受的
- 必须与现有数据库（PostgreSQL）一起工作

优化想法：
1. 修复n+1查询（急切加载或单独连接查询）
2. 添加分页（每页20个用户）
3. 在前端使用虚拟滚动
4. 缓存结果（React Query已配置）
5. 仅返回所需字段（不是完整帖子对象）

哪个是最好的方法？请实施最有效的解决方案。"
```

**结果：**AI识别n+1问题，建议分页+正确急切加载，实施解决方案。

---

## 测试

### 概述

**目标**：让AI编写全面、有意义的测试。

---

### 模式1：测试生成

**真实示例：**

**好的提示：**
```
"为UserService.createUser方法编写全面测试。

要测试的代码（src/services/UserService.ts）：
```typescript
async createUser(data: CreateUserDTO): Promise<User> {
 // 验证电子邮件格式
  if (!isValidEmail(data.email)) {
    throw new ValidationError('Invalid email format');
  }

  // 检查电子邮件是否已存在
  const existingUser = await this.userRepo.findByEmail(data.email);
  if (existingUser) {
    throw new ConflictError('Email already exists');
  }

  // 哈希密码
  const hashedPassword = await bcrypt.hash(data.password, 10);

  // 创建用户
  const user = await this.userRepo.create({
    ...data,
    password: hashedPassword
  });

  // 发送欢迎电子邮件（异步，不等待）
  emailService.sendWelcomeEmail(user.email).catch(err => {
    logger.error('Failed to send welcome email', err);
 });

  return user;
}
```

测试文件：tests/services/UserService.test.ts

测试框架：Jest
模拟：使用jest.mock()模拟依赖项

要覆盖的场景：

快乐路径：
- [ ] 有效数据 → 成功创建用户
- [ ] 密码被哈希（未存储明文）
- [ ] 发送欢迎电子邮件
- [ ] 返回创建的用户

验证错误：
- [ ] 无效电子邮件格式 → 抛出ValidationError
- [ ] 缺少必填字段 → 抛出ValidationError
- [ ] 空电子邮件 → 抛出ValidationError
- [ ] 空密码 → 抛出ValidationError

冲突错误：
- [ ] 重复电子邮件 → 抛出ConflictError
- [ ] 不区分大小写的电子邮件检查（John@example.com == john@example.com）

边缘情况：
- [ ] 非常长的电子邮件 → 适当处理
- [ ] 名称中的特殊字符 → 适当处理
- [ ] 电子邮件发送失败 → 用户仍创建，错误记录
- [ ] 数据库错误 → 抛出适当错误

安全：
- [ ] 响应中不返回密码
- [ ] 密码实际上是哈希的（不是明文）
- [ ] 哈希是bcrypt格式

测试结构：
```typescript
describe('UserService', () => {
 describe('createUser', () => {
    describe('当数据有效时', () => {
      it('成功创建用户', () => {});
      it('哈希密码', () => {});
      it('发送欢迎电子邮件', () => {});
    });

    describe('当数据无效时', () => {
      it('抛出ValidationError用于无效电子邮件', () => {});
      // ...
    });

    describe('当电子邮件存在时', () => {
      it('抛出ConflictError', () => {});
    });

    describe('边缘情况', () => {
      // ...
    });
  });
});
```

模拟设置：
- 模拟UserRepository方法（findByEmail, create）
- 模拟emailService.sendWelcomeEmail
- 模拟logger
- 模拟bcrypt.hash

请编写遵循此结构的完整、可运行测试。"
```

**结果：**全面测试套件，涵盖所有场景，正确模拟，遵循项目模式。

---

## 文档

### 概述

**目标**：让AI编写清晰、有用的文档。

---

### 模式1：API文档

**真实示例：**

**好的提示：**
```
"生成用户端点的API文档。

要记录的端点：
- POST /api/users（创建用户）
- GET /api/users/:id（获取用户）
- PUT /api/users/:id（更新用户）
- DELETE /api/users/:id（删除用户）
- GET /api/users（列出用户）

文档格式：OpenAPI 3.0（Swagger）

位置：docs/api/users.yaml

要求：

对于每个端点，包括：
- 描述
- 请求体架构（如果适用）
- 查询参数（如果适用）
- 响应架构（成功+错误）
- 示例请求/响应
- 身份验证要求
- 错误代码及解释

具体细节：

POST /api/users:
- 公共端点（无身份验证）
- 必需字段：电子邮件、密码、姓名
- 电子邮件必须唯一
- 密码最小8个字符
- 成功时返回201
- 错误：400（验证）、409（电子邮件存在）

GET /api/users/:id:
- 需要身份验证（JWT令牌）
- 返回用户对象（无密码）
- 错误：401（未授权）、404（未找到）

PUT /api/users/:id:
- 需要身份验证（JWT令牌）
- 只能更新自己的用户（除非管理员）
- 可更新字段：姓名、电子邮件（电子邮件仍必须唯一）
- 错误：401（未授权）、403（禁止）、404（未找到）、409（电子邮件存在）

DELETE /api/users/:id:
- 需要身份验证（JWT令牌）
- 仅管理员
- 软删除（设置deletedAt时间戳）
- 错误：401（未授权）、403（禁止）、404（未找到）

GET /api/users:
- 需要身份验证（JWT令牌）
- 分页：?page=1&limit=20（默认：第1页，限制20）
- 过滤：?search=john（搜索姓名和电子邮件）
- 排序：?sort=name&order=asc（默认：createdAt desc）
- 返回用户数组+分页元数据

使用我们现有的User架构来自src/models/User.ts：
```typescript
interface User {
  id: string;
  email: string;
  name: string;
  role: 'user' | 'admin';
  createdAt: Date;
  updatedAt: Date;
  deletedAt: Date | null;
}
```

示例风格：
遵循docs/api/auth.yaml中使用的格式（我们现有的API文档）。"
```

**结果：**在OpenAPI格式中完成、准确的API文档，可与Swagger UI一起使用。

---

## 安全加固

### 概述

**目标**：让AI识别和修复安全漏洞。

---

### 模式1：安全审计

**真实示例：**

**好的提示：**
```
"身份验证系统的安全审计。

范围：
- src/api/auth/（所有身份验证端点）
- src/middleware/auth.ts（JWT验证）
- src/models/User.ts（用户模型）

审计以下：

身份验证：
- [ ] 密码存储（应该是bcrypt/argon2）
- [ ] 密码最小要求强制执行
- [ ] JWT密钥强且为环境变量
- [ ] JWT过期适当设置
- [ ] 刷新令牌机制安全

授权：
- [ ] 基于角色的访问控制强制执行
- [ ] 用户只能访问自己的资源
- [ ] 管理员路由正确保护

输入验证：
- [ ] SQL注入预防
- [ ] XSS预防（输入清理）
- [ ] 电子邮件验证
- [ ] 参数验证

会话管理：
- [ ] 会话适当过期
- [ ] 注销使令牌无效
- [ ] 并发会话处理

速率限制：
- [ ] 登录端点速率限制
- [ ] 密码重置速率限制
- [ ] API端点速率限制

信息泄露：
- [ ] 错误不泄露敏感信息
- [ ] 堆栈跟踪不在生产中返回
- [ ] 计时攻击预防（等时比较）

当前实现：
[包含相关代码文件]

请提供：
1. 严重漏洞（必须立即修复）
2. 中等优先级问题
3. 最佳实践改进
4. 需要的具体代码更改

对于每个问题：
- 漏洞是什么
- 如何利用它
- 如何修复
- 修复的代码示例"
```

**结果：**详细安全审计，识别漏洞、利用场景和具体修复。

---

## 总结

### 何时使用每种模式

| 任务类型 | 何时使用 | 关键元素 |
|-----------|----------|--------------|
| **功能实施** | 构建新功能 | 要求 + 上下文 + 模式 + 示例 |
| **调试** | 修复错误/错误 | 错误 + 堆栈跟踪 + 上下文 + 已尝试 |
| **重构** | 改进代码结构 | 问题 + 目标 + 保持 + 迁移 |
| **代码审查** | 审查更改 | 文件 + 标准 + 上下文 + 关注点 |
| **测试** | 编写测试 | 代码 + 场景 + 结构 + 模拟 |
| **文档** | 编写文档 | 范围 + 格式 + 详细信息 + 示例 |
| **性能** | 优化速度 | 数据 + 瓶颈 + 约束 + 目标 |
| **安全** | 加固安全 | 范围 + 清单 + 标准 + 严重性 |

---

**下一步：**
- [高级技术](./advanced-techniques.md) → 多步骤工作流程、提示链接
- [模板库](./template-library.md) → 即用型提示
- [基础](./foundations.md) → 核心提示原则

**返回：** [提示指南主页](./README.md)