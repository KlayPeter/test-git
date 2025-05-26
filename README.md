# Git 团队协作流程指南（以“学习笔记网站”为例）

适用于前端、后端、全栈项目多人协作开发，覆盖 Git 基础、高级用法、冲突解决、远程协作及常见问题。

项目背景：团队协作开发“学习笔记分享网站 study-note-site”，使用 Git + GitHub。

---

## 1️⃣  项目初始化与配置（本地）

```bash
# 创建项目目录并进入
mkdir study-note-site && cd study-note-site

# 初始化本地 Git 仓库
git init

# 配置用户名邮箱（只需配置一次）
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# 设置默认主分支为 main（推荐）
git config --global init.defaultBranch main
```

## 2️⃣ 创建远程仓库并关联 GitHub

```bash
# 创建 GitHub 仓库后绑定远程
git remote add origin https://github.com/yourname/study-note-site.git

# 提交第一个版本
touch README.md
git add .
git commit -m "init: initialize project"
git push -u origin main  # 首次 push 添加 -u，后续只需 git push

```

## 3️⃣ 分支管理规范（建议采用功能分支模型）

| 分支名称 | 说明                           |
| -------- | ------------------------------ |
| main     | 稳定生产版本                   |
| dev      | 开发主干（所有功能从这里派生） |
| feat/xxx | 新功能开发（如 feat/ui）       |
| fix/xxx  | Bug 修复                       |

```bash
git commit -m "feat: 添加笔记标签功能"
```

```bash
# 创建开发主干分支
git checkout -b dev
git push -u origin dev
```

## 4️⃣ 日常开发流程

```bash
# 1. 从 dev 拉出功能分支
git checkout dev
git pull origin dev  # 获取最新代码
git checkout -b feat/markdown-editor  # 新建并切换

# 2. 编码 & 提交（小步提交）
git status              # 查看修改状态
git add .               # 添加所有变更
git commit -m "feat: 增加 markdown 编辑器 UI"  # 提交说明

# 3. 推送远程
git push -u origin feat/markdown-editor
```

> 建议遵循 [Conventional Commits](https://www.conventionalcommits.org/) 提交规范。

------

## 5️⃣高级开发命令汇总

###  stash 暂存改动（临时保存现场）

```bash
git stash         # 保存当前改动
git stash pop     # 恢复改动并从栈中移除
git stash list    # 查看所有 stash 项
```

###  cherry-pick 拣选提交

```bash
git cherry-pick <commit-hash>  # 将指定提交合并到当前分支
```

###  rebase（保持提交线性）

```bash
git checkout feat/xxx
git rebase dev  # 将当前分支最新变基到 dev 上
```

### revert / reset（撤销操作）

```bash
git revert <commit>   # 生成一个新提交，撤销指定提交（安全）
git reset --soft HEAD~1   # 撤销上次 commit，保留改动
git reset --hard HEAD~1   # 强制回退并删除本地更改（危险）
```

## 6️⃣ 功能合并与冲突解决

### 合并流程（以 feat 分支合并回 dev 为例）：

```bash
# 1. 切换到 dev 分支
git checkout dev
git pull origin dev

# 2. 合并功能分支
git merge feat/markdown-editor
```

如果出现冲突：

- `<<<<<<< HEAD` 是当前分支代码
- `=======` 分隔符
- `>>>>>>> 分支名` 是要合并的分支代码

- 打开冲突文件，手动解决冲突区域
- 使用 `git add` 标记已解决
- 再 `git commit` 合并结果

```bash
git add .
git commit -m "fix: 解决编辑器功能冲突"
```

## 7️⃣  远程协作流程（典型开发节奏）

```bash
# 克隆远程仓库
git clone https://github.com/yourname/study-note-site.git

# 获取远程新分支
git fetch origin
git checkout -b feat/xx origin/feat/xx

# 合并远程更新到本地（经常使用）
git pull origin dev

```

## 8️⃣ Tag 标签发布版本

```bash
# 创建标签
git tag v1.0.0

# 推送标签
git push origin v1.0.0

# 删除标签
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0

```

## 9️⃣ 多人协作建议

- **功能分支开发**：每人创建自己的 `feat/xxx` 分支
- **每天 push & pull**：保持与远程仓库同步
- **合并需要代码审核**：先发起 PR（pull request），团队通过后合并
- **避免直接在 main/dev 修改**

------

## 9️⃣ Git 图示命令（推荐）

```bash
git log --oneline --graph --all --decorate
```

退出：按键盘 `q`

其他模式：

```bash
git log --stat              # 每个提交改动的文件和行数
git log -p                  # 显示每次提交具体 diff
git log --author="mmx"      # 查看某人提交历史
```

## .gitignore 推荐（前端项目）

```
node_modules/
dist/
.env
.vscode/
.DS_Store
*.log
```

## SSH 配置 GitHub（推荐）

```bash
# 1. 生成 SSH 密钥
ssh-keygen -t ed25519 -C "你的邮箱"

# 2. 拷贝 SSH 公钥
cat ~/.ssh/id_ed25519.pub

# 3. 添加到 GitHub 账号 > Settings > SSH and GPG keys

# 4. 设置远程仓库为 SSH
git remote set-url origin git@github.com:用户名/study-note-site.git
```
