# OpenSpec CLI — 规范框架 📐

OpenSpec CLI是一个强大的规范驱动开发工具，将简单的语言描述转换为综合项目计划、任务和技术规范。它通过创建结构化、可编辑的文档来指导开发过程，同时保持对话式vibecoding方法，弥合了想法和实施之间的差距。

## 🎯 核心价值主张

### 规范驱动的开发

OpenSpec的魔力在于它能够将用户描述转换为完整的开发框架：

1. **简单语言输入** → 用自然语言描述您想要构建的内容
2. **AI驱动生成** → OpenSpec自动创建提案、任务和详细规范
3. **Markdown文档** → 所有内容都保存为结构化.md文件，以便于编辑和审查
4. **自然语言交互** → 在整个过程中继续与您的代理进行vibecoding
5. **分阶段实施** → 大型功能分解为可管理的阶段

**您的初始提示越具描述性，输出质量越好。**

### 通用集成

OpenSpec几乎可以与任何编码代理无缝工作：
- **Zed**（通过AGENTS.md文件）
- **Claude**（通过CLAUDE.md指令）
- **Droid CLI**（通过自定义指令文件）
- **任何AI编码代理**，可以遵循基于文件的指令

## 🚀 主要用例

### 1. 从头开始功能开发

**工作流程：**
```
用户："I want to build a comprehensive user authentication system with social login, email verification, role-based access control, and admin dashboard"

OpenSpec输出：
- 阶段1：数据库架构设计和核心身份验证
- 阶段2：社交登录集成（Google、GitHub等）
- 阶段3：电子邮件验证和密码重置
- 阶段4：基于角色的访问控制实施
- 阶段5：管理仪表板和用户管理
```

**结果**：5个阶段中15-20个详细任务，每个都有技术规范和实施指导。

### 2. 大规模项目规划

OpenSpec擅长将复杂项目分解为可消化的组件：

- **电子商务平台** → 支付处理、库存管理、用户账户、管理面板
- **SaaS应用程序** → 多租户、计费、分析、用户入职
- **移动应用程序后端** → API设计、数据库架构、身份验证、实时功能

### 3. 重构和系统升级

非常适合规划主要架构更改：
```
"I need to migrate from REST to GraphQL, implement caching layer, and redesign the database schema"
```

### 4. 快速原型设计

快速生成MVP和概念验证的规范：
```
"I need a simple task management app with drag-and-drop, real-time updates, and team collaboration features"
```

## 💡 关键功能

### 1. 持久上下文管理

所有规范都保存为`.md`文件，提供：
- **版本控制** → 随时间跟踪规范的更改
- **上下文保留** → AI代理保持对项目范围的理解
- **易于审查** → 用于团队协作的人类可读文档
- **可编辑工作流程** → 根据需要修改任务和规范

### 2. 基于阶段的开发

OpenSpec自动将大型功能分解为逻辑阶段：
- **基础阶段** → 核心架构和数据库设计
- **功能阶段** → 单个功能实施
- **集成阶段** → 连接组件和测试
- **完善阶段** → UI/UX完善和优化

### 3. 多LLM工作流程支持

OpenSpec最强大的功能之一是启用不同LLM用于不同开发阶段：

**示例工作流程：**
```
阶段1：规划和规范
→ 使用GPT-5-high（通过Droid CLI）进行复杂架构决策
→ 生成综合计划和详细规范

阶段2：实施
→ 切换到GLM-4.6以进行具有成本效益的开发
→ 按照批准的规范执行单个任务

结果：优质质量规划 + 预算友好的实施
```