# 开发工具 🛠️

为vibecoding生产力和效率优化的开发工具综合集合。这些工具构成了现代、简化的开发工作流程的骨干。

## 核心开发栈

### 推荐工具
为vibecoding生产力优化的开发工具：
- **[Claude Code CLI](./recommended-tools/claude-code-cli-zh.md)** — 具有计划模式、多代理架构和全局计划持久性的主要编码代理
- **[Mistral Vibe CLI](./recommended-tools/mistral-vibe-cli-zh.md)** — 由Devstral 2驱动的开源编码代理（目前免费！）
- **[Droid CLI](./recommended-tools/droid-cli-zh.md)** — 用于大型项目和团队协调的规范驱动开发代理
- **[Zed.dev](./recommended-tools/zed-zh.md)** — 用于快速、现代开发的主要IDE
- **[Windsurf](./recommended-tools/windsurf-zh.md)** — 具有950 tok/s SWE-1.5代理的高速AI IDE
- **[Clavix](./recommended-tools/clavix-zh.md)** — 基于CLEAR的PRD生成器，具有无缝任务列表创建
- **[Warp](./recommended-tools/warp-zh.md)** — 用于增强生产力的现代终端
- **[TRAE](./recommended-tools/trae-zh.md)** — 统一IDE + SOLO自动化，Gemini 3支持，慷慨的配额

### MCP服务器
用于增强功能的模型上下文协议服务器：
- **[Context7 MCP](./mcp-servers/context7-mcp-zh.md)** — 文档管理和检索
- **[DevTools MCP](./mcp-servers/devtools-mcp-zh.md)** — 浏览器自动化和测试
- **[Sequential Thinking MCP](./mcp-servers/sequential-thinking-mcp-zh.md)** — AI助手的结构化认知增强
- **[Task Manager MCP](./mcp-servers/task-manager-mcp-zh.md)** — 跨对话会话的持久任务管理
- **[Shadcn MCP](./mcp-servers/shadcn-mcp-zh.md)** — 用于AI辅助开发的专业UI组件

### 其他资源
- **[我放弃的工具](./tools-i-dropped-zh.md)** — 以前使用的工具和迁移理由

## 配置和兼容性

- **[工具兼容性](./compatibility-zh.md)** — 集成指南和BYOK要求
- **[核心技术](../core-technologies-zh.md)** — 推荐技术栈（Astro、Tailwind、Cloudflare Pages）

## 免费和替代选项

- **[荣誉提及](./honorable-mentions/README-zh.md)** — 免费替代方案和高性价比选项
  - [Kilo Code](./honorable-mentions/kilocode-zh.md) • [Qwen Coder](./honorable-mentions/qwen-coder-zh.md) • [Gemini CLI](./honorable-mentions/gemini-cli-zh.md) • [AmpCode](./honorable-mentions/ampcode-zh.md) • [Octofriend](./honorable-mentions/octofriend-zh.md)

## 快速入门指南

### 何时使用本指南
此工具栈专为vibecoding工作流程的**阶段0-2**设计：
- [阶段0：Vibecoder准备](../workflow/phase-0-vibecoder-preparation-zh.md) - 初始工具选择
- [阶段1：规划](../workflow/phase-1-planning-zh.md) - Clavix PRD生成
- [阶段2：开发](../workflow/phase-2-development-zh.md) - 日常编码工作流程

### 1. 基本设置（阶段0）
从核心栈开始以获得最佳vibecoding体验：
1. 安装[Zed](./recommended-tools/zed-zh.md)作为您的主要IDE
2. 设置[Claude Code CLI](./recommended-tools/claude-code-cli-zh.md)作为您的主要编码代理
3. 配置[Context7 MCP](./mcp-servers/context7-mcp-zh.md)以进行文档访问
4. 添加[DevTools MCP](./mcp-servers/devtools-mcp-zh.md)以进行测试功能
5. 安装[Sequential Thinking MCP](./mcp-servers/sequential-thinking-mcp-zh.md)以增强问题解决
6. 设置[Task Manager MCP](./mcp-servers/task-manager-mcp-zh.md)以进行持久工作流程管理
7. 添加[Shadcn MCP](./mcp-servers/shadcn-mcp-zh.md)以进行专业UI组件访问
8. （可选）安装[Droid CLI](./recommended-tools/droid-cli-zh.md)用于规范驱动的项目

### 2. 配置
- 为您首选的LLM提供商配置API密钥
- 设置基本MCP服务器：
  - [Sequential Thinking MCP](./mcp-servers/sequential-thinking-mcp-zh.md)用于结构化分析
  - [Task Manager MCP](./mcp-servers/task-manager-mcp-zh.md)用于任务持久性
  - [Shadcn MCP](./mcp-servers/shadcn-mcp-zh.md)用于UI组件
- 查看兼容性指南以获得最佳集成

### 3. 工作流程集成
这些工具设计为无缝协作：
- 使用Zed进行编辑和开发
- 利用Claude Code CLI进行AI辅助编码（主要代理）
- 使用Droid CLI进行规范驱动的项目和团队协调
- 通过Context7 MCP访问文档
- 使用DevTools MCP进行测试和调试
- 使用Sequential Thinking MCP增强问题解决
- 使用Task Manager MCP持久管理任务
- 使用Shadcn MCP组件构建专业UI

## 工具哲学

### 自带密钥（BYOK）
所有主要工具都支持外部API集成，提供：
- **成本效率**：使用您首选的LLM提供商
- **灵活性**：根据任务复杂性在模型之间切换
- **隐私**：保持您的数据和API密钥安全

### MCP集成
通过外部数据源增强开发：
- **上下文感知**：实时访问相关文档
- **工具编排**：开发工具之间的无缝集成
- **可扩展性**：轻松添加新工具和功能

### 性能重点
工具选择基于：
- **速度**：快速响应时间和最小延迟
- **可靠性**：在各种工作负载下的稳定性能
- **效率**：针对快速开发迭代进行优化

## 从其他工具迁移

如果您正在从其他开发环境迁移：
1. 查看兼容性指南以了解集成要求