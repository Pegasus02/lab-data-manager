# Lab Data Manager — Agent Instructions

This document tells an AI agent how to set up a structured experimental research data management system for a user. Read it fully before starting.

The result is a self-contained project folder that any future AI agent can navigate, with clean separation between raw and processed data, structured indexes, and daily experiment logs.

---

## Step 1: Interview the user

Ask all questions in a single message — do not ask one question at a time. Adapt your language to the user's domain and familiarity level.

### Questions to ask

**Project identity**
- What is the name of this research project? (This becomes the root folder name — use ASCII characters, underscores or hyphens only, no spaces.)
- One sentence: what is the research goal?

**Research schemes / parallel approaches**
- Does this project have multiple parallel research routes or schemes? (e.g., "Route A uses synthesis method X, Route B uses method Y")
- For each scheme: a short name and a brief description.
- If the user has only one scheme or is unsure, use `01_Main_Scheme/` — it can be expanded later.

**Experiment categories** — ask the user to select all that apply:
- Fabrication / synthesis (making samples)
- Materials characterization (microscopy, spectroscopy, diffraction, etc.)
- Device / functional testing (electrical, optical, mechanical measurements)
- Process development (optimizing a single process step)
- Cross-batch comparison / meta-analysis
- Any domain-specific categories not listed above?

**Raw data file types**
- What file formats does their equipment export? (CSV, DAT, TXT, TIFF, HDF5, proprietary formats, etc.)

**Team**
- Who are the main operators / researchers? (used in log template default)
- Solo or team project?

**AI agent usage**
- Will they use AI agents to help manage this project? If yes, generate a detailed `AGENTS.md`. If no, create a lightweight version.

**Language preference**
- English, Chinese, or mixed documentation? (Filenames must always be ASCII regardless.)

---

## Step 2: Design the directory structure

Based on the user's answers, draft the folder structure and show it as a tree. Ask the user to confirm or request changes **before creating any files**.

### Top-level layout

```
<ProjectName>/
├── AGENTS.md                        # AI agent operation rules
├── .agent-context.md                # Condensed quick-reference for any AI agent
├── README.md                        # Project overview and goals
├── DataManagement_Overview.md       # Data system entry point
├── Experimental_Data/
├── Scripts/
├── Progress/
└── PPT/
```

### Experimental_Data layout rules

- `00_Index/` always comes first.
- Research scheme directories are numbered from `01_` — one per scheme.
- Category directories for each selected experiment type follow sequentially.
- Daily logs always come last (conventionally numbered to follow the last category).

Example for 2 schemes + characterization + device testing + process development:

```
Experimental_Data/
├── 00_Index/
├── 01_<Scheme1>/
├── 02_<Scheme2>/
├── 03_Characterization/
├── 04_Device_Testing/          # Cross-batch summaries only — no raw data copies here
├── 05_Process_Development/
└── 06_Daily_Experiment_Log/
```

Renumber cleanly — no gaps. If the user has 1 scheme + characterization only, the result is `01_Main_Scheme/`, `02_Characterization/`, `03_Daily_Experiment_Log/`.

### Per-batch structure

Each experiment batch has exactly one primary home directory. Create an example inside the first scheme:

```
<scheme-dir>/<process-or-topic-node>/
├── README.md
├── Raw_Data/<Batch_ID>/        # Instrument exports only, never modified
├── Processed_Data/<Batch_ID>/  # Denoised, reconstructed, unit-converted
├── Figures/<Batch_ID>/         # Report and paper figures
└── Analysis/<Batch_ID>/        # Scripts, notebooks, fit logs, parameter tables
```

---

## Step 3: Create all files

After the user confirms the structure, create every file below. Adapt content to the user's project, preferred language, and domain. Use relative paths inside all generated documentation.

### File list

| File | Notes |
|------|-------|
| `README.md` | Project title, goal, schemes overview, navigation table |
| `AGENTS.md` | Full AI operation rules — see template below |
| `.agent-context.md` | Condensed AGENTS.md for any AI agent |
| `DataManagement_Overview.md` | 4 core principles, directory trees, quick-ref table, naming examples |
| `Experimental_Data/README.md` | Authoritative data structure reference |
| `Experimental_Data/00_Index/README.md` | Explains the three index CSVs |
| `Experimental_Data/00_Index/Experiment_Index.csv` | Header row only |
| `Experimental_Data/00_Index/Sample_Index.csv` | Header row only |
| `Experimental_Data/00_Index/Device_Index.csv` | Header row only (omit if no device testing) |
| `Experimental_Data/06_Daily_Experiment_Log/README.md` | Log naming convention and workflow |
| `Experimental_Data/06_Daily_Experiment_Log/ExperimentLog_Template.md` | Log template |
| `Experimental_Data/01_<Scheme1>/README.md` | Batch table + directory structure explanation |
| `Scripts/README.md` | How to use analysis scripts |
| `Scripts/Data_Analysis/templates/plot_template.py` | Minimal matplotlib template |

