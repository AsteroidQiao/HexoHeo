---
layout: title
title: 🍌Git常用操作整理
date: 2025-06-18 14:40:51
categories: 开发
tags: git
cover: https://gitee.com/AsteroidQiao/library-management-system/raw/master/book-avatar/17502269655781750226964708.png
---

# Git 常用操作整理

## 一、分支管理

### 1. 创建新分支

```bash
# 基于当前分支创建新分支并切换（-b 参数：创建并切换分支）
git checkout -b <新分支名>

# 基于指定提交创建新分支（-b 参数与<commit-hash>结合）
git checkout -b <新分支名> <提交哈希>
```

**参数说明**：

- `-b`：`--create-branch` 的缩写，强制创建新分支并切换，等价于先执行 `git branch <新分支名>` 再执行 `git checkout <新分支名>`。

### 2. 切换分支

```bash
git checkout <分支名>
```

**参数扩展**：

- `-f`：`--force` 强制切换，忽略未保存的修改（危险操作）。

### 3. 查看分支

```bash
# 查看本地分支（-l 参数：显示本地分支，默认行为）
git branch -l

# 查看远程分支（-r 参数：remote，显示远程分支）
git branch -r

# 查看所有分支（-a 参数：all，显示本地和远程分支）
git branch -a

# 查看分支详细信息（-v 参数：verbose，显示最后一次提交）
git branch -v
```

### 4. 合并分支

```bash
# 切换到目标分支后合并源分支（-m 参数：自定义合并提交信息）
git checkout <目标分支>
git merge -m "合并说明" <源分支>
```

**参数说明**：

- `-m <message>`：指定合并提交的信息，若不指定则默认使用分支名称作为信息。

### 5. 删除分支

```bash
# 删除本地分支（-d 参数：--delete，需已合并）
git branch -d <分支名>

# 强制删除本地分支（-D 参数：--delete --force，忽略合并状态）
git branch -D <分支名>

# 删除远程分支（--delete 参数与远程名结合）
git push origin --delete <分支名>
```

## 二、提交管理

### 1. 提交流程

```bash
# 暂存文件（-A 参数：暂存所有修改，包括删除和新增）
git add -A <文件路径> 或 git add .

# 提交更改（-m 参数：必填，提交信息；-a 参数：自动暂存已跟踪文件）
git commit -m "提交信息"
git commit -a -m "提交信息"  # 等价于 git add -u + commit

# 推送到远程（-u 参数：设置上游分支，后续可直接 git push）
git push -u origin <分支名>
```

**参数说明**：

- `-a`：`--all`，仅暂存已被 Git 跟踪的文件（不包含新增未跟踪文件）。
- `-u`：`--set-upstream`，推送时设置本地分支跟踪远程分支，后续可直接使用 `git push` 推送。

### 2. 还原提交

```bash
# 还原单个提交（-n 参数：不自动提交，仅暂存变更）
git revert -n <提交哈希>

# 批量还原连续提交（-m 1 参数：指定主分支方向；--no-commit 不自动提交）
git revert -m 1 --no-commit <起始提交>..<结束提交>
git commit -m "批量还原提交"
```

**参数说明**：

- `-m parent-number`：当还原合并提交时，指定主分支的父提交（`1` 表示第一个父分支，`2` 表示第二个父分支）。
- `--no-commit`：撤销后不自动提交，允许先检查变更。

### 3. 重置提交

```bash
# 软重置：仅修改分支指针（--soft 参数：保留工作区和暂存区）
git reset --soft <提交哈希>

# 混合重置：清空暂存区（默认，--mixed 参数可省略）
git reset --mixed <提交哈希>

# 硬重置：危险操作（--hard 参数：清空工作区和暂存区）
git reset --hard <提交哈希>
```

## 三、远程仓库操作

### 1. 克隆仓库

```bash
# 克隆完整仓库（-b 参数：指定克隆的分支）
git clone <仓库地址>

# 克隆指定分支（-b 参数：--branch，克隆时切换到目标分支）
git clone -b <分支名> <仓库地址>
```

### 2. 拉取与推送

```bash
# 拉取远程更新（-r 参数：--rebase，使用变基方式合并）
git pull origin <分支名>
git pull --rebase origin <分支名>

# 推送到远程（-f 参数：--force，强制推送，覆盖远程历史）
git push origin <分支名>
git push -f origin <分支名>
```

