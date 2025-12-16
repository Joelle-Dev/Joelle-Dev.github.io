---
title: Git 命令操作完全指南：从入门到精通
date: 2025-12-15 20:30:00
tags:
  - Git
  - 版本控制
  - 笔记
---

## 前言

Git 是目前最流行的分布式版本控制系统，掌握 Git 命令是每个开发者的必备技能。本文将从基础到高级，全面介绍 Git 的常用命令和最佳实践。

## 一、Git 基础概念

### 1.1 三个工作区域

Git 有三个主要的工作区域：

- **工作区（Working Directory）**：你正在编辑的文件
- **暂存区（Staging Area）**：准备提交的文件
- **版本库（Repository）**：已提交的历史记录

```
工作区 → git add → 暂存区 → git commit → 版本库
```

### 1.2 文件状态

文件在 Git 中有四种状态：

- **未跟踪（Untracked）**：新文件，Git 还未管理
- **已修改（Modified）**：文件已修改但未暂存
- **已暂存（Staged）**：文件已添加到暂存区
- **已提交（Committed）**：文件已保存到版本库

## 二、初始配置

### 2.1 用户信息配置

```bash
# 设置全局用户名和邮箱
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 查看配置
git config --global user.name
git config --global user.email

# 查看所有配置
git config --list
```

### 2.2 其他常用配置

```bash
# 设置默认编辑器
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"          # Vim

# 设置默认分支名
git config --global init.defaultBranch main

# 设置换行符处理（Windows）
git config --global core.autocrlf true

# 设置换行符处理（Linux/Mac）
git config --global core.autocrlf input

# 设置别名（简化命令）
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```

## 三、仓库操作

### 3.1 创建和克隆仓库

```bash
# 初始化新仓库
git init

# 克隆远程仓库
git clone https://github.com/user/repo.git

# 克隆到指定目录
git clone https://github.com/user/repo.git my-project

# 克隆指定分支
git clone -b branch-name https://github.com/user/repo.git

# 浅克隆（只获取最新提交，加快速度）
git clone --depth 1 https://github.com/user/repo.git
```

### 3.2 查看仓库状态

```bash
# 查看工作区状态
git status

# 简洁模式
git status -s
git status --short

# 查看文件变更详情
git status -v
```

## 四、文件操作

### 4.1 添加文件到暂存区

```bash
# 添加单个文件
git add filename.txt

# 添加多个文件
git add file1.txt file2.txt

# 添加所有修改的文件
git add .

# 添加所有文件（包括删除的）
git add -A
git add --all

# 交互式添加（选择性地添加文件的部分内容）
git add -p
git add --patch

# 添加所有 .js 文件
git add *.js

# 添加某个目录下的所有文件
git add src/
```

### 4.2 提交更改

```bash
# 提交暂存区的文件
git commit -m "提交信息"

# 提交并添加所有已跟踪的修改（跳过 git add）
git commit -a -m "提交信息"
git commit -am "提交信息"

# 修改最后一次提交信息
git commit --amend -m "新的提交信息"

# 修改最后一次提交，添加遗漏的文件
git add forgotten-file.txt
git commit --amend --no-edit

# 查看提交历史
git log

# 简洁模式
git log --oneline

# 图形化显示分支
git log --oneline --graph --all

# 查看最近 N 次提交
git log -n 5

# 查看文件修改历史
git log filename.txt

# 查看提交的详细变更
git log -p
```

### 4.3 查看差异

```bash
# 查看工作区与暂存区的差异
git diff

# 查看暂存区与版本库的差异
git diff --cached
git diff --staged

# 查看工作区与版本库的差异
git diff HEAD

# 查看两个提交之间的差异
git diff commit1 commit2

# 查看某个文件的差异
git diff filename.txt

# 查看统计信息
git diff --stat
```

### 4.4 撤销操作

