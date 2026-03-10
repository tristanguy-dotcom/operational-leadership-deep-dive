# Financial Deep Dive QC

Evaluates a Financial Deep Dive against quality standards. Checks structure, 5-Why rigor, evidence quality, conciseness, cognitive bias, and action plan specificity. Produces a scored QC report with actionable fixes.

---

## Before Starting

Read the Deep Dive framework to understand what a good analysis looks like:
- `../financial-deep-dive/references/deep-dive-framework.md` — Root cause quality standards, 5-Why rules, good/bad examples
- `../financial-deep-dive/references/operations-overview.md` — Crossover operations model: Central Factory, Operations Playbook, antipatterns
- `../financial-deep-dive/templates/deep-dive-report.md` — Expected output format

---

## Input

Accept the Deep Dive via:
- **File path** — read the markdown file (typically `../../analysis/deep-dive-report.md`)
- **Paste** — user pastes the report text

If the report is missing, ask the user for it. QC cannot proceed without the document.

**Recommended**: Also read the CSV data files in `../../data/` to spot-check claims in the report.

---

## Scoring

Each check is scored on a 5-point scale. The overall score is the average across all checks, mapped to a status.

| Level | Score | Meaning |
|-------|-------|---------|
| NY (Not Yet) | 1 | Fundamental problems — section missing, no data, or actively wrong |
| P (Progressing) | 2 | Present but shallow — needs significant rework |
| M (Meeting) | 3 | Meets baseline — acceptable with minor fixes |
| S (Strong) | 4 | Above standard — clear, specific, well-supported |
| E (Exemplary) | 5 | Exceptional — could serve as a reference example |

**Overall status**:
- **PASS** (avg ≥ 3.5): Ready for submission, may have minor suggestions
- **CONDITIONAL PASS** (avg 2.5–3.4): Needs targeted fixes before submission
- **FAIL** (avg < 2.5): Needs substantial rework

---

## Hard Fails

Any of these is an automatic FAIL regardless of score. Check these first.

| # | Hard Fail | What to look for |
|---|-----------|-----------------|
| HF1 | **Fabricated evidence** | Claims cite numbers that don't match the CSV data, or the report presents fabricated internal documents (meeting notes, surveys, process docs, audit reports, internal dashboards, employee feedback) as pre-existing evidence. Inferences from P&L data clearly labeled as inferences are fine. Fabricated "internal sources" are not. |
| HF2 | **Root cause is a symptom** | Final root cause restates the problem ("costs are too high") instead of explaining why. Apply the "so what?" test. |
| HF3 | **No evidence in any Why** | An entire Why level has no dollar amounts, percentages, or specific facts. |
| HF4 | **Action plan is "plan to have a plan"** | Any fix that only says "audit" / "investigate" / "evaluate" without stating what changes. No Day-0/Month-1 actions or concrete milestones. "TBD" or "to be defined" in any section. Flag individually; if ALL fixes are investigatory, Hard Fail. |
| HF5 | **Customer impact ignored** | A cost-reduction fix could meaningfully degrade SLAs, CSAT, or feature availability, and the report includes no mitigation or customer-impact consideration. |
| HF6 | **No AI/automation opportunities** | Action plan contains zero AI or automation opportunities connected to specific P&L lines. Every deep dive must identify at least one. |

---

## Quality Checks

### 1. Structure Completeness

Does the Deep Dive contain all required sections with appropriate content?

| Required Section | What to check |
|-----------------|---------------|
| Header (Company, Analyst, Date) | All three present |
| Executive Summary | Financial situation: 1-2 sentences (revenue, margin, gap). Root cause summary: 1-2 sentences, plain language. |
| Root Cause Analysis (5-Why) | At least 3 Why levels. Each has Question, Answer (1-2 sentences), Evidence (key numbers only), Source. |
| Root Cause Description | 2-4 sentences total connecting the chain to the margin gap. Not a rehash of the Whys. |
| Action Plan | Table with Fix, Expected Impact ($), Status, Owner. At least one fix. |
| Data Limitations | Present (even if "none"). |