### 3. 关联远程分支

```bash
# 本地新建分支后关联远程（-u 参数：设置上游跟踪）
git push -u origin <本地分支名>

# 设置已有分支跟踪远程（--set-upstream-to 参数：指定跟踪的远程分支）
git branch --set-upstream-to=origin/<远程分支名> <本地分支名>
```

## 四、高级操作

### 1. 交互式变基

```bash
# 从指定提交开始变基（-i 参数：--interactive，交互式修改提交历史）
git rebase -i <提交哈希>
```

### 2. 暂存区管理

```bash
# 暂存当前修改（-u 参数：暂存未跟踪文件；-a 参数：暂存所有修改）
git stash
git stash save "描述"  # 带描述的暂存
git stash push -u  # 暂存未跟踪文件

# 恢复暂存的修改（-p 参数：--patch，交互式恢复）
git stash pop
git stash apply --patch
```

### 3. 补丁操作

```bash
# 生成补丁文件（-p 参数：交互式选择部分修改生成补丁）
git diff > patch.diff
git diff -p > patch.diff

# 应用补丁（-R 参数：反向应用补丁）
git apply patch.diff
git apply -R patch.diff  # 撤销补丁
```

## 五、常见问题解决方案

### 1. 冲突处理

```bash
# 合并时出现冲突
git merge <分支名>  # 出现冲突提示
vi <冲突文件>  # 手动编辑冲突（删除<<<<<<<等标记）
git add <冲突文件>
git commit -m "解决冲突"
```

### 2. 分离头指针处理

```bash
# 从分离头指针状态创建新分支
git checkout -b <新分支名>
```

### 3. 远程分支同步

```bash
# 同步远程分支列表
git fetch --prune
```

## 六、实用技巧

### 1. 查看提交历史

```bash
# 简洁查看提交历史
git log --oneline

# 图形化查看提交历史
git log --graph --oneline --all
```

### 2. 忽略文件

```bash
# 在项目根目录创建.gitignore文件
vi .gitignore

# 添加忽略规则（示例）
*.log
node_modules/
```

### 3. 查看文件修改历史

```bash
# 查看文件的提交历史
git log <文件路径>

# 查看文件的具体修改内容
git blame <文件路径>
```



## 七、参数速查表

| 命令           | 常用参数      | 说明                           |
| -------------- | ------------- | ------------------------------ |
| `git checkout` | `-b`          | 创建并切换分支                 |
|                | `-f`          | 强制切换，忽略未保存修改       |
| `git branch`   | `-d`          | 删除已合并分支                 |
|                | `-D`          | 强制删除分支                   |
|                | `-r`          | 查看远程分支                   |
|                | `-a`          | 查看所有分支                   |
| `git commit`   | `-m`          | 指定提交信息                   |
|                | `-a`          | 自动暂存已跟踪文件             |
| `git push`     | `-u`          | 设置上游跟踪分支               |
|                | `-f`          | 强制推送，覆盖远程历史         |
| `git pull`     | `--rebase`    | 使用变基方式合并               |
| `git revert`   | `-m`          | 指定合并提交的父分支方向       |
|                | `--no-commit` | 撤销后不自动提交               |
| `git reset`    | `--soft`      | 仅修改分支指针                 |
|                | `--hard`      | 清空工作区和暂存区             |
| `git rebase`   | `-i`          | 交互式变基                     |
| `git stash`    | `-u`          | 暂存未跟踪文件                 |
| `git add`      | `-A`          | 暂存所有修改（包括删除和新增） |





### Git 常用参数详解

#### **一、仓库初始化与克隆**

##### `git init` - 初始化仓库

- **参数**：无特殊常用参数
- **示例**：`git init`

##### `git clone` - 克隆远程仓库

- 常用参数：
  - `-b <branch>`：指定克隆的分支（如 `git clone -b main https://github.com/repo.git`）
  - `--depth <n>`：浅克隆，仅获取最近 `n` 次提交（加速克隆大仓库）
  - `--recursive`：递归克隆包含子模块的仓库
- **示例**：
  `git clone -b dev --depth 10 https://github.com/user/repo.git`

#### **二、提交与版本控制**

##### `git add` - 添加文件到暂存区

