# 恢复模式

常见AI辅助开发事故的快速恢复策略。当事情出错时，使用这些模式回到正轨。

## "最后已知良好状态"恢复

### 何时使用
- AI做出了意外的更改
- 遵循AI建议后代码损坏
- 不确定什么改变了

### 策略：Git时间机器

**步骤1：找到最后工作的提交**
```bash
# 查看最近的提交
git log --oneline -10

# 或带有详细信息
git log -5 --stat

# 查看每个提交中更改了什么
git show abc123
```

**步骤2：创建安全分支**
```bash
# 在恢复之前，保存当前状态
git branch broken-state

# 现在可以安全地尝试恢复
```

**步骤3：恢复选项**

**选项A：恢复特定提交**
```bash
# 撤消一个糟糕的提交（创建新提交）
git revert abc123

# 撤消多个提交
git revert abc123 def456

# 结果：原始提交保留在历史中，新提交撤消它们
```

**选项B：重置到良好状态**
```bash
# 软重置：将更改保留为未提交
git reset --soft abc123

# 查看更改
git diff

# 选择性地重新应用良好的更改
git add path/to/good-file.js
git commit -m "仅重新应用良好的更改"
```

**选项C：硬重置（核选项）**
```bash
# 丢弃所有内容，回到提交abc123
git reset --hard abc123

# 警告：这会破坏未提交的更改！
# 确保您首先创建了安全分支！
```

**步骤4：挑选良好的更改**
```bash
# 如果broken-state有一些良好的更改：
git checkout broken-state
git log --oneline  # 找到良好的提交

git checkout main
git cherry-pick good-commit-hash
```

### 示例：AI破坏了身份验证

```bash
# 1. 注意到AI重构auth代码后登录损坏

# 2. 找到最后工作的版本
git log --oneline --grep="auth"
# 显示：abc123 "Fix login bug"（2天前）← 最后已知良好
#        def456 "Refactor authentication"（今天）← 破坏了它

# 3. 创建安全分支
git branch auth-broken

# 4. 重置到良好状态
git reset --hard abc123

# 5. 测试 - 登录再次工作！✅

# 6. 逐渐重新应用重构的良好部分
git checkout auth-broken -- src/utils/jwt.ts  # 这个文件是好的
# 测试
# 对其他良好的更改重复
# 7. 记录出了什么问题
echo "重构破坏了中间件错误处理" >> docs/learnings.md
```

## 使用二分查找找到AI引入的错误

### 何时使用
- 错误最近出现但不知道哪个提交
- 多个AI更改，其中一个破坏了某些东西
- 需要识别确切的问题提交

### 策略：Git二分查找

**工作原理：**
Git通过提交执行二进制搜索以找到第一个错误的提交。

**步骤1：开始二分查找**
```bash
git bisect start

# 标记当前状态为错误
git bisect bad

# 标记最后已知良好的提交
git log --oneline -20  # 找到良好的提交
git bisect good abc123

# Git检出中间提交
```

**步骤2：测试每个提交**
```bash
# 在Git检出的每个提交上，测试错误：
npm test  # 或手动测试

# 如果错误存在：
git bisect bad

# 如果错误不存在：
git bisect good

# Git检出下一个要测试的提交

# 重复直到Git找到第一个错误的提交
```

**步骤3：找到了！**
```bash
# Git显示："abc123是第一个错误的提交"

# 查看错误的提交
git show abc123

# 结束二分查找
git bisect reset
```

**步骤4：修复问题**
```bash
# 现在您确切知道什么破坏了
# 选项：
# A) 恢复提交
git revert abc123

# B) 修复特定问题
# 编辑文件，然后：
git commit -m "修复abc123中引入的问题"
```

### 自动化二分查找

如果您有自动化测试：

```bash
git bisect start
git bisect bad HEAD
git bisect good abc123

# 让git自动运行测试
git bisect run npm test

# Git将自动找到错误的提交！
```

### 示例：性能回归

