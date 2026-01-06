# Vibecoding的Git策略

针对AI辅助开发优化的实用两阶段Git工作流程。这种方法在初始开发期间平衡速度，在维护期间平衡稳定性，在数十个商业项目中证明有效。

> **另请参阅**：[Git安全](./git-safety-zh.md)了解AI特定的灾难预防实践——提示前提交、即时恢复以及要避免的三个关键错误。

## 概述：两阶段Git工作流程

**核心原则**：
您的Git策略应该匹配您的项目生命周期阶段。

**发布前（开发阶段）：**
- 速度和迭代至关重要
- 破坏东西是可以接受的
- 直接提交到master
- 最小的分支开销

**发布后（维护阶段）：**
- 稳定性至关重要
- 破坏生产是不可接受的
- 强制功能分支
- 在合并到master之前测试

**为什么这有效：**
- 在开发期间节省时间（无不必要的合并开销）
- 在发布后提供稳健性（保护生产）
- 匹配每个阶段的实际风险概况
- 在现实世界客户项目中证明

## 发布前策略（开发阶段）

### 何时使用此方法

**项目状态：**
- 尚无实时用户
- 无生产环境
- 从头开始构建
- MVP/初始开发
- 重度迭代阶段

**团队规模：**
- 单个开发人员（最常见）
- 小团队（2-3人）
- 清晰的沟通渠道

**持续时间：**
- 从项目开始
- 直到第一次生产部署
- 通常1-4周

### 策略：将所有内容推送到Master

**核心工作流程：**
```bash
# 日常工作流程
git add .
git commit -m "Implement user authentication"
git push origin main

# 就是这样！
```

**为什么这有效：**

**1. 速度**
- 无分支开销
- 无合并冲突
- 无上下文切换
- 更快地迭代

**2. 简单性**
- 一个分支要跟踪
- 清晰的进展
- 易于理解
- 更少的认知负荷

**3. 安全网**
- 每次提交都是一个检查点
- 如果需要，易于回滚
- Git历史是您的备份
- 远程仓库防止数据丢失

**4. AI友好**
- AI可以直接提交
- 无分支复杂性
- 清晰的线性历史
- 易于向AI解释

### 何时使用功能分支（即使发布前）

**仅限大型功能：**
- 需要>3天工作的功能
- 实验性实施
- 重大架构更改
- 当您想尝试多种方法时

**示例**：
```bash
# 处理复杂的身份验证系统
git checkout -b feat/auth-system

# 3天的工作，多次提交
git add .
git commit -m "Add JWT token generation"
# ... 更多提交 ...

# 准备集成时
git checkout main
git merge feat/auth-system
git push origin main
```

**不要使用分支用于：**
- 小功能（<1天）
- 错误修复
- 内容更新
- 快速迭代

### 提交消息策略

**保持简单和描述性：**

**良好：**
```bash
git commit -m "Add user registration form with validation"
git commit -m "Fix authentication token expiration bug"
git commit -m "Update homepage hero section"
```

**糟糕：**
```bash
git commit -m "stuff"
git commit -m "more changes"
git commit -m "fix"
```

**AI生成的提交：**
让AI根据更改建议提交消息：
```
提示："审查更改并建议一个提交消息，描述已实现的内容。"
```

### 优势：速度和简单性

**节省的时间：**
- 无分支创建：每个功能+30秒
- 无分支切换：每次上下文更改+20秒
- 无合并冲突：每次冲突+15分钟
- **总计**：小型项目每周节省1-2小时

**复杂性降低：**
- 一个分支要记住
- 无"我在哪个分支上？"困惑
- 直观的git历史
- 更容易向AI助手解释

**示例时间线：**
```
传统分支（1周项目）：
- 创建8个功能分支：4分钟
- 切换分支20次：10分钟
- 解决3个合并冲突：45分钟
- PR审查和合并：60分钟
总开销：约2小时

直接到Master（1周项目）：
- 无分支开销：0分钟
总开销：0分钟

节省时间：2小时（16小时项目周的12%）
```

### 恢复策略

**如果出现问题：**

**选项1：恢复最后一次提交**
```bash
# 查看最近提交
git log --oneline

# 恢复错误提交
git revert abc1234

# 推送恢复
git push origin main
```

**选项2：重置到最后良好状态**
```bash
# 找到最后良好提交
git log --oneline

# 重置到该提交（破坏性）
git reset --hard abc1234

# 强制推送（谨慎使用）
git push origin main --force
```

**选项3：快速向前修复**
```bash
# 只需修复并提交
git add .
git commit -m "Fix issue from previous commit"
git push origin main
```

