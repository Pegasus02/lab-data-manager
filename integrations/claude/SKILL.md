---
name: research-project-setup
description: "Set up a structured experimental research project management system for scientific research. Use this skill whenever a researcher wants to organize their lab data, create a new research project folder structure, set up experiment tracking and indexing, or establish a reproducible data management workflow. Works for any experimental science: nanofabrication, chemistry, biology, materials science, physics, and beyond. Trigger on phrases like 'set up a research project', 'organize my experiment data', 'create a lab data management system', 'help me structure my research files', '建立课题管理结构', '整理实验数据文件夹', or whenever someone is starting a new research project and needs a systematic way to track experiments, samples, and data batches. Also use this skill if the user shares an existing research project structure and asks to replicate or adapt it."
---

# Research Project Setup Skill

This skill guides you through creating a structured, AI-friendly experimental research data management system — modeled on proven practices from real nanofabrication research projects. It works for any experimental science domain.

The final result is a self-contained project folder with:
- Clear separation of raw vs. processed data
- Structured CSV indexes for fast lookup
- Daily experiment logs with a standard template  
- Per-batch 4-layer directory layout (Raw → Processed → Figures → Analysis)
- An `AGENTS.md` so any future AI agent can operate the project correctly

---

## Phase 1: Interview the user

Before creating anything, gather the following information. Ask everything in one message — don't drip-feed questions one at a time. Group related questions naturally. Use the `AskUserQuestion` tool for structured choices and open-ended prose for free-form answers.

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
Will they use AI agents (Claude or similar) to help manage this project? If yes, generate a detailed `AGENTS.md`. If no, create a lightweight version or skip it.

**7. Output location**
Where should the project folder be created? Default to the connected workspace folder. Ask only if they might want it elsewhere.

**8. Language preference**
English, Chinese, or mixed documentation? (Filenames are always ASCII regardless of choice.)

---

## Phase 2: Design the directory structure

Based on the user's answers, design the structure before creating files. Show it to the user as a tree and ask for confirmation or changes before proceeding.

### Top-level layout

```
<ProjectName>/
├── AGENTS.md                        # AI agent operation rules
├── CLAUDE.md                        # Alias / condensed version of AGENTS.md
├── README.md                        # Project overview and research goals
├── DataManagement_Overview.md       # Data system entry point
├── Experimental_Data/               # All experimental data and logs
├── Scripts/                         # Data analysis and plotting scripts
├── Progress/                        # Progress reports
└── PPT/                             # Presentation materials
```

### Experimental_Data layout

Always start with `00_Index` and end daily logs at `06_Daily_Experiment_Log`. Number intermediate directories by category starting from `01`. Adapt names to the user's domain.

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

Create each file below with content tailored to the user's project. Use the user's preferred language. Keep all filenames ASCII.

---

### README.md (top level)

Include:
- Project title and one-sentence research goal
- Research schemes overview (name + description + resource allocation if known)
- Current status (e.g., "Early stage — first experiments underway")
- Navigation table: purpose → which file/folder to look at

---

### AGENTS.md

This is the single most important file — it tells any AI agent how to operate this project safely and consistently. Write it in the user's preferred language. Structure:

**1. Project positioning**  
What this project is and what it contains (data, logs, scripts, reports, indexes).

**2. Mandatory reading list**  
Which files an agent must read before starting any task. Minimum:
- `DataManagement_Overview.md`
- `Experimental_Data/README.md`
- `Experimental_Data/00_Index/README.md`

For experiment log tasks, also read the log README and template.  
For existing data batches, also read the README nearest that batch.

**3. Top-level directory responsibilities**  
One paragraph per top-level folder explaining its purpose.

**4. Experimental_Data subdirectory mapping**  
A table: data type → which numbered subdirectory it belongs in. Be explicit about edge cases (e.g., cross-cutting characterization data belongs in characterization, not in a scheme folder).