```bash
# 页面加载突然变慢，但不知道为什么

# 开始二分查找
git bisect start
git bisect bad HEAD
git bisect good v1.2.0  # 上一个版本是快的

# Git检出中间提交

# 测试性能
time curl http://localhost:3000
# 花费5秒（慢）
git bisect bad

# 下一个提交
time curl http://localhost:3000
# 花费1秒（快）
git bisect good

# 继续直到...
# Git说："def456是第一个错误的提交"

git show def456
# 提交消息："添加全面日志记录"
# 啊哈！AI到处添加了console.log语句！

# 修复
git bisect reset
# 删除过多的日志记录，保留必要的
git commit -m "删除性能杀手调试日志"
```

## 紧急回滚程序

### 生产站点宕机

**场景：**部署了AI生成的代码，站点损坏，正在损失收入。

**优先级：**尽快让站点工作，稍后调试。

**程序：**

**步骤1：立即回滚（< 2分钟）**
```bash
# 选项A：Cloudflare Pages
# 1. 转到仪表板
# 2. Pages → 您的站点 → 部署
# 3. 找到最后工作的部署
# 4. 点击"..." → 回滚到此部署
# 完成！站点恢复。

# 选项B：基于Git的部署
git revert HEAD --no-edit
git push
# CI/CD将部署以前的版本
```

**步骤2：通知利益相关者（< 5分钟）**
```
状态更新：
"站点暂时宕机，已回滚到以前的版本。
站点现在运行正常。正在调查问题。"
```

**步骤3：安全调试（站点稳定后）**
```bash
# 不要在主分支上调试！
git checkout -b debug-production-issue

# 拉取损坏的代码
git reset --hard origin/main

# 调查
# 修复问题
# 彻底测试
# 创建PR

# 永远不要直接将未经测试的修复推送到生产环境！
```

### 数据库迁移失败

**场景：**AI生成的迁移运行，破坏了数据库架构。

**立即行动：**

**步骤1：停止应用程序**
```bash
# 防止进一步损坏
# 停止所有访问数据库的实例
```

**步骤2：检查是否可逆**
```bash
# 如果使用具有回滚功能的迁移工具：
npm run migrate:rollback

# 或手动回滚：
psql -U user -d database < backup.sql
```

**步骤3：从备份恢复**
```bash
# 如果没有可用的回滚，从备份恢复
# （这就是为什么您总是在迁移前备份！）

# PostgreSQL示例：
dropdb database_name
createdb database_name
pg_restore -d database_name backup.dump

# D1示例：
wrangler d1 execute DB --file=backup.sql --remote
```

**步骤4：修复迁移**
```bash
# 查看出错的地方
cat migrations/xxxx_broken_migration.sql

# 创建修正的迁移
npm run migrate:create fix_schema

# 首先在本地数据库上测试！
npm run migrate:up --local

# 验证工作
npm run test:db

# 然后生产
npm run migrate:up --remote
```

**预防：**
```markdown
# 在生产迁移之前始终：

1. 备份数据库
2. 在暂存环境上测试迁移
3. 仔细审查AI生成的SQL
4. 有回滚计划
5. 在低流量时间运行
6. 监控错误
7. 准备好恢复程序
```

## 上下文丢失恢复

### 项目中途上下文丢失

**场景：**AI在会话中途"忘记"了项目上下文，建议现在与您的模式不匹配。

**恢复：**

**步骤1：识别上下文丢失**
迹象：
- AI建议与以前不同的模式
- AI"忘记"了技术栈
- 建议与早期建议相矛盾
- AI询问它应该知道的事情

**步骤2：显式重置上下文**
```
停止。让我们重置上下文。

我正在处理：[项目名称]
技术栈：[Astro、D1、Cloudflare等]
当前任务：[您正在做的事情]

项目上下文：
[粘贴相关的.cursorrules或摘要]

现在，[继续您的请求]
```

**步骤3：渐进式上下文重新加载**
```
不要一次加载所有内容。相反：

"让我们重新开始。首先读取.cursorrules。"

[AI读取.cursorrules]

"现在，我正在处理身份验证。读取src/pages/api/auth/login.ts"

[AI读取文件]

"现在帮我添加速率限制。"
```

