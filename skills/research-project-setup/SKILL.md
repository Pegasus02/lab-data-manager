---
name: research-project-setup
description: "Set up a structured experimental research project management system for scientific research. Use this skill whenever a researcher wants to organize their lab data, create a new research project folder structure, set up experiment tracking and indexing, or establish a reproducible data management workflow. Works for any experimental science: nanofabrication, chemistry, biology, materials science, physics, and beyond. Trigger on phrases like 'set up a research project', 'organize my experiment data', 'create a lab data management system', 'help me structure my research files', '建立课题管理结构', '整理实验数据文件夹', '搭建科研项目', '实验数据管理', or whenever someone is starting a new research project and needs a systematic way to track experiments, samples, and data batches. Also use this skill if the user shares an existing research project structure and asks to replicate or adapt it."
---

# Research Project Setup Skill

This skill guides an AI agent through creating a structured, AI-friendly experimental research data management system. It works across AI coding assistants (Qoder, Qoderwork, Claude Code, Cowork, Cursor, etc.) — the instructions are platform-agnostic.

The final result is a self-contained project folder with:
- Clear separation of raw vs. processed data
- Structured CSV indexes for fast lookup
- Daily experiment logs with a standard template
- Per-batch 4-layer directory layout (Raw → Processed → Figures → Analysis)
- An `AGENTS.md` so any future AI agent can operate the project correctly

> **Templates available:** This repository ships ready-to-use templates in `templates/`. When generating files, prefer reading these templates first and adapting them rather than writing from scratch. Template files:
> - `templates/AGENTS_md_template.md` — AGENTS.md skeleton
> - `templates/ExperimentLog_Template.md` — daily log template
> - `templates/Experiment_Index_header.csv` — experiment index header
> - `templates/Sample_Index_header.csv` — sample index header
> - `templates/Device_Index_header.csv` — device index header

---

## Phase 1: Interview the user

Before creating anything, gather the following information. Ask everything in one message — don't drip-feed questions one at a time. Group related questions naturally.

**Tool usage:** Use the `AskUserQuestion` tool for structured choices (multi-select, options) and open-ended prose for free-form answers. If `AskUserQuestion` is not available in the current environment, ask in plain text.

### Questions to ask

**1. Project identity**
- What is the name of this research project? (This becomes the root folder name — ASCII characters, no spaces, use underscores or hyphens.)
- One sentence: what is the research goal?

**2. Research schemes / parallel approaches**
- Does the project have multiple parallel research routes or schemes? (e.g., "Route A uses method X, Route B uses method Y")
- For each scheme: a short English name and a brief description.
- If the user has only one scheme or is unsure, create a single `01_Main_Scheme/` and note it can be expanded.

**3. Experiment categories**
Which of the following experiment types apply? (Select all that apply)
- Fabrication / synthesis (making samples)
- Materials characterization (microscopy, spectroscopy, diffraction, etc.)
- Device / functional testing (electrical, optical, mechanical measurements)
- Process development (optimizing a single process step)
- Cross-batch comparison / meta-analysis

Also ask: any domain-specific categories not listed above?

**4. Raw data file types**
What kinds of files does their equipment export? (CSV, DAT, TXT, TIFF, HDF5, proprietary formats, etc.) This helps set expectations in documentation.

**5. Team**
- Who are the main operators / researchers? (used in log template)
- Solo or team project?

**6. AI agent usage**
Will they use AI agents to help manage this project? If yes, generate a detailed `AGENTS.md`. If no, create a lightweight version or skip it.

**7. Output location**
Where should the project folder be created? Default to the current workspace directory. Ask only if they might want it elsewhere.

**8. Language preference**
English, Chinese, or mixed documentation? (Filenames are always ASCII regardless of choice.)

---

## Phase 2: Design the directory structure

Based on the user's answers, design the structure before creating files. Show it to the user as a tree and ask for confirmation or changes before proceeding.

### Top-level layout

