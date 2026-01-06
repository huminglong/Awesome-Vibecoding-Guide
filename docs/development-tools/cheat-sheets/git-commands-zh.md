# Git命令速查表

按工作流程组织的基本git命令快速参考。

## 日常命令

### 检查状态
```bash
# 查看更改了什么
git status

# 短格式
git status -s

# 显示分支和跟踪信息
git status -sb
```

### 暂存更改
```bash
# 暂存所有更改
git add .

# 暂存特定文件
git add path/to/file.js

# 按模式暂存特定文件
git add "*.js"

# 交互式暂存（选择要暂存的内容）
git add -p

# 暂存已修改和已删除的文件（不包括新文件）
git add -u
```

### 提交更改
```bash
# 使用消息提交
git commit -m "Add feature X"

# 使用多行消息提交
git commit -m "Add feature X" -m "Detailed description here"

# 使用heredoc提交消息（更好的格式）
git commit -m "$(cat <<'EOF'
Add user authentication feature

- Implement login/logout
- Add password hashing
- Create session management
EOF
)"

# 提交所有已修改的文件（跳过暂存）
git add . && git commit -m "message"

# 修改最后一次提交（更改消息或添加文件）
git commit --amend -m "New message"

# 修改而不更改消息
git commit --amend --no-edit
```

### 推送到远程
```bash
# 推送当前分支
git push

# 推送并设置上游
git push -u origin branch-name

# 强制推送（危险！谨慎使用）
git push --force-with-lease

# 推送所有分支
git push --all

# 推送标签
git push --tags
```

### 从远程拉取
```bash
# 拉取当前分支
git pull

# 拉取特定分支
git pull origin main

# 使用rebase而不是merge拉取
git pull --rebase

# 拉取所有分支
git fetch --all
```

### 查看更改
```bash
# 查看未暂存的更改