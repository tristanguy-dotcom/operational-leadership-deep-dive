# Financial Deep Dive: B2B Software Company

**Company**: B2B Software Company
**Analyst**: Claude Code
**Date**: 03/09/2026

---

## Executive Summary

**Financial Situation**: $79.2M revenue (87% recurring) with 8.9% operating margin — 61 points below the 70% benchmark. Expenses of $72.2M exceed the 30% target by $48.4M.

**Root Cause Summary**: Cloud infrastructure operations cost 2.5x the industry median because unautomated infrastructure requires a large manual operations team. Engineering that should modernize this infrastructure runs mostly at expensive edge rates when Central Engineering delivers equivalent work 40-60% cheaper — leaving no budget headroom for infrastructure automation.

---

## Root Cause Analysis

### Why 1

- **Question**: Why is margin 8.9% vs the 70% benchmark?
- **Answer**: Expenses are $72.2M (91% of revenue) vs a $23.8M target. Cost of Product ($18.1M, 22.9%) has the largest gap — $14.2M above its ~5% combined benchmark.
- **Evidence**: R&D 21.8% vs 10% (+$9.4M). G&A 19.9% vs 9% (+$8.6M). S&M 17.6% vs 6% (+$7.9M).
- **Source**: pl-summary.csv, benchmarks.csv

### Why 2

- **Question**: Why is Cost of Product $18.1M?
- **Answer**: Cloud Operations alone is $10.0M — $8.3M hosting COGS plus 58 employees ($1.6M). At 12.6% of revenue, this is 2.5x the industry median of 5% (SaaS Capital 2025, 1,000+ private B2B SaaS companies).
- **Evidence**: Internal benchmark: hosting at 1% ($0.8M). Industry median: 5% ($4.0M).
- **Source**: cogs-non-employee.csv, employees.csv, SaaS Capital 2025

### Why 3

- **Question**: Why does Cloud Ops cost $10M for a $79M company?
- **Answer**: 58 FTEs at avg $28K (offshore/nearshore) perform manual infrastructure work that cloud-native peers automate with 15-20 engineers. The 3x headcount ratio signals infrastructure lacking auto-scaling, automated monitoring, and infrastructure-as-code.
- **Evidence**: Cloud-native B2B SaaS at $80M: 15-20 DevOps/SRE, hosting at 3-5% of revenue.
- **Source**: employees.csv, SaaS Capital 2025

### Why 4

- **Question**: Why hasn't the infrastructure been automated?
- **Answer**: R&D runs edge-heavy: 67 engineers at $103K avg ($6.9M) while only $2.7M flows through Central Engineering (Dev Factory) at $60-100K rates. With 80% of dev spend at expensive edge rates — plus $3.2M in external contractors — there's no budget headroom for infrastructure modernization. The engineering capacity that should automate Cloud Ops is consumed by product maintenance at premium cost.
- **Evidence**: Edge engineering: $6.9M (80% of dev spend). Central Engineering: $2.7M (20%). Central rates: $60-100K.
- **Source**: opex-non-employee.csv, employees.csv, Central rate card

---

## Root Cause Description

The margin gap traces to unmodernized infrastructure that the company can't fix because engineering runs at the wrong cost point. Cloud Ops lacks automation, so the company staffs it with a large offshore team doing manual work. The engineering team that should drive modernization sits at expensive edge rates when Central Engineering delivers at 40-60% less — but only 20% of dev spend flows through Central today. External contractors fill remaining feature gaps. G&A compounds the problem: $3.4M in outsourced administrative services and $2.6M in distributed office management add cost without contributing capacity to solve the core infrastructure problem.

---

## Action Plan

| # | Fix | Expected Impact | Status | Owner |
|---|-----|----------------|--------|-------|
| 1 | **Expand Central Engineering and eliminate external contractors.** Move 40 of 67 edge engineering roles ($103K avg) to Central Factory at $60K rate: 40 × $103K = $4.1M → 40 × $60K = $2.4M, saving $1.7M. Eliminate external contractors ($3.2M to $0.8M, saving $2.4M). Deploy AI code assistants to boost Central team productivity during absorption. Day-0: Identify edge roles suitable for Central migration. Month-1: Begin knowledge transfer for first 10 roles. Customer impact: Feature delivery maintained through Central; parallel staffing during 90-day handover. | $4.1M / +5.2pp | Recommended | VP Product |
| 2 | **Consolidate G&A outsourced services and reduce office footprint.** Reduce Corporate + F&A outsourcing from $3.4M to $1.0M (retain essential legal/audit). Consolidate GMs & Office Admins from $2.6M to $1.5M through office closures. Automate AP/AR/payroll with AI tools to replace F&A outsourcing. Day-0: Inventory all outsourced contracts. Month-1: Cancel non-essential contracts. Month-3: Complete office consolidation. No customer impact — back-office functions. | $3.5M / +4.4pp | Recommended | CFO |
| 3 | **Optimize hosting usage and move Cloud Ops to Central SaaSOps.** Decommission unused instances (est. $1.7M from 32% avg cloud waste per Flexera 2025) and right-size remaining workloads. Move operations team to 20 Central Factory engineers at $60K ($1.2M vs current $1.6M). Deploy Terraform IaC + AI-powered monitoring (CloudWatch/PagerDuty) + auto-scaling to replace manual ops. Day-0: Freeze new hosting contracts, audit instance utilization. Month-1: Begin decommissioning idle resources; start Central SaaSOps team buildout. Customer impact: No migration required — optimization within existing infrastructure. | $2.1M / +2.7pp | Recommended | VP Engineering |

**Total Expected Impact**: $9.7M savings / margin from 8.9% to 21.1%. Remaining gap to 70%: 48.9pp — reflects that the 70% target is top-decile for SaaS (industry median is breakeven). Closing the full gap requires revenue growth, further Central migration of remaining 27 edge engineers, and business model restructuring beyond cost optimization.

---

## Data Limitations

| Data Gap | Impact on Analysis |
|----------|-------------------|
| Single year of data (2018) | Cannot identify cost trends or validate whether costs are growing or stable |
| No hosting architecture details | Cannot confirm legacy vs hybrid vs cloud-native; limits Fix 3 specificity |
| No contract terms for hosting vendors | Cannot confirm remaining commitment period or early termination costs; Fix 3 timeline depends on contract flexibility |
| No product/SKU breakdown | Cannot determine margin by product line |
| Internal benchmarks are aspirational | 70% margin is top-decile, not industry median (~breakeven). Analysis cites both. |

---

**Analysis by**: Claude Code with Financial Deep Dive Skill