**步骤4：记录会话**
```bash
# 保存成功的交互模式
echo "今天的上下文丢失通过首先显式加载.cursorrules恢复" >> docs/ai-sessions.md
```

### 从Git历史重建

**场景：**失去了对正在构建的内容的跟踪，AI需要有关最近更改的上下文。

**重建上下文：**

```bash
# 步骤1：查看您一直在做什么
git log --oneline --since="1 week ago"

# 步骤2：查看文件更改
git diff --name-only HEAD~10 HEAD

# 步骤3：获取更改摘要
git log --since="1 week ago" --pretty=format:"%h - %s" --graph

# 步骤4：提供给AI
```

```
这是我一直在做的事情（来自git历史）：

最近的提交：
[粘贴git log输出]

更改的文件：
[粘贴git diff --name-only输出]

现在我需要帮助：[您的请求]
```

## AI混淆恢复

### AI给出矛盾建议

**场景：**AI建议A，后来建议与A相反的内容。

**恢复：**

**步骤1：停止并澄清**
```
停止。您刚刚与早期建议相矛盾。

早些时候您说：[引用矛盾建议]
现在您说：[引用当前建议]

哪个对我的用例是正确的？
[清楚描述用例]

请：
1. 解释哪种方法是正确的
2. 为什么另一种不适用
3. 承诺向前推进一种方法
```

**步骤2：记录决策**
```markdown
# docs/decisions/authentication-approach.md

# 决定：使用JWT，而不是会话

日期：2025-01-15

## 背景
AI最初建议会话，后来建议JWT。

## 澄清
对于无服务器（Cloudflare Pages），JWT是正确的。
会话需要持久存储（KV），增加复杂性。

## 已决定
在httpOnly cookies中使用JWT。

## 永远不要再建议
不要为此项目重新考虑会话。
无服务器架构决定了这一点。
```

**步骤3：更新.cursorrules**
```markdown
# 添加到.cursorrules

## 已确定的决策
- 身份验证：在httpOnly cookies中使用JWT（不是会话）
  - 原因：无服务器架构
  - 不要建议会话
```

### AI建议错误的框架功能

**场景：**AI在Astro中建议React功能，反之亦然。

**恢复：**

**立即纠正：**
```
停止。错误的框架。

这是Astro，不是React。
Astro组件使用不同的语法：
- 通过Astro.props传递props
- 没有useState/useEffect
- 使用client:*指令进行交互性

请提供Astro组件语法。
```

**防止复发：**
```markdown
# 添加到.cursorrules顶部（高优先级）

# 框架：ASTRO
这是一个ASTRO项目。
- 对组件使用.astro文件
- 对props使用Astro.props
- 在.astro文件中没有useState/useEffect
- 对React岛屿使用client:load/client:visible

不要为Astro组件建议React模式。
```

## 代码质量恢复

### 大规模重构出错

**场景：**AI重构了大部分代码，现在许多测试失败。

**恢复：**

**步骤1：评估损坏**
```bash
# 运行测试
npm test

# 多少失败了？
# 如果> 50%，可能是大型重构破坏了模式

# 查看更改了什么
git diff
```

**步骤2：回滚重构**
```bash
# 保存当前状态
git stash

# 回到重构前
git log --oneline -5
git reset --hard abc123  # 重构前
```

**步骤3：增量重构**
```
而不是大型重构，让我们做增量：

1. 一次重构一个文件
2. 每个文件后运行测试
3. 提交每个成功的重构
4. 如果测试失败，仅回滚该文件

从：src/utils/auth.ts开始
仅重构此文件。
```

**步骤4：创建重构清单**
```markdown
# 重构清单

对于每个文件：
- [ ] 重构代码
- [ ] 运行测试：`npm test`
- [ ] 在浏览器中手动测试
- [ ] 审查差异
- [ ] 如果通过则提交
- [ ] 如果失败，回滚并尝试不同的方法

要重构的文件：
- [ ] src/utils/auth.ts
- [ ] src/middleware/auth.ts
- [ ] src/pages/api/auth/login.ts
- [ ] src/pages/api/auth/logout.ts
```

