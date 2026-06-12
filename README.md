# lab-data-manager

**[中文文档](README_zh.md)**

A structured, AI-friendly data management system for experimental research projects. Works with any LLM agent (Qoder, Qoderwork, Claude, GPT, Gemini, Cursor, etc.) — the instructions are plain Markdown that any agent can follow.

## What this does

An AI agent reads `INSTRUCTIONS.md` and guides you through creating a complete research project folder from scratch, including:

- Clear separation of **raw vs. processed data** (raw data is never modified)
- **CSV indexes** for experiments, samples, and devices — fast lookup across batches
- **Daily experiment logs** with a standard template
- **4-layer per-batch structure**: `Raw_Data/` → `Processed_Data/` → `Figures/` → `Analysis/`
- An `AGENTS.md` file so any future AI agent can operate the project correctly
- Adapted to **any experimental science**: nanofabrication, chemistry, biology, materials science, physics, etc.

## Quick start

### With Qoder / Qoderwork

Install the ready-made skill from `skills/research-project-setup/SKILL.md`:

1. Clone or download this repository.
2. Place `SKILL.md` into one of:
   - **Project-level:** `<project-root>/.qoder/skills/research-project-setup/SKILL.md`
   - **User-level:** `~/.qoder/skills/research-project-setup/SKILL.md`
3. Restart or reload Qoder.
4. Say: `"Set up a research project"` — the skill triggers automatically.

### With Claude Code

1. Place `skills/research-project-setup/SKILL.md` into `.claude/skills/research-project-setup.md` in your project.
2. Say: `"Set up a research project"` — the skill triggers automatically.

### With Claude Cowork

1. Settings → Capabilities → Install skill → select `skills/research-project-setup/SKILL.md`.
2. Say: `"Set up a research project"`.

### With any LLM agent

Paste the contents of [`INSTRUCTIONS.md`](./INSTRUCTIONS.md) into your agent's context (or upload the file), then say:

> "Follow these instructions to set up a research project for me."

The agent will ask you questions about your project, design the folder structure, confirm it with you, and then create all the files.

## Project structure created

```
YourProject/
├── AGENTS.md                    # AI operation rules for this project
├── .agent-context.md            # Condensed quick-reference for any AI agent
├── README.md                    # Project overview
├── DataManagement_Overview.md   # Data system entry point
├── Experimental_Data/
│   ├── 00_Index/                # Experiment / Sample / Device CSV indexes
│   ├── 01_<Scheme1>/            # First research scheme (raw → processed → figures → analysis)
│   ├── 02_<Scheme2>/            # Second scheme, if applicable
│   ├── 03_Characterization/     # Microscopy, spectroscopy, diffraction, etc.
│   ├── 04_Device_Testing/       # Cross-batch summaries (no raw data copies here)
│   ├── 05_Process_Development/  # Process optimization experiments
│   └── 06_Daily_Experiment_Log/ # Per-month experiment logs
├── Scripts/Data_Analysis/       # Plotting and analysis script templates
├── Progress/                    # Progress reports
└── PPT/                         # Presentation materials
```

Directory numbers adapt to your project — only categories you actually need get created.

## Core principles

1. **Raw data is immutable.** Files in `Raw_Data/` are never modified, renamed, or deleted.
2. **One primary location per batch.** No duplicating raw data across directories.
3. **ASCII filenames only.** No Unicode or special characters in file/directory names.
4. **Traceability.** Every processed result links back to its source `Raw_Data/` path.
5. **Log failures with root cause.** "Failed, will retry" is not acceptable documentation.
6. **Use relative paths** throughout — supports cross-device sync (Synology, OneDrive, etc.).

## Templates

The `templates/` directory contains ready-to-use Markdown and CSV templates that the agent fills in with your project details:

| File | Purpose |
|------|---------|
| `AGENTS_md_template.md` | AI agent operation rules |
| `ExperimentLog_Template.md` | Daily experiment log |
| `Experiment_Index_header.csv` | Experiment index CSV header |
| `Sample_Index_header.csv` | Sample tracking CSV header |
| `Device_Index_header.csv` | Device tracking CSV header |

## Examples

See `examples/gaafet_project_structure.md` for a real-world example: a 3D-stacked GAAFET semiconductor research project with two fabrication schemes, multiple characterization methods, and device electrical testing.

## Repository structure

```
lab-data-manager/
├── INSTRUCTIONS.md              # Full agent instructions (platform-agnostic)
├── skills/
│   └── research-project-setup/
│       └── SKILL.md             # Installable skill for Qoder / Claude / etc.
├── templates/                   # Reusable file templates
├── examples/                    # Real-world project examples
└── integrations/
    └── claude/
        └── SKILL.md             # Legacy redirect → skills/research-project-setup/
```

## License

MIT