**Scoring**:
- NY: Missing 2+ required sections
- P: Missing 1 section or major content gaps
- M: All sections present with adequate content
- S: All sections well-populated, no extra bloat
- E: Every section is precise, complete, and nothing unnecessary

### 2. 5-Why Rigor

Is the causal chain logical, deep enough, and properly evidenced?

| Check | Pass | Fail |
|-------|------|------|
| **Logical progression** | Each Why follows from the previous answer | Leaps between levels |
| **No circular reasoning** | Chain moves to deeper causes | Why 3 answer restates Why 1 |
| **Depth** | Reaches an actionable root cause | Stops at surface observation |
| **Each Why has evidence** | Every Answer cites specific $ amounts or percentages | Any Why has vague evidence |
| **Sources named** | Every Why identifies the data file(s) used | Source says "data analysis" |
| **Branching justified** | Branches represent genuinely independent cost drivers | Branches are variations of the same point |

**Scoring**:
- NY: Circular reasoning or skips multiple levels
- P: 3 levels but evidence is spotty
- M: Reaches a root cause with evidence at each level
- S: Explores alternatives, rules them out with data
- E: Multi-branch with comparative data, definitive conclusion

### 3. Root Cause Quality

Does the final root cause meet the quality bar?

| Test | Question |
|------|----------|
| **Specific** | Does it name the exact cost driver with $ amount? |
| **Data-backed** | Does it cite numbers from the CSV data? |
| **Actionable** | Does it point to something that can be changed? |
| **Not a symptom** | Can you ask "why?" and get a ready answer? If yes, not deep enough. |
| **Causal** | Does it explain the mechanism, not just correlation? |

**Scoring**:
- NY: Root cause is generic / could apply to any company
- P: Specific to this company but missing data or mechanism
- M: Passes all five tests
- S: Passes all tests with strong specificity and clear mechanism
- E: Immediately actionable, cites precise data, explains mechanism

### 4. Evidence & Data Quality

Are claims supported by real, traceable data?

| Check | What to look for |
|-------|-----------------|
| **Metrics are specific** | "$8.1M in hosting costs (10.2% of revenue)" not "hosting costs are high" |
| **Sources are data files** | "pl-summary.csv", "opex-non-employee.csv" — not "data analysis" |
| **Comparisons to benchmarks** | "20% vs 4.5% benchmark" — not just raw numbers |
| **Calculations verifiable** | Reader can reproduce the numbers from the CSV files |
| **No unsupported claims** | Every assertion has evidence |

**Spot-check**: Pick 2-3 key claims and verify against the CSV data. Flag any that don't match.

**External research check**: At least one Why should cite an external benchmark or industry comparison. Flag if the entire analysis uses only the provided CSVs.

**Scoring**:
- NY: Most claims lack specific numbers or sources
- P: Some metrics cited but inconsistent
- M: Every Why has at least one specific metric with source
- S: Strong quantitative evidence throughout with benchmark comparisons
- E: Every claim traceable, external benchmarks cited

### 5. Conciseness & Clarity

| Check | Pass | Fail |
|-------|------|------|
| **Section lengths** | Why Answers 1-2 sentences, Evidence is key numbers only, Root Cause Description 2-4 sentences | Any section exceeds target |
| **No restating** | Each fact appears once, at most twice | Same metric repeated 3+ times |
| **No filler** | Every sentence adds information | Hedging, methodology explanations, padding |
| **Professional tone** | Direct, objective | Blame, excessive hedging, narrative essay style |

**Target: 700-1200 words.** Under 700 risks insufficient depth. Over 1500 has bloat. Over 2000 is automatic score of 2 or lower.

**Scoring**:
- NY: Over 2000 words or stream of consciousness
- P: Over 1500 words with identifiable bloat, or under 500 with insufficient depth
- M: 1000-1500 words, minor trimming needed
- S: 700-1200 words, tight and focused with full evidence
- E: 900-1200 words with complete analytical depth, nothing wasted