```bash
# 撤销工作区的修改（未暂存）
git checkout -- filename.txt
git restore filename.txt  # Git 2.23+

# 撤销所有工作区的修改
git checkout -- .
git restore .

# 从暂存区移除文件（保留工作区修改）
git reset HEAD filename.txt
git restore --staged filename.txt  # Git 2.23+

# 从暂存区移除所有文件
git reset HEAD
git restore --staged .

# 撤销最后一次提交（保留修改）
git reset --soft HEAD~1

# 撤销最后一次提交（保留工作区，清除暂存区）
git reset --mixed HEAD~1
git reset HEAD~1  # 默认是 mixed

# 撤销最后一次提交（完全删除）
git reset --hard HEAD~1

# 撤销到指定提交
git reset --hard commit-hash
```

### 4.5 删除文件

```bash
# 删除文件并添加到暂存区
git rm filename.txt

# 强制删除（即使文件已修改）
git rm -f filename.txt

# 删除暂存区的文件，但保留工作区文件
git rm --cached filename.txt

# 删除目录
git rm -r directory/
```

### 4.6 移动/重命名文件

```bash
# 移动或重命名文件
git mv old-name.txt new-name.txt

# 等价于以下命令：
# mv old-name.txt new-name.txt
# git rm old-name.txt
# git add new-name.txt
```

## 五、分支操作

### 5.1 查看分支

```bash
# 查看本地分支
git branch

# 查看所有分支（包括远程）
git branch -a

# 查看远程分支
git branch -r

# 查看分支的详细信息
git branch -v

# 查看已合并的分支
git branch --merged

# 查看未合并的分支
git branch --no-merged
```

### 5.2 创建和切换分支

```bash
# 创建新分支
git branch branch-name

# 创建并切换到新分支
git checkout -b branch-name
git switch -c branch-name  # Git 2.23+

# 切换到已有分支
git checkout branch-name
git switch branch-name  # Git 2.23+

# 切换到上一个分支
git checkout -
git switch -  # Git 2.23+
```

### 5.3 合并分支

```bash
# 合并指定分支到当前分支
git merge branch-name

# 合并时创建合并提交（即使可以快进）
git merge --no-ff branch-name

# 合并时只允许快进
git merge --ff-only branch-name

# 合并时使用 squash（将多个提交压缩成一个）
git merge --squash branch-name
```

### 5.4 删除分支

```bash
# 删除已合并的分支
git branch -d branch-name

# 强制删除分支（即使未合并）
git branch -D branch-name

# 只查看，不删除
git branch | grep "feature-v"

# 删除所有匹配的分支
git branch | grep "feature-v" | xargs git branch -d

# 排除当前分支，避免误删
git branch | grep "feature-v" | grep -v "^\*" | xargs git branch -d

# 强制删除所有 feature-v 开头的分支
git branch | grep "feature-v" | xargs git branch -D

# 或者更精确的模式
git branch | grep -E "feature-v[0-9]+" | xargs git branch -D

# 删除远程分支
git push origin --delete branch-name

```

### 5.5 分支重命名

```bash
# 重命名当前分支
git branch -m new-name

# 重命名指定分支
git branch -m old-name new-name
```

## 六、远程仓库操作

### 6.1 查看远程仓库

```bash
# 查看远程仓库列表
git remote

# 查看远程仓库详细信息
git remote -v

# 查看远程仓库 URL
git remote get-url origin

# 查看远程分支信息
git remote show origin
```

### 6.2 添加和删除远程仓库

```bash
# 添加远程仓库
git remote add origin https://github.com/user/repo.git

# 修改远程仓库 URL
git remote set-url origin https://github.com/user/new-repo.git

# 删除远程仓库
git remote remove origin
git remote rm origin
```

### 6.3 拉取和推送

```bash
# 拉取远程更新（fetch + merge）
git pull

# 拉取指定分支
git pull origin branch-name

# 只获取远程更新，不合并
git fetch

# 获取所有远程分支
git fetch --all

# 推送本地分支到远程
git push

# 推送指定分支
git push origin branch-name

# 推送并设置上游分支
git push -u origin branch-name
git push --set-upstream origin branch-name

# 强制推送（谨慎使用）
git push --force
git push -f

# 推送所有分支
git push --all

# 推送所有标签
git push --tags
```