```
<ProjectName>/
├── AGENTS.md                        # AI agent operation rules (primary)
├── .agent-context.md                # Condensed quick-reference for any AI agent
├── README.md                        # Project overview and research goals
├── DataManagement_Overview.md       # Data system entry point
├── Experimental_Data/               # All experimental data and logs
├── Scripts/                         # Data analysis and plotting scripts
├── Progress/                        # Progress reports
└── PPT/                             # Presentation materials
```

> **Note on `.agent-context.md`:** This replaces the older `CLAUDE.md` convention. It serves the same purpose — a condensed reference that points to `AGENTS.md` — but uses a platform-neutral name so Qoder, Qoderwork, Claude, Cursor, and other tools all benefit equally.

### Experimental_Data layout

Always start with `00_Index` and end daily logs at the last numbered directory. Number intermediate directories by category starting from `01`. Adapt names to the user's domain. Renumber cleanly — no gaps.

Example for 2 schemes + characterization + device testing + process development:

```
Experimental_Data/
├── README.md
├── 00_Index/
│   ├── README.md
│   ├── Experiment_Index.csv
│   ├── Sample_Index.csv
│   └── Device_Index.csv              # Only if device/functional testing selected
├── 01_<Scheme1_ShortName>/
├── 02_<Scheme2_ShortName>/           # If second scheme exists
├── 03_Material_Characterization/     # If characterization selected
├── 04_Device_Testing/                # If device testing selected (summaries only, no raw data copies)
├── 05_Process_Development/           # If process development selected
├── 06_Daily_Experiment_Log/
│   ├── README.md
│   └── ExperimentLog_Template.md
└── Docs/                             # Historical documents, archived reports
```

If the user has 1 scheme + characterization only, the result is `01_Main_Scheme/`, `02_Characterization/`, `03_Daily_Experiment_Log/`.

### Per-batch structure

Each experiment batch lives in exactly one primary location. Create an example batch inside the first scheme directory.

```
<scheme-dir>/<process-or-topic-node>/
├── README.md
├── Raw_Data/<Batch_ID>/             # Instrument exports only, never modified
├── Processed_Data/<Batch_ID>/       # Denoised, reconstructed, unit-converted
├── Figures/<Batch_ID>/              # Report and paper figures
└── Analysis/<Batch_ID>/             # Scripts, notebooks, fit logs, parameter tables
```

### Scripts layout

```
Scripts/
├── README.md
└── Data_Analysis/
    └── templates/
        └── plot_template.py          # Minimal matplotlib template
```

---

## Phase 3: Generate file contents

Create each file below with content tailored to the user's project. Keep all filenames ASCII.

**Language policy:**
- **README.md files** (project-level and all sub-levels): Write in **bilingual format** (English first, then Chinese). This ensures both English-speaking and Chinese-speaking team members can navigate the project.
- **All other files** (AGENTS.md, .agent-context.md, DataManagement_Overview.md, experiment logs, CSV indexes, scripts, Analysis READMEs): Write in **English only**.

**Important:** Before writing each file, check if a corresponding template exists in the repository's `templates/` directory. If it does, read the template first and use it as the structural skeleton — fill in project-specific details on top of it rather than rewriting from scratch.

---

### README.md (top level) — **BILINGUAL (English + Chinese)**

Include (in both English and Chinese):
- Project title and one-sentence research goal / 项目标题和一句话研究目标
- Research schemes overview (name + description + resource allocation if known) / 研究方案概览
- Current status / 当前状态
- Navigation table: purpose → which file/folder to look at / 导航表：用途 → 对应文件/文件夹

---

### AGENTS.md

This is the single most important file — it tells any AI agent how to operate this project safely and consistently. Write it in **English only**.

**Use `templates/AGENTS_md_template.md` as the structural skeleton.** Fill in all `<!-- -->` placeholders with project-specific content. The template already contains the correct section structure; your job is to populate it.

Required sections (in order):