**最佳实践：**
- 频繁提交（每30-60分钟）
- 每次提交应该是工作状态
- 提交前测试
- 较小的提交 = 更容易恢复

## 发布后策略（维护阶段）

### 何时切换到此方法

**触发器：**
- ✅ 第一次生产部署完成
- ✅ 真实用户访问站点
- ✅ 客户依赖站点在线
- ✅ 停机时间有实际后果

**立即行动：**
锁定master分支并建立基于分支的工作流程。

### 策略：锁定Master，使用分支

**保护主分支：**

**GitHub设置：**
```
设置 → 分支 → 分支保护规则
→ 为"main"添加规则

启用：
☑ 合并前需要拉取请求
☑ 需要状态检查（如果您有CI）
☐ 需要批准（对于团队）
```

**工作流程：**
```bash
# 1. 创建功能分支
git checkout -b feat/add-services-page

# 2. 本地开发和测试
# ... 工作 工作 工作 ...

# 3. 提交您的更改
git add .
git commit -m "Add services page with contact form"

# 4. 推送分支到远程
git push origin feat/add-services-page

# 5. 部署分支到测试环境
# （Cloudflare Pages自动创建预览）

# 6. 在实时预览URL上测试
# 点击，测试移动设备，检查表单

# 7. 验证后合并到main
git checkout main
git merge feat/add-services-page

# 8. 推送到生产
git push origin main

# 9. 清理分支
git branch -d feat/add-services-page
git push origin --delete feat/add-services-page
```

### 分支命名约定

**功能分支：**
```
feat/user-registration
feat/contact-form
feat/image-gallery
feat/blog-section
```

**错误修复分支：**
```
fix/authentication-bug
fix/mobile-layout
fix/form-validation
```

**热修复分支（紧急生产问题）：**
```
hotfix/critical-security-patch
hotfix/site-down
```

**内容更新分支：**
```
content/update-pricing
content/new-team-photos
```

**重构分支：**
```
refactor/api-restructure
refactor/component-cleanup
```

### 部署分支到测试环境

**Cloudflare Pages预览部署：**

推送到GitHub的每个分支都会获得自动预览部署：
```
分支：feat/add-services-page
预览URL：feat-add-services-page.project.pages.dev

更改立即部署到预览URL
在不影响生产的情况下测试
```

**测试清单：**
- [ ] 功能按预期工作
- [ ] 无控制台错误
- [ ] 在移动设备上工作
- [ ] 在桌面上工作
- [ ] 表单正确提交
- [ ] 链接工作
- [ ] 图像加载
- [ ] 无损坏的布局

**仅在测试后：**
合并到main并部署到生产。

### 为什么这保护生产

**没有分支：**
```
提交损坏代码 → 推送到main → 生产中断
→ 客户抱怨 → 紧急修复 → 压力
```

**有分支：**
```
提交损坏代码 → 推送到分支 → 在预览上测试
→ 看到它损坏 → 在分支上修复 → 再次测试
→ 工作 → 合并到main → 生产保持稳定
```

**真实示例：**
"添加了联系表单，但忘记连接电子邮件服务。没有分支测试，表单会在损坏时上线。通过预览测试，立即发现，修复它，然后部署。"

### 合并到Main工作流程

**分步进行：**

**1. 确保分支是最新的**
```bash
git checkout main
git pull origin main
git checkout feat/your-feature
git merge main  # 合并对main所做的任何更改
```

**2. 最后测试一次**
将main合并到您的分支后，再次测试。

**3. 合并到Main**
```bash
git checkout main
git merge feat/your-feature
```

**4. 推送到生产**
```bash
git push origin main
```

**5. 监控**
注意：
- 成功部署
- 日志中无错误
- 站点正确加载
- 功能在生产中工作

**6. 清理**
```bash
git branch -d feat/your-feature
```

### 回滚策略

**如果生产中断：**

**选项1：立即恢复**
```bash
# 找到错误提交
git log --oneline

# 恢复它
git revert abc1234

# 立即推送
git push origin main

# 生产在约60秒内恢复
```

**选项2：从分支挑选修复**
```bash
# 创建热修复分支
git checkout -b hotfix/critical-fix

# 进行修复
git add .
git commit -m "Fix critical production issue"

# 在预览上测试

# 挑选到main
git checkout main
git cherry-pick xyz5678

# 推送
git push origin main
```

**选项3：重置到最后已知良好**
```bash
# 核选项 - 仅在必要时使用
git reset --hard [last-good-commit]
git push origin main --force

# 生产立即恢复
```

