# Financial Deep Dive

Structured root cause analysis for company financial performance. Classifies the Most Important Problem (Margin vs Growth), explores the P&L top-down, applies the 5-Why framework to find specific data-backed root causes, and produces a clear action plan.

---

## Before Starting

Read these reference files:
- `references/deep-dive-framework.md` — 5-Why methodology, MIP classification, fix generation, operational transformation framework, AI/automation identification, customer impact assessment, cognitive bias guards
- `references/operations-overview.md` — Crossover operations model: Central Factory vs edge, Operations Playbook (Simplify → Central → CI → Staff → AI), role expectations, antipatterns

Also familiarize yourself with the data available:
- `../../data/` — CSV files containing the company's P&L, expenses, employees, and revenue data

---

## Quality Standards (Read Before Any Analysis)

These standards apply to the final report. Internalize them before starting — they should shape how you analyze, not just how you write.

### Hard Rules (Violating Any = Report Fails)

1. **Every claim must trace to data.** If you can't point to a CSV row or an external source, don't say it. Speculation is not evidence.
2. **Root causes must pass the "so what?" test.** If someone can ask "why?" and get a ready answer, you haven't gone deep enough. "Tech debt" is a symptom. "Legacy architecture" is a description. Keep going.
3. **Fixes must state what changes, not what to study.** A fix that begins with "audit," "investigate," "evaluate," "review," or "assess" is not a fix. It's a plan to have a plan.
4. **Numbers must be specific.** "$2.1M savings" not "$2-3M savings." Commit to a number with your reasoning. Ranges signal you haven't done the work.
5. **Fixes must address customer impact.** Any cost reduction that could degrade service quality (SLAs, CSAT, feature availability) needs a mitigation plan. Ignoring customer impact is a hard fail.
6. **AI/automation opportunities are required.** The action plan must connect at least one AI or automation opportunity to a specific P&L line with estimated impact.

### Fact vs Inference

Throughout the analysis, clearly separate what the data shows from what you infer:
- **Data says**: "$8.3M in Cloud Ops COGS, 58 employees at avg $28K" — this is verifiable
- **Inference**: "The 58-person team suggests legacy infrastructure" — this is a hypothesis

In the investigation log, inferences are fine and useful. In the final report, minimize inferences. When you must include one, make it clear it's an inference, not a data point.

### Report Targets

- **700-1200 words** is the target range. Under 700 risks insufficient evidence depth. Over 1500 has bloat. Over 2000 means rework.
- **Each fact appears at most twice** in the entire report.
- **No hedging language**: cut "it's important to note," "this suggests that," "it's worth mentioning," "it appears that."
- **No methodology**: don't explain how you calculated. State the result.

---

## Two Modes

This skill operates in two modes. Ask the user which they prefer if not obvious from context.

### Interactive Mode

Walk through the analysis step-by-step with the user. At each 5-Why level:
1. Present the current evidence and your proposed answer
2. Ask if the direction is correct or if there's context you're missing
3. When the analysis branches, present the options and let the user choose which to pursue (or confirm pursuing both)
4. Confirm the root cause before moving to fixes

**Use when**: User wants to be involved in the analysis, has domain context to contribute, or the company situation is complex.

### Autonomous Mode

Complete the full Deep Dive independently — all data gathering, MIP classification, exploration, 5-Why analysis, and fix generation. Present the finished draft for review.

**Use when**: User wants results, not a walkthrough. They'll review and provide feedback on the finished product.

**In autonomous mode, still pause and ask the user if**:
- Data seems inconsistent and you can't determine the right interpretation
- The analysis branches into 3+ independent root causes and you need to prioritize
- You encounter unusual patterns in the data you haven't seen before

---

## Investigation Log

Maintain a running scratch file throughout the Deep Dive at `../../analysis/investigation_log.md`. This is **not** for stakeholders — it's a working document for you and the user to track what's been explored, what hasn't, and what changed along the way.

### Purpose

- **Prevent rehashing**: Don't re-investigate something already explored and ruled out
- **Track assumptions vs evidence**: Every claim starts as an assumption until data confirms or refutes it. Log both.
- **Preserve branching logic**: The final report is clean and linear. The log captures every branch pursued, including dead ends.
- **Survive context breaks**: If the conversation runs long, the log ensures continuity.

### Structure