## 七、标签操作

### 7.1 创建标签

```bash
# 创建轻量标签
git tag v1.0.0

# 创建附注标签（推荐）
git tag -a v1.0.0 -m "版本 1.0.0 发布"

# 在指定提交上创建标签
git tag -a v1.0.0 commit-hash -m "版本说明"

# 创建带签名的标签
git tag -s v1.0.0 -m "签名标签"
```

### 7.2 查看和删除标签

```bash
# 查看所有标签
git tag

# 查看标签详细信息
git show v1.0.0

# 搜索标签
git tag -l "v1.*"

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
git push origin :refs/tags/v1.0.0
```

### 7.3 推送标签

```bash
# 推送单个标签
git push origin v1.0.0

# 推送所有标签
git push --tags
git push origin --tags
```

## 八、子模块操作

### 8.1 添加子模块

```bash
# 添加子模块
git submodule add https://github.com/user/repo.git path/to/submodule

# 初始化子模块
git submodule init

# 更新子模块
git submodule update

# 初始化和更新一起执行
git submodule update --init --recursive
```

### 8.2 克隆包含子模块的仓库

```bash
# 克隆时同时初始化子模块
git clone --recursive https://github.com/user/repo.git

# 或者克隆后初始化
git clone https://github.com/user/repo.git
cd repo
git submodule update --init --recursive
```

### 8.3 更新子模块

```bash
# 更新子模块到最新版本
git submodule update --remote

# 更新所有子模块
git submodule update --remote --recursive
```

### 8.4 查看子模块状态

```bash
# 查看子模块状态
git submodule status
```

## 九、高级操作

### 9.1 暂存更改（Stash）

```bash
# 暂存当前更改
git stash

# 暂存并添加说明
git stash save "说明信息"

# 查看暂存列表
git stash list

# 应用最近的暂存
git stash apply

# 应用并删除暂存
git stash pop

# 应用指定的暂存
git stash apply stash@{0}

# 删除暂存
git stash drop stash@{0}

# 清空所有暂存
git stash clear

# 暂存未跟踪的文件
git stash -u
git stash --include-untracked
```

### 9.2 查看历史

```bash
# 查看提交历史
git log

# 单行显示
git log --oneline

# 图形化显示
git log --graph

# 显示文件变更统计
git log --stat

# 搜索提交信息
git log --grep="关键词"

# 按作者筛选
git log --author="作者名"

# 按日期筛选
git log --since="2024-01-01"
git log --until="2024-12-31"

# 查看文件的修改历史
git log --follow filename.txt

# 查看文件的每一行修改历史
git blame filename.txt
```

### 9.3 查找和恢复

```bash
# 查找包含特定内容的提交
git log -S "关键词"

# 查找引入 bug 的提交
git bisect start
git bisect bad
git bisect good commit-hash
git bisect reset

# 恢复已删除的文件
git checkout commit-hash -- filename.txt

# 查看某个文件在某个提交时的内容
git show commit-hash:filename.txt
```

### 9.4 清理操作

```bash
# 清理未跟踪的文件
git clean -n  # 预览
git clean -f  # 删除文件
git clean -fd # 删除文件和目录

# 清理未跟踪的文件和目录（交互式）
git clean -i

# 清理并移除忽略的文件
git clean -x
```

## 十、常用工作流程

### 10.1 基本工作流程

```bash
# 1. 查看状态
git status

# 2. 添加文件
git add .

# 3. 提交
git commit -m "提交信息"

# 4. 推送到远程
git push
```

### 10.2 分支开发流程

```bash
# 1. 创建功能分支
git checkout -b feature/new-feature

# 2. 开发并提交
git add .
git commit -m "实现新功能"

# 3. 推送到远程
git push -u origin feature/new-feature

# 4. 合并到主分支
git checkout main
git merge feature/new-feature

# 5. 删除功能分支
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

### 10.3 修复紧急 Bug

```bash
# 1. 从主分支创建热修复分支
git checkout -b hotfix/critical-bug main