### 与客户沟通

**维护前：**
```
"嗨 [客户]，

我今天将添加新的服务页面。站点更新时可能会有短暂的
时刻，但应该是无缝的。

完成后我会通知您！

祝好，
[您的名字]"
```

**部署后：**
```
"嗨 [客户]，

新的服务页面现已上线！在[URL]查看。

所有内容都已测试并顺利运行。

如果您想要任何调整，请告诉我！

祝好，
[您的名字]"
```

**如果出现问题（罕见）：**
```
"嗨 [客户]，

部署后我发现了一个问题，并立即回滚
到稳定版本。您的站点正常工作。

我现在正在修复问题，稍后将通过适当的
测试再次部署。没有造成任何损害。

为短暂的困惑感到抱歉！

祝好，
[您的名字]"
```

## 分支管理

### 按AI会话创建分支

**模式：**
一个AI对话 = 一个功能分支（通常）。

**工作流程：**
```bash
# 开始新的AI会话
# AI："我们今天在做什么？"
# 您："添加用户注册"

# 创建分支
git checkout -b feat/user-registration

# 与AI一起完成功能
# ... 多次提交 ...

# 完成时
git checkout main
git merge feat/user-registration
git push origin main
```

**优势：**
- 工作清晰分离
- 易于回滚特定的AI生成更改
- 每个功能的上下文隔离
- 安全实验

### AI上下文的分支命名

**描述性名称帮助AI：**
```bash
# 良好 - AI理解目标
feat/add-contact-form-with-validation

# 糟糕 - AI不知道上下文
branch-1
```

**AI提示：**
```
"我们正在处理[功能]。我创建了一个名为
feat/[feature-name]的分支。让我们逐步实现这个。"
```

### 何时合并与变基

**使用合并（推荐大多数情况）：**
```bash
git checkout main
git merge feat/your-feature
```

**优点：**
- 保留完整历史
- 显示功能何时集成
- 更容易理解
- AI可以无问题地执行此操作

**何时使用变基（高级）：**
```bash
git checkout feat/your-feature
git rebase main
```

**优点：**
- 干净、线性历史
- 无合并提交

**缺点：**
- 重写历史（如果共享则危险）
- 更难向AI解释
- 如果涉及多个人可能会导致问题

**建议**：坚持使用合并以保持简单。

### 清理旧分支

**本地清理：**
```bash
# 列出所有分支
git branch

# 删除已合并的分支
git branch -d feat/completed-feature

# 强制删除未合并的分支（小心！）
git branch -D feat/abandoned-feature
```

**远程清理：**
```bash
# 删除远程分支
git push origin --delete feat/old-feature
```

**自动清理（GitHub）：**
在存储库设置中启用"自动删除头分支"。

**定期维护：**
每月审查：删除>30天前合并的分支。

## 提交策略

### AI生成的提交消息

**让AI建议消息：**
```
提示："审查git差异并建议遵循传统提交格式的提交消息。"

AI响应："feat: 添加带有电子邮件验证的用户注册表单"
```

**格式：**
```
<类型>: <描述>

类型：
- feat: 新功能
- fix: 错误修复
- docs: 文档
- style: 代码样式（格式化，无逻辑更改）
- refactor: 代码重构
- test: 添加测试
- chore: 维护
```

**示例：**
```bash
git commit -m "feat: 添加带有电子邮件验证的用户注册"
git commit -m "fix: 解决身份验证令牌过期问题"
git commit -m "docs: 使用新端点更新API文档"
git commit -m "refactor: 简化数据库查询逻辑"
```

### 您应该压缩提交吗？

**开发期间（发布前）：否**
- 保留所有提交
- 更容易调试
- 清晰进展
- 可以挑选特定更改

**合并到Main之前（发布后）：有时**

**不要压缩：**
- 如果提交讲述清晰的故事
- 如果您可能需要恢复部分
- 如果提交组织良好

**要压缩：**
- 如果许多"WIP"提交
- 如果提交是"修复拼写错误"、"再次修复拼写错误"、"实际修复拼写错误"
- 如果您想要干净的main分支历史

**如何压缩：**
```bash
# 交互式变基
git rebase -i HEAD~5  # 最近5次提交

# 编辑器打开，将"pick"更改为"squash"以组合提交
# 保存并关闭

# Git提示输入新提交消息
# 编写干净的消息

# 强制推送到分支（在合并到main之前）
git push origin feat/your-feature --force
```

### 保留AI决策历史

**为什么保留提交：**
- 显示AI推理进展
- 更容易理解尝试了什么
- 可以稍后审查AI决策
- 有助于学习

