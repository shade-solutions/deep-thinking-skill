# Example: Distributed Systems Debugging — The Phantom Duplicate

> **Domain**: Backend Engineering · Distributed Systems · MongoDB  
> **Difficulty**: Hard  
> **Frameworks Used**: 5 Whys, Failure Mode Analysis  

## 💬 User Prompt

> "Our payment processing worker occasionally fails with a `Duplicate Key` error in MongoDB, even though we check if the transaction ID exists before inserting. It happens about 1 in 1000 transactions."

---

## Phase 1: Problem Intelligence 🔍

### 1.1 Restatement
A distributed payment worker sporadically throws `E11000 duplicate key` errors on MongoDB despite an application-level existence check. The error rate (~0.1%) suggests a timing-dependent issue rather than a logic error.

### 1.2 Objective
**Eliminate 100% of Duplicate Key errors** without introducing payment data loss or processing delays above 50ms.

### 1.3 Constraint Space
| Dimension | Value |
|-----------|-------|
| **Hard Constraints** | Node.js runtime, MongoDB 6.x, cannot lose payment records |
| **Soft Constraints** | Prefer minimal infrastructure changes (no new services) |
| **Unknowns** | Number of concurrent workers, retry policy, idempotency key generation |
| **Stakeholders** | Payment ops team (on-call fatigue), customers (failed payments) |

### 1.4 Hidden Assumptions
- 🟢 MongoDB has a unique index on `txId` (confirmed by the error itself)
- 🟡 Only one service writes to this collection (unverified — could be multiple)
- 🔴 The `txId` is truly unique at generation time (what if the ID generator has its own collision problem?)

> ⚠️ The 🔴 assumption should be verified: Is the transaction ID generated with sufficient entropy?

---

## Phase 2: Decomposition Engine 🔧

### Sub-problems
1. **Race Condition Window**: How wide is the time gap between `find()` and `insert()`?
2. **Concurrency Model**: How many workers can process the same `txId` simultaneously?
3. **Retry Policy**: Is the frontend or queue retrying, creating duplicate submissions?
4. **Atomicity Gap**: Why isn't the check-and-insert atomic?

### Dependency Graph
```
Retry Policy → Concurrency Model → Race Condition → Atomicity Gap
                                                        ↑
                                              (root cause likely here)
```

### Complexity Assessment
| Sub-problem | Difficulty | Confidence | Risk if Wrong |
|---|---|---|---|
| Race condition window | Moderate | 90% | Medium |
| Concurrency model | Trivial | 95% | Low |
| Retry policy | Moderate | 70% | High — could be the ACTUAL root cause |
| Atomicity gap | Hard | 85% | Critical — this is the fix |

---

## Phase 3: Systematic Reasoning 🧮

### Hypothesis: Classic TOCTOU Race Condition
The `find()` and `insert()` are two separate operations. Under concurrency, two workers can both execute `find()` before either executes `insert()`.

### Simulation (Concrete Walkthrough)
```
t=0ms   Worker A: find({txId: "PAY-1001"}) → null ✓
t=1ms   Worker B: find({txId: "PAY-1001"}) → null ✓  ← stale read!
t=5ms   Worker A: insert({txId: "PAY-1001", ...}) → SUCCESS
t=6ms   Worker B: insert({txId: "PAY-1001", ...}) → E11000 DUPLICATE KEY 💥
```

### Verification
- **Why 1 in 1000?** The race window is ~5ms. At 200 TPS, the probability of two requests for the same `txId` arriving within 5ms is ~0.1%. ✅ Math checks out.
- **Disconfirming evidence**: Could it be a MongoDB replication lag issue? No — `E11000` fires on the primary node, replication doesn't affect unique index enforcement on writes.

### Confidence: **92%** — the timing math aligns with the observed error rate.

---

## Phase 4: Multi-Path Analysis 🔀

| Criterion | Path A: Atomic Upsert | Path B: Distributed Lock (Redis) | Path C: Catch & Retry |
|---|---|---|---|
| **Correctness** | ✅ Guaranteed by DB | ✅ Guaranteed by lock | ⚠️ Masks the bug |
| **Performance** | +2ms (negligible) | +10-15ms (Redis RTT) | 0ms (but adds retries) |
| **Complexity** | 1 line of code change | New infrastructure dependency | Error handling spaghetti |
| **Maintainability** | Excellent | Requires Redis monitoring | Tech debt magnet |
| **Risk** | Very low | Lock expiry edge cases | Silent data issues |
| **Time to Implement** | 5 minutes | 2-4 hours | 30 minutes |

### Decision
**Path A (Atomic Upsert)** is the clear winner. It solves the problem at the correct layer (database), requires no new infrastructure, and is the idiomatic MongoDB pattern.

Path B is over-engineered for this problem. Path C is an anti-pattern that hides bugs.

---

## Phase 5: Failure Mode Detection 🛡️

### Pre-Mortem
If the upsert solution "fails," it would be because:
1. `$setOnInsert` accidentally overwrites data on a second write → **Impossible** by design; `$setOnInsert` only writes on insert, not update.
2. The `txId` field isn't indexed → Upsert on an unindexed field causes a collection scan. **Mitigation**: Verify index exists.

### Blast Radius
- **Scope**: Only the payment worker
- **Severity**: Zero — upsert is strictly safer than the current code
- **Recovery**: Instant rollback (revert 1 line of code)

---

## Phase 6: Confidence Scoring 📊

| Metric | Score |
|--------|-------|
| Problem Understanding | 95 |
| Solution Correctness | 95 |
| Completeness | 90 |
| Evidence Quality | 88 |
| **Overall Confidence** | **92** |

---

## Phase 7: Final Output 📋

### 📌 Executive Summary
The error is a **TOCTOU (Time-of-Check-to-Time-of-Use) race condition**. Two workers read "not found" before either inserts, causing the second insert to violate the unique index. Replace the check-then-insert with an **atomic upsert**.

### 🎯 The Solution
```javascript
// ❌ BEFORE: Two-step, non-atomic (race condition)
const exists = await db.payments.findOne({ txId });
if (!exists) {
  await db.payments.insertOne(paymentData);
}

// ✅ AFTER: Single atomic operation
const result = await db.payments.findOneAndUpdate(
  { txId },
  { $setOnInsert: paymentData },
  { upsert: true, returnDocument: 'after' }
);

// result.lastErrorObject.updatedExisting tells you if it was a duplicate
if (result.lastErrorObject.updatedExisting) {
  logger.warn(`Duplicate txId detected: ${txId} — skipped safely`);
}
```

### 📋 Implementation Steps
1. Verify a unique index exists on `txId`: `db.payments.createIndex({ txId: 1 }, { unique: true })`
2. Replace the `findOne` + `insertOne` with the `findOneAndUpdate` upsert above
3. Add the duplicate detection log for observability
4. Deploy and monitor the error rate dashboard for 24 hours
5. Remove any error-suppression or retry logic that was masking this bug

### ⚠️ Caveats & Risks
- Ensure the write concern is `majority` for payment-critical data
- This fix does NOT address the upstream question of *why* duplicate requests arrive — consider adding request deduplication at the API gateway

### 📊 Confidence: **92/100**
High confidence. The failure mode analysis confirms the fix is safe, the timing math validates the hypothesis, and the solution is the idiomatic MongoDB pattern.
