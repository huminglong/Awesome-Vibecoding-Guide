# Claude Code CLI — 主要编码代理 🚀

> **主要驱动**：Claude Code CLI已成为我的主要编码代理，归功于计划模式、多代理架构和全局计划持久性的最新改进。

## 为什么Claude Code CLI现在是#1

Claude Code的最新版本（v2.0.28+）引入了从根本上改变我处理AI辅助开发方式的功能。改进的规划、多代理协调和高效代码库探索的组合使其在大多数日常编码任务中更优越。

### 主要区别

**计划模式超能力：**
- 计划现在全局保存并在会话之间引用
- 专用计划子代理构建更精确、可操作的计划
- 计划作为markdown文件持久存在，用于审查、共享和迭代
- 在知道上下文得到保留的情况下执行批准的计划

**多代理架构：**
- 为不同任务启动专用子代理（探索、计划、实施）
- 子代理可以恢复，在交互之间保持上下文
- 动态模型选择 — 为每个子任务使用正确的模型
- 并行代理执行以获得最大效率

**探索子代理：**
- 由Haiku驱动的高效代码库搜索
- 通过智能导航大型代码库减少上下文开销
- 在不将所有内容加载到上下文的情况下找到相关文件和模式
- 非常适合快速理解不熟悉的代码库

**插件和可扩展性：**
- 用于自定义命令、代理和钩子的插件系统（v2.0.12+）
- MCP服务器市场集成
- 团队可共享的配置和工作流程
- 在不离开CLI的情况下扩展功能

## 与提供商集成

Claude Code CLI通过在系统级别覆盖Anthropic API端点与高性价比提供商一起工作。

**主要设置（GLM Coding Plan）：**

GLM提供与Claude Code CLI一起工作的OpenAI兼容API。查看官方设置指南：
- **[GLM + Claude Code集成指南](https://docs.z.ai/devpack/tool/claude)**

**次要设置（Synthetic.new）：**

Synthetic.new也提供Claude Code兼容性。查看：
- **[Synthetic + Claude Code集成指南](https://dev.synthetic.new/docs/guides/claude-code)**

**它是如何工作的：**
两个提供商都需要在系统级别覆盖`ANTHROPIC_BASE_URL`和`ANTHROPIC_API_KEY`环境变量。上面的文档介绍了确切的设置步骤。

**注意**：在提供商之间切换时（GLM ↔ Synthetic），您需要重新配置环境变量。

## 何时使用Claude Code与Droid

| 场景                   | 使用Claude Code | 使用Droid  |
| ---------------------- | --------------- | ---------- |
| 快速迭代和紧密循环     | ✅ 最佳选择      | 过度       |
| 单文件更改和重构       | ✅ 最佳选择      | 过度       |
| 多文件功能             | ✅ 最佳选择      | 好的选择   |
| 探索新代码库           | ✅ 探索子代理    | 手动       |
| 具有正式规范的大型项目 | 考虑            | ✅ 最佳选择 |
| 具有阶段的团队协调     | 考虑            | ✅ 最佳选择 |
| 规范驱动的企业工作     | 考虑            | ✅ 最佳选择 |

**经验法则**：大多数工作从Claude Code开始。当您需要正式规范阶段或结构化团队工作流程时切换到Droid。

## 最大生产力的专业提示

### 1. 对复杂任务使用计划模式
```bash
# 在实施之前，让Claude规划它
claude --plan "Add user authentication with OAuth2"
# 审查计划，批准，然后执行
```

### 2. 利用探索子代理
不要手动grep代码库。让探索子代理找到相关文件：
```bash
claude "Where is the authentication middleware implemented?"
# 探索子代理高效搜索而不加载整个代码库
```

### 3. 保存和引用计划
计划全局持久存在 — 在未来的会话中引用它们：
```bash
claude "Continue implementing the auth plan from yesterday"
# Claude自动引用保存的计划
```

### 4. 并行代理执行
对于独立任务，启动多个代理：
```bash
claude "Run tests AND check for linting issues"
# 独立任务并行运行以获得更快反馈
```

### 5. 为您的工作流程配置

提供商切换需要更新环境变量。一个简单的方法：