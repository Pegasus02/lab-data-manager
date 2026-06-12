# AGENTS.md

> **This file gives AI agents operating instructions for this project.**  
> Read it before any task. It takes precedence over all other documentation.
>
> **本文件为 AI agent 提供项目操作规范。**  
> 每次任务前必须阅读。本文件优先于所有其他文档。

---

## ⚠️ Mandatory first step — do this before ANY task / 强制前置步骤——任何任务前必须执行

Before writing, moving, or creating any file, you MUST complete this sequence:

在写入、移动或创建任何文件之前，你必须完成以下序列：

1. **Scan the directory tree** — read the full output of `tree` or `ls -R` from the project root to understand what exists.  
   **扫描目录树** — 读取项目根目录的 `tree` 或 `ls -R` 完整输出，了解现有结构。
2. **Read the mandatory reading list** below (all three files, every time, no exceptions).  
   **阅读下方必读清单**（三个文件，每次任务都要读，无例外）。
3. **Read the task-specific files** from the routing table below.  
   **阅读任务路由表**中对应的文件。
4. **Read the nearest README.md** to the directory you will be working in.  
   **阅读目标工作目录最近的 README.md**。
5. Only after completing steps 1–4, begin the actual task.  
   完成步骤 1–4 后，才能开始实际任务。

If you skip any step, the task output will be incorrect. There is no shortcut.  
跳过任何一步都会导致输出不正确。没有捷径。

---

## Mandatory reading (always, for every task) / 必读文件（每次任务，无条件）

Read these three files in order before doing anything:  
在做任何事之前，按顺序阅读以下三个文件：

1. `DataManagement_Overview.md` — system-wide overview and principles / 系统总览与原则
2. `Experimental_Data/README.md` — authoritative data structure reference / 数据结构的权威参考
3. `Experimental_Data/00_Index/README.md` — index file conventions / 索引文件规范

---

## Task routing table / 任务路由表

After the mandatory reading, read additional files based on the task type:  
完成必读后，根据任务类型阅读额外文件：

| Task type / 任务类型 | Also read / 额外必读 |
|-----------|-----------|
| Writing an experiment log / 撰写实验记录 | `Experimental_Data/06_Daily_Experiment_Log/README.md` + `ExperimentLog_Template.md` |
| Adding or organizing data / 添加或整理数据 | Target scheme `README.md` / 目标方案的 `README.md` + `Experiment_Index.csv` |
| Data processing or analysis / 数据处理或分析 | `Analysis/<Batch_ID>/README.md` + raw data path listed there / 其中列出的原始数据路径 |
| Updating indexes / 更新索引 | `Experimental_Data/00_Index/README.md` + target CSV / 目标 CSV 文件 |
| Cross-batch comparison / 跨批次比较 | `Device_Index.csv` (if present / 如有) + all relevant batch `README.md` / 所有相关批次的 `README.md` |
| Script creation or modification / 创建或修改脚本 | `Scripts/README.md` + existing scripts in `Scripts/Data_Analysis/` / 已有脚本 |
| Documentation update / 文档更新 | The document being modified / 被修改的文档 + `AGENTS.md` |

If you are unsure which category a task falls into, read all files listed above. It is better to over-read than to miss context.  
不确定任务属于哪个类别时，阅读以上所有文件。多读好过漏读。

---

## Project positioning / 项目定位

<!-- One paragraph: what this project is and what it contains. / 一段话：项目是什么，包含什么内容。 -->

---

## Directory responsibilities / 目录职责

<!-- One paragraph per top-level folder. / 每个顶级目录一段说明。 -->

---

## Data type → directory mapping / 数据类型 → 目录映射

| Data type / 数据类型 | Primary directory / 主目录 |
|-----------|------------------|
| <!-- fill in / 填写 --> | `Experimental_Data/01_...` |

---

## Work principles / 工作原则