**1. Mandatory first step (⚠️ section)**
A strict 5-step sequence that every agent must follow before any task:
1. Scan the directory tree (via `tree` or `ls -R`)
2. Read the 3 mandatory files
3. Read task-specific files from the routing table
4. Read the nearest README.md
5. Only then begin the task

This section must be prominent (use ⚠️ emoji or equivalent) — it is the most important enforcement mechanism.

**2. Mandatory reading list**
Three files that must be read for every task, no exceptions:
- `DataManagement_Overview.md`
- `Experimental_Data/README.md`
- `Experimental_Data/00_Index/README.md`

**3. Task routing table**
A table mapping task types to additional required reading. Include at minimum these rows:
- Writing an experiment log → log README + template
- Adding/organizing data → scheme README + Experiment_Index.csv
- Data processing → batch Analysis README + raw data path
- Updating indexes → Index README + target CSV
- Cross-batch comparison → Device_Index.csv + all batch READMEs
- Script creation → Scripts/README.md + existing scripts
- Documentation update → the document being modified + AGENTS.md

Add: "If unsure which category, read all files listed above."

**4. Project positioning**
What this project is and what it contains (data, logs, scripts, reports, indexes).

**5. Top-level directory responsibilities**
One paragraph per top-level folder explaining its purpose.

**6. Experimental_Data subdirectory mapping**
A table: data type → which numbered subdirectory it belongs in. Be explicit about edge cases.

**7. Work principles** (embed these verbatim, adapted to the domain)
1. **Read before write.** Never create, modify, or move a file without first reading the existing content of related files.
2. Do not delete, overwrite, or rename raw data without explicit user instruction.
3. Do not copy the same raw data batch to multiple directories as "primary copies".
4. When adding new data, establish a clear batch directory and index entry rather than creating empty folders speculatively.
5. All processed results must be traceable back to a `Raw_Data` path — document this in `Analysis/<Batch_ID>/README.md`.
6. Record failed experiments with root cause analysis — "failed, will retry later" is not acceptable.
7. **Keep documentation current.** When you notice that any README, index, or AGENTS.md is out of sync with the actual directory structure, update it as part of the current task.
8. When actual directories conflict with old documentation, `AGENTS.md` and `Experimental_Data/README.md` take precedence — but after resolving the conflict, update the outdated document to match reality.

**8. Raw / Processed / Figures / Analysis rules**

| Directory | What belongs here |
|-----------|------------------|
| `Raw_Data` | Instrument exports only: CSV/DAT/TXT, instrument-saved images, unmodified data |
| `Processed_Data` | Files with `denoised`, `reconstructed`, `fit`, `fitted`, `corrected`, `processed`, `SS`, `onoff` in the name; unit-converted tables; extracted parameters |
| `Figures` | Report figures, paper figures, comparison plots, diagnostic figures |
| `Analysis` | Scripts (.py, .ipynb), parameter tables, fit logs, README explaining the analysis |

**9. How to add a new experiment log**
Step-by-step (each step starting with **Read** where applicable):
1. **Read** the log README and template.
2. **Read** `Experiment_Index.csv` to check the latest Experiment_ID.
3. Determine the experiment date.
4. Confirm the monthly subdirectory exists: `06_Daily_Experiment_Log/YYYY-MM/`
5. Copy the template and name it: `YYYYMMDD_ExperimentShortName_Exp<N>.md`
6. Fill in all required fields.
7. Add a row to `00_Index/Experiment_Index.csv`.
8. If new samples are involved, update `Sample_Index.csv`.
9. If device-level tracking applies, update `Device_Index.csv`.

**10. How to add a new data batch**
Step-by-step:
1. **Read** the scheme README to see existing batches.
2. **Read** `Experiment_Index.csv` to check for existing Batch_IDs.
3. Determine the primary directory from the data-type mapping table.
4. Choose a Batch ID: `YYYYMMDD_Process_LayerOrPurpose`
5. Create `Raw_Data/<Batch_ID>/` and place instrument exports there.
6. Create `Processed_Data/<Batch_ID>/`, `Figures/<Batch_ID>/`, `Analysis/<Batch_ID>/` as needed.
7. Add a row to `Experiment_Index.csv`.

