# VS Code + Git + GitHub 自查手册

> 适用场景：Windows + VS Code + GitHub（HTTPS 方式）
> 核心原则：一个 VS Code 窗口 = 一个项目文件夹

---

## 一、环境体检（每次重装系统或换电脑后跑一次）

### 1.1 Git 是否安装
打开 VS Code 底部终端（`Ctrl + \``），输入：
```bash
git --version
```
- **正常**：显示 `git version 2.xxx.x`
- **异常**：`command not found` → 去 https://git-scm.com/downloads 重装

### 1.2 Git 身份是否配置
```bash
git config --global user.name
git config --global user.email
```
- **正常**：显示你的名字和邮箱
- **异常**：空白 → 补配置：
  ```bash
  git config --global user.name "你的GitHub用户名"
  git config --global user.email "你注册GitHub的邮箱"
  ```

### 1.3 VS Code 是否识别 Git
打开一个已有 `.git` 文件夹的项目，看左下角：
- **正常**：显示分支名（如 `master` 或 `main`）
- **异常**：没有分支图标 → `Ctrl+Shift+P` → 输入 `Git: 安装`

### 1.4 能否连上 GitHub
```bash
git remote -v
```
- **正常**：显示 `origin https://github.com/...`
- **异常**：空白 → 见"三、首次关联 GitHub"

---

## 二、日常标准工作流（每次改代码/写论文必走）

### 2.1 开始工作前（拉取最新）
```bash
git pull
```
或在 VS Code 左侧 Git 面板点 **⬇️ 拉取**。

### 2.2 修改文件
在 VS Code 里正常编辑、保存（`Ctrl+S`）。

### 2.3 暂存（Staging）
左侧 Git 面板 → 找到改动的文件 → 点文件旁的 **➕**。
- 或点"更改"旁的 **➕** 一次性全选。
- 文件会进入"暂存的更改"区域。

### 2.4 提交（Commit）
在顶部消息框输入备注，点 **提交**。
- 备注规范：
  - `feat: 添加新功能`
  - `fix: 修复Bug`
  - `docs: 更新文档`
  - `chore: 配置调整`

### 2.5 推送到 GitHub（Push）
点 Git 面板下方的 **🔄 同步** 或 **⬆️ 推送**。
```bash
git push
```

### 2.6 结束工作
关闭 VS Code 即可，云端已有备份。

---

## 三、首次关联 GitHub（一个项目只做一次）

### 3.1 在 GitHub 创建仓库
1. 登录 github.com → 右上角 `+` → `New repository`
2. 填仓库名（建议与本地文件夹同名）
3. **不要勾选** "Initialize this repository with a README"
4. 点 `Create repository`
5. 复制 HTTPS 地址（如 `https://github.com/用户名/仓库.git`）

### 3.2 本地关联
```bash
git remote add origin https://github.com/用户名/仓库.git
```

### 3.3 验证
```bash
git remote -v
```
应显示两行 `origin` 地址。

### 3.4 首次推送
```bash
git push -u origin master
```
- 会弹出浏览器让你登录 GitHub，授权即可。
- 以后直接 `git push` 就行，不用再加后面那一串。

---

## 四、VS Code Git 面板按钮速查

### 4.1 文件行按钮（每个文件右侧）

| 图标 | 含义 | 点击效果 |
|------|------|---------|
| 📄 | 打开文件 | 在编辑器里打开 |
| ↩️ | 放弃更改 | **一键恢复成上次提交的样子**（改错了用） |
| ➕ | 暂存更改 | 把文件放进"准备提交"的篮子 |
| U/A/M | 状态标签 | U=未追踪, A=新增, M=修改 |

### 4.2 底部操作栏按钮

| 图标 | 含义 | 点击效果 |
|------|------|---------|
| 👤 自动 | 自动拉取开关 | 一般保持开启 |
| ⬇️ | 拉取（Pull） | 从 GitHub 下载最新版 |
| ⬆️ | 推送（Push） | 把本地提交上传到 GitHub |
| 🔄 | 同步（Sync） | **拉取+推送，一键搞定，最常用** |
| 🔄 | 刷新 | 刷新面板状态 |

---

## 五、常见问题急救

| 症状 | 原因 | 解决 |
|------|------|------|
| `git push` 报错 `rejected` | 远程有你没拉取的更新 | `git pull` 后再 `git push` |
| 改错了想恢复 | 还没提交 | 点文件旁 **↩️ 放弃更改** |
| 误删了已提交的文件 | 需要回滚 | `git log --oneline` 找版本号 → `git revert 版本号` |
| 提交了不该提交的大文件 | 历史记录被污染 | 用 `git-filter-repo` 清理，不要只删文件 |
| `SSL certificate problem` | 证书问题（公司/校园网常见） | 临时绕过：`git config --global http.sslVerify false` |
| 左下角没有 Git 分支图标 | VS Code 没找到 Git | 检查 `git.path` 设置，或重装 Git |
| 多个项目塞在一个文件夹里 | 结构错误 | **每个项目单独一个文件夹，单独一个 VS Code 窗口** |

---

## 六、项目结构规范

### 6.1 正确做法
```
D:\Projects\n├── thesis-climate\          ← VS Code 窗口 1
├── software-copyright\      ← VS Code 窗口 2
├── heritage-lab\            ← VS Code 窗口 3
└── test-git\                ← VS Code 窗口 4
```

### 6.2 错误做法
```
D:\我的工作\                 ← 一个窗口打开这个
├── thesis-climate\          ← 混在一起
├── software-copyright\
└── heritage-lab\
```

### 6.3 每个项目根目录必备
- `.gitignore`：告诉 Git 哪些文件不要追踪（临时文件、大数据、密码等）
- `README.md`：项目说明（可选）

---

## 七、核心命令速记卡

| 场景 | 命令 |
|------|------|
| 新建项目，让 Git 接管 | `git init` |
| 查看当前状态 | `git status` |
| 暂存所有改动 | `git add .` |
| 提交到本地 | `git commit -m "备注"` |
| 上传到 GitHub | `git push` |
| 从 GitHub 下载 | `git pull` |
| 查看提交历史 | `git log --oneline` |
| 撤销上次提交（未推送时） | `git reset --soft HEAD~1` |
| 放弃某个文件的修改 | `git checkout -- 文件名` |

---

## 八、与 AI 编程工具（TRAE/Claude Code）配合

1. **让 AI 改动前**：先 `git commit` 或打标签 `git tag backup-before-ai`
2. **AI 生成大量代码后**：审查 diff，确认无误再手动提交
3. **AI 建议删除文件**：必须人工确认，禁止自动删除
4. **AI 生成 Commit Message**：可以采纳，但建议自己修改后再提交

---

> 最后更新：2026-07-25
> 适用账号：Architect-Mingwei
> 邮箱：mingwei3434@163.com
