# Operational Leadership — Financial Deep Dive

This repository contains the data and AI-powered analysis tools for conducting a financial deep dive on a software company's P&L.

## What's Here

```
data/                          ← Company financial data (CSV files)
  benchmarks.csv               ← Internal benchmark percentages
  pl-summary.csv               ← High-level P&L summary
  opex-non-employee.csv        ← Non-employee operating expenses
  cogs-non-employee.csv        ← Non-employee cost of goods sold
  employees.csv                ← Employee costs by function/department
  revenue-recurring.csv        ← Recurring revenue by customer
  revenue-pso.csv              ← Professional services revenue by customer
  revenue-perpetual.csv        ← Perpetual license revenue by customer

.claude/skills/
  financial-deep-dive/         ← Skill to create a Financial Deep Dive analysis
  financial-deep-dive-qc/      ← Skill to quality-check a completed Deep Dive

analysis/                      ← Where analysis outputs go
  deep-dive-report.md          ← Completed Deep Dive (provided)
  investigation_log.md         ← Investigation log (provided)
  deep-dive-qc-report.md       ← QC report (provided)
```

## Getting Started

### 1. Set Up Your AI Environment

This repo is designed to work with Claude Code (or any AI tool that supports the `.claude/skills/` format). Connect your AI tool to this repository so it can access the data files and skills.

### 2. Review the Data

Start by understanding the company's financial situation. The data files in `data/` contain a full P&L breakdown:

- **Start with** `pl-summary.csv` — this gives you the high-level picture
- **Compare to** `benchmarks.csv` — these are internal targets by function
- **Drill into** the expense files (`employees.csv`, `opex-non-employee.csv`, `cogs-non-employee.csv`) to understand where money is going
- **Check** the revenue files to understand the customer base and revenue mix

All amounts are in USD for the 2018 fiscal year.

### 3. Review the Existing Analysis

A completed Financial Deep Dive and investigation log are provided in `analysis/`. Review these to understand:
- What root causes were identified
- What action items were proposed
- What data was used and what limitations exist
- Whether the analysis is thorough and well-supported

### 4. Use the Skills

- **`/financial-deep-dive`** — Run your own analysis or extend the existing one
- **`/financial-deep-dive-qc`** — Quality-check the existing Deep Dive (or your own)

## Data Notes

- Employee names are anonymized (shown as empty/null)
- All amounts are annual figures in USD
- Revenue files show individual customer-level data
- Expense files show line-item detail including vendor names
- Benchmarks represent internal cost targets as % of revenue
