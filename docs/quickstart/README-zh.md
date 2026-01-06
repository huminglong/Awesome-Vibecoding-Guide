# 快速入门 🚀

欢迎使用Awesome Vibecoding指南！本快速入门将让您开始使用现代AI驱动的开发工作流程，包括基本的Git/GitHub实践。

## 🎯 阶段1：项目设置与规划

### 1. 定义您的项目愿景
- 编写简短的产品需求文档（PRD）
- 定义关键功能和结果
- 设定明确的成功标准

### 2. 创建规范
- 使用Clavix进行基于CLEAR的PRD生成，具有可实施的任务列表
- 将复杂功能分解为可管理的任务
- 定义技术要求和约束

### 3. 初始化版本控制 ⭐ **关键步骤**

**永远不要在没有Git的情况下编码！** 远程仓库是您的安全网和协作基础。

#### 基本Git设置：
```bash
# 配置Git（仅第一次）
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 创建本地仓库
git init

# 在GitHub/GitLab上创建远程仓库
# 访问github.com → 新仓库 → 克隆或添加远程
git remote add origin https://github.com/yourusername/yourproject.git
```

#### 推送初始设置：
```bash
# 添加所有文件
git add .
git commit -m "Initial project setup with specifications"
git push -u origin main
```

> **必读：** [AI开发的Git安全](../workflow/git-safety-zh.md) — 为什么Git是您使用AI编码时的终极安全网。涵盖预提示提交、即时恢复以及要避免的三个关键错误。

## 🛠️ 阶段2：开发环境设置

### 4. 设置您的技术栈
- **编辑器**：Zed（推荐）或您首选的IDE
- **AI助手**：GLM编码代理或类似工具
- **MCP服务器**：Context7 MCP、DevTools MCP
- **版本控制**：Git + GitHub/GitLab

### 5. 项目结构最佳实践
```
your-project/
├── docs/              # 项目文档
├── specs/             # 功能规范
├── src/               # 源代码
├── tests/             # 测试文件
└── README.md          # 项目概述
```

## 🚀 阶段3：开发工作流程

### 6. 分支策略：安全开发

#### 🟢 对于新项目（尚未有生产环境）
**简单方法 - 直接提交到主分支：**
```bash
# 直接在主分支上工作
git add .
git commit -m "Feature: User authentication system"
git push origin main
```

**为什么这对新项目有效：**
- 没有生产代码会破坏
- 提交作为护栏和检查点
- 如果出现问题很容易回滚
- 快速原型设计的更快工作流程

#### 🔴 对于实时项目（生产环境）
**安全方法 - 功能分支：**
```bash
# 创建功能分支
git checkout -b feature/user-dashboard
# 开发功能...
git add .
git commit -m "Implement user dashboard UI"
git push origin feature/user-dashboard

# 彻底测试，然后合并
git checkout main
git merge feature/user-dashboard
git push origin main
```

**为什么分支对生产环境至关重要：**
- 防止破坏实时功能
- 允许隔离测试和审查
- 支持并行开发
- 提供回滚安全网

### 7. 开发过程

#### 上下文管理：
- 仅将相关文件加载到AI上下文中
- 专注于当前任务
- 避免用整个代码库压倒AI

#### 任务实施：
- 一次处理一个功能
- 在接受之前审查AI生成的代码
- 使用DevTools MCP进行自动验证测试
- 执行手动测试以进行用户体验验证

## 🔄 阶段4：提交和推送工作流程

### 8. 提交最佳实践

**提交频率：**
- **在每次有意义的功能完成后推送**
- **在主要里程碑提交**（设置、核心功能、测试）
- **不要等待太久** - 频繁推送防止数据丢失

**提交消息格式：**
```bash
git commit -m "Type: Brief description"

# 示例：
git commit -m "feat: Add user registration form"
git commit -m "fix: Resolve authentication token issue"
git commit -m "docs: Update API documentation"
git commit -m "test: Add unit tests for user service"
```

**提交类型：**
- `feat:` 新功能
- `fix:` 错误修复
- `docs:` 文档更改
- `test:` 测试添加/改进
- `refactor:` 代码重构
- `chore:` 维护任务