**在提交消息中：**
```bash
git commit -m "feat: 使用JWT实现用户身份验证

- AI建议使用bcrypt进行密码哈希
- 尝试了argon2但有依赖问题
- 根据docs/decisions/auth.md选择bcrypt"
```

**使用扩展提交消息：**
```bash
git commit -m "feat: 添加联系表单" -m "使用Claude Code生成。
包括电子邮件验证、通过Turnstile的垃圾邮件保护以及通过
Resend API的电子邮件传递。"
```

### 有意义的提交消息

**糟糕：**
```bash
git commit -m "update"
git commit -m "fix stuff"
git commit -m "changes"
```

**良好：**
```bash
git commit -m "添加带有电子邮件验证和Turnstile的联系表单"
git commit -m "修复移动导航栏切换在链接点击时不关闭"
git commit -m "更新主页英雄图像和CTA文本"
```

**测试：**
有人（包括未来的您）能否仅通过阅读提交消息来理解更改了什么？

## 恢复工作流程

### 处理AI引入的错误

**场景：** AI生成了破坏某些东西的代码。

**步骤1：识别何时中断**
```bash
# 检查最近提交
git log --oneline

# 识别可疑提交
```

**步骤2：审查更改**
```bash
# 查看提交更改的内容
git show abc1234
```

**步骤3：决定修复**

**选项A：恢复整个提交**
```bash
git revert abc1234
git push origin main
```

**选项B：向前修复**
```bash
# 告诉AI关于错误
"提交abc1234的代码有一个错误：[描述]。修复它。"

# AI生成修复
git add .
git commit -m "修复：解决先前AI实现的错误"
git push origin main
```

**选项C：挑选好的部分**
```bash
# 恢复整个提交
git revert abc1234

# 手动重新应用仅好的更改
# 或使用AI帮助
"保留提交abc1234的功能X和Y，但删除导致错误的
功能Z。"
```

### 使用Git Bisect

**何时：** 错误存在，但不确定哪个提交引入了它。

**工作流程：**
```bash
# 开始bisect
git bisect start

# 将当前状态标记为错误
git bisect bad

# 找到您知道良好的提交
git log --oneline
git bisect good xyz5678

# Git检出中间的提交
# 测试错误是否存在
# 如果是：
git bisect bad
# 如果否：
git bisect good

# 重复直到Git识别出错误提交
# Git会说："abc1234是第一个错误提交"

# 结束bisect
git bisect reset

# 现在您确切知道哪个提交破坏了东西
```

**使用AI：**
```
"Git bisect识别提交abc1234引入了错误。
这是提交差异：[粘贴差异]

是什么导致了问题以及我们如何修复它？"
```

### 回滚程序

**紧急回滚（站点关闭）：**

```bash
# 1. 检查当前状态
git log --oneline

# 2. 识别最后已知良好提交
# （在问题开始之前）

# 3. 重置到该提交
git reset --hard [good-commit]

# 4. 强制推送以恢复生产
git push origin main --force

# 5. 通知客户（如果适用）
# 6. 在分支上修复问题，测试，然后正确部署
```

**恢复时间：** < 2分钟

**更安全的回滚（非紧急）：**
```bash
# 恢复有问题的提交
git revert [bad-commit]

# 推送
git push origin main

# 这会创建一个新提交来撤销错误的提交
# 保留历史
```

### 挑选修复

**场景：** 在错误的分支上进行了修复，需要它在main上。

```bash
# 找到带有修复的提交
git log --oneline feat/wrong-branch

# 切换到正确分支
git checkout main

# 挑选修复提交
git cherry-pick abc1234

# 推送
git push origin main
```

**AI集成：**
```
"我不小心将修复abc1234提交到feat/wrong-branch。
为我挑选它到main。"
```

## AI的Git钩子

### 提交前验证

**在提交前自动检查：**

**创建`.git/hooks/pre-commit`：**
```bash
#!/bin/bash

# 运行linter
npm run lint
if [ $? -ne 0 ]; then
    echo "Linting failed. Please fix errors before committing."
    exit 1
fi

# 运行测试
npm run test
if [ $? -ne 0 ]; then
    echo "Tests failed. Please fix before committing."
    exit 1
fi

# 检查console.logs（可选）
if git diff --cached | grep -q "console.log"; then
    echo "Warning: console.log found. Remove before committing."
    # 取消注释以阻止：
    # exit 1
fi

echo "Pre-commit checks passed!"
```

**使其可执行：**
```bash
chmod +x .git/hooks/pre-commit
```

