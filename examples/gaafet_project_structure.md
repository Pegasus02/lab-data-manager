# Example: 3D-Stacked GAAFET Research Project

This example shows what a real project looks like after running `INSTRUCTIONS.md`.

**Project:** Three-dimensional stacked Gate-All-Around FET using MoS₂ 2D materials  
**Domain:** Semiconductor nanofabrication  
**Schemes:** 2 parallel fabrication routes  
**Language:** Mixed (Chinese for daily logs, English for filenames and indexes)

---

## Directory structure

```
三维堆叠GAAFET/
├── AGENTS.md
├── CLAUDE.md
├── README.md
├── 实验数据管理系统总览.md
├── Experimental_Data/
│   ├── README.md
│   ├── 00_Index/
│   │   ├── Experiment_Index.csv
│   │   ├── Sample_Index.csv
│   │   └── Device_Index.csv
│   ├── 01_ALD_AlOx_Scheme/              # Scheme 1 (70% resources): ALD-AlOx gate dielectric
│   │   ├── Ozone_Treatment_Optimization/
│   │   │   ├── UV_Ozone/
│   │   │   │   ├── README.md
│   │   │   │   ├── Raw_Data/20260517_UV_Ozone_1L/
│   │   │   │   ├── Processed_Data/20260517_UV_Ozone_1L/
│   │   │   │   └── Figures/20260517_UV_Ozone_1L/
│   │   │   └── ALD_Ozone/
│   │   │       ├── Raw_Data/20260522_ALD_Ozone_1L/
│   │   │       └── Raw_Data/20260522_ALD_Ozone_2L/
│   │   ├── GAAFET_Stack_Devices/
│   │   └── Edge_Contact_Improvement/
│   ├── 02_AlAu_Alloy_Scheme/            # Scheme 2 (30% resources): Al-Au alloy IMI structure
│   ├── 03_Material_Characterization/    # AFM, Raman, PL, XPS, TEM, SEM, EDS
│   ├── 04_Device_Testing/               # Cross-batch electrical summaries only
│   ├── 05_Process_Development/          # IBE, spacer, lateral etch, suspension bridge
│   │   ├── IBE_Etch_Profile/
│   │   └── Gate_Edge_Isolation/
│   └── 06_Daily_Experiment_Log/
│       ├── README.md
│       ├── 实验记录模板.md
│       ├── 2026-05/
│       │   ├── 20260517_UV_Ozone_Exp1.md
│       │   ├── 20260522_ALD_Ozone_Exp1.md
│       │   └── 20260527_Suspend_Bridge_Exp1.md
│       └── 2026-06/
│           ├── 20260601_Suspend_Bridge_Exp2.md
│           ├── 20260603_GAAFET_Stack_Exp32_33.md
│           └── 20260605_IBE_Etch_Profile_AFM.md
├── Scripts/
│   └── Data_Analysis/
│       └── templates/
│           ├── transfer_curve_template.py
│           └── multiple_devices_template.py
├── Progress/
└── PPT/
```

---

## Experiment_Index.csv (excerpt)

```csv
Date,Experiment_ID,Batch_ID,Scheme,Experiment_Type,Main_Data_Dir,Daily_Log,Status,Key_Result,Next_Action
2026-05-17,Exp-UV-Ozone-01,20260517_UV_Ozone_1L,01_ALD_AlOx_Scheme,Device testing / ozone treatment,Experimental_Data/01_ALD_AlOx_Scheme/Ozone_Treatment_Optimization/UV_Ozone,,Completed,Single-layer MoS2 device data; s2 noisy,Use as baseline for UV-O3 damage reference
2026-05-27,Exp-Suspend-01,20260527_Suspend_Bridge_Exp1,05_Process_Development,Process verification,Experimental_Data/05_Process_Development,Experimental_Data/06_Daily_Experiment_Log/2026-05/20260527_Suspend_Bridge_Exp1.md,Failed,AlOx suspended bridge not observed; ALD instability + drying collapse,Retry with stable ALD/HfOx + SiNx hard mask + CPD
2026-06-05,Exp-IBE-AFM-01,20260605_IBE_Dummy_AFM,05_Process_Development,Process verification — IBE etch profile (AFM),Experimental_Data/05_Process_Development/IBE_Etch_Profile,Experimental_Data/06_Daily_Experiment_Log/2026-06/20260605_IBE_Etch_Profile_AFM.md,Completed,IBE etch step profile ~90° vertical; likely cause of edge contact failure,Optimize IBE for sloped profile; evaluate tilted IBE
```

---

## Key decisions made during setup

**Why two scheme directories instead of one?**  
The project runs two fabrication approaches in parallel with different resource allocations. Keeping them separate makes it easy to archive or compare results from each route independently.

**Why is `04_Device_Testing/` summaries-only?**  
Device measurement data always originates from a specific fabrication batch (in `01_` or `02_`). Copying it to `04_` would create ambiguity about which copy is authoritative. The index (`Device_Index.csv`) provides cross-batch lookup without duplicating files.

**Why ASCII filenames even for a Chinese-language project?**  
Synology Drive, Git, and most analysis tools handle Unicode paths inconsistently across operating systems. English filenames and directory names eliminate sync errors; Chinese content stays in file bodies where it's safe.

**Example of a correctly recorded failure:**

```markdown
## Analysis and Discussion

Observation: No continuous suspended AlOx bridge observed under SEM. 
Local areas show residue consistent with incomplete resist removal.

Judgment: Release step failed. Three contributing factors identified:
1. ALD tool was later found to have depleted TMA source — film may not have grown.
2. BOE wet etch may have partially dissolved the AlOx layer before release.
3. Capillary forces during air drying likely caused collapse even if film was intact.

Next step: 
- Confirm ALD film growth with a test piece before next attempt.
- Replace BOE with dilute HF timed etch; evaluate supercritical CO2 drying.
- Use SiNx hard mask to protect AlOx during etch steps.
```
