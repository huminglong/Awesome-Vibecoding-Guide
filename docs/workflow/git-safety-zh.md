# AI辅助开发的Git安全

为什么Git是您使用AI编码时的终极安全网。本指南专注于防止AI编码灾难——有关详细的工作流程策略，请参阅[Git策略](./git-strategies-zh.md)。

## 问题：AI可以一次破坏所有内容

AI编码工具很强大。最新的模型可以在几秒钟内重新生成大块代码。这是它们的优势——但这意味着：

- **一个糟糕的提示可以同时破坏多个文件**
- 没有版本控制，没有撤消按钮
- "AI刚刚重写了我的整个组件，现在什么都不起作用"

解决方案不是避免AI——而是使用Git作为您的安全网。

---

## 15分钟设置（不可协商）

在使用AI辅助编写任何代码之前：

```bash
# 初始化Git
git init
git add .
git commit -m "Initial working state"

# 创建远程备份（GitHub、GitLab等）
git remote add origin https://github.com/yourusername/project.git
git push -u origin main
```

**就是这样。** 一个命令，您就有了一个恢复点。跳过这一步，您就是在建造纸牌屋。

---

## 日常安全工作流程

### 早上：创建功能分支

```bash
git checkout -b feature/todays-work
```

这将今天的AI实验与您的稳定代码隔离开来。

### 在任何主要AI提示之前

```bash
git add .
git commit -m "Before AI regeneration - working"
```

**这很关键。** AI操作之前的每次提交都给您一个有保证的恢复点。

### 如果AI破坏了某些东西

```bash
git reset --hard HEAD
```

**一个命令，混乱就消失了。** 您回到了最后的工作状态。

### 一天结束

```bash
git push origin feature/todays-work
```

您的工作已备份。明天，您可以继续或合并到main。

---

## 要避免的三个关键错误

### 1. 直接在Main（生产分支）上工作

**问题**：您将AI生成的代码直接推送到main，它破坏了生产，客户受到影响。

**修复**：
```bash
# 始终创建功能分支
git checkout -b feature/new-functionality

# 在分支上彻底测试
# 仅在验证工作后合并
git checkout main
git merge feature/new-functionality
```

### 2. 仅在AI工具设置中存储机密

**问题**：您在AI工具的设置中配置API密钥。您切换工具或平台有问题——现在您失去了对您自己项目机密的访问权限。

**修复**：
```bash
# 创建.env.example（提交这个 - 没有实际值）
DATABASE_URL=
API_KEY=
STRIPE_SECRET=

# 创建.env（永不提交 - 添加到.gitignore）
DATABASE_URL=postgres://actual/connection
API_KEY=sk-actual-key
STRIPE_SECRET=sk_live_actual
```

添加到 `.gitignore`：
```
.env
.env.local
.env.production
```

现在您的机密既已记录又安全。

### 3. 没有Git历史下载项目

**问题：** 您从AI平台使用"下载ZIP"导出。没有历史记录，没有恢复选项。

**修复：**
- 始终`git clone`仓库
- 从AI平台导出时，立即初始化Git
- 在进行任何AI更改之前推送到远程

---

## AI特定安全模式

### 每个AI会话一个分支

**模式：** 一个AI对话 = 一个功能分支

```bash
# 开始新的AI会话用于用户认证
git checkout -b feat/user-authentication

# 所有AI生成的代码都放在这里
# 迭代时的多个提交

# 功能完成并测试后
git checkout main
git merge feat/user-authentication
git push origin main

# 清理
git branch -d feat/user-authentication
```

**好处：**
- 清晰的回滚边界
- 如果AI会话出错，很容易丢弃整个会话
- 每个功能的上下文隔离
- 安全实验

### AI操作前的安全提交

**规则：** 在要求AI重新生成代码之前提交。

```bash
# 工作状态
git add .
git commit -m "Working: user form complete"

# 现在要求AI重构/重新生成
# 如果它破坏了...
git reset --hard HEAD
# 立即回到工作状态
```

**提交频率指南：**
- 每30-60分钟工作一次
- 在任何"重新生成此文件"提示之前
- 在任何"重构"请求之前
- 每个成功工作的功能之后

### 恢复速度目标

| 恢复类型 | 目标时间 | 方法 |
|--------------|-------------|--------|
| 紧急回滚 | < 2分钟 | `git reset --hard [hash]` |
| 功能还原 | < 1分钟 | `git revert [hash]` |
| 文件恢复 | 瞬间 | `git checkout HEAD -- file.js` |

---

## 快速参考：恢复命令

| 情况 | 命令 |
|-----------|---------|
| AI刚刚破坏了一切 | `git reset --hard HEAD` |
| 查找最后良好状态 | `git log --oneline` |
| 撤消上次提交（保留更改） | `git reset --soft HEAD~1` |
| 恢复特定提交 | `git revert [hash]` |
| 恢复单个文件 | `git checkout HEAD -- path/to/file` |
| 查看更改了什么 | `git diff` |
| 核选项（重置为已知良好） | `git reset --hard [good-hash]`然后`git push --force` |

---

## 推荐分支结构

```
main (生产)
  └── dev (集成 - 单人可选)
       ├── feat/user-login
       ├── feat/dashboard
       ├── fix/auth-bug
       └── hotfix/critical-issue
```

**分支命名约定：**
- `feat/` - 新功能
- `fix/` - 错误修复
- `hotfix/` - 关键生产修复
- `refactor/` - 代码改进
- `content/` - 内容更新

---

## 安全检查清单

### 开始任何AI编码会话之前

- [ ] Git仓库已初始化
- [ ] 远程备份已配置（GitHub/GitLab）
- [ ] 功能分支已创建
- [ ] 初始提交已完成

### 开发期间

- [ ] 主要AI提示之前提交
- [ ] 每个AI生成的更改后测试
- [ ] 会话结束时推送到远程
- [ ] 不仅在AI工具设置中存储机密

### 当某些东西损坏时

- [ ] 不要恐慌——Git支持您
- [ ] 检查`git log --oneline`以获取最后良好提交
- [ ] 使用`git reset --hard HEAD`进行快速恢复
- [ ] 如需要，`git reset --hard [specific-hash]`

---

## 为什么这很重要

> "移动最快的开发人员不是跳过Git。他们将其用作安全网。"

使用适当的Git工作流程：

- **不用担心"破坏一切"**——您总是可以恢复
- **自由地与AI实验**——坏结果很容易撤销
- **从坏提示中瞬间恢复**——一个命令
- **AI生成的清晰历史**——用于学习和调试

Git不是使用AI编码时的开销。它是使无畏AI辅助开发成为可能的基础。

---

**相关文档：**
- [Git策略](./git-strategies-zh.md) - 开发和维护的详细两阶段工作流程
- [第2阶段：开发](./phase-2-development-zh.md) - 开发工作流程
- [快速开始](../quickstart/README-zh.md) - Git和项目设置入门
- [故障排除](../troubleshooting/README-zh.md) - 恢复模式和调试

**返回：** [工作流程概述](./README-zh.md)