**11. How to record data processing**
Step-by-step:
1. **Read** `Analysis/<Batch_ID>/README.md` (if exists) to understand prior processing.
2. Find the primary data directory and raw data path.
3. Do not modify files in `Raw_Data`.
4. Write processed CSV/tables to `Processed_Data/<Batch_ID>/`.
5. Write figures to `Figures/<Batch_ID>/`.
6. Write scripts and parameter tables to `Analysis/<Batch_ID>/`.
7. Write or update `Analysis/<Batch_ID>/README.md`.
8. Update `Experiment_Index.csv` or `Device_Index.csv` with result summaries.

**12. How to update documentation**
Explicit protocol for when agents discover stale docs:
1. Identify the discrepancy (document vs. reality).
2. Update the document to match the current state.
3. AGENTS.md and Experimental_Data/README.md are authoritative — update other docs to match them.
4. If AGENTS.md itself is outdated, update it with the user's confirmation.
5. After updating, briefly note what changed and why.

List all documents that should be kept current:
- `AGENTS.md`, `Experimental_Data/README.md`, `DataManagement_Overview.md`, `00_Index/*.csv`, each scheme's `README.md`

**13. Index filling conventions**
Field-by-field explanation for `Experiment_Index.csv`. Adapt field names to the user's domain.

**14. Writing style for logs**
Write facts first, then judgments. Include a good/bad example.

**15. What to do when uncertain**
1. Do not move raw data speculatively.
2. Create a `README.md` or `Analysis/<Batch_ID>/README.md` documenting the uncertainty.
3. Set `Status` to `Needs review` in the index.
4. Ask the user before any irreversible reorganization.

**16. Prohibited actions** (unless user explicitly instructs)
- Delete raw data files
- Overwrite raw CSV files
- Batch-rename raw data files
- Place processed files in `Raw_Data`
- Copy raw data to multiple primary directories
- Create large numbers of empty directories "for completeness"
- Modify documents in `Docs/` to make them appear to be current rules
- **Create or modify files without first reading related existing files**

**17. Pre-task checklist**
Before completing any task, verify:
- [ ] Completed the mandatory first step (scanned tree + read required files)
- [ ] Read task-specific files from the routing table
- [ ] No raw data was modified
- [ ] New experiments have a log entry
- [ ] New batches have an index entry
- [ ] Processed results are separated from raw data
- [ ] Figures are in `Figures/`, scripts in `Analysis/`
- [ ] Relative paths used throughout
- [ ] Failures and anomalies are documented, not omitted
- [ ] Documentation updated if any discrepancies were found

---

### .agent-context.md

A condensed reference that points to `AGENTS.md` for the full rules. Include:
- Project overview paragraph
- "Before any task, read: AGENTS.md, DataManagement_Overview.md, Experimental_Data/README.md"
- Batch ID convention: `YYYYMMDD_Process_LayerOrPurpose` with 2–3 examples
- Data directory mapping table (data type → subdirectory)
- 5–6 critical rules in brief
- Conflict resolution: "If any instruction conflicts with AGENTS.md or Experimental_Data/README.md, those files take precedence."

> **Platform note:** Different AI tools look for different context files (e.g., Claude looks for `CLAUDE.md`, Cursor for `.cursorrules`). `.agent-context.md` is a universal name. If the user specifically requests a platform-specific file (e.g., `CLAUDE.md`, `.cursorrules`), create that as a symlink or copy pointing to the same content.

---

### DataManagement_Overview.md