### 9. 推送工作流程

#### 基本工作流程（新项目）：
```bash
# 完成功能后：
git add .
git commit -m "feat: Implement user profile page"
git push origin main
```

#### 高级工作流程（生产项目）：
```bash
# 创建功能分支
git checkout -b feature/payment-integration

# 开发和测试...
git add .
git commit -m "feat: Add Stripe payment integration"
git push origin feature/payment-integration

# 测试和审查后：
git checkout main
git merge feature/payment-integration
git push origin main

# 清理
git branch -d feature/payment-integration
```

## 🛡️ 阶段5：安全和恢复

### 10. Git作为您的终极安全网

**提交是您的护栏：**
- 每次提交都是您可以返回的保存点
- 不用担心"破坏一切" - 您总是可以回滚
- 实验性更改在版本控制下是安全的

**恢复命令：**
```bash
# 查看提交历史
git log --oneline

# 回滚到以前的提交（创建新提交）
git revert <commit-hash>

# 重置到以前的提交（谨慎使用）
git reset --hard <commit-hash>

# 提交前查看更改
git diff
git status
```

### 11. 远程备份好处

**为什么远程仓库是必不可少的：**
- **数据安全**：代码在云端备份
- **协作**：轻松的团队协调
- **部署**：CI/CD集成
- **作品集**：向潜在客户/雇主展示您的工作

**GitHub/GitLab功能：**
- **拉取/合并请求**：代码审查和协作
- **问题**：错误跟踪和功能请求
- **操作**：自动化测试和部署
- **页面**：文档/演示的免费托管

## 📋 快速入门检查清单

### 开始编码之前：
- [ ] 创建GitHub/GitLab账户
- [ ] 安装和配置Git
- [ ] 创建远程仓库
- [ ] 编写初始项目规范
- [ ] 设置开发环境

### 开发期间：
- [ ] 将初始设置推送到远程
- [ ] 选择适当的分支策略
- [ ] 频繁提交并使用描述性消息
- [ ] 推送前彻底测试
- [ ] 使用DevTools MCP进行自动化测试

### 对于生产项目：
- [ ] 始终使用功能分支
- [ ] 创建拉取/合并请求以进行审查
- [ ] 在预发布环境中测试
- [ ] 部署时制定回滚计划
- [ ] 密切监控生产环境

## 🚀 下一步

您现在已准备好开始vibecoding！通过适当的Git工作流程和AI辅助，您可以更快、更安全、更高效地开发。

---

## 相关文档

**入门：**
- [介绍与哲学](../introduction/README-zh.md) - 理解Vibecoding
- [核心技术](../core-technologies-zh.md) - Astro、Tailwind、Cloudflare技术栈

**开发工具：**
- [开发工具](../development-tools/README-zh.md) - 基本工具和设置
- [AI模型提供商](../ai-model-providers/README-zh.md) - 选择您的AI助手
- [MCP服务器](../development-tools/mcp-servers/README-zh.md) - 扩展AI功能

**工作流程与过程：**
- [工作流程概述](../workflow/README-zh.md) - 完整开发过程
- [阶段1：规划](../workflow/phase-1-planning-zh.md) - 项目规范
- [阶段2：开发](../workflow/phase-2-development-zh.md) - 实施指南
- [上下文管理](../context-management/README-zh.md) - 优化AI交互
- [提示指南](../prompting/README-zh.md) - 有效的AI通信

**质量与标准：**
- [质量标准](../quality-standards/README-zh.md) - 可访问性、SEO、性能
- [故障排除](../troubleshooting/README-zh.md) - 常见问题和解决方案

**部署与托管：**
- [托管工具](../hosting-tools/README-zh.md) - Cloudflare平台指南
- [阶段4：部署](../workflow/phase-4-deployment-zh.md) - 进入生产环境

---

**记住：** Git是您在AI驱动开发中的最好朋友。版本控制和AI辅助的结合创造了不可阻挡的开发工作流程！

---

返回索引：[顶层README](../../README-zh.md)