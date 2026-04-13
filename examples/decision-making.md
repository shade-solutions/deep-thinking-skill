# Example: Strategic Decision Making — The Pivot or Perish Problem

> **Domain**: Startup Strategy · Product-Market Fit  
> **Difficulty**: Hard  
> **Frameworks Used**: Porter's Five Forces, RICE Scoring, Eisenhower Matrix  

## 💬 User Prompt

> "Our SaaS for real estate agents isn't getting traction. We have 500 signups but only 2 paying customers. High churn. Should we pivot to a 'Lead Gen as a Service' model or double down on our existing CRM features?"

---

## Phase 1: Problem Intelligence 🔍

### 1.1 Restatement
A B2B SaaS targeting real estate agents has catastrophic conversion (0.4%) and high churn, indicating a fundamental Product-Market Fit (PMF) failure. The question is whether to iterate on the existing product or pivot entirely.

### 1.2 Objective
**Identify the business model with the highest probability of reaching $10K MRR within 90 days** (the remaining runway).

### 1.3 Constraint Space
| Dimension | Value |
|-----------|-------|
| **Hard Constraints** | 3 months of runway, team of 3 (2 devs + 1 sales) |
| **Soft Constraints** | Existing codebase is CRM-focused, domain expertise in real estate |
| **Unknowns** | Why exactly users churn, willingness to pay for lead gen, lead quality achievable |
| **Stakeholders** | Founders (survival), existing 2 paying customers (continuity), investors (ROI signal) |

### 1.4 Hidden Assumptions
- 🟢 The 500 signups indicate market interest in *something* related to real estate
- 🟡 Agents would pay $500/mo for leads (unvalidated price point)
- 🟡 The team can build a lead gen MVP in 2-4 weeks
- 🔴 The churn is product-related (it could be sales-process related — did they understand what they signed up for?)

> ⚠️ **Critical**: Before making any pivot decision, the churn reason MUST be validated through user interviews. Pivoting based on assumed churn reasons is how startups die twice.

---

## Phase 2: Decomposition Engine 🔧

### Sub-problems
1. **Churn Diagnosis**: Why are 498 of 500 signups not paying?
2. **CRM Viability**: Can the existing product reach PMF with iteration?
3. **Lead Gen Viability**: Is "Lead Gen as a Service" a real business at this stage?
4. **Execution Risk**: Can this team execute a pivot in 90 days?

### Dependency Graph
```
Churn Diagnosis ─┬─→ CRM Viability ──────→ Final Decision
                 └─→ Lead Gen Viability ──↗
                      ↑
                 Execution Risk ──────────↗
```

---

## Phase 3: Systematic Reasoning 🧮

### Sub-problem 1: Churn Diagnosis
**Hypothesis**: Users signed up expecting a quick win, found a complex CRM, and left.
**Evidence**: The prompt states agents "don't have time to manage a CRM; they just want more closings." This is a **jobs-to-be-done mismatch** — they hired the product for "more closings," but it delivers "better organization."
**Confidence**: 80%

