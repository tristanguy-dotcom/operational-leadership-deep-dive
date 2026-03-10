# Financial Deep Dive Framework

## MIP Classification: Margin vs Growth

**Classify the Most Important Problem before deep analysis.** This determines the exploration path.

### The Rule

```
actual_margin = (revenue - total_expenses) / revenue
benchmark_margin = from benchmarks.csv (e.g., 70%)
margin_gap = benchmark_margin - actual_margin

If margin_gap > 10 percentage points → MIP = Margin
If revenue declining year-over-year → Consider MIP = Growth
If both → MIP = Both (but prioritize the larger $ impact)
```

### Sub-classification for Margin MIP

Break expenses into categories and compare each to its benchmark:

| Category | Benchmark Source | Calculation |
|----------|-----------------|-------------|
| Sales | benchmarks.csv | Total S&M cost / Revenue |
| Marketing | benchmarks.csv | Marketing subset / Revenue |
| Engineering (R&D) | benchmarks.csv | Total R&D cost / Revenue |
| G&A | benchmarks.csv | Total G&A cost / Revenue |
| Hosting | benchmarks.csv | Hosting costs / Revenue |
| Shared Services | benchmarks.csv | Shared services / Revenue |
| Executive | benchmarks.csv | Executive costs / Revenue |

The category with the **largest gap above benchmark** is the primary driver.

---

## The 5-Why Technique for Financial Analysis

### Rules

- **Always go deeper than the first answer.** "Costs are too high" is a symptom, not a root cause.
- **5 is a guideline, not a rule.** You may need 3 or 7 depending on complexity.
- **Branch when needed.** If a Why has multiple independent answers, follow each branch.
- **Quantify everything.** Every Why should have a dollar amount or percentage attached.
- **No blame.** Focus on "why does this cost exist?" not "who approved this?"

### Example

```
Why 1: Why is this company's margin 8.9% vs the 70% benchmark?
→ Total expenses are $72.2M against $79.2M revenue. The company needs to cut
  ~$48M in costs to reach benchmark margin.

Why 2: Why are expenses so high relative to revenue?
→ Three functions are significantly above benchmark: G&A at 20% of revenue
  (benchmark 4.5%), R&D at 22% (benchmark 10%), and S&M at 18% (benchmark 6%).

Why 3: Why is G&A at 20% of revenue vs the 4.5% benchmark?
→ Corporate Technology and Enterprise Systems departments account for $12.3M,
  with $8.1M in hosting and infrastructure costs alone. This is an IT function
  embedded in G&A rather than a true administrative overhead.

Root Cause: The company's cost structure reflects an on-premise software company
that hasn't optimized for cloud economics — hosting costs alone consume 10% of
revenue vs a 2% benchmark, and the IT organization is oversized for the company's
revenue base.
```

---

## Root Cause Quality Standards

### Must Be

| Criterion | Description |
|-----------|-------------|
| **Specific** | Names the exact cost driver. "$8.1M in hosting across 3 vendors" not "hosting costs are high." |
| **Data-backed** | Cites $ amounts, percentages, or ratios from the data. |
| **Actionable** | Points to something that can be changed. "Consolidate hosting vendors" not "market is expensive." |
| **Not a symptom** | Passes the "so what?" test — if someone asks "why?", you shouldn't have a ready answer. |
| **Causal** | Explains the mechanism. "Legacy on-premise architecture requires dedicated hosting per client" not "hosting costs are above benchmark." |

### Good vs. Bad Examples

| Bad (Generic) | Good (Specific) |
|---------------|-----------------|
| "Company is overspending" | "G&A consumes 20% of revenue vs 4.5% benchmark — $12.3M excess driven by Corporate Technology hosting costs" |
| "Too many employees" | "Cloud Operations has 12 FTEs ($1.4M) managing infrastructure that comparable SaaS companies handle with 3-4 FTEs via managed services" |
| "Revenue is too low" | "PSO revenue ($9.3M, 12% of total) carries 24% margin vs 85%+ for recurring — each PSO dollar generates $0.24 vs $0.85 for recurring" |

