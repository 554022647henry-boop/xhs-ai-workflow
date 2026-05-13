# 小红书 AI 运营工作流

一套基于 Claude Code 的小红书账号 AI 运营系统。通过对话驱动完整的内容发布流程：选题 → 调研 → 写稿 → 审稿 → 发布。

**不需要写代码。** 下载配置后，打开 Claude Code 说一句话就能启动。

---

## 它能做什么

- 一句话启动全流程：说「开始发文」，AI 自动走完选题到发布
- 智能选题：从选题库匹配 + 搜小红书热度 + 去重检查
- 内容调研：自动搜索同类高赞内容，生成参考报告
- 协作写稿：你提供核心观点，AI 按你的风格起草打磨
- 自动发布：生成图片卡片，配标签，一键发布
- 选题库维护：每周扫描热门内容，提取新选题候选

---

## 安装步骤

### 1. 安装 Claude Code

前往 [claude.ai/code](https://claude.ai/code) 下载安装。

### 2. 下载本项目

```bash
git clone https://github.com/YOUR_USERNAME/xhs-ai-workflow.git
# 或者点击页面右上角「Code → Download ZIP」解压
```

### 3. 安装依赖

```bash
pip install markdown pyyaml playwright
playwright install chromium
```

### 4. 安装 Claude Code Skills

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills

# 小红书自动化（登录/搜索/发布）
git clone https://github.com/xpzouying/xiaohongshu-skills xhs-autoclaw

# 图片卡片生成
git clone https://github.com/comeonzhj/Auto-Redbook-Skills xhs-note-creator
```

### 5. 安装 Chrome 扩展

1. 打开 Chrome → `chrome://extensions/`
2. 开启右上角「开发者模式」
3. 点击「加载已解压的扩展程序」
4. 选择 `~/.claude/skills/xhs-autoclaw/extension/`

### 6. 登录小红书

```bash
cd ~/.claude/skills/xhs-autoclaw
python scripts/cli.py login
```

扫码登录，完成后 cookies 自动保存。

### 7. 配置你的账号信息

按顺序修改以下 3 个文件（都有注释说明怎么填）：

| 文件 | 内容 |
|------|------|
| `项目主旨.md` | 账号定位、目标人群、你的人设背景 |
| `写作风格指南.md` | 你的写作风格偏好 |
| `选题库.md` | 你的账号方向和初始选题 |

### 8. 配置唤醒指令

将以下内容追加到 `~/.claude/CLAUDE.md`（没有就新建）：

```markdown
# 小红书运营项目

如果用户提到：小红书、发文、选题、内容发布、开工，立即执行：
1. 读取 {你的项目完整路径}/主控Prompt.md
2. 按照主控 Prompt 的任务路由判断用户意图
3. 开始执行对应流程

项目根目录：{你的项目完整路径}
```

把 `{你的项目完整路径}` 替换为实际路径，例如：
- Mac/Linux：`/Users/yourname/xhs-ai-workflow`
- Windows：`C:/Users/yourname/xhs-ai-workflow`

---

## 开始使用

配置完成后，打开 Claude Code（**无需**打开项目文件夹），直接说：

```
小红书开工
```

AI 会自动读取你的账号背景、已发文章、选题库，然后问你今天想做什么。

---

## 项目结构

```
xhs-ai-workflow/
├── README.md                ← 本文件
├── 主控Prompt.md            ← 主控 Agent，所有流程的统一入口
├── 项目主旨.md              ← 【需要修改】账号定位和人设
├── 项目进程.md              ← 项目进度记录（自动维护）
├── 内容发布进程.md          ← 每篇文章状态（自动维护）
├── 选题库.md                ← 【需要修改】待发选题列表
├── 写作风格指南.md          ← 【需要修改】你的写作风格
├── agents/
│   ├── 初始化Agent.md           ← 首次使用引导
│   ├── 选题Agent.md
│   ├── 内容调研Agent.md
│   ├── 内容生产Agent.md
│   ├── 审稿Agent.md
│   ├── 发布准备Agent.md
│   └── 选题库更新Agent.md
└── 内容草稿/                ← 每篇文章的素材和发布文件
```

---

## 常见问题

**Q：发布超时怎么办？**
确认 Chrome 里 XHS Bridge 扩展已启用，然后重试。

**Q：search-feeds 加 --sort-by 报错？**
不要加这个参数，用默认排序。

**Q：标签只挂上了1个？**
把 `#话题` 直接写在 content.txt 正文末尾，不要用 `--tags` 参数。

**Q：Windows 下 Python 命令报错？**
用 `python` 替换 `python3`，或确认 Python 已加入环境变量。
