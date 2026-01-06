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