---

## AGENTS.md template

Use `templates/AGENTS_md_template.md` as the structural skeleton. The template is bilingual (English + Chinese) — fill in project-specific details in both languages. The template enforces a strict read-before-write protocol for all AI agents.

Key sections to populate:

1. **⚠️ Mandatory first step** — 5-step sequence: scan tree → read 3 mandatory files → read task-specific files → read nearest README → begin task
2. **Mandatory reading** — 3 files for every task
3. **Task routing table** — maps task types to additional required reading
4. **Project positioning** — what the project is and contains
5. **Directory responsibilities** — one paragraph per top-level folder
6. **Data type → directory mapping** — explicit table with edge cases
7. **Work principles** — 8 principles including "read before write" and "keep documentation current"
8. **Raw/Processed/Figures/Analysis rules**
9. **How-to workflows** — each workflow starts with a **Read** step before any action
10. **How to update documentation** — explicit protocol for fixing stale docs
11. **Index field conventions**
12. **Writing style, uncertainty handling, prohibited actions, pre-task checklist**

```markdown
# AGENTS.md

> **This file gives AI agents operating instructions for this project.**
> Read it before any task. It takes precedence over all other documentation.

## ⚠️ Mandatory first step — do this before ANY task

Before writing, moving, or creating any file, you MUST complete this sequence:

1. **Scan the directory tree** — read the full output of `tree` or `ls -R` from the project root.
2. **Read the mandatory reading list** below (all three files, every time).
3. **Read the task-specific files** from the routing table below.
4. **Read the nearest README.md** to the directory you will be working in.
5. Only after completing steps 1–4, begin the actual task.

If you skip any step, the task output will be incorrect.

## Mandatory reading (always, for every task)

1. `DataManagement_Overview.md`
2. `Experimental_Data/README.md`
3. `Experimental_Data/00_Index/README.md`

## Task routing table

| Task type | Also read |
|-----------|----------|
| Writing an experiment log | Log README + template |
| Adding or organizing data | Scheme README + Experiment_Index.csv |
| Data processing or analysis | Batch Analysis README + raw data path |
| Updating indexes | Index README + target CSV |
| Cross-batch comparison | Device_Index.csv + all batch READMEs |
| Script creation | Scripts/README.md + existing scripts |
| Documentation update | The document being modified + AGENTS.md |

If unsure which category, read all files listed above.

## Project positioning

<One paragraph: what this project is, what it contains.>

## Directory responsibilities

<One paragraph per top-level folder.>

## Data type → directory mapping

| Data type | Primary directory |
|-----------|------------------|
| <type> | <directory> |

## Work principles

1. **Read before write.** Never create, modify, or move a file without first reading the existing content of related files.
2. Do not delete, overwrite, or rename raw data without explicit user instruction.
3. Do not copy the same raw data batch to multiple directories as "primary copies".
4. When adding data, establish a clear batch directory and index entry.
5. All processed results must trace back to a `Raw_Data/` path; document in `Analysis/<Batch_ID>/README.md`.
6. Record failed experiments with root cause — "failed, will retry" is not acceptable.
7. **Keep documentation current.** When any README, index, or AGENTS.md is out of sync, update it as part of the current task.
8. When directories conflict with older documentation, this file and `Experimental_Data/README.md` take precedence — but update the outdated document after resolving.

## Raw / Processed / Figures / Analysis rules

| Directory | Contents |
|-----------|----------|
| `Raw_Data` | Instrument exports only: CSV/DAT/TXT, unmodified images |
| `Processed_Data` | Denoised, reconstructed, fitted, corrected, unit-converted files |
| `Figures` | Report figures, paper figures, comparison plots |
| `Analysis` | Scripts (.py, .ipynb), parameter tables, fit logs, analysis README |

## How to add a new experiment log

1. **Read** the log README and template.
2. **Read** `Experiment_Index.csv` to check the latest Experiment_ID.
3. Determine the experiment date.
4. Confirm monthly directory exists: `06_Daily_Experiment_Log/YYYY-MM/`
5. Copy the template; name it: `YYYYMMDD_ExperimentShortName_Exp<N>.md`
6. Fill in all fields.
7. Add a row to `00_Index/Experiment_Index.csv`.
8. If new samples: update `Sample_Index.csv`.
9. If device-level tracking: update `Device_Index.csv`.

## How to add a new data batch

1. **Read** the scheme README to see existing batches.
2. **Read** `Experiment_Index.csv` to check for existing Batch_IDs.
3. Determine the primary directory from the data-type mapping table.
4. Choose a Batch ID: `YYYYMMDD_Process_LayerOrPurpose`
5. Create `Raw_Data/<Batch_ID>/` and place instrument exports there.
6. Create `Processed_Data/`, `Figures/`, `Analysis/` subdirectories as needed.
7. Add a row to `Experiment_Index.csv`.

## How to record data processing

1. **Read** `Analysis/<Batch_ID>/README.md` (if exists) to understand prior processing.
2. Locate the primary data directory and raw data path.
3. Do not modify any file in `Raw_Data/`.
4. Write processed data to `Processed_Data/<Batch_ID>/`.
5. Write figures to `Figures/<Batch_ID>/`.
6. Write scripts and parameter tables to `Analysis/<Batch_ID>/`.
7. Write or update `Analysis/<Batch_ID>/README.md`.
8. Update `Experiment_Index.csv` or `Device_Index.csv` with result summaries.

## How to update documentation

1. **Identify the discrepancy** — document vs. reality.
2. **Update the document** to match the current state.
3. If the conflict involves AGENTS.md or Experimental_Data/README.md, those are authoritative.
4. If AGENTS.md itself is outdated, update it with the user's confirmation.
5. After updating, note what changed and why.

## Index field conventions

### Experiment_Index.csv
- `Date` — YYYY-MM-DD
- `Experiment_ID` — unique ID, e.g., `Exp-<Topic>-<N>`
- `Batch_ID` — follows the naming convention
- `Scheme` — which research scheme
- `Experiment_Type` — fabrication / characterization / device testing / process development
- `Main_Data_Dir` — relative path to primary data directory
- `Daily_Log` — relative path to log file
- `Status` — Planned / Running / Completed / Failed / Completed with issues / Needs review
- `Key_Result` — one-sentence finding
- `Next_Action` — one-sentence next step

## Writing style for logs

Write facts first, then judgments. Good example:
> Observation: No suspended bridge structure seen under SEM; local areas show possible resist residue.
> Judgment: Release failed. Uncertainties: ALD film quality and capillary collapse during wet release.
> Next step: Switch to stable ALD tool; use SiNx hard mask; evaluate supercritical drying.

Never write: "Experiment failed, will retry."

## When uncertain

1. Do not move raw data speculatively.
2. Document the uncertainty in a README or Analysis/<Batch_ID>/README.md.
3. Set `Status` to `Needs review` in the index.
4. Ask the user before any irreversible reorganization.

## Prohibited actions (unless user explicitly instructs)

- Delete raw data files
- Overwrite raw CSV files
- Batch-rename raw data files
- Place processed files in `Raw_Data/`
- Copy raw data to multiple primary directories
- Create large numbers of empty directories "for completeness"
- Edit files in `Docs/` to make them appear to be current rules
- **Create or modify files without first reading related existing files**

## Pre-task checklist

- [ ] Completed the mandatory first step (scanned tree + read required files)
- [ ] Read task-specific files from the routing table
- [ ] No raw data was modified
- [ ] New experiments have a log entry
- [ ] New batches have an index entry
- [ ] Processed results are separated from raw data
- [ ] Figures in `Figures/`, scripts in `Analysis/`
- [ ] Relative paths used throughout
- [ ] Failures documented, not omitted
- [ ] Documentation updated if any discrepancies were found
```