```markdown
# Investigation Log: [Company Name] Financial Deep Dive

## Assumptions Register
| # | Assumption | Status | Evidence |
|---|-----------|--------|----------|
| A1 | [Initial assumption] | Confirmed / Refuted / Open | [What data showed] |

## Branches Explored
### [Branch name]
- **Hypothesis**: [What we thought]
- **Data gathered**: [What we looked at]
- **Finding**: [What we found]
- **Disposition**: [Used in report / Ruled out / Needs more data]

## User Context Collected
- [Date]: [What the user shared about the company, industry, expectations]

## External Research
- [Topic]: [Source, key finding, how it was used]

## Dead Ends
- [What was investigated and why it didn't lead anywhere]

## Open Questions
- [Things we couldn't answer with available data]
```

### Rules

- **Update the log as you go**, not at the end. Every data query result, every user insight, every branch decision gets logged.
- **Log assumptions immediately.** When you form an initial hypothesis before data (e.g., "this company probably has a hosting cost problem"), log it as an assumption. Update when data confirms or refutes.
- **Log dead ends.** A branch that was explored and ruled out is valuable — it prevents re-investigation and shows thoroughness.
- **Log user corrections.** When the user says "that's already been addressed" or "that's not how this business works," log both the original assumption and the correction.
- **Update after each round of user feedback.** Corrections, refuted assumptions, scope changes, and rejected fixes are all log-worthy events. Don't let the log fall behind during iterative review.

---

## Phase 1: Input & Scoping

### Understand the Context

Collect from the user:

1. **Company context** — industry, business model, competitive landscape
2. **What triggered the analysis** — a specific concern, board question, acquisition due diligence, etc.
3. **Known context** (optional) — what the user already knows about the financial situation (use as input, not anchor)
4. **Mode preference** — interactive or autonomous

### Load the Data

Read the CSV files from `../../data/`:

| File | Contains |
|------|----------|
| `benchmarks.csv` | Industry benchmark percentages by category |
| `pl-summary.csv` | High-level P&L with revenue and expense line items |
| `opex-non-employee.csv` | Non-employee operating expenses by function, department, vendor |
| `cogs-non-employee.csv` | Non-employee cost of goods sold by function, department, vendor |
| `employees.csv` | Employee costs by function, department, tier |
| `revenue-recurring.csv` | Recurring revenue by customer |
| `revenue-pso.csv` | Professional services revenue by customer |
| `revenue-perpetual.csv` | Perpetual license revenue by customer |

### Initial Scan

Before diving deep, build a quick financial snapshot:

1. **Revenue mix**: What % recurring vs PSO vs perpetual?
2. **Margin**: Actual vs benchmark
3. **Cost structure**: HC vs non-HC, COGS vs OPEX
4. **Top expense areas**: Which functions/departments are the largest?
5. **Concentration risk**: Top customer revenue concentration

Log this snapshot in the Investigation Log.

---

## Phase 2: MIP Classification

**Classify before analyzing.** The Most Important Problem determines where to focus.

### The Rule

Compare actual margin to benchmark margin:

```
If actual_margin < benchmark_margin AND revenue_growth is healthy → MIP = Margin (cost problem)
If actual_margin < benchmark_margin AND revenue is declining → MIP = Both (cost + revenue)
If margin is healthy but revenue declining → MIP = Growth (revenue problem)
```

For this data: Benchmark margin is 70%. If actual margin is significantly below that, the MIP is Margin — the company is spending too much relative to revenue.

### Sub-classification

If MIP = Margin, classify further:
- **HC-driven**: Employee costs are the primary driver
- **Non-HC-driven**: Vendor/contractor/hosting costs are primary
- **Mixed**: Both contribute meaningfully

---

## Phase 3: Exploration (Top-Down)

Diagnose from top to bottom: **P&L Summary → Function → Department → Line Item**. Only drill down when the higher level shows an anomaly.

### For Each Major Cost Area

1. **Compare to benchmark**: Is this function's cost % of revenue above the benchmark?
2. **Break down HC vs Non-HC**: Which is the driver?
3. **Identify top departments**: Which departments within the function are largest?
4. **Examine vendor concentration**: Are there large single-vendor costs?
5. **Look for anomalies**: Unusual patterns, duplications, categories that seem misallocated

### Revenue Analysis

1. **Customer concentration**: Top 10 customers as % of total revenue
2. **Revenue type mix**: Recurring health vs PSO dependency
3. **Long tail**: How many customers contribute the bottom 20%?

### At Each Level

Classify the finding:
- **Above benchmark significantly** → Drill deeper to find specific drivers
- **At or near benchmark** → No issue, move on
- **Below benchmark** → Potential strength or under-investment (note for context)

### ✓ Checkpoint: Before Proceeding to 5-Why

Before starting the 5-Why, verify you can answer these from data (not guesses):

