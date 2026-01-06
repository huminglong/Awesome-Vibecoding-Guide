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
```