**5. Work principles** (embed these verbatim, adapted to the domain)  
1. Do not delete, overwrite, or rename raw data without explicit user instruction.  
2. Do not copy the same raw data batch to multiple directories as "primary copies".  
3. When adding new data, establish a clear batch directory and index entry rather than creating empty folders speculatively.  
4. All processed results must be traceable back to a `Raw_Data` path — document this in `Analysis/<Batch_ID>/README.md`.  
5. Record failed experiments with root cause analysis — "failed, will retry later" is not acceptable.  
6. When actual directories conflict with old documentation, `AGENTS.md` and `Experimental_Data/README.md` take precedence.

**6. Raw / Processed / Figures / Analysis rules**

| Directory | What belongs here |
|-----------|------------------|
| `Raw_Data` | Instrument exports only: CSV/DAT/TXT, instrument-saved images, unmodified data |
| `Processed_Data` | Files with `denoised`, `reconstructed`, `fit`, `fitted`, `corrected`, `processed`, `SS`, `onoff` in the name; unit-converted tables; extracted parameters |
| `Figures` | Report figures, paper figures, comparison plots, diagnostic figures |
| `Analysis` | Scripts (.py, .ipynb), parameter tables, fit logs, README explaining the analysis |

**7. How to add a new experiment log**  
Step-by-step:
1. Determine the experiment date.
2. Confirm the monthly subdirectory exists: `06_Daily_Experiment_Log/YYYY-MM/`
3. Copy the template and name it: `YYYYMMDD_ExperimentShortName_Exp<N>.md`
4. Fill in all required fields.
5. Add a row to `00_Index/Experiment_Index.csv`.
6. If new samples are involved, update `Sample_Index.csv`.
7. If device-level tracking applies, update `Device_Index.csv`.

**8. How to add a new data batch**  
Step-by-step:
1. Determine the primary directory from the data-type mapping table.
2. Choose a Batch ID: `YYYYMMDD_Process_LayerOrPurpose`
3. Create `Raw_Data/<Batch_ID>/` and place instrument exports there.
4. Create `Processed_Data/<Batch_ID>/`, `Figures/<Batch_ID>/`, `Analysis/<Batch_ID>/` as needed (only create what will actually be used).
5. Add a row to `Experiment_Index.csv`.

**9. How to record data processing**  
Step-by-step:
1. Find the primary data directory and raw data path.
2. Do not modify files in `Raw_Data`.
3. Write processed CSV/tables to `Processed_Data/<Batch_ID>/`.
4. Write figures to `Figures/<Batch_ID>/`.
5. Write scripts and parameter tables to `Analysis/<Batch_ID>/`.
6. Write `Analysis/<Batch_ID>/README.md` documenting: processing date, method/script used, input raw data paths, output paths, key parameters, excluded/corrected data points, preliminary conclusions, open questions.
7. Update `Experiment_Index.csv` or `Device_Index.csv` with result summaries.

**10. Index filling conventions**  
Field-by-field explanation for `Experiment_Index.csv`. Adapt field names to the user's domain:
- `Date` — experiment date, format `YYYY-MM-DD`
- `Experiment_ID` — unique ID, e.g., `Exp-<Topic>-<N>`
- `Batch_ID` — batch identifier following the naming convention
- `Scheme` — which research scheme this belongs to
- `Experiment_Type` — fabrication / characterization / device testing / process development
- `Main_Data_Dir` — relative path to the primary data directory
- `Daily_Log` — relative path to the experiment log file
- `Status` — `Planned` / `Running` / `Completed` / `Failed` / `Completed with issues` / `Needs review`
- `Key_Result` — one-sentence summary of the main finding
- `Next_Action` — one-sentence next step

**11. Writing style for logs**  
Write facts first, then judgments. Example of good style:
```
Observation: No continuous suspended bridge structure seen under SEM; local areas show possible resist residue.
Judgment: Release failed. Main uncertainties: whether ALD film grew successfully, and whether capillary collapse occurred during wet release.
Next step: Switch to a stable ALD tool; use SiNx hard mask; evaluate supercritical drying.
```
Never write just: "Experiment failed, will retry."