1. Which function has the **largest gap to benchmark in dollars**? What's the number?
2. Within that function, which **department** is the primary driver? What's its cost?
3. Within that department, what's the **HC vs non-HC split**?
4. Is there a **second function** with a material gap? How large compared to #1?

If you can't answer these with specific numbers, you haven't explored deeply enough. Go back and aggregate the data.

---

## Phase 4: 5-Why Analysis

Apply the 5-Why framework per `references/deep-dive-framework.md`. The MIP frames the starting question; the exploration provides the evidence.

**Why 1** always starts: "Why is this company's margin significantly below the internal benchmark?"

Each subsequent Why takes the previous answer as the new problem statement.

### Chain Depth: The Non-Negotiable Rule

**Every chain (or branch) must reach at least 3 levels of "why" from where it starts.** A finding that's only 1-2 levels deep is an observation, not a root cause analysis.

Test your final root cause: **Can someone ask "why?" and get a ready answer?**
- "Why is hosting $8.3M?" → "Because we have legacy architecture" → **Keep going. Why is the architecture legacy?**
- "Why hasn't the architecture been modernized?" → "Because 70% of R&D capacity goes to maintenance" → **Keep going. Why does maintenance consume 70%?**
- "Why does maintenance consume 70%?" → This requires organizational decisions and historical context beyond the data → **Now you've reached a root cause or a data limitation.**

### Branching

Branch when a Why has multiple genuinely independent answers (test: could you fix one without affecting the other?). Label as "Why 2a", "Why 2b", etc.

**Branch rules:**
- **Pick one primary chain** — the one with the largest dollar gap. Develop this to full depth (4-5 levels).
- **Secondary branches** — if genuinely independent, develop each to at least 3 levels. Otherwise, mention them in the Root Cause Description instead of giving them a shallow 1-level Why.
- **Never have a 1-level branch.** "Why 4 (Branch: G&A): G&A is 20% because of outsourcing" is not analysis — it's an observation. Either develop it or put it in Root Cause Description as a contributing factor.

In the final report, converge branches: merge if they lead to the same root cause; keep separate only if they're truly independent.

### External Research (Mandatory)

**At least one Why in the final report must include an external benchmark or data point.** This grounds the analysis in reality beyond the provided data.

Use web search to research:
- **Industry norms**: What's typical for this cost category in this industry/company size?
- **Vendor pricing**: Are vendor costs in line with market rates?
- **Compensation benchmarks**: Are employee costs competitive or inflated?
- **Revenue benchmarks**: What's typical revenue per employee or per customer in this space?

Always cite the specific source and data point. Log all external research in the Investigation Log.

### ✓ Checkpoint: Root Cause Validation

Before moving to fixes, test each root cause against ALL five criteria:

| Test | Question | If it fails |
|------|----------|-------------|
| **Specific** | Does it name the exact cost driver with a $ amount? | Add the specific driver and number |
| **Data-backed** | Does it cite data from the CSVs or external sources? | Ground it in evidence or remove it |
| **Actionable** | Does it point to something that can be changed? | If it's unchangeable (e.g., "the market is expensive"), it's context, not a root cause |
| **Not a symptom** | Can someone ask "why?" and get a ready answer? | Go deeper until the answer requires information beyond the data |
| **Causal** | Does it explain the mechanism, not just correlation? | Add the "because" — what structural factor creates this cost? |

**Common failures to catch:**
- "Legacy architecture" → Why is it legacy? What prevents modernization?
- "Tech debt" → Why has it accumulated? What perpetuates it?
- "Organizational complexity" → What specifically is complex? Why hasn't it been simplified?
- "Outsourcing overhead" → Why is this work outsourced? What would insourcing require?