### 6. Action Plan Quality

| Check | Pass | Fail |
|-------|------|------|
| **Fixes are specific** | "$X savings from [specific action]" | "Reduce costs" |
| **Expected impact quantified** | Single dollar amount and margin point improvement | Ranges ("$2-3M") or no impact estimate |
| **Status and owner** | Real status with named owner/role | Status blank or "TBD" |
| **Ranked by impact** | Highest-impact fix first | Random ordering |
| **Total impact stated** | Bottom row shows current margin → post-fix margin → remaining gap | No summary |
| **Structural transformation** | At least one fix proposes changing how work gets done (consolidation, automation, restructuring) with side-by-side cost comparison | All fixes are incremental trims |
| **AI/automation opportunity** | At least one fix maps AI to a specific P&L line with estimated impact | No AI/automation in action plan |
| **Customer impact addressed** | Cost-reduction fixes note customer risk and mitigation | Ignores service quality implications |
| **Timeline concrete** | Day-0/Month-1 actions with milestones | Vague timelines ("6-12 months") or no timeline |

**Scoring**:
- NY: No fixes, or all fixes are "investigate"
- P: Fixes exist but vague, unquantified, or missing customer impact/timeline
- M: Fixes are specific with $ impacts, basic timeline, customer impact noted
- S: Fixes are specific, ranked, with structural transformation, AI opportunities, clear owners, Day-0 actions
- E: Immediately implementable: structural transformation with cost comparison, AI mapped to P&L lines, customer mitigation plan, phased roadmap with milestones, total impact shows path to benchmark

### 7. Cognitive Bias Check

| Bias | Red flag |
|------|----------|
| **Anchoring** | First cost area examined dominates the entire analysis. Other areas not checked. |
| **Confirmation** | Analysis only pursued evidence supporting one hypothesis |
| **Narrative** | Multiple independent cost problems forced into a single story |
| **Round number** | Treating benchmark as exact (5.1% vs 5.0% flagged as a problem) |
| **Attribution** | Root cause blames a department instead of a structural issue |

**Scoring**:
- NY: Clear bias driving the analysis
- P: Signs of bias but some data support
- M: No obvious bias; multiple angles considered
- S: Explicitly considered alternatives with data
- E: Actively sought contradicting evidence

---

## QC Report Format

```markdown
# Financial Deep Dive QC Report

**Report**: [Company Name] Financial Deep Dive
**QC Date**: [Today]

---

## Status: [PASS / CONDITIONAL PASS / FAIL]

**Score**: [X.X / 5.0]

[1-2 sentence summary of quality and what needs attention.]

---

## Hard Fail Checks

| # | Check | Result |
|---|-------|--------|
| HF1 | Fabricated evidence | PASS / FAIL |
| HF2 | Root cause is a symptom | PASS / FAIL |
| HF3 | No evidence in any Why | PASS / FAIL |
| HF4 | Plan to have a plan | PASS / FAIL |
| HF5 | Customer impact ignored | PASS / FAIL |
| HF6 | No AI/automation opportunities | PASS / FAIL |

---

## Scored Checks

| # | Check | Score | Notes |
|---|-------|-------|-------|
| 1 | Structure Completeness | [1-5] | [Key finding] |
| 2 | 5-Why Rigor | [1-5] | [Key finding] |
| 3 | Root Cause Quality | [1-5] | [Key finding] |
| 4 | Evidence & Data Quality | [1-5] | [Key finding] |
| 5 | Conciseness & Clarity | [1-5] | [Key finding] |
| 6 | Action Plan Quality | [1-5] | [Key finding] |
| 7 | Cognitive Bias Check | [1-5] | [Key finding] |

**Average**: [X.X / 5.0]

---

## Issues

### Critical (must fix)
1. [Issue with specific location in the report]

### Important (should fix)
1. [Issue]

### Minor (consider fixing)
1. [Issue]

---

## Suggested Rewrites

[For Critical/Important issues, provide specific rewrite — show what the fixed text should look like.]
```

---

**End of Skill**