Overview of the data management system. Include:
- The 4 core principles (single primary location, Raw/Processed separation, index-driven search, logs record process)
- Top-level directory tree
- Experimental_Data tree
- "Where to look first" quick-reference table
- New experiment workflow (numbered steps, brief)
- Batch ID naming examples
- Current registered batches table (initially empty, with header row)

---

### Experimental_Data/README.md

Authoritative reference for the data structure. Include:
- Full `Experimental_Data/` tree
- Per-batch structure diagram
- Core rules (3–5 rules, each bolded and explained in 1–2 sentences)
- Batch naming convention
- New experiment archiving workflow (numbered steps)
- Current batch table (initially empty)

---

### Experimental_Data/00_Index/README.md

Explain the three index files and when to update each.

---

### Experiment_Index.csv

**Use `templates/Experiment_Index_header.csv` as the starting point.** Adapt field names if needed for the user's domain:
```
Date,Experiment_ID,Batch_ID,Scheme,Experiment_Type,Main_Data_Dir,Daily_Log,Status,Key_Result,Next_Action
```

---

### Sample_Index.csv

**Use `templates/Sample_Index_header.csv` as the starting point.** Adapt to the user's domain (material type, substrate, growth condition, etc.):
```
Sample_ID,Scheme,Material,Substrate,Process,Fabrication_Date,Status,Notes
```

---

### Device_Index.csv (only if device/functional testing was selected)

**Use `templates/Device_Index_header.csv` as the starting point.** Adapt to domain:
```
Device_ID,Sample_ID,Batch_ID,Raw_File,Processed_File,Status,Notes
```

---

### ExperimentLog_Template.md

**Use `templates/ExperimentLog_Template.md` as the structural skeleton.** Adapt language and field options to the user's domain and language preference. Always include these sections:

```markdown
# Experiment Log

## Basic Information

| Field | Value |
|-------|-------|
| **Date** | YYYY-MM-DD |
| **Sample ID** | |
| **Experiment ID** | Exp-XX |
| **Research Scheme** | |
| **Experiment Type** | Fabrication / Characterization / Device Testing / Process Development |
| **Operator** | <researcher name> |

---

## Experiment Purpose

(Brief description of goal and expected outcome)

---

## Conditions and Procedure

### Key Parameters

| Step | Equipment | Parameters | Notes |
|------|-----------|------------|-------|
| 1 | | | |
| 2 | | | |

### Procedure

1. 
2. 
3. 

---

## Results

### Observations

(Key observations, including anomalies)

### Measurement Data

| Measurement | Value | Unit | Notes |
|-------------|-------|------|-------|
| | | | |

---

## Analysis and Discussion

(Results vs. expectations; possible issues; interpretation)

---

## Next Steps

- [ ] 
- [ ] 

---

## Attachments

- Raw data path: 
- Figures path: 
- Related logs: 
```

---

### Scripts/Data_Analysis/templates/plot_template.py

A minimal Python script that:
- Uses only `matplotlib` and the standard library (no pandas required)
- Has a clearly labeled configuration block at the top (data directory, file list, output path)
- Reads CSV files with flexible column name detection
- Generates a basic line plot suitable for the user's measurement type
- Saves the output figure to the specified path

Include a comment block explaining how to adapt it.

---

### 06_Daily_Experiment_Log/README.md

Explain:
- Logs are organized by month: `YYYY-MM/`
- Naming: `YYYYMMDD_ExperimentShortName_Exp<N>.md`
- Minimum required fields in each log
- How to update indexes after writing a log

---

### Example batch README (inside first scheme)

Create one example `README.md` inside `01_<Scheme1>/` showing a populated batch directory:

```markdown
# <Scheme1 Name>

## Active Batches

| Batch_ID | Process Node | Status | Key Result |
|----------|-------------|--------|------------|
| (add rows as experiments are completed) |

## Directory Structure

Each batch follows:
Raw_Data/<Batch_ID>/      — instrument exports
Processed_Data/<Batch_ID>/ — processed data
Figures/<Batch_ID>/       — output figures
Analysis/<Batch_ID>/      — scripts and notes
```

