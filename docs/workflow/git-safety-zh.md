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