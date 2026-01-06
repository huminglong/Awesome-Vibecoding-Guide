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
git diff

# 查看已暂存的更改
git diff --staged

# 查看特定文件的更改
git diff path/to/file.js

# 查看分支之间的更改
git diff main..feature-branch

# 查看提交之间的更改
git diff abc123..def456

# 词级差异（更适合文本）
git diff --word-diff
```

### 查看历史
```bash
# 查看提交历史
git log

# 每次提交一行
git log --oneline

# 最后5次提交
git log -5

# 按作者提交
git log --author="John"

# 自某日期以来的提交
git log --since="2 weeks ago"

# 带文件更改的提交
git log --stat

# 美化格式
git log --oneline --graph --decorate --all

# 搜索提交消息
git log --grep="bug fix"

# 显示修改了文件的提交
git log --follow path/to/file.js
```

## 分支

### 创建分支
```bash
# 创建新分支
git branch feature-name

# 创建并切换到新分支
git checkout -b feature-name

# 现代：创建并切换
git switch -c feature-name

# 从特定提交创建分支
git branch feature-name abc123

# 从远程分支创建分支
git checkout -b feature-name origin/feature-name
```

### 切换分支
```bash
# 切换到现有分支
git checkout branch-name

# 现代：切换分支
git switch branch-name

# 切换到上一个分支
git checkout -

# 切换并创建（如果不存在）
git checkout -b branch-name
```

### 列出分支
```bash
# 列出本地分支
git branch

# 列出所有分支（本地+远程）
git branch -a

# 仅列出远程分支
git branch -r

# 列出带最后提交消息
git branch -v

# 列出已合并的分支
git branch --merged

# 列出未合并的分支
git branch --no-merged
```

### 删除分支
```bash
# 删除本地分支（安全，如果未合并则阻止）
git branch -d feature-name

# 强制删除本地分支
git branch -D feature-name

# 删除远程分支
git push origin --delete feature-name

# 删除多个分支
git branch -d branch1 branch2 branch3
```

### 合并分支
```bash
# 将分支合并到当前分支
git merge feature-name

# 合并不使用快进（创建合并提交）
git merge --no-ff feature-name

# 合并并压缩所有提交
git merge --squash feature-name

# 中止合并（如果冲突）
git merge --abort
```

## 撤销和恢复

### 取消暂存文件
```bash
# 取消暂存特定文件
git restore --staged path/to/file.js

# 旧语法（仍然有效）
git reset HEAD path/to/file.js

# 取消暂存所有文件
git restore --staged .
```

### 丢弃更改
```bash
# 丢弃特定文件的更改
git restore path/to/file.js

# 旧语法（仍然有效）
git checkout -- path/to/file.js

# 丢弃所有更改
git restore .

# 丢弃未跟踪的文件
git clean -fd

# 预览将要删除的内容
git clean -n
```

### 撤销提交
```bash
# 撤销最后一次提交，保持更改已暂存
git reset --soft HEAD~1

# 撤销最后一次提交，保持更改未暂存
git reset HEAD~1

# 撤销最后一次提交，丢弃更改（危险！）
git reset --hard HEAD~1

# 撤销最后3次提交
git reset --soft HEAD~3

# 撤销到特定提交
git reset --soft abc123
```

### 恢复提交（安全撤销）
```bash
# 创建新提交来撤销最后一次提交
git revert HEAD

# 恢复特定提交
git revert abc123

# 恢复一系列提交
git revert abc123..def456

# 恢复但不提交（仅暂存更改）
git revert --no-commit HEAD
```

### 暂存更改
```bash
# 暂存当前更改
git stash

# 带消息暂存
git stash save "WIP: feature X"

# 暂存包括未跟踪的文件
git stash -u

# 列出暂存
git stash list

# 应用最近的暂存
git stash apply

# 应用并移除暂存
git stash pop

# 应用特定暂存
git stash apply stash@{2}

# 删除暂存
git stash drop stash@{0}

# 清除所有暂存
git stash clear

# 显示暂存内容
git stash show -p stash@{0}
```

## 高级操作

### 摘取
```bash
# 从另一个分支应用提交
git cherry-pick abc123

# 摘取多个提交
git cherry-pick abc123 def456

# 摘取一系列提交
git cherry-pick abc123..def456

# 摘取但不提交
git cherry-pick --no-commit abc123
```

### 变基
```bash
# 将当前分支变基到main
git rebase main

# 交互式变基（编辑历史）
git rebase -i HEAD~5

# 解决冲突后继续变基
git rebase --continue

# 在变基期间跳过提交
git rebase --skip

