# Project Instructions

## Context

This repository contains financial data and analysis tools for an operational leadership exercise. The company is a B2B software company with ~$79M in revenue and significant margin challenges (8.9% actual vs 70% benchmark).

## Data Location

All financial data is in `data/` as CSV files. Read these files directly — do not ask the user for data that's already available in the CSVs.

## Skills Available

- **financial-deep-dive** — Creates a structured 5-Why root cause analysis of the company's financial performance
- **financial-deep-dive-qc** — Quality-checks a completed Financial Deep Dive

## Working Files

- Save investigation logs to `analysis/investigation_log.md`
- Save completed reports to `analysis/deep-dive-report.md`
- A completed analysis is already provided in `analysis/` — read it before starting new work

## Analysis Standards

- Every claim must cite specific data from the CSV files
- Use dollar amounts and percentages, not vague statements
- Compare costs to benchmarks in `benchmarks.csv`
- Keep reports concise: 500-1000 words for the Deep Dive report
- Action plan fixes must include expected $ impact