### Sub-problem 2: CRM Viability
**Hypothesis**: Doubling down on CRM will not work.
**Reasoning (Porter's Five Forces)**:
| Force | Assessment |
|-------|-----------|
| **Rivalry** | Extreme — Salesforce, HubSpot, Zillow CRM, Follow Up Boss |
| **Buyer Power** | High — agents are price-sensitive, many free alternatives |
| **Substitutes** | Abundant — spreadsheets, email, pen and paper |
| **Supplier Power** | Low (irrelevant) |
| **New Entrants** | Low barrier — another CRM launches every week |

**Verdict**: The CRM market for real estate is a **red ocean**. A 3-person startup cannot compete on features. ❌
**Confidence**: 90%

### Sub-problem 3: Lead Gen Viability
**Hypothesis**: Lead gen directly addresses the "more closings" JTBD.
**RICE Score**:
| Factor | Score | Rationale |
|--------|-------|-----------|
| **Reach** | 8/10 | Every agent wants leads |
| **Impact** | 9/10 | Directly tied to revenue |
| **Confidence** | 5/10 | Unvalidated — lead quality unknown |
| **Effort** | 6/10 | MVP is achievable but non-trivial |
| **RICE** | **(8 × 9 × 5) / 6 = 60** | Above threshold for pivot |

**Confidence**: 65% — high potential but must be validated before committing.

### Sub-problem 4: Execution Risk
The team has real estate data + engineering skills. A lead gen MVP (scraper + emailer + dashboard) is feasible in 3-4 weeks. But the **real risk** is lead quality, not engineering.

---

## Phase 4: Multi-Path Analysis 🔀

| Criterion | Path A: Double Down on CRM | Path B: Pivot to Lead Gen | Path C: Hybrid (CRM + Leads) |
|---|---|---|---|
| **Market Fit Signal** | 0.4% conversion (terrible) | Unvalidated (unknown) | Dilutes focus |
| **Differentiation** | Near zero | High (if quality is good) | Medium |
| **Revenue Potential** | $29/mo × low conversion | $500/mo × high perceived value | Confused pricing |
| **Execution Speed** | Fast (incremental) | 3-4 weeks for MVP | 6-8 weeks |
| **Risk** | Certain slow death | Fast learn / fast fail | Slow everything |

### Decision
**Path B (Pivot to Lead Gen)** — but NOT a blind pivot. Execute a **validation-first pivot** using the following protocol.

Path A is eliminated: the market data is unambiguous. Path C is eliminated: hybrid strategies kill startups by preventing focus.

---

## Phase 5: Failure Mode Detection 🛡️

### Pre-Mortem
If the lead gen pivot fails in 90 days, the most likely causes are:

| Failure Mode | Probability | Mitigation |
|---|---|---|
| Lead quality is too low | 40% | Manual curation in Week 1 to validate |
| Agents don't trust a new platform for leads | 25% | Offer free trial with the 2 existing customers |
| Engineering takes longer than expected | 20% | Week 1 is manual-only (no code) |
| Price point is wrong | 15% | A/B test $299 vs $499 vs $799 |

### Blast Radius
- **If it fails**: Company has ~30 days of runway left. Enough for one more micro-pivot or an acqui-hire conversation.
- **If it succeeds**: First 5 paying customers at $500/mo = $2,500 MRR. Enough to extend runway and raise a seed round.

---

## Phase 6: Confidence Scoring 📊

| Metric | Score |
|--------|-------|
| Problem Understanding | 92 |
| Solution Correctness | 78 |
| Completeness | 85 |
| Evidence Quality | 72 |
| **Overall Confidence** | **80** |

> Confidence is moderate (not high) because the lead gen value proposition is **unvalidated**. The strategy itself is sound, but the outcome depends on market reception.

---

## Phase 7: Final Output 📋

### 📌 Executive Summary
Pivot to **Lead Gen as a Service**, but execute a **1-week manual validation** before writing any code. The CRM market is saturated, the churn data signals a jobs-to-be-done mismatch, and agents have explicitly expressed they want "more closings, not more tools."

### 🎯 The Solution: Validation-First Pivot Protocol

**Week 1 — Manual Concierge (No Code)**
- Personally curate 20 leads for each of the 2 paying customers
- Deliver via email or spreadsheet
- Measure: Did they respond? Did any lead convert to a showing/sale?
- **Gate**: If neither customer finds value → abort pivot, explore other directions

**Week 2-4 — MVP Build (If Week 1 passes)**
- Build: Web scraper → Lead scoring → Auto-emailer → Agent dashboard
- Tech: Node.js + existing MongoDB + SendGrid
- Target: Onboard 5 paying customers at $499/mo

**Week 5-12 — Scale**
- Add self-serve onboarding
- Build referral program (agents refer agents)
- Target: $10K MRR

### 📋 Implementation Steps
1. Today: Email 2 paying customers offering free lead gen pilot
2. Day 1-3: Manually source 40 leads (20 per customer) from public listings
3. Day 4-7: Deliver leads, collect feedback via call
4. Day 8: Go/No-Go decision based on lead quality feedback
5. Day 9-28: Build automated MVP (only if Step 4 is "Go")

### ⚠️ Caveats & Risks
- This strategy assumes the 2 existing customers will participate in the pilot
- Lead gen has legal considerations (CAN-SPAM, TCPA) — consult a lawyer before automating outreach
- The $499/mo price point is a hypothesis; be prepared to adjust

### 📊 Confidence: **80/100**
The strategic logic is strong, but the unvalidated lead quality is the key risk. The 1-week manual pilot is specifically designed to de-risk this before committing engineering resources.