---

## Batch ID convention

Always use:
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

## Key rules to embed in all generated documentation

1. **Raw data is immutable.** Instrument exports in `Raw_Data/` are never modified, renamed, or deleted.
2. **One primary location per batch.** No raw data copies across multiple directories.
3. **ASCII filenames only.** No Unicode, Chinese characters, or full-width brackets. Use suffixes like `_short`, `_failed`, `_retry`, `_noise`.
4. **Traceability.** Every processed result links back to its source `Raw_Data/` path in `Analysis/<Batch_ID>/README.md`.
5. **Scripts in `Analysis/`.** Not in `Processed_Data/`.
6. **Log failures with root cause.**
7. **Relative paths** throughout all documentation and scripts.
8. **When uncertain, mark and ask.** Set `Status` to `Needs review`; do not make irreversible moves without user confirmation.

---

## Domain adaptation

This structure works for any experimental science. When adapting:

| Domain | Schemes | Characterization | Device/Testing |
|--------|---------|-----------------|----------------|
| Nanofabrication | Fabrication routes | AFM / SEM / TEM / Raman | Electrical (Id-Vg) |
| Chemistry | Synthesis routes | NMR / MS / HPLC | Product testing |
| Biology | Experimental conditions | Microscopy / sequencing | Cell / organism assay |
| Materials science | Compositions or processes | XRD / TEM / SEM | Mechanical / electrical |
| Physics | Experimental configurations | Detector output | Measurement device |

The 4-layer batch structure and CSV index system are domain-agnostic. Keep them regardless of domain.

---

## Step 4: Present the result

After creating all files:

1. Show the full directory tree as a code block.
2. Tell the user the 3 most important files to read first: `AGENTS.md`, `Experimental_Data/README.md`, `ExperimentLog_Template.md`.
3. Explain how to add the first experiment (point to log template + `AGENTS.md` workflow).
4. Offer to create a first example batch entry or review any generated file.
