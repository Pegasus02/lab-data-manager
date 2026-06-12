# lab-data-manager

**[English](README.md)**

一个结构化的、AI 友好的实验科研数据管理系统。兼容任意 LLM agent（Qoder、Qoderwork、Claude、GPT、Gemini、Cursor 等）——指令为纯 Markdown，任何 agent 均可遵循。

## 功能简介

AI agent 读取 `INSTRUCTIONS.md`，引导你从零创建一个完整的科研项目文件夹，包括：

- **原始数据与处理数据**分离（原始数据永不修改）
- 实验、样品、器件的 **CSV 索引**——跨批次快速查找
- 标准模板的**每日实验记录**
- **每批次4层结构**：`Raw_Data/` → `Processed_Data/` → `Figures/` → `Analysis/`
- `AGENTS.md` 文件，确保任何未来的 AI agent 都能正确操作项目
- 适用于**任何实验科学**：纳米加工、化学、生物、材料科学、物理等

## 快速开始

### 使用 Qoder / Qoderwork

从 `skills/research-project-setup/SKILL.md` 安装现成的 skill：

1. 克隆或下载本仓库。
2. 将 `SKILL.md` 放入以下位置之一：
   - **项目级：** `<project-root>/.qoder/skills/research-project-setup/SKILL.md`
   - **用户级：** `~/.qoder/skills/research-project-setup/SKILL.md`
3. 重启或重新加载 Qoder。
4. 说：`"搭建科研项目"` ——skill 自动触发。

### 使用 Claude Code

1. 将 `skills/research-project-setup/SKILL.md` 放入项目的 `.claude/skills/research-project-setup.md`。
2. 说：`"搭建科研项目"` ——自动触发。

### 使用 Claude Cowork

1. 设置 → 能力 → 安装 skill → 选择 `skills/research-project-setup/SKILL.md`。
2. 说：`"搭建科研项目"`。

### 使用任意 LLM agent

将 [`INSTRUCTIONS.md`](./INSTRUCTIONS.md) 的内容粘贴到 agent 的上下文中（或上传文件），然后说：

> "按照这些指令帮我搭建一个科研项目。"

Agent 会询问你的项目信息，设计文件夹结构，与你确认后创建所有文件。

## 创建的项目结构

```
YourProject/
├── AGENTS.md                    # AI 操作规范
├── .agent-context.md            # 任意 AI agent 的快速参考
├── README.md                    # 项目概览
├── DataManagement_Overview.md   # 数据系统入口
├── Experimental_Data/
│   ├── 00_Index/                # 实验/样品/器件 CSV 索引
│   ├── 01_<Scheme1>/            # 第一研究方案（原始→处理→图表→分析）
│   ├── 02_<Scheme2>/            # 第二方案（如有）
│   ├── 03_Characterization/     # 显微、光谱、衍射等
│   ├── 04_Device_Testing/       # 跨批次汇总（不存放原始数据副本）
│   ├── 05_Process_Development/  # 工艺优化实验
│   └── 06_Daily_Experiment_Log/ # 按月实验记录
├── Scripts/Data_Analysis/       # 绘图和分析脚本模板
├── Progress/                    # 进展报告
└── PPT/                         # 演示材料
```

目录编号根据项目自适应——只创建你实际需要的类别。

## 核心原则

1. **原始数据不可变。** `Raw_Data/` 中的文件永远不会被修改、重命名或删除。
2. **每批次唯一主目录。** 不在多个目录中重复原始数据。
3. **仅 ASCII 文件名。** 文件/目录名中不使用 Unicode 或特殊字符。
4. **可追溯。** 每个处理结果都可追溯到源 `Raw_Data/` 路径。
5. **记录失败根因。** "失败了下次重试"不是合格的记录。
6. **全文使用相对路径**——支持跨设备同步（群晖、OneDrive 等）。

## 模板

`templates/` 目录包含现成的 Markdown 和 CSV 模板，agent 会根据你的项目信息填充：

| 文件 | 用途 |
|------|------|
| `AGENTS_md_template.md` | AI agent 操作规范 |
| `ExperimentLog_Template.md` | 每日实验记录 |
| `Experiment_Index_header.csv` | 实验索引 CSV 表头 |
| `Sample_Index_header.csv` | 样品追踪 CSV 表头 |
| `Device_Index_header.csv` | 器件追踪 CSV 表头 |

## 示例

参见 `examples/gaafet_project_structure.md` 了解实际案例：一个三维堆叠 GAAFET 半导体研究项目，包含两种制备方案、多种表征方法和器件电学测试。

## 仓库结构

```
lab-data-manager/
├── INSTRUCTIONS.md              # 完整 agent 指令（平台无关）
├── skills/
│   └── research-project-setup/
│       └── SKILL.md             # 可安装的 skill
├── templates/                   # 可复用文件模板
├── examples/                    # 实际项目示例
└── integrations/
    └── claude/
        └── SKILL.md             # 旧路径重定向
```

## 许可证

MIT