# 2. 修复并提交
git add .
git commit -m "修复紧急 bug"

# 3. 合并到主分支和开发分支
git checkout main
git merge hotfix/critical-bug
git checkout develop
git merge hotfix/critical-bug

# 4. 删除热修复分支
git branch -d hotfix/critical-bug
```

## 十一、最佳实践

### 11.1 提交信息规范

**格式**：
```
<type>(<scope>): <subject>

<body>

<footer>
```

**类型（type）**：
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档更新
- `style`: 代码格式调整
- `refactor`: 代码重构
- `test`: 测试相关
- `chore`: 构建/工具相关

**示例**：
```bash
git commit -m "feat(user): 添加用户登录功能"
git commit -m "fix(auth): 修复登录 token 过期问题"
git commit -m "docs: 更新 README 文档"
```

### 11.2 分支命名规范

- `feature/功能名称`：功能开发分支
- `bugfix/问题描述`：Bug 修复分支
- `hotfix/紧急问题`：紧急修复分支
- `release/版本号`：发布分支
- `develop`：开发分支
- `main`/`master`：主分支

### 11.3 日常操作建议

1. **频繁提交**：小步快跑，每次提交一个完整的功能点
2. **清晰的提交信息**：说明做了什么，为什么这样做
3. **定期推送**：避免本地提交丢失
4. **使用分支**：每个功能或修复使用独立分支
5. **代码审查**：合并前进行代码审查
6. **保持主分支稳定**：主分支应该始终是可部署的状态

## 十二、常见问题解决

### 12.1 合并冲突

```bash
# 查看冲突文件
git status

# 手动解决冲突后
git add resolved-file.txt
git commit
```

### 12.2 撤销已推送的提交

```bash
# 创建新提交来撤销（推荐）
git revert commit-hash

# 强制推送（谨慎使用，需要团队协调）
git reset --hard HEAD~1
git push --force
```

### 12.3 找回丢失的提交

```bash
# 查看所有操作历史
git reflog

# 恢复到指定提交
git checkout commit-hash
git checkout -b recovery-branch
```

### 12.4 修改远程仓库地址

```bash
# 查看当前远程地址
git remote -v

# 修改远程地址
git remote set-url origin new-url

# 验证修改
git remote -v
```

## 十三、实用技巧

### 13.1 命令别名

```bash
# 设置别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'

# 使用别名
git st    # 等同于 git status
git co    # 等同于 git checkout
```

### 13.2 忽略文件

创建 `.gitignore` 文件：

```gitignore
# 依赖目录
node_modules/
vendor/

# 构建输出
dist/
build/
*.log

# 环境变量
.env
.env.local

# IDE 配置
.idea/
.vscode/
*.swp

# 操作系统文件
.DS_Store
Thumbs.db
```

### 13.3 查看文件历史

```bash
# 查看文件的完整历史
git log --follow -- filename.txt

# 查看文件的每一行是谁修改的
git blame filename.txt

# 查看文件在某个提交时的内容
git show commit-hash:filename.txt
```

## 十四、总结

Git 是一个强大的版本控制工具，掌握这些命令可以大大提高开发效率。关键要点：

1. **理解三个工作区域**：工作区、暂存区、版本库
2. **掌握基本流程**：add → commit → push
3. **善用分支**：每个功能独立分支开发
4. **规范提交信息**：便于追踪和回滚
5. **定期同步**：保持本地和远程同步

## 参考资源

- [Git 官方文档](https://git-scm.com/doc)
- [Pro Git 电子书](https://git-scm.com/book)
- [GitHub 帮助文档](https://docs.github.com/)
- [Git 可视化学习](https://learngitbranching.js.org/)

---

**提示**：建议在实际项目中多练习这些命令，熟能生巧。遇到问题时，可以使用 `git help <command>` 查看命令的详细帮助信息。