# 中止变基
git rebase --abort

# 变基并保留合并提交
git rebase --preserve-merges main
```

**交互式变基选项：**
```
pick abc123  # 使用提交
reword abc123  # 使用提交，编辑消息
edit abc123  # 使用提交，停止修改
squash abc123  # 合并到前一个提交
fixup abc123  # 合并，丢弃消息
drop abc123  # 移除提交
```

### 二分查找（查找引入错误的提交）
```bash
# 开始二分查找
git bisect start

# 标记当前提交为错误
git bisect bad

# 标记最后已知的良好提交
git bisect good abc123

# Git将检出中间提交，测试后：
git bisect good  # 如果正常工作
git bisect bad   # 如果损坏

# 重复直到找到错误

# 结束二分查找
git bisect reset

# 使用脚本自动化二分查找
git bisect run npm test
```

### 引用日志（恢复）
```bash
# 查看所有HEAD移动
git reflog

# 查看特定分支的引用日志
git reflog show branch-name

# 恢复"丢失"的提交
git checkout abc123

# 恢复"丢失"的分支
git branch recovered-branch abc123

# 清理旧的引用日志条目
git reflog expire --expire=90.days.ago --all
```

## 远程仓库

### 添加/移除远程
```bash
# 添加远程
git remote add origin https://github.com/user/repo.git

# 查看远程
git remote -v

# 移除远程
git remote remove origin

# 重命名远程
git remote rename origin upstream

# 更改远程URL
git remote set-url origin https://new-url.git
```

### 获取
```bash
# 从origin获取
git fetch origin

# 获取所有远程
git fetch --all

# 获取并清理已删除的分支
git fetch --prune

# 获取特定分支
git fetch origin main
```

### 跟踪远程分支
```bash
# 为当前分支设置上游
git branch --set-upstream-to=origin/main

# 简短形式
git branch -u origin/main

# 推送并设置上游
git push -u origin branch-name
```

## 标签

### 创建标签
```bash
# 创建轻量级标签
git tag v1.0.0

# 创建注释标签（推荐）
git tag -a v1.0.0 -m "Release version 1.0.0"

# 标记特定提交
git tag -a v1.0.0 abc123 -m "Release 1.0.0"
```

### 列出和查看标签
```bash
# 列出所有标签
git tag

# 列出匹配模式的标签
git tag -l "v1.*"

# 显示标签详情
git show v1.0.0
```

### 推送标签
```bash
# 推送特定标签
git push origin v1.0

# 推送所有标签
git push --tags

# 推送提交和标签
git push --follow-tags
```

### 删除标签
```bash
# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
```

## 配置

### 设置用户信息
```bash
# 全局设置姓名
git config --global user.name "John Smith"

# 全局设置邮箱
git config --global user.email "john@example.com"

# 仅为当前仓库设置（无--global）
git config user.name "John Smith"
git config user.email "john@example.com"
```

### 设置编辑器
```bash
# 设置默认编辑器
git config --global core.editor "code --wait"

# VS Code
git config --global core.editor "code --wait"

# Nano
git config --global core.editor "nano"

# Vim
git config --global core.editor "vim"
```

### 别名
```bash
# 创建快捷命令
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual 'log --oneline --graph --decorate --all'

# 现在使用为：
git st  # 替代 git status
git co main  # 替代 git checkout main
```

### 查看配置
```bash
# 列出所有配置
git config --list

# 列出全局配置
git config --global --list

# 列出本地配置（当前仓库）
git config --local --list

# 获取特定配置值
git config user.name
```

## 故障排除

### 修复错误

#### 提交到错误的分支
```bash
# 在错误的分支上：
git reset --soft HEAD~1  # 撤销提交，保持更改
git stash

# 切换到正确分支：
git checkout correct-branch
git stash pop
git add .
git commit -m "message"
```

#### 意外提交了机密信息
```bash
# 从最后一次提交中移除
git rm --cached path/to/secret-file
git commit --amend --no-edit

# 从历史中移除（使用BFG或git-filter-repo）
# https://rtyley.github.io/bfg-repo-cleaner/
```

#### 需要撤销已推送的提交
```bash
# 创建恢复提交（安全）
git revert abc123
git push

# 强制推送（危险，仅在其他人未拉取时使用）
git reset --hard HEAD~1
git push --force-with-lease
```

#### 合并冲突
```bash
# 合并期间：
git status # 查看冲突文件

# 编辑文件，移除冲突标记：
# <<<<<<< HEAD
# your changes
# =======
# their changes
# >>>>>>> branch-name

# 解决后：
git add resolved-file.js
git commit  # 完成合并

