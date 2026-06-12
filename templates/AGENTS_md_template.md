# AGENTS.md

> **This file gives AI agents operating instructions for this project.**  
> Read it before any task. It takes precedence over all other documentation.

## ⚠️ Mandatory first step — do this before ANY task

Before writing, moving, or creating any file, you MUST complete this sequence:

1. **Scan the directory tree** — read the full output of `tree` or `ls -R` from the project root to understand what exists.
2. **Read the mandatory reading list** below (all three files, every time, no exceptions).
3. **Read the task-specific files** from the routing table below.
4. **Read the nearest README.md** to the directory you will be working in.
5. Only after completing steps 1–4, begin the actual task.

If you skip any step, the task output will be incorrect. There is no shortcut.

## Mandatory reading (always, for every task)

Read these three files in order before doing anything:

1. `DataManagement_Overview.md` — system-wide overview and principles
2. `Experimental_Data/README.md` — authoritative data structure reference
3. `Experimental_Data/00_Index/README.md` — index file conventions

## Task routing table

After the mandatory reading, read additional files based on the task type:

| Task type | Also read |
|-----------|-----------|
| Writing an experiment log | `Experimental_Data/06_Daily_Experiment_Log/README.md` + `ExperimentLog_Template.md` |
| Adding or organizing data | The `README.md` inside the target scheme directory + `Experiment_Index.csv` |
| Data processing or analysis | `Analysis/<Batch_ID>/README.md` for the relevant batch + the raw data path listed there |
| Updating indexes | `Experimental_Data/00_Index/README.md` + the CSV file(s) to be updated |
| Cross-batch comparison | `Device_Index.csv` (if present) + all relevant batch `README.md` files |
| Script creation or modification | `Scripts/README.md` + any existing scripts in `Scripts/Data_Analysis/` |
| Documentation update | The specific document being modified + this file (`AGENTS.md`) |

If you are unsure which category a task falls into, read all files listed above. It is better to over-read than to miss context.

## Project positioning

<!-- One paragraph: what this project is and what it contains. -->

## Directory responsibilities

<!-- One paragraph per top-level folder. -->

## Data type → directory mapping

| Data type | Primary directory |
|-----------|------------------|
| <!-- fill in --> | `Experimental_Data/01_...` |

## Work principles

1. **Read before write.** Never create, modify, or move a file without first reading the existing content of related files. Check for duplicates, existing batches, and naming conventions already in use.
2. Do not delete, overwrite, or rename raw data without explicit user instruction.
3. Do not copy the same raw data batch to multiple directories as "primary copies".
4. When adding data, establish a clear batch directory and index entry — do not create empty folders speculatively.
5. All processed results must trace back to a `Raw_Data/` path; document this in `Analysis/<Batch_ID>/README.md`.
6. Record failed experiments with root cause — "failed, will retry" is not acceptable.
7. **Keep documentation current.** When you notice that any README, index, or AGENTS.md is out of sync with the actual directory structure, update it as part of the current task. Do not leave stale documentation for the next agent.
8. When actual directories conflict with older documentation, this file and `Experimental_Data/README.md` take precedence — but after resolving the conflict, update the outdated document to match reality.

## Raw / Processed / Figures / Analysis rules

| Directory | Contents |
|-----------|----------|
| `Raw_Data/` | Instrument exports only: CSV/DAT/TXT, unmodified images |
| `Processed_Data/` | Denoised, reconstructed, fitted, corrected, unit-converted files |
| `Figures/` | Report figures, paper figures, comparison and diagnostic plots |
| `Analysis/` | Scripts (.py, .ipynb), parameter tables, fit logs, analysis README |

**Do not place these in `Raw_Data/`:** files containing `denoised`, `reconstructed`, `fit`, `fitted`, `corrected`, `processed`, `SS`, `onoff` in the name.

## How to add a new experiment log

1. **Read** `Experimental_Data/06_Daily_Experiment_Log/README.md` and the template.
2. **Read** `Experiment_Index.csv` to check the latest Experiment_ID and avoid duplicates.
3. Determine the experiment date.
4. Confirm the monthly directory exists: `06_Daily_Experiment_Log/YYYY-MM/`
5. Copy the template; name it: `YYYYMMDD_ExperimentShortName_Exp<N>.md`
6. Fill in all required fields.
7. Add a row to `00_Index/Experiment_Index.csv`.
8. If new samples are involved, update `Sample_Index.csv`.
9. If device-level tracking applies, update `Device_Index.csv`.

