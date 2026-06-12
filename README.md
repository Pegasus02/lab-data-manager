# lab-data-manager

A structured, AI-friendly data management system for experimental research projects. Works with any LLM agent (Qoder, Qoderwork, Claude, GPT, Gemini, Cursor, etc.) — the instructions are plain Markdown that any agent can follow.

一个结构化的、AI 友好的实验科研数据管理系统。兼容任意 LLM agent（Qoder、Qoderwork、Claude、GPT、Gemini、Cursor 等）——指令为纯 Markdown，任何 agent 均可遵循。

---

## What this does / 功能简介

An AI agent reads `INSTRUCTIONS.md` and guides you through creating a complete research project folder from scratch, including:

AI agent 读取 `INSTRUCTIONS.md`，引导你从零创建一个完整的科研项目文件夹，包括：

- Clear separation of **raw vs. processed data** (raw data is never modified) / **原始数据与处理数据**分离（原始数据永不修改）
- **CSV indexes** for experiments, samples, and devices — fast lookup across batches / 实验、样品、器件的 **CSV 索引**——跨批次快速查找
- **Daily experiment logs** with a standard template / 标准模板的**每日实验记录**
- **4-layer per-batch structure**: `Raw_Data/` → `Processed_Data/` → `Figures/` → `Analysis/` / **每批次4层结构**：原始→处理→图表→分析
- An `AGENTS.md` file so any future AI agent can operate the project correctly / `AGENTS.md` 文件，确保任何未来的 AI agent 都能正确操作项目
- Adapted to **any experimental science**: nanofabrication, chemistry, biology, materials science, physics, etc. / 适用于**任何实验科学**：纳米加工、化学、生物、材料科学、物理等

---

## Quick start / 快速开始

### With Qoder / Qoderwork / 使用 Qoder / Qoderwork

Install the ready-made skill from `skills/research-project-setup/SKILL.md`:

从 `skills/research-project-setup/SKILL.md` 安装现成的 skill：

1. Clone or download this repository. / 克隆或下载本仓库。
2. Place `SKILL.md` into one of: / 将 `SKILL.md` 放入以下位置之一：
   - **Project-level / 项目级:** `<project-root>/.qoder/skills/research-project-setup/SKILL.md`
   - **User-level / 用户级:** `~/.qoder/skills/research-project-setup/SKILL.md`
3. Restart or reload Qoder. / 重启或重新加载 Qoder。
4. Say: `"Set up a research project"` — the skill triggers automatically. / 说：`"搭建科研项目"` ——skill 自动触发。

### With Claude Code / 使用 Claude Code

1. Place `skills/research-project-setup/SKILL.md` into `.claude/skills/research-project-setup.md` in your project. / 将文件放入项目的 `.claude/skills/` 目录。
2. Say: `"Set up a research project"` — the skill triggers automatically. / 说：`"搭建科研项目"` ——自动触发。

### With Claude Cowork / 使用 Claude Cowork

1. Settings → Capabilities → Install skill → select `skills/research-project-setup/SKILL.md`. / 设置 → 能力 → 安装 skill → 选择文件。
2. Say: `"Set up a research project"`. / 说：`"搭建科研项目"`。

### With any LLM agent / 使用任意 LLM agent

Paste the contents of [`INSTRUCTIONS.md`](./INSTRUCTIONS.md) into your agent's context (or upload the file), then say:

将 [`INSTRUCTIONS.md`](./INSTRUCTIONS.md) 的内容粘贴到 agent 的上下文中（或上传文件），然后说：

> "Follow these instructions to set up a research project for me."
> "按照这些指令帮我搭建一个科研项目。"

---

## Project structure created / 创建的项目结构