---

## Fix Generation Checklist

Starting points — explore beyond this based on data:

### Cost Reduction
- Vendor consolidation / renegotiation
- Headcount optimization (attrition, reallocation, offshoring)
- Hosting optimization (reserved instances, right-sizing, migration)
- Eliminate duplicate functions across departments
- Outsource non-core functions
- Reclassify misallocated costs (is this really G&A or is it COGS?)

### Revenue Improvement
- Pricing optimization on renewals
- Reduce customer churn (recurring revenue protection)
- Convert PSO engagements to recurring revenue
- Reduce PSO delivery cost (improve margin on existing revenue)
- Upsell/cross-sell to existing customer base
- Address customer concentration risk

### Structural
- Reorganize departments to align with benchmarks
- Consolidate overlapping teams
- Automate manual processes
- Renegotiate contracts at renewal

### Ranking Criteria

1. **Impact on margin** ($ and percentage points — highest weight)
2. **Directness** to root cause
3. **Effort/risk** (lower is better)
4. **Speed to impact** (faster is better)

---

## Operational Transformation Framework

When the root cause identifies a structural cost problem (not just overspending, but a broken delivery model), at least one fix should propose changing how the work gets done — not just doing less of the same thing.

### The Operations Playbook Sequence

Follow this sequence when proposing fixes (see `operations-overview.md` for full context):

1. **Simplify** — Remove complexity, legacy processes, and redundancy from the current operation
2. **Move to Central Factory** — Can this function be delivered by shared services? If yes, propose the move with edge-to-Central cost comparison
3. **Continuous Improvement** — If the function stays at the edge for now, rework it so it CAN move to Central next quarter
4. **Upgrade Staff** — If edge-delivered, upgrade to top-tier talent at competitive rates
5. **Implement AI** — Identify automation at every step

**Key concept**: "Central Factory" = shared services (engineering, support, SaaSOps, F&A) that serve all business units. "The edge" = go-to-market business units. The Ops Leader's primary lever is moving work from edge to Central.

**Antipattern**: Don't dump bad processes on Central. Simplify FIRST, then move. The process must be defined, documented, and ready for Central to assume.

### Transformation Types

| Type | When to Propose | Key Elements |
|------|----------------|--------------|
| **Move to Central** | Function exists at edge that Central already delivers at scale | Edge vs Central cost comparison, simplification steps, transition plan |
| **Consolidation** | Multiple teams/vendors do overlapping work | Target state, transition plan, savings calculation |
| **Automation** | Manual processes drive headcount or error costs | Tool/platform, processes automated, headcount impact |
| **Insourcing** | Outsourced work has excessive margins or quality issues | In-house cost (FTEs + tools), transition risk, quality comparison |
| **Model change** | Delivery model is fundamentally inefficient | New model description, cost comparison, migration path |

### Required Elements for Each Transformation

