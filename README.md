# xhs-ai-workflow · 小红书 AI 运营工作流

> 一套即插即用的小红书账号 AI 运营系统。配置好账号信息，对 AI 说一句「小红书开工」，它会自动走完选题 → 调研 → 写稿 → 审稿 → 发布的完整流程。

**不需要写代码。** 所有逻辑都写在 Markdown 文件里，任何能读取 Prompt 的 AI Agent 均可驱动。

---

## 兼容的 AI Agent

| Agent | 支持方式 |
|-------|---------|
| **Claude Code** | 推荐，原生支持，零配置 |
| **Cursor / Windsurf / Cline** | 将 `主控Prompt.md` 作为 System Prompt 或 Rules 载入 |
| **ChatGPT / Copilot** | 手动粘贴 `主控Prompt.md` 内容开启对话 |
| **任意支持长上下文的 LLM** | 直接读取项目文件夹中的 Prompt 文件 |

---

## 它能做什么

- **一句话启动全流程**：说「开始发文」，AI 自动走完选题到发布
- **智能选题**：从选题库匹配 + 搜小红书热度 + 去重检查
- **内容调研**：自动搜索同类高赞内容，生成参考报告
- **协作写稿**：你提供核心观点，AI 按你的风格起草打磨
- **自动发布**：生成图片卡片，配标签，一键发布小红书
- **选题库维护**：每周扫描热门内容，提取新选题候选

---

## 快速开始

### 第一步：克隆项目

```bash
git clone https://github.com/554022647henry-boop/xhs-ai-workflow.git
```

或点击页面右上角 **Code → Download ZIP** 解压。

### 第二步：配置你的账号（3 个文件，改完即用）

| 文件 | 填写内容 |
|------|---------|
| `项目主旨.md` | 账号定位、目标人群、你的人设背景 |
| `写作风格指南.md` | 你的写作风格偏好 |
| `选题库.md` | 你的账号方向和初始选题 |

### 第三步：安装依赖（用于自动发布）

```bash
pip install markdown pyyaml playwright
playwright install chromium
```

自动发布依赖小红书 MCP 工具，安装方式见下方「自动发布配置」。如果只需要辅助写稿，跳过这步即可。

### 第四步：启动

**Claude Code 用户：** 将以下内容追加到 `~/.claude/CLAUDE.md`：

```markdown
# 小红书运营项目

如果用户提到：小红书、发文、选题、内容发布、开工，立即执行：
1. 读取 {项目完整路径}/主控Prompt.md
2. 按照主控 Prompt 的任务路由判断用户意图
3. 开始执行对应流程

项目根目录：{项目完整路径}
```

然后在任意目录打开 Claude Code，说：

```
小红书开工
```

**其他 AI Agent 用户：** 把 `主控Prompt.md` 的内容粘贴为 System Prompt，再把其他 `.md` 文件内容一并提供给 AI 作为上下文，即可开始对话。

---

## 自动发布配置

自动发布需要以下两个工具（可选，不影响写稿功能）：

```bash
# 小红书自动化（登录 / 搜索 / 发布）
npx skills add xpzouying/xiaohongshu-skills

# 图片卡片生成
npx skills add comeonzhj/Auto-Redbook-Skills
```

安装完成后扫码登录小红书：

```bash
python scripts/cli.py login
```

---

## 项目结构

```
xhs-ai-workflow/
├── README.md                ← 本文件
├── 主控Prompt.md            ← 主控 Agent，所有流程的统一入口（从这里开始）
├── 项目主旨.md              ← 【需修改】账号定位和人设
├── 项目进程.md              ← 项目进度记录（AI 自动维护）
├── 内容发布进程.md          ← 每篇文章状态（AI 自动维护）
├── 选题库.md                ← 【需修改】待发选题列表
├── 写作风格指南.md          ← 【需修改】你的写作风格
├── agents/
│   ├── 初始化Agent.md
│   ├── 选题Agent.md
│   ├── 内容调研Agent.md
│   ├── 内容生产Agent.md
│   ├── 审稿Agent.md
│   ├── 发布准备Agent.md
│   └── 选题库更新Agent.md
└── 内容草稿/                ← 每篇文章的素材和发布文件（AI 自动生成）
```

---

## 常见问题

**Q：发布超时怎么办？**
确认 Chrome 里 XHS Bridge 扩展已启用，然后重试。

**Q：标签只挂上了 1 个？**
把 `#话题` 直接写在 content.txt 正文末尾，不要用 `--tags` 参数。

**Q：Windows 下 Python 命令报错？**
用完整路径调用 Python，例如 `C:\Python312\python.exe`，或确认 Python 已加入环境变量。

**Q：不用自动发布，只想用 AI 辅助写稿可以吗？**
完全可以。跳过「自动发布配置」章节，只需完成第二步配置文件，把 `主控Prompt.md` 提供给任意 AI 即可开始写稿。

---

## License

MIT