1. **Read before write.** Never create, modify, or move a file without first reading the existing content of related files. Check for duplicates, existing batches, and naming conventions already in use.  
   **先读后写。** 创建、修改或移动任何文件之前，必须先阅读相关文件已有内容。检查是否有重复、已有批次和正在使用的命名规范。

2. Do not delete, overwrite, or rename raw data without explicit user instruction.  
   未经用户明确指示，不得删除、覆盖或重命名原始数据。

3. Do not copy the same raw data batch to multiple directories as "primary copies".  
   不得将同一批次原始数据复制到多个目录作为"主副本"。

4. When adding data, establish a clear batch directory and index entry — do not create empty folders speculatively.  
   添加数据时，建立清晰的批次目录和索引条目——不要猜测性地创建空文件夹。

5. All processed results must trace back to a `Raw_Data/` path; document this in `Analysis/<Batch_ID>/README.md`.  
   所有处理结果必须可追溯到 `Raw_Data/` 路径；记录在 `Analysis/<Batch_ID>/README.md` 中。

6. Record failed experiments with root cause — "failed, will retry" is not acceptable.  
   记录失败实验时必须包含根因分析——"失败了，下次重试"不可接受。

7. **Keep documentation current.** When you notice that any README, index, or AGENTS.md is out of sync with the actual directory structure, update it as part of the current task. Do not leave stale documentation for the next agent.  
   **保持文档最新。** 发现任何 README、索引或 AGENTS.md 与实际目录结构不同步时，在当前任务中一并更新。不要把过时的文档留给下一个 agent。

8. When actual directories conflict with older documentation, this file and `Experimental_Data/README.md` take precedence — but after resolving the conflict, update the outdated document to match reality.  
   当实际目录与旧文档冲突时，以本文件和 `Experimental_Data/README.md` 为准——但解决冲突后，要更新过时的文档使其与实际一致。

---

## Raw / Processed / Figures / Analysis rules / 原始/处理/图表/分析 规则

| Directory / 目录 | Contents / 内容 |
|-----------|----------|
| `Raw_Data/` | Instrument exports only: CSV/DAT/TXT, unmodified images / 仅仪器导出：CSV/DAT/TXT、未修改的图片 |
| `Processed_Data/` | Denoised, reconstructed, fitted, corrected, unit-converted files / 降噪、重建、拟合、校正、单位转换文件 |
| `Figures/` | Report figures, paper figures, comparison and diagnostic plots / 报告图、论文图、对比和诊断图 |
| `Analysis/` | Scripts (.py, .ipynb), parameter tables, fit logs, analysis README / 脚本、参数表、拟合日志、分析 README |

**Do not place these in `Raw_Data/`:** files containing `denoised`, `reconstructed`, `fit`, `fitted`, `corrected`, `processed`, `SS`, `onoff` in the name.  
**以下文件不得放入 `Raw_Data/`：** 文件名中包含 `denoised`、`reconstructed`、`fit`、`fitted`、`corrected`、`processed`、`SS`、`onoff` 的文件。

---

## How to add a new experiment log / 如何添加新实验记录

1. **Read** `Experimental_Data/06_Daily_Experiment_Log/README.md` and the template.  
   **阅读**日志 README 和模板。
2. **Read** `Experiment_Index.csv` to check the latest Experiment_ID and avoid duplicates.  
   **阅读** `Experiment_Index.csv`，检查最新的 Experiment_ID，避免重复。
3. Determine the experiment date. / 确定实验日期。
4. Confirm the monthly directory exists: `06_Daily_Experiment_Log/YYYY-MM/`  
   确认月度目录存在。
5. Copy the template; name it: `YYYYMMDD_ExperimentShortName_Exp<N>.md`  
   复制模板并命名。
6. Fill in all required fields. / 填写所有必填字段。
7. Add a row to `00_Index/Experiment_Index.csv`. / 在索引中添加一行。
8. If new samples are involved, update `Sample_Index.csv`. / 如涉及新样品，更新样品索引。
9. If device-level tracking applies, update `Device_Index.csv`. / 如需器件级追踪，更新器件索引。