**12. What to do when uncertain**  
1. Do not move raw data speculatively.  
2. Create a `README.md` or `Analysis/<Batch_ID>/README.md` documenting the uncertainty.  
3. Set `Status` to `Needs review` in the index.  
4. Ask the user before any irreversible reorganization.

**13. Prohibited actions** (unless user explicitly instructs)
- Delete raw data files
- Overwrite raw CSV files
- Batch-rename raw data files
- Place processed files in `Raw_Data`
- Copy raw data to multiple primary directories
- Create large numbers of empty directories "for completeness"
- Modify documents in `Docs/` to make them appear to be current rules

**14. Pre-task checklist**  
Before completing any task, verify:
- [ ] Read `AGENTS.md` and relevant `README.md` files
- [ ] No raw data was modified
- [ ] New experiments have a log entry
- [ ] New batches have an index entry
- [ ] Processed results are separated from raw data
- [ ] Figures are in `Figures/`, scripts in `Analysis/`
- [ ] Relative paths used throughout
- [ ] Failures and anomalies are documented, not omitted

---

### CLAUDE.md

A condensed reference that points to `AGENTS.md` for the full rules. Include:
- Project overview paragraph
- "Before any task, read: AGENTS.md, DataManagement_Overview.md, Experimental_Data/README.md"
- Batch ID convention: `YYYYMMDD_Process_LayerOrPurpose` with 2–3 examples
- Data directory mapping table (data type → subdirectory)
- 5–6 critical rules in brief
- Conflict resolution: "If any instruction conflicts with AGENTS.md or Experimental_Data/README.md, those files take precedence."

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

Header row only. Adapt field names if needed:
```
Date,Experiment_ID,Batch_ID,Scheme,Experiment_Type,Main_Data_Dir,Daily_Log,Status,Key_Result,Next_Action
```

---

### Sample_Index.csv

Header row only. Adapt to the user's domain (material type, substrate, growth condition, etc.):
```
Sample_ID,Scheme,Material,Substrate,Process,Fabrication_Date,Status,Notes
```

---

### Device_Index.csv (only if device/functional testing was selected)

Header row only. Adapt to domain:
```
Device_ID,Sample_ID,Batch_ID,Raw_File,Processed_File,Status,Notes
```

---

### ExperimentLog_Template.md (or 实验记录模板.md if Chinese preferred)

A complete Markdown template. Always include these sections, with language adapted to the user's domain:

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

Create all directories and files determined above. Use the `Write` tool for each file. Use relative paths within generated documentation.

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

## Key rules (always embed in AGENTS.md and CLAUDE.md)

These rules exist to protect data integrity and AI reliability:

1. **Raw data is immutable.** Instrument exports in `Raw_Data/` are never modified, renamed, or deleted. Processing always happens downstream.

2. **One primary location per batch.** Each raw data batch has exactly one home directory. Cross-batch summary directories (like `04_Device_Testing/`) hold summaries only — never raw data copies.

3. **ASCII filenames only.** No Unicode characters, Chinese characters, or full-width brackets in file or directory names. Use English suffixes for status: `_short`, `_failed`, `_retry`, `_noise`, `_strange`.

4. **Traceability.** Every processed result must link back to its source `Raw_Data/` path, documented in `Analysis/<Batch_ID>/README.md`.

5. **Scripts belong in Analysis/.** Python scripts and notebooks go in `Analysis/<Batch_ID>/`, not `Processed_Data/`.

6. **Log failures with root cause.** "Failed, will retry" is unacceptable. The failure reason is often the most important finding.

7. **Use relative paths** throughout all documentation and scripts — supports cross-device sync.

8. **When uncertain, mark and ask.** Set `Status` to `Needs review` in the index; do not make irreversible moves without user confirmation