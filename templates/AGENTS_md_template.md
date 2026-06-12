# AGENTS.md

> **This file gives AI agents operating instructions for this project.**  
> Read it before any task. It takes precedence over all other documentation.

## Project positioning

<!-- One paragraph: what this project is and what it contains. -->

## Mandatory reading

Before starting any task, read:

1. `DataManagement_Overview.md`
2. `Experimental_Data/README.md`
3. `Experimental_Data/00_Index/README.md`

For experiment log tasks, also read:
- `Experimental_Data/06_Daily_Experiment_Log/README.md`
- `Experimental_Data/06_Daily_Experiment_Log/ExperimentLog_Template.md`

For existing data batches, also read the README nearest that batch.

## Directory responsibilities

<!-- One paragraph per top-level folder. -->

## Data type → directory mapping

| Data type | Primary directory |
|-----------|------------------|
| <!-- fill in --> | `Experimental_Data/01_...` |

## Work principles

1. Do not delete, overwrite, or rename raw data without explicit user instruction.
2. Do not copy the same raw data batch to multiple directories as "primary copies".
3. When adding data, establish a clear batch directory and index entry — do not create empty folders speculatively.
4. All processed results must trace back to a `Raw_Data/` path; document this in `Analysis/<Batch_ID>/README.md`.
5. Record failed experiments with root cause — "failed, will retry" is not acceptable.
6. When actual directories conflict with older documentation, this file and `Experimental_Data/README.md` take precedence.

## Raw / Processed / Figures / Analysis rules

| Directory | Contents |
|-----------|----------|
| `Raw_Data/` | Instrument exports only: CSV/DAT/TXT, unmodified images |
| `Processed_Data/` | Denoised, reconstructed, fitted, corrected, unit-converted files |
| `Figures/` | Report figures, paper figures, comparison and diagnostic plots |
| `Analysis/` | Scripts (.py, .ipynb), parameter tables, fit logs, analysis README |

**Do not place these in `Raw_Data/`:** files containing `denoised`, `reconstructed`, `fit`, `fitted`, `corrected`, `processed`, `SS`, `onoff` in the name.

## How to add a new experiment log

1. Determine the experiment date.
2. Confirm the monthly directory exists: `06_Daily_Experiment_Log/YYYY-MM/`
3. Copy the template; name it: `YYYYMMDD_ExperimentShortName_Exp<N>.md`
4. Fill in all required fields.
5. Add a row to `00_Index/Experiment_Index.csv`.
6. If new samples are involved, update `Sample_Index.csv`.
7. If device-level tracking applies, update `Device_Index.csv`.

## How to add a new data batch

1. Determine the primary directory from the data-type mapping table above.
2. Choose a Batch ID: `YYYYMMDD_Process_LayerOrPurpose`
3. Create `Raw_Data/<Batch_ID>/` and place instrument exports there.
4. Create `Processed_Data/<Batch_ID>/`, `Figures/<Batch_ID>/`, `Analysis/<Batch_ID>/` as needed.
5. Add a row to `Experiment_Index.csv`.

## How to record data processing

1. Locate the primary data directory and raw data path.
2. Do not modify any file in `Raw_Data/`.
3. Write processed data to `Processed_Data/<Batch_ID>/`.
4. Write figures to `Figures/<Batch_ID>/`.
5. Write scripts and parameter tables to `Analysis/<Batch_ID>/`.
6. Write `Analysis/<Batch_ID>/README.md`: processing date, method, input paths, output paths, key parameters, excluded data, conclusions, open questions.
7. Update `Experiment_Index.csv` or `Device_Index.csv` with result summaries.

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

## Pre-task checklist

Before completing any task, verify:

- [ ] Read `AGENTS.md` and relevant `README.md` files
- [ ] No raw data was modified
- [ ] New experiments have a log entry
- [ ] New batches have an index entry
- [ ] Processed results are separated from raw data
- [ ] Figures in `Figures/`, scripts in `Analysis/`
- [ ] Relative paths used throughout
- [ ] Failures and anomalies documented, not omitted