---

## How to add a new data batch / 如何添加新数据批次

1. **Read** the `README.md` in the target scheme directory to see existing batches.  
   **阅读**目标方案目录的 `README.md`，查看已有批次。
2. **Read** `Experiment_Index.csv` to check for existing Batch_IDs.  
   **阅读** `Experiment_Index.csv`，检查已有 Batch_ID。
3. Determine the primary directory from the data-type mapping table above.  
   根据上方数据类型映射表确定主目录。
4. Choose a Batch ID: `YYYYMMDD_Process_LayerOrPurpose` / 选择批次 ID。
5. Create `Raw_Data/<Batch_ID>/` and place instrument exports there.  
   创建 `Raw_Data/<Batch_ID>/` 并放入仪器导出文件。
6. Create `Processed_Data/<Batch_ID>/`, `Figures/<Batch_ID>/`, `Analysis/<Batch_ID>/` as needed.  
   按需创建处理数据、图表、分析目录。
7. Add a row to `Experiment_Index.csv`. / 在实验索引中添加一行。

---

## How to record data processing / 如何记录数据处理

1. **Read** `Analysis/<Batch_ID>/README.md` (if it exists) to understand prior processing.  
   **阅读** `Analysis/<Batch_ID>/README.md`（如存在），了解之前的处理。
2. Locate the primary data directory and raw data path. / 定位主数据目录和原始数据路径。
3. Do not modify any file in `Raw_Data/`. / 不得修改 `Raw_Data/` 中的任何文件。
4. Write processed data to `Processed_Data/<Batch_ID>/`. / 将处理数据写入处理目录。
5. Write figures to `Figures/<Batch_ID>/`. / 将图表写入图表目录。
6. Write scripts and parameter tables to `Analysis/<Batch_ID>/`. / 将脚本和参数表写入分析目录。
7. Write or update `Analysis/<Batch_ID>/README.md`: processing date, method, input paths, output paths, key parameters, excluded data, conclusions, open questions.  
   写入或更新分析 README：处理日期、方法、输入路径、输出路径、关键参数、排除的数据、结论、待解决问题。
8. Update `Experiment_Index.csv` or `Device_Index.csv` with result summaries. / 用结果摘要更新索引。

---

## How to update documentation / 如何更新文档

When you discover that documentation is out of date:  
发现文档过时的时候：

1. **Identify the discrepancy** — what does the document say vs. what actually exists.  
   **识别差异** — 文档描述 vs. 实际状态。
2. **Update the document** to match the current state of the project.  
   **更新文档**使其与项目当前状态一致。
3. **If the conflict involves AGENTS.md or Experimental_Data/README.md**, those files are authoritative — update the other document to match them.  
   **如果冲突涉及 AGENTS.md 或 Experimental_Data/README.md**，以这些文件为准——更新其他文档使其一致。
4. **If AGENTS.md itself is outdated** (e.g., a new directory category was added but not listed in the data-type mapping), update AGENTS.md with the user's confirmation.  
   **如果 AGENTS.md 本身过时**（如新增了目录类别但未在映射表中列出），经用户确认后更新 AGENTS.md。
5. After updating, briefly note what changed and why (in the conversation, not in the file).  
   更新后，简要说明改了什么以及原因（在对话中说明，不写入文件）。

Documents that should be kept current: / 应保持最新的文档：
- `AGENTS.md` — this file (data-type mapping, directory list) / 本文件（数据类型映射、目录列表）
- `Experimental_Data/README.md` — directory tree, batch table / 目录树、批次表
- `DataManagement_Overview.md` — top-level tree, registered batches / 顶层目录树、已登记批次
- `00_Index/*.csv` — experiment/sample/device indexes / 实验/样品/器件索引
- Each scheme's `README.md` — active batch table / 各方案的 `README.md`——活跃批次表

---

