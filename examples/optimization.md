# Example: Performance Optimization — The 12-Second API

> **Domain**: Backend Performance · Database Optimization · Caching  
> **Difficulty**: Hard  
> **Frameworks Used**: Theory of Constraints, Pareto Analysis (80/20)  

## 💬 User Prompt

> "Our main dashboard API endpoint takes 12 seconds to respond. The query hits PostgreSQL and joins 6 tables. We've already added indexes. Users are complaining, and we're losing customers. How do we fix this?"

---

## Phase 0: Activation Check ⚡

1. **Complex?** Yes — performance problems rarely have a single root cause.
2. **Sufficient context?** Partially — we need the query plan and data volumes.
3. **Cost of being wrong?** High — customer churn is already happening.

---

## Phase 1: Problem Intelligence 🔍

### 1.1 Restatement
A dashboard API endpoint is responding in 12 seconds (likely 60x slower than the 200ms target), driven by a complex 6-table PostgreSQL join. Standard indexing hasn't resolved the issue. This is causing user churn.

### 1.2 Objective
**Reduce dashboard API P95 response time from 12s to under 500ms** without data loss or accuracy degradation.

### 1.3 Constraint Space
| Dimension | Value |
|-----------|-------|
| **Hard Constraints** | PostgreSQL (cannot switch DB), data must be real-time accurate |
| **Soft Constraints** | Prefer minimal schema changes, team knows Python/Django |
| **Unknowns** | Table sizes, query execution plan, index hit rates, N+1 queries |
| **Stakeholders** | End users (speed), product team (retention), DBA (stability) |

### 1.4 Hidden Assumptions
- 🟡 "Already added indexes" — but were they the *right* indexes for this query? Composite indexes?
- 🟡 The bottleneck is the database — it could be serialization, network, or application logic
- 🔴 The query is being run on every page load — has anyone checked if this data changes frequently enough to warrant real-time queries?

---

## Phase 2: Decomposition Engine 🔧

### Sub-problems (Theory of Constraints: find the bottleneck)
1. **Profiling**: Where is the 12 seconds actually spent?
2. **Query Analysis**: Is the SQL itself the problem?
3. **Data Volume**: Are we scanning millions of rows?
4. **Application Layer**: Are there N+1 queries or serialization overhead?
5. **Caching Opportunity**: Can we cache the result?

### The 80/20 Question
In performance problems, **80% of the latency** usually comes from **one bottleneck**. We must find it before optimizing anything else.

---

## Phase 3: Systematic Reasoning 🧮

### Step 1: Profile First, Optimize Second

```sql
-- Enable query timing
EXPLAIN (ANALYZE, BUFFERS, FORMAT TEXT)
SELECT ...
FROM orders o
JOIN customers c ON c.id = o.customer_id
JOIN products p ON p.id = o.product_id
JOIN categories cat ON cat.id = p.category_id
JOIN regions r ON r.id = c.region_id
JOIN sales_reps sr ON sr.id = o.sales_rep_id
WHERE o.created_at > NOW() - INTERVAL '30 days'
GROUP BY r.name, cat.name
ORDER BY total_revenue DESC;
```

### Step 2: Common Root Causes (Hypothesis Tree)

```
12-second query
├── [A] Sequential scan on `orders` (missing index on created_at)
├── [B] Hash join exploding (Cartesian product from bad join order)
├── [C] Sorting millions of rows (ORDER BY without index support)
├── [D] N+1 queries from ORM (Django loading related objects in a loop)
└── [E] Lock contention (concurrent writes blocking reads)
```

### Step 3: Most Likely Culprit — Diagnosis

**Hypothesis A (Sequential Scan)**:
- With a 30-day filter on `orders.created_at`, if there's no index → full table scan on potentially millions of rows
- **Test**: `EXPLAIN ANALYZE` will show `Seq Scan` vs `Index Scan`
- **Fix**: `CREATE INDEX idx_orders_created_at ON orders(created_at);`

**Hypothesis B (Join Explosion)**:
- 6-table join can produce massive intermediate result sets if the planner picks a bad join order
- **Test**: Check `EXPLAIN` for `Nested Loop` on large tables (should be `Hash Join`)
- **Fix**: `SET join_collapse_limit = 8;` or restructure query

**Hypothesis D (N+1 in ORM)**:
- If using Django ORM: `Order.objects.filter(...)` then accessing `order.customer.region.name` triggers separate queries per row
- **Test**: Django Debug Toolbar → SQL panel → count queries
- **Fix**: `.select_related('customer__region', 'product__category', 'sales_rep')`