## How to add a new data batch

1. **Read** the `README.md` in the target scheme directory to see existing batches.
2. **Read** `Experiment_Index.csv` to check for existing Batch_IDs.
3. Determine the primary directory from the data-type mapping table above.
4. Choose a Batch ID: `YYYYMMDD_Process_LayerOrPurpose`
5. Create `Raw_Data/<Batch_ID>/` and place instrument exports there.
6. Create `Processed_Data/<Batch_ID>/`, `Figures/<Batch_ID>/`, `Analysis/<Batch_ID>/` as needed.
7. Add a row to `Experiment_Index.csv`.

## How to record data processing

1. **Read** `Analysis/<Batch_ID>/README.md` (if it exists) to understand prior processing.
2. Locate the primary data directory and raw data path.
3. Do not modify any file in `Raw_Data/`.
4. Write processed data to `Processed_Data/<Batch_ID>/`.
5. Write figures to `Figures/<Batch_ID>/`.
6. Write scripts and parameter tables to `Analysis/<Batch_ID>/`.
7. Write or update `Analysis/<Batch_ID>/README.md`: processing date, method, input paths, output paths, key parameters, excluded data, conclusions, open questions.
8. Update `Experiment_Index.csv` or `Device_Index.csv` with result summaries.

## How to update documentation

When you discover that documentation is out of date:

1. **Identify the discrepancy** — what does the document say vs. what actually exists.
2. **Update the document** to match the current state of the project.
3. **If the conflict involves AGENTS.md or Experimental_Data/README.md**, those files are authoritative — update the other document to match them.
4. **If AGENTS.md itself is outdated** (e.g., a new directory category was added but not listed in the data-type mapping), update AGENTS.md with the user's confirmation.
5. After updating, briefly note what changed and why (in the conversation, not in the file).

Documents that should be kept current:
- `AGENTS.md` — this file (data-type mapping, directory list)
- `Experimental_Data/README.md` — directory tree, batch table
- `DataManagement_Overview.md` — top-level tree, registered batches
- `00_Index/*.csv` — experiment/sample/device indexes
- Each scheme's `README.md` — active batch table

## Index field conventions

### Experiment_Index.csv

| Field | Description |
|-------|-------------|
| `Date` | YYYY-MM-DD |
| `Experiment_ID` | Unique ID, e.g. `Exp-<Topic>-<N>` |
| `Batch_ID` | Follows `YYYYMMDD_Process_LayerOrPurpose` |
| `Scheme` | Which research scheme |
| `Experiment_Type` | Fabrication / Characterization / Device Testing / Process Development |
| `Main_Data_Dir` | Relative path to primary data directory |
| `Daily_Log` | Relative path to experiment log file |
| `Status` | Planned / Running / Completed / Failed / Completed with issues / Needs review |
| `Key_Result` | One-sentence finding |
| `Next_Action` | One-sentence next step |

## Writing style for logs

Write facts first, then judgments.

**Good:**
> Observation: No suspended bridge structure seen under SEM; local areas show possible resist residue.  
> Judgment: Release failed. Uncertainties: ALD film quality and capillary collapse during wet release.  
> Next step: Switch to stable ALD tool; use SiNx hard mask; evaluate supercritical drying.

**Not acceptable:** "Experiment failed, will retry."

## When uncertain

1. Do not move raw data speculatively.
2. Document the uncertainty in a README or `Analysis/<Batch_ID>/README.md`.
3. Set `Status` to `Needs review` in the index.
4. Ask the user before any irreversible reorganization.

## Prohibited actions (unless user explicitly instructs)

- Delete raw data files
- Overwrite raw CSV or instrument export files
- Batch-rename raw data files
- Place processed files in `Raw_Data/`
- Copy raw data to multiple primary directories
- Create large numbers of empty directories "for completeness"
- Edit files in `Docs/` to make them appear to be current rules
- **Create or modify files without first reading related existing files**

## Pre-task checklist

Before completing any task, verify:

- [ ] Completed the mandatory first step (scanned tree + read required files)
- [ ] Read task-specific files from the routing table
- [ ] No raw data was modified
- [ ] New experiments have a log entry
- [ ] New batches have an index entry
- [ ] Processed results are separated from raw data
- [ ] Figures in `Figures/`, scripts in `Analysis/`
- [ ] Relative paths used throughout
- [ ] Failures and anomalies documented, not omitted
- [ ] Documentation updated if any discrepancies were found