### 从失败的分支中挽救更改

**场景：**分支混合了良好和不良更改，不想失去良好的更改。

**恢复：**

```bash
# 步骤1：识别良好的更改
git diff main..failed-branch --name-only
# 列出所有更改的文件

# 步骤2：创建新的干净分支
git checkout main
git checkout -b salvage-good-changes

# 步骤3：挑选良好的文件
git checkout failed-branch -- src/components/GoodComponent.tsx
git add src/components/GoodComponent.tsx
git commit -m "挽救：GoodComponent"

# 对每个良好文件重复

# 步骤4：验证
npm test
npm run build

# 步骤5：如果都好则合并
git checkout main
git merge salvage-good-changes
```

## 依赖地狱恢复

### 破坏性依赖更新

**场景：**AI建议更新包，现在应用程序无法构建/运行。

**恢复：**

**步骤1：识别问题包**
```bash
# 检查更新了什么
git diff package.json

# 示例输出显示：
- "astro": "^3.0.0"
+ "astro": "^4.0.0"
```

**步骤2：检查破坏性更改**
```bash
# 阅读更新日志
npm info astro

# 或访问
# https://github.com/withastro/astro/releases
```

**步骤3：回滚package.json**
```bash
git checkout HEAD -- package.json package-lock.json
npm install
```

**步骤4：增量更新**
```bash
# 一次更新一个主要版本
npm install astro@^3.5.0
npm test && npm run build

# 如果工作：
npm install astro@^4.0.0
npm test && npm run build

# 如果破坏，检查迁移指南
```

**步骤5：修复破坏性更改**
```
AI：我更新到Astro 4，但构建失败。

错误：
[粘贴错误]

根据Astro 4迁移指南，需要更改什么？

Astro 4破坏性更改：
[从文档粘贴相关破坏性更改]

请更新我的代码以兼容Astro 4。
```

### 版本锁定文件冲突

**场景：**合并后package-lock.json有冲突。

**恢复：**

```bash
# 删除锁定文件
rm package-lock.json

# 从package.json重新生成
npm install

# 验证一切工作
npm test
npm run build

# 提交新的锁定文件
git add package-lock.json
git commit -m "合并后重新生成锁定文件"
```

## 预防策略

### 检查点

**创建安全检查点：**

```bash
# 在主要AI辅助更改之前：
git add .
git commit -m "检查点：在[更改描述]之前"

# 进行AI更改
# 如果工作：
git add .
git commit -m "feat：[描述]"

# 如果破坏：
git reset --hard HEAD~1  # 回到检查点
```

### 频繁小提交

**最佳实践：**
```bash
# 每个工作更改后提交
git add src/components/NewComponent.tsx
git commit -m "添加：NewComponent"

# 不是一次10个更改！
# 小提交 = 容易回滚
```

### 分支保护

**对于生产：**

```bash
# 永远不要直接推送到main
git checkout -b feature/new-feature

# 在这里工作，彻底测试

# 准备好时：
git push origin feature/new-feature
# 创建PR，审查，然后合并

# 永远不要：
git checkout main
git push  # ← 不要这样做！
```

### 迁移前备份

**数据库更改前始终：**

```bash
# PostgreSQL
pg_dump database_name > backup_$(date +%Y%m%d_%H%M%S).sql

# D1
wrangler d1 execute DB --command="SELECT * FROM table" --json > backup.json

# 然后迁移
npm run migrate:up

# 保留备份30天
```

## 总结：恢复原则

### 1. 安全第一
在实验前始终创建备份分支。

### 2. 小步骤
增量恢复，而不是一次性全部。

### 3. 持续测试
每个恢复步骤后，验证它工作。

### 4. 记录学习
记录出了什么问题，您如何恢复。

### 5. 防止复发
更新.cursorrules，改进提示以避免重复。

### 6. Git是您的朋友
经常提交，分支便宜，历史有价值。

### 7. 不要恐慌
几乎所有东西都可以用git恢复。

---

**黄金法则：**如果您不确定，首先创建一个分支。分支什么都不花费，丢失的工作花费一切。