1. **Current state with numbers**: What it costs today, who's involved, what they do
2. **Proposed state**: How it would work, at what cost (Central Factory rates if applicable)
3. **Side-by-side comparison**: Current vs proposed, showing the delta
4. **Simplification steps**: What must be cleaned up before the transformation (don't dump complexity)
5. **Implementation roadmap**: Day-0, Month-1, Month-3 actions with milestones
6. **Customer impact & mitigation**: How customers are protected during transition
7. **Risk mitigation**: What could go wrong, how to prevent it
8. **KPIs**: 2-3 measurable indicators to track success

---

## AI/Automation Opportunity Identification

Every Deep Dive action plan must identify at least one AI or automation opportunity mapped to a specific P&L line. This isn't about mentioning AI — it's about connecting automation to concrete cost savings.

### Where to Look

| P&L Area | Common AI/Automation Opportunities |
|----------|-----------------------------------|
| **COGS - Operations** | Infrastructure auto-scaling, automated monitoring, self-healing systems |
| **COGS - QA/Testing** | Automated regression testing, AI-assisted test generation |
| **COGS - Support** | Ticket triage automation, chatbot for L1, knowledge base auto-generation |
| **OPEX - Development** | Code review automation, AI-assisted development, automated documentation |
| **OPEX - G&A** | Invoice processing, expense categorization, report generation |
| **OPEX - Sales** | Lead scoring, proposal generation, CRM data hygiene |

### Required for Each AI Opportunity

- **Target cost line**: Which P&L line it reduces (e.g., "$1.1M QA external contractors")
- **Specific use case**: What the AI/automation does (not "use AI for QA" but "automated regression suite replacing 60% of manual test execution")
- **Estimated impact**: Dollar savings with reasoning
- **Implementation outline**: Tool/approach, timeline, ownership

---

## Customer Impact Assessment

Cost cuts that degrade customer experience aren't savings — they're deferred revenue losses.

### Required Analysis

For every cost-reduction fix, answer:

1. **Does this touch a customer-facing function?** (Support, CS, Product, Ops)
2. **What's the service risk?** (Response time, availability, feature velocity, quality)
3. **What's the mitigation?** (Automation, process change, SLA adjustment, phased approach)

### Red Flags

| Proposed Fix | Customer Risk | Required Mitigation |
|-------------|--------------|-------------------|
| Headcount reduction in Support | Longer response times, lower CSAT | Automation of L1, SLA monitoring, escalation paths |
| Reducing QA spend | More bugs reaching production | Automated testing, staged rollouts, monitoring |
| Infrastructure cost cuts | Reliability degradation | SLA commitments, monitoring, rollback plan |
| Outsourcing customer-facing roles | Quality inconsistency | Training, QA framework, customer feedback loop |

If a fix could impact customers and the report doesn't address it — that's a hard fail.

---

## Cognitive Bias Guards

| Bias | Risk | Guard |
|------|------|-------|
| **Recency** | Overweighting recent changes | Look at the full year data, not just recent trends |
| **Confirmation** | Seeking data supporting initial hypothesis | If user said "hosting is the problem," explicitly check other cost areas first |
| **Anchoring** | Letting the biggest number dominate | The largest cost isn't always the most above-benchmark cost |
| **Attribution** | Blaming departments instead of structure | "This team is too big" → "Why does this function require this many people?" |
| **Narrative** | Forcing a clean single story | Multiple independent cost drivers are normal — don't pretend they connect |
| **Round number** | Assuming benchmarks are exact | Benchmarks are guides — a function at 5.1% vs 5.0% benchmark isn't a problem |

---

## Data Navigation Guide

### How the CSV Files Connect

```
pl-summary.csv (top-level view)
  ├── Revenue line items
  │   ├── revenue-recurring.csv (customer detail)
  │   ├── revenue-pso.csv (customer detail)
  │   └── revenue-perpetual.csv (customer detail)
  └── Expense line items
      ├── employees.csv (HC costs by function/dept)
      ├── opex-non-employee.csv (non-HC OPEX by dept/vendor)
      └── cogs-non-employee.csv (non-HC COGS by dept/vendor)

benchmarks.csv (internal comparison targets)
```

### Reading the Data

- **Amounts** are in USD, annual (2018)
- **Employees** file has anonymized names (empty/null) — focus on function, department, and cost
- **Revenue files** have customer-level detail — useful for concentration analysis
- **Expense files** have vendor-level detail — useful for finding consolidation opportunities
- **Benchmarks** are expressed as % of revenue

### Common Calculations

```
function_cost_pct = SUM(function costs) / total_revenue * 100
benchmark_gap = function_cost_pct - benchmark_pct
gap_in_dollars = benchmark_gap / 100 * total_revenue
```