If a root cause is blocked by data limitations (you can't go deeper with available data), say so explicitly: "Root cause limited by data: the CSVs don't show contract terms / historical spend / organizational history."

---

## Phase 5: Fix Generation & Action Plan

### Generate Broadly (Internal)

Before ranking, brainstorm 8-12 potential fixes. For each, note what root cause it addresses, expected impact ($ and margin points), and effort/timeline.

### Ranking Criteria

1. **Impact on margin** (highest weight)
2. **Directness** to root cause
3. **Effort/risk** (lower is better)
4. **Speed to impact** (faster is better)

### Fix Quality Rules

Every fix must be:
- **Specific**: "Renegotiate hosting contract targeting 30% reduction ($2.5M savings)" not "reduce hosting costs"
- **Quantified with a single number**: "$2.5M savings, +3.2 margin points" — not "$2-3M." Pick the most defensible estimate and commit.
- **Action-first, not study-first**: State what changes, who does it, and the expected result.
- **Owned**: Name the role responsible.

**Banned fix verbs** (these signal "plan to have a plan"):
- ❌ Audit, Investigate, Evaluate, Review, Assess, Explore, Analyze, Examine
- ✅ Consolidate, Reduce, Renegotiate, Eliminate, Migrate, Automate, Restructure, Convert

| Bad (plan to have a plan) | Good (concrete fix) |
|--------------------------|---------------------|
| "Evaluate hosting costs" | "Consolidate hosting to primary cloud provider, targeting 30% cost reduction ($2.5M savings, +3.2 margin points)" |
| "Review staffing levels" | "Reduce Cloud Operations from 58 to 30 headcount through automation of manual infrastructure tasks ($0.8M savings, +1.0 margin points)" |
| "Assess revenue mix" | "Increase recurring revenue pricing 5% on renewal, adding $3.4M at near-100% margin (+4.3 margin points)" |
| "Audit outsourced services" | "Eliminate $1.4M F&A outsourced services by bringing payroll and AP in-house with 2 additional FTEs ($1.2M net savings)" |

### Operational Transformation (The Operations Playbook)

When the root cause points to a structural cost problem, at least one fix must propose a structural transformation following the Operations Playbook sequence (see `references/operations-overview.md`):

1. **Simplify first** — Strip out complexity, legacy processes, and redundancy before proposing any move
2. **Move to Central Factory** — Can this function be delivered by shared services (engineering, support, SaaSOps, F&A)? If yes, propose the move with cost comparison vs current edge delivery
3. **Continuous Improvement** — If the function stays at the edge, rework it so it CAN move to Central next quarter
4. **Upgrade staff** — If edge-delivered, propose upgrading to top-tier talent at competitive rates
5. **Implement AI** — Identify automation opportunities at each step

**Critical**: Don't just say "move to Central." Show:

1. **Which function** and why (based on root cause, largest gap)
2. **Cost comparison**: Current edge cost vs Central Factory rate, side-by-side with specific numbers
3. **Savings calculation**: Show the formula. E.g., "58 FTEs × $28K avg = $1.6M → Central rate for equivalent output = $0.7M, savings = $0.9M"
4. **Simplification required**: What must be simplified before the move? What's the process today vs the process Central needs?
5. **Implementation roadmap**: Day-0, Month-1, Month-3 actions with milestones
6. **Risk mitigation**: What could go wrong (customer impact, knowledge loss, service gaps) and how to prevent it
7. **KPIs**: 2-3 measurable indicators to track post-implementation

**Antipattern: Dumping bad processes on Central.** Simplify the operation FIRST, then move the work. The process needs to be defined, documented, automated if possible, and ready for Central to assume.

### AI/Automation Opportunities

Every action plan must include at least one AI or automation opportunity connected to a specific P&L line. See `references/deep-dive-framework.md` → AI/Automation Opportunity Identification for where to look.

For each AI opportunity:

- **Target**: Which cost line or process it addresses (e.g., "reduce QA COGS via automated testing")
- **Use case**: Specific application, not "use AI" (e.g., "automated regression testing replacing $1.1M in external QA contractors")
- **Estimated impact**: Dollar savings or efficiency gain with reasoning
- **Implementation**: Brief outline — what tool/approach, who owns it, rough timeline

| Bad (generic) | Good (specific) |
|---------------|-----------------|
| "Use AI to improve efficiency" | "Deploy automated infrastructure monitoring to replace 15 of 58 Cloud Ops manual monitoring roles ($420K savings), using CloudWatch + PagerDuty with 3-month rollout" |
| "Implement automation" | "Automate L1 ticket triage with AI classification, reducing Technical Support non-HC costs from $1.4M to $0.9M (+0.6 margin points)" |

### Customer Impact Assessment

Every cost-reduction fix must address what happens to the customer. See `references/deep-dive-framework.md` → Customer Impact Assessment.

| Fix Type | Customer Risk | Required Mitigation |
|----------|--------------|-------------------|
| Headcount reduction | Service degradation, slower response times | Transition plan, automation to maintain SLAs |
| Vendor consolidation | Capability gaps, migration disruption | Phased migration, interim backup |
| Function restructuring | Knowledge loss, process gaps | Knowledge transfer, documentation, parallel run |
| Automation replacement | Quality changes, edge case handling | Testing period, human escalation path |

If a fix could meaningfully impact SLAs, CSAT, or feature availability — and the report doesn't address it — **that's a hard fail.**

### Timeline Requirements

Every fix needs a concrete timeline, not just "6-12 months":

- **Day-0 actions**: What can start immediately with no dependencies?
- **Month-1 milestones**: What's the first measurable progress point?
- **Phase gates**: What must be true before the next step?

| Bad (vague) | Good (concrete) |
|-------------|-----------------|
| "6-12 months" | "Day 0: Freeze new hosting contracts. Month 1: Complete utilization audit. Month 3: Migrate first 3 services." |
| "Short-term" | "Week 1: Identify top 5 unused instances. Week 2: Decommission. Savings begin Month 1." |
| "Recommended" | "Day 0: Issue RFP for consolidated hosting. Month 1: Select vendor. Month 2: Begin migration of non-critical workloads." |

### Total Impact: Show the Path

The bottom line of the Action Plan must show:
1. **Current margin**: X%
2. **Post-fix margin**: Y% (sum of all fix impacts)
3. **Remaining gap to benchmark**: Z points — with a note on whether it's addressable or structural

---

## Phase 6: Generate Report

Write the Deep Dive using `templates/deep-dive-report.md`. The report contains:

1. **Header** — Company, Analyst, Date
2. **Executive Summary** — Financial situation + root cause summary
3. **Root Cause Analysis** — 5-Why chain with Question/Answer/Evidence/Source
4. **Root Cause Description** — Short narrative connecting the chain
5. **Action Plan** — Table of fixes with expected impact, status, and owner
6. **Data Limitations** — Gaps that constrained the analysis

### Output File

Save to: `../../analysis/deep-dive-report.md`

### Writing Standard: Brevity First

The Deep Dive is a decision document, not a research paper. Every sentence must earn its place.

**Rules**:
- **Why Answers**: 1-2 sentences. State the cause and its magnitude.
- **Evidence**: Bullet the key numbers. No narrative wrapping.
- **Root Cause Description**: 2-4 sentences total connecting the chain.
- **Action Plan Fix descriptions**: 1-2 sentences. What changes and expected impact.
- **No restating**: If a fact appeared in Why 2, don't repeat it in Why 3 or the Root Cause Description.
- **Key statistics**: A specific number should appear at most twice in the report.
- **No hedging**: Cut "it's important to note", "this suggests that", "it's worth mentioning".
- **No methodology**: Don't explain how you calculated things. State the result.

**Target: 500-700 words.** Over 1000 means rework.

### ✓ Pre-Submit Checklist

Run this before delivering the report. Every item must pass.

**Hard fails (any one = rewrite needed):**
- [ ] Pick 3 claims and verify the numbers against CSV data
- [ ] Read each root cause and ask "why?" — if you have a ready answer, it's not deep enough
- [ ] Check every fix verb — does any fix start with audit/investigate/evaluate/review/assess?
- [ ] Are impact numbers specific (not ranges)?

**Quality checks:**
- [ ] Word count is 700-1200 (over 1500 = bloat, over 2000 = rework)
- [ ] No fact appears more than twice
- [ ] Every Why has a $ amount AND a source file
- [ ] Every branch goes at least 3 levels deep
- [ ] At least one Why cites an external benchmark
- [ ] Root Cause Description adds synthesis, not rehash
- [ ] Action Plan total shows current margin → post-fix margin → gap remaining
- [ ] No speculation presented as data (inferences flagged or removed)
- [ ] At least one fix proposes structural transformation with cost comparison
- [ ] At least one AI/automation opportunity mapped to a specific P&L line
- [ ] Every cost-reduction fix addresses customer impact or explains why there's none
- [ ] Every fix has Day-0 and Month-1 actions (not just "6-12 months")
- [ ] No fabricated evidence (only P&L data, external sources, or labeled inferences)

---

## Edge Cases

| Scenario | Handling |
|----------|----------|
| Data has gaps or zeros | Note the limitation. Analyze what's available. Don't fabricate. |
| Multiple equally large cost drivers | Pick the largest-$ driver for the primary chain. Develop others as secondary branches or address in Root Cause Description. |
| Revenue problem dominates margin | Pivot the MIP to Growth. Analyze revenue side first. |
| Cost structure is benchmark-appropriate | Shift focus to revenue. Note that costs aren't the problem. |
| Root cause blocked by data limitation | State explicitly: "Cannot determine further from available data because [specific gap]." Don't speculate to fill the gap. |

---

## Cross-Skill References

- **financial-deep-dive-qc** (`../financial-deep-dive-qc/`) — QC skill for validating Deep Dive output. Run after generating a report to check structure, 5-Why rigor, evidence quality, conciseness, and bias.

---

**End of Skill**