- 常用参数：
  - `-A` / `--all`：添加所有文件（修改、删除、新增）
  - `-u` / `--update`：仅添加已跟踪文件的修改或删除
  - `-f` / `--force`：强制添加（即使被 `.gitignore` 忽略）
  - `--patch`：交互式选择文件的部分修改添加
- **示例**：
  `git add -A`（添加所有变更）
  `git add --patch file.txt`（交互式添加文件修改）

##### `git commit` - 提交暂存区变更

- 常用参数：
  - `-m <message>`：直接指定提交信息（如 `git commit -m "fix bug"`）
  - `-a` / `--all`：自动添加所有已跟踪文件的修改（跳过 `git add`）
  - `--amend`：修改最后一次提交（补充或修改信息）
  - `--no-verify`：跳过提交验证钩子
- **示例**：
  `git commit -am "优化代码结构"`
  `git commit --amend -m "修正提交信息"`

##### `git revert` - 回退提交

- 常用参数：
  - `-n` / `--no-commit`：回退变更但不自动提交（需手动 `git commit`）
  - `-m <parent-number>`：指定合并提交的父分支（回退合并提交时用）
- **示例**：
  `git revert -m 1 --no-commit <commit-id>`（回退合并提交的主分支变更）

#### **三、分支管理**

##### `git branch` - 管理分支

- 常用参数：
  - `-b <branch>`：创建新分支（如 `git branch -b feature/login`）
  - `-d <branch>`：删除已合并分支
  - `-D <branch>`：强制删除未合并分支
  - `-a`：显示所有分支（含远程）
- **示例**：
  `git branch -b dev origin/main`（基于远程 `main` 分支创建 `dev` 分支）

##### `git checkout` - 切换分支或文件

- 常用参数：
  - `-b <branch>`：创建并切换到新分支
  - `-f` / `--force`：强制切换（忽略未保存变更）
  - `-- <file>`：恢复文件到上一次提交状态
- **示例**：
  `git checkout -b hotfix/1.0`（创建并切换分支）
  `git checkout -- readme.md`（还原文件）

##### `git merge` - 合并分支

- 常用参数：
  - `-m <message>`：指定合并提交信息
  - `--no-ff`：禁用快进合并（强制生成合并提交）
- **示例**：
  `git merge -m "合并 feature 分支" feature`
  `git merge --no-ff dev`（保留分支合并历史）

#### **四、远程仓库操作**

##### `git push` - 推送本地变更到远程

- 常用参数：
  - `-u` / `--set-upstream`：推送并关联远程分支（首次推送用）
  - `-f` / `--force`：强制推送（覆盖远程分支，谨慎使用）
  - `--delete <branch>`：删除远程分支（如 `git push --delete feature`）
- **示例**：
  `git push -u origin main`（首次推送并关联）

##### `git pull` - 拉取远程变更并合并

- 常用参数：
  - `--rebase`：使用 `rebase` 方式合并（保持线性提交历史）
- **示例**：
  `git pull --rebase origin main`

#### **五、历史与日志查看**

##### `git log` - 查看提交历史

- 常用参数：
  - `-p`：显示每次提交的详细变更
  - `-n <num>`：仅显示最近 `num` 次提交（如 `git log -n 5`）
  - `--oneline`：单行显示提交信息
  - `--graph`：图形化显示分支合并历史
- **示例**：
  `git log --oneline --graph --all`（图形化显示所有分支日志）

##### `git diff` - 查看变更差异

- 常用参数：
  - `--staged`：查看暂存区与上次提交的差异
  - `--name-only`：仅显示变更的文件名
  - `<commit1>..<commit2>`：比较两提交间的差异
- **示例**：
  `git diff --staged`（查看暂存区变更）

#### **六、其他实用命令与参数**

##### `git reset` - 重置分支状态

- 常用参数：
  - `--soft`：回退暂存区，保留工作区变更
  - `--hard`：彻底回退到指定提交（丢失未提交变更，危险！）
- **示例**：
  `git reset --soft HEAD^`（撤销最后一次提交，保留变更）

##### `git stash` - 暂存工作区变更

- 常用参数：
  - `save <message>`：保存暂存并添加描述
  - `pop`：恢复最近暂存并删除记录
- **示例**：
  `git stash save "临时保存变更"`
  `git stash pop`（恢复并删除暂存）