**AI集成：**
```
"每次提交前，通过git钩子自动运行linter和测试。
如果它们失败，阻止提交。"
```

### 自动测试钩子

**预推送钩子：**
```bash
#!/bin/bash

echo "Running tests before push..."

npm run test:unit
if [ $? -ne 0 ]; then
    echo "Unit tests failed. Push aborted."
    exit 1
fi

npm run test:integration
if [ $? -ne 0 ]; then
    echo "Integration tests failed. Push aborted."
    exit 1
fi

echo "All tests passed. Pushing..."
```

### 提交前Linting

**修复问题的钩子：**
```bash
#!/bin/bash

echo "Running auto-fix linter..."

# 运行带有自动修复的linter
npm run lint:fix

# 暂存修复的文件
git add .

echo "Linting complete!"
```

**AI可以生成这些：**
```
"创建一个预提交git钩子，运行'npm run lint:fix'并
自动暂存修复的文件。"
```

## 与多个AI代理的协作Git

### 管理并行开发

**场景：** 在不同功能上使用多个AI代理。

**策略：**
```bash
# 代理1：前端
git checkout -b feat/frontend-dashboard
# ... 工作 ...

# 代理2：后端API
git checkout -b feat/backend-api
# ... 工作 ...

# 准备时合并两者
git checkout main
git merge feat/frontend-dashboard
git merge feat/backend-api
```

**协调：**
- 每个代理/功能使用单独分支
- 在docs/中记录依赖关系
- 频繁合并以避免大冲突
- 在生产部署前测试集成系统

### 合并冲突解决

**当冲突发生时：**

```bash
# 尝试合并
git merge feat/other-feature

# Git说：src/components/Header.astro中的CONFLICT

# 查看冲突
git status

# 打开文件，查看冲突标记：
<<<<<<< HEAD
当前代码
=======
传入代码
>>>>>>> feat/other-feature
```

**使用AI：**
```
"我在Header.astro中有两个版本之间的合并冲突：

版本A（当前）：
[粘贴代码]

版本B（传入）：
[粘贴代码]

通过结合两者的优点同时保持
功能来解决冲突。"
```

**解决后：**
```bash
git add src/components/Header.astro
git commit -m "Merge feat/other-feature, resolve Header conflicts"
```

### 跨分支上下文保留

**使用文档进行共享上下文：**

```
docs/
├── architecture/
│   └── api-design.md          # 两个代理都引用
├── context/
│   ├── agent1-frontend.md     # 代理1的当前状态
│   └── agent2-backend.md      # 代理2的当前状态
```

**代理1（前端）：**
```
"阅读docs/architecture/api-design.md了解API契约。
继续在feat/frontend-dashboard分支上进行前端工作。"
```

**代理2（后端）：**
```
"阅读docs/architecture/api-design.md了解API契约。
继续在feat/backend-api分支上进行后端工作。"
```

**两者保持一致：** 共享文档使代理保持同步。

## 关键要点

### 两阶段方法
**发布前：快速简单**
- 推送到main
- 快速迭代
- 无分支开销

**发布后：稳定安全**
- 强制分支
- 合并前测试
- 保护生产

### 时间投资与风险
**开发阶段：**
- 风险：低（无用户）
- 分支价值：低
- 直接到main：适当

**维护阶段：**
- 风险：高（用户依赖站点）
- 分支价值：高
- 分支工作流程：必要

### 恢复 > 预防
**您会犯错误：**
- 频繁提交
- 推送前测试
- 有回滚计划
- 不要害怕破坏东西（您可以修复它们）

**Git是您的安全网：**
- 每次提交都是一个恢复点
- 恢复很简单
- 历史被保留
- 远程是您的备份

### AI集成
**与AI良好工作的Git工作流程：**
- 简单分支模型
- 清晰提交消息
- 频繁提交
- 记录的流程

**要避免：**
- 复杂变基
- 交互式变基
- 挑选马拉松
- 令人困惑的分支结构

---

**相关文档：**
- [Git命令备忘单](../development-tools/cheat-sheets/git-commands-zh.md) - 常见Git操作的快速参考
- [阶段2：开发](./phase-2-development-zh.md) - 开发工作流程
- [阶段4：部署](./phase-4-deployment-zh.md) - 生产部署
- [上下文管理](../context-management/README-zh.md) - 使用docs/进行协调
- [故障排除：恢复模式](../troubleshooting/recovery-patterns-zh.md) - 紧急程序
- [客户管理](../business-model/client-management-zh.md) - 沟通部署

**返回：** [工作流程概述](./README-zh.md)