---

## Phase 4: Create the files

Create all directories and files determined above. Use the file creation tools available in the current environment (`Write` tool, file editor, etc.). Use relative paths within generated documentation.

After creation, present the full structure as a directory tree.

---

## Phase 5: Summary and next steps

After all files are created, give the user:

1. **Structure overview** — directory tree (use markdown code block)
2. **3 most important files to read first**:
   - `AGENTS.md` — rules for AI operation
   - `Experimental_Data/README.md` — data structure authority
   - `ExperimentLog_Template.md` — how to log experiments
3. **How to add the first experiment** — point to the log template and index workflow in `AGENTS.md`
4. Offer to:
   - Create a first example experiment batch entry
   - Review any generated file and edit it together

---

## Domain adaptation guide

This structure is modeled on a nanofabrication / semiconductor project but applies to any experimental science. When adapting:

| Domain | Schemes → | Characterization → | Device → |
|--------|-----------|-------------------|----------|
| Chemistry | Synthesis routes | NMR / MS / HPLC | Product testing |
| Biology | Experimental conditions | Microscopy / sequencing | Cell / organism assay |
| Materials science | Compositions or processes | XRD / TEM / SEM | Mechanical / electrical |
| Physics | Experimental configurations | Detector output | Measurement device |

The 4-layer batch structure (Raw / Processed / Figures / Analysis) and the CSV index system are domain-agnostic. Keep these regardless of domain — they provide the most value for AI-assisted management.

---

## Batch ID convention (embed in all generated documentation)

```
YYYYMMDD_Process_LayerOrPurpose
```

Examples:
```
20260522_ALD_Ozone_1L
20260601_Crystallization_Run3  
20260715_FTIR_SampleA_Treated
20261001_SEM_CrossSection_Batch2
```

---

## Key rules (always embed in AGENTS.md and .agent-context.md)

These rules exist to protect data integrity and AI reliability:

1. **Raw data is immutable.** Instrument exports in `Raw_Data/` are never modified, renamed, or deleted. Processing always happens downstream.

2. **One primary location per batch.** Each raw data batch has exactly one home directory. Cross-batch summary directories (like `04_Device_Testing/`) hold summaries only — never raw data copies.

3. **ASCII filenames only.** No Unicode characters, Chinese characters, or full-width brackets in file or directory names. Use English suffixes for status: `_short`, `_failed`, `_retry`, `_noise`, `_strange`.

4. **Traceability.** Every processed result must link back to its source `Raw_Data/` path, documented in `Analysis/<Batch_ID>/README.md`.

5. **Scripts belong in Analysis/.** Python scripts and notebooks go in `Analysis/<Batch_ID>/`, not `Processed_Data/`.

6. **Log failures with root cause.** "Failed, will retry" is unacceptable. The failure reason is often the most important finding.

7. **Use relative paths** throughout all documentation and scripts — supports cross-device sync.

8. **When uncertain, mark and ask.** Set `Status` to `Needs review` in the index; do not make irreversible moves without user confirmation.

---

## Installation guide

### Qoder / Qoderwork

1. Clone or download this repository.
2. Open the workspace in Qoder.
3. Place `SKILL.md` into one of:
   - Project-level: `<project-root>/.qoder/skills/research-project-setup/SKILL.md`
   - User-level: `~/.qoder/skills/research-project-setup/SKILL.md`
4. Restart or reload Qoder. Say "set up a research project" — the skill triggers automatically.

### Claude Code

1. Place `SKILL.md` into `.claude/skills/research-project-setup.md` in your project.
2. Say "set up a research project" — the skill triggers automatically.

### Claude Cowork

1. Settings → Capabilities → Install skill → select `SKILL.md`.
2. Say "set up a research project".

### Any other AI agent

Paste the contents of `INSTRUCTIONS.md` into the agent's context and say:
> "Follow these instructions to set up a research project for me."