## Index field conventions / 索引字段规范

### Experiment_Index.csv

| Field / 字段 | Description / 说明 |
|-------|-------------|
| `Date` | YYYY-MM-DD |
| `Experiment_ID` | Unique ID, e.g. `Exp-<Topic>-<N>` / 唯一 ID |
| `Batch_ID` | Follows `YYYYMMDD_Process_LayerOrPurpose` / 遵循命名规范 |
| `Scheme` | Which research scheme / 所属研究方案 |
| `Experiment_Type` | Fabrication / Characterization / Device Testing / Process Development / 制备 / 表征 / 器件测试 / 工艺开发 |
| `Main_Data_Dir` | Relative path to primary data directory / 主数据目录的相对路径 |
| `Daily_Log` | Relative path to experiment log file / 实验记录文件的相对路径 |
| `Status` | Planned / Running / Completed / Failed / Completed with issues / Needs review / 计划中 / 进行中 / 已完成 / 失败 / 有问题地完成 / 待审查 |
| `Key_Result` | One-sentence finding / 一句话结论 |
| `Next_Action` | One-sentence next step / 一句话下一步 |

---

## Writing style for logs / 实验记录写作风格

Write facts first, then judgments.  
先写事实，再写判断。

**Good / 好的写法：**
> Observation / 观察: No suspended bridge structure seen under SEM; local areas show possible resist residue.  
> Judgment / 判断: Release failed. Uncertainties: ALD film quality and capillary collapse during wet release.  
> Next step / 下一步: Switch to stable ALD tool; use SiNx hard mask; evaluate supercritical drying.

**Not acceptable / 不可接受：** "Experiment failed, will retry." / "实验失败，下次重试。"

---

## When uncertain / 遇到不确定时

1. Do not move raw data speculatively. / 不要猜测性地移动原始数据。
2. Document the uncertainty in a README or `Analysis/<Batch_ID>/README.md`.  
   在 README 或分析 README 中记录不确定事项。
3. Set `Status` to `Needs review` in the index. / 在索引中将状态设为 `Needs review` / `待审查`。
4. Ask the user before any irreversible reorganization. / 进行任何不可逆的重组之前，先询问用户。

---

## Prohibited actions (unless user explicitly instructs) / 禁止行为（除非用户明确指示）

- Delete raw data files / 删除原始数据文件
- Overwrite raw CSV or instrument export files / 覆盖原始 CSV 或仪器导出文件
- Batch-rename raw data files / 批量重命名原始数据文件
- Place processed files in `Raw_Data/` / 将处理后的文件放入 `Raw_Data/`
- Copy raw data to multiple primary directories / 将原始数据复制到多个主目录
- Create large numbers of empty directories "for completeness" / 为了"完整性"创建大量空目录
- Edit files in `Docs/` to make them appear to be current rules / 编辑 `Docs/` 中的文件使其看起来像当前规则
- **Create or modify files without first reading related existing files** / **未阅读相关现有文件就创建或修改文件**

---

## Pre-task checklist / 任务前检查清单

Before completing any task, verify:  
完成任何任务前，验证：

- [ ] Completed the mandatory first step (scanned tree + read required files) / 完成强制前置步骤（扫描目录树 + 阅读必读文件）
- [ ] Read task-specific files from the routing table / 阅读了任务路由表中的对应文件
- [ ] No raw data was modified / 未修改原始数据
- [ ] New experiments have a log entry / 新实验有记录条目
- [ ] New batches have an index entry / 新批次有索引条目
- [ ] Processed results are separated from raw data / 处理结果与原始数据分离
- [ ] Figures in `Figures/`, scripts in `Analysis/` / 图表在 `Figures/`，脚本在 `Analysis/`
- [ ] Relative paths used throughout / 全文使用相对路径
- [ ] Failures and anomalies documented, not omitted / 失败和异常已记录，未遗漏
- [ ] Documentation updated if any discrepancies were found / 发现差异时已更新文档
