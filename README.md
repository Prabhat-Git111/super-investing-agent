# Super Investing — AI Innovator Research Agent

An evidence-first AI research agent for the Super Investing Part B assignment.

## What it does

Input:
- NSE ticker (default: `SRVCABLE`)
- A folder of company documents

Pipeline:

```text
Research documents
      ↓
Document parser + source metadata
      ↓
PASS 1: evidence extraction
      ↓
Evidence JSON with source IDs
      ↓
PASS 2: grounded report generation
      ↓
Cited Markdown research brief
```

The important design choice is **evidence first**: the final report is generated from an intermediate evidence set rather than asking the model to summarize the raw documents directly. This makes source tracing and conflict detection easier.


## Model

Default: **Gemini 3.5 Flash Lite**.

Why:
- fast enough for a small research workflow;
- suitable for document summarization and structured extraction;
- lightweight for an assignment-sized workload;
- supports the same API pattern for both evidence extraction and report generation.

## Setup — Windows PowerShell

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

Open `.env` and add your Gemini API key.

If PowerShell blocks activation:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\venv\Scripts\Activate.ps1
```

## Run

```powershell
python agent.py --ticker SRVCABLE --company "Sarvottam Cables Ltd"
```

The generated files appear in `outputs/`:
- `evidence.json` — intermediate evidence layer
- `srvcable_research_brief.md` — final brief

## Grounding and safety against prompt injection

The research pack is treated as scraped web content. One source contains an instruction aimed at AI summarisation tools. The agent
explicitly  treats source text as data, not instructions, and the final pass is restricted to the extracted evidence layer.

## Known limitation

This version is intentionally focused on the required test case and does not browse live NSE/news data. A future version could add a live-data tool, but live data is not required for the assignment.

## Test case

The supplied pack contains the Sarvottam Cables fictional-company materials from the assignment. The source files should remain unchanged so that outputs are reproducible.

## Evaluation

The agent was tested in three iterative runs:

1. Baseline evidence-first prompt.
2. Added explicit handling for conflicting numerical claims.
3. Added retail-investor prioritization for financially material information.

The outputs are preserved in `outputs/run1_*`, `outputs/run2_*`, and `outputs/run3_*`, with the corresponding experiment documented in `test_log.md`.