### Step 4: Layered Optimization Strategy

| Layer | Optimization | Expected Impact | Effort |
|---|---|---|---|
| **Query** | Add composite index `(created_at, customer_id)` | Reduce scan from 12s to ~2s | 5 min |
| **ORM** | Add `select_related()` / `prefetch_related()` | Eliminate N+1 (often 10-100x speedup) | 30 min |
| **SQL** | Rewrite as materialized view refreshed every 5 min | Reduce to <100ms reads | 2 hours |
| **Cache** | Redis cache with 60-second TTL | <10ms reads (cache hit) | 1 hour |
| **Architecture** | Pre-compute dashboard data via async worker | <5ms reads | 4 hours |

---

## Phase 4: Multi-Path Analysis 🔀

| Criterion | Path A: Index + ORM Fix | Path B: Materialized View | Path C: Redis Cache | Path D: Pre-computed (CQRS) |
|---|---|---|---|---|
| **Latency** | ~500ms | ~100ms | ~10ms (hit) | ~5ms |
| **Data Freshness** | Real-time | 5-min stale | 60s stale | Configurable |
| **Complexity** | Very Low | Low | Medium | High |
| **Durability** | N/A | N/A | Cache invalidation risk | Event sourcing needed |
| **Time to Ship** | 1 hour | 3 hours | 2 hours | 1-2 days |

### Recommended: **Layered Approach**
1. **Immediate** (today): Path A — fix indexes + ORM (gets you to ~500ms)
2. **Short-term** (this week): Path C — add Redis cache (gets you to <10ms)
3. **Long-term** (if scale demands): Path D — CQRS pre-computation

---

## Phase 5: Failure Mode Detection 🛡️

| Failure Mode | Impact | Mitigation |
|---|---|---|
| Cache stampede (100 users hit expired cache simultaneously) | DB overload | Use cache-aside with **probabilistic early expiration** |
| Materialized view refresh fails | Stale data served | Alert on refresh lag, fallback to live query |
| New columns added to query → ORM select_related breaks | N+1 regression | Add a performance regression test |
| Index bloat over time | Slow writes | Schedule `REINDEX CONCURRENTLY` during off-hours |

---

## Phase 6: Confidence Scoring 📊

| Metric | Score |
|--------|-------|
| Problem Understanding | 88 |
| Solution Correctness | 90 |
| Completeness | 85 |
| Evidence Quality | 80 |
| **Overall Confidence** | **86** |

> Confidence is 86% because the exact root cause requires `EXPLAIN ANALYZE` output we don't have. But the layered strategy addresses all common causes, so it's robust regardless of which hypothesis is correct.

---

## Phase 7: Final Output 📋

### 📌 Executive Summary
The 12-second API is caused by one or more of: missing composite indexes, ORM N+1 queries, or sorting large result sets. Apply a **3-layer fix**: (1) fix indexes + ORM today, (2) add Redis cache this week, (3) consider CQRS if needed later.

### 🎯 Immediate Fix (Ship Today)

```python
# Django ORM: BEFORE (N+1 nightmare)
orders = Order.objects.filter(created_at__gte=thirty_days_ago)
for order in orders:
    print(order.customer.region.name)  # N+1 query per order!

# Django ORM: AFTER (single query with JOINs)
orders = (
    Order.objects
    .filter(created_at__gte=thirty_days_ago)
    .select_related('customer__region', 'product__category', 'sales_rep')
    .only('id', 'total', 'customer__name', 'customer__region__name',
          'product__category__name', 'sales_rep__name')
)
```

```sql
-- Add composite index for the date range + join column
CREATE INDEX CONCURRENTLY idx_orders_date_customer
ON orders(created_at DESC, customer_id);
```

### 📋 Implementation Steps
1. Run `EXPLAIN ANALYZE` on the exact query (confirm the bottleneck)
2. Add `.select_related()` to the Django queryset
3. Create composite index (one-time, <5 min)
4. Deploy + measure (target: <500ms)
5. If still >500ms: add Redis cache with 60-second TTL
6. Add a performance regression test: `assert response_time < 1.0`

### ⚠️ Caveats & Risks
- `EXPLAIN ANALYZE` output is essential to confirm the root cause before investing in caching
- Redis cache adds a consistency window (60s stale data) — confirm with product team that this is acceptable for a dashboard

### 📊 Confidence: **86/100**
High confidence in the strategy. The remaining uncertainty is which specific sub-cause is dominant, but the layered approach covers all of them.