# 或中止合并：
git merge --abort
```

#### 分离的HEAD状态
```bash
# 您处于分离的HEAD状态（不在分支上）
# 从当前状态创建分支：
git checkout -b new-branch-name

# 返回到分支：
git checkout main
```

### 常见错误

#### "fatal: not a git repository"
```bash
# 在当前目录初始化git
git init
```

#### "error: failed to push some refs"
```bash
# 其他人先推送了，先拉取
git pull --rebase
git push
```

#### "fatal: refusing to merge unrelated histories"
```bash
# 强制合并无关的仓库
git pull origin main --allow-unrelated-histories
```

#### "Updates were rejected"
```bash
# 先拉取，再推送
git pull
# 解决任何冲突
git push
```

## Git工作流程

### 功能分支工作流程
```bash
# 1. 创建功能分支
git checkout -b feature/add-user-auth

# 2. 进行更改并提交
git add .
git commit -m "Implement user authentication"

# 3. 推送到远程
git push -u origin feature/add-user-auth

# 4. 创建拉取请求（通过GitHub/GitLab UI）

# 5. PR批准后，通过UI或：
git checkout main
git pull
git merge --no-ff feature/add-user-auth
git push

# 6. 删除分支
git branch -d feature/add-user-auth
git push origin --delete feature/add-user-auth
```

### 热修复工作流程
```bash
# 1. 从main创建热修复分支
git checkout main
git checkout -b hotfix/critical-bug

# 2. 修复并提交
git add .
git commit -m "Fix critical authentication bug"

# 3. 合并到main
git checkout main
git merge --no-ff hotfix/critical-bug
git push

# 4. 合并到develop（如果有的话）
git checkout develop
git merge --no-ff hotfix/critical-bug
git push

# 5. 删除热修复分支
git branch -d hotfix/critical-bug
```

### 同步Fork工作流程
```bash
# 1. 添加上游远程（一次）
git remote add upstream https://github.com/original/repo.git

# 2. 获取上游
git fetch upstream

# 3. 合并上游更改
git checkout main
git merge upstream/main

# 4. 推送到您的fork
git push origin main
```

## 有用的Git别名

将这些添加到您的全局配置中：

```bash
# 快捷方式
git config --global alias.st 'status -sb'
git config --global alias.co 'checkout'
git config --global alias.br 'branch'
git config --global alias.cm 'commit -m'
git config --global alias.ca 'commit --amend'
git config --global alias.unstage 'restore --staged'

# 日志
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.last 'log -1 HEAD --stat'
git config --global alias.ls 'log --pretty=format:"%C(yellow)%h %C(blue)%ad%C(red)%d %C(reset)%s%C(green) [%cn]" --decorate --date=short'

# 差异
git config --global alias.df 'diff --color --word-diff'
git config --global alias.ds 'diff --staged'

# 操作
git config --global alias.undo 'reset --soft HEAD~1'
git config --global alias.amend 'commit --amend --no-edit'
git config --global alias.save 'stash save'
git config --global alias.pop 'stash pop'
git config --global alias.branches 'branch -a'
git config --global alias.tags 'tag -l'
git config --global alias.remotes 'remote -v'
```

## Git最佳实践

### 提交消息
```bash
# 良好提交消息结构：
# <类型>: <主题>（最多50个字符）
#
# <正文>（每行72个字符）
#
# <页脚>

# 类型：
# feat: 新功能
# fix: 错误修复
# docs: 文档
# style: 格式化
# refactor: 代码重构
# test: 添加测试
# chore: 维护

# 示例：
git commit -m "feat: add user authentication"
git commit -m "fix: resolve memory leak in image processing"
git commit -m "docs: update API documentation"
```

### 分支命名
```bash
# 良好模式：
feature/add-user-auth
fix/memory-leak
hotfix/critical-bug
release/v1.2.0
docs/update-readme

# 不良模式：
johns-branch
test
fix
new-feature
```

### 提交前
```bash
# 始终检查您要提交的内容
git status
git diff
git diff --staged

# 运行测试
npm test

# 检查代码
npm run lint
```

### 保持历史记录整洁
```bash
# 合并前压缩工作中的提交
git rebase -i HEAD~5

# 使用有意义的提交消息
# 提交逻辑工作单元
# 不要提交注释掉的代码
# 不要提交机密信息/凭据
```

---

**快速提示：**
- 经常提交，定期推送
- 写描述性的提交消息
- 保持提交专注（每次提交一个逻辑更改）
- 不要提交生成的文件（添加到.gitignore）
- 推送前总是先拉取
- 为功能使用分支
- 提交前进行测试