```
YourProject/
├── AGENTS.md                    # AI operation rules / AI 操作规范
├── .agent-context.md            # Quick-reference for any AI agent / 任意 AI agent 的快速参考
├── README.md                    # Project overview / 项目概览
├── DataManagement_Overview.md   # Data system entry point / 数据系统入口
├── Experimental_Data/
│   ├── 00_Index/                # Experiment / Sample / Device CSV indexes / 实验/样品/器件索引
│   ├── 01_<Scheme1>/            # First research scheme / 第一研究方案
│   ├── 02_<Scheme2>/            # Second scheme, if applicable / 第二方案（如有）
│   ├── 03_Characterization/     # Microscopy, spectroscopy, etc. / 显微、光谱等
│   ├── 04_Device_Testing/       # Cross-batch summaries / 跨批次汇总
│   ├── 05_Process_Development/  # Process optimization / 工艺优化
│   └── 06_Daily_Experiment_Log/ # Per-month experiment logs / 按月实验记录
├── Scripts/Data_Analysis/       # Plotting and analysis templates / 绘图和分析模板
├── Progress/                    # Progress reports / 进展报告
└── PPT/                         # Presentation materials / 演示材料
```

Directory numbers adapt to your project — only categories you actually need get created.

目录编号根据项目自适应——只创建你实际需要的类别。

---

## Core principles / 核心原则

1. **Raw data is immutable.** Files in `Raw_Data/` are never modified, renamed, or deleted. / **原始数据不可变。** `Raw_Data/` 中的文件永远不会被修改、重命名或删除。
2. **One primary location per batch.** No duplicating raw data across directories. / **每批次唯一主目录。** 不在多个目录中重复原始数据。
3. **ASCII filenames only.** No Unicode or special characters in file/directory names. / **仅 ASCII 文件名。** 文件/目录名中不使用 Unicode 或特殊字符。
4. **Traceability.** Every processed result links back to its source `Raw_Data/` path. / **可追溯。** 每个处理结果都可追溯到源 `Raw_Data/` 路径。
5. **Log failures with root cause.** "Failed, will retry" is not acceptable documentation. / **记录失败根因。** "失败了下次重试"不是合格的记录。
6. **Use relative paths** throughout — supports cross-device sync (Synology, OneDrive, etc.). / **全文使用相对路径**——支持跨设备同步（群晖、OneDrive 等）。

---

## Templates / 模板

The `templates/` directory contains ready-to-use Markdown and CSV templates that the agent fills in with your project details:

`templates/` 目录包含现成的 Markdown 和 CSV 模板，agent 会根据你的项目信息填充：

| File / 文件 | Purpose / 用途 |
|------|---------|
| `AGENTS_md_template.md` | AI agent operation rules / AI agent 操作规范 |
| `ExperimentLog_Template.md` | Daily experiment log / 每日实验记录 |
| `Experiment_Index_header.csv` | Experiment index CSV header / 实验索引 CSV 表头 |
| `Sample_Index_header.csv` | Sample tracking CSV header / 样品追踪 CSV 表头 |
| `Device_Index_header.csv` | Device tracking CSV header / 器件追踪 CSV 表头 |

---

## Examples / 示例

See `examples/gaafet_project_structure.md` for a real-world example: a 3D-stacked GAAFET semiconductor research project with two fabrication schemes, multiple characterization methods, and device electrical testing.

参见 `examples/gaafet_project_structure.md` 了解实际案例：一个三维堆叠 GAAFET 半导体研究项目，包含两种制备方案、多种表征方法和器件电学测试。

---

## Repository structure / 仓库结构

```
lab-data-manager/
├── INSTRUCTIONS.md              # Full agent instructions / 完整 agent 指令
├── skills/
│   └── research-project-setup/
│       └── SKILL.md             # Installable skill / 可安装的 skill
├── templates/                   # Reusable file templates / 可复用文件模板
├── examples/                    # Real-world project examples / 实际项目示例
└── integrations/
    └── claude/
        └── SKILL.md             # Legacy redirect / 旧路径重定向
```

---

## License / 许可证

MIT
