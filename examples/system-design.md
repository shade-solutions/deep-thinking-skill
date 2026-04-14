# Example: System Design — Real-Time Chat at Scale

> **Domain**: System Architecture · Distributed Systems · WebSockets  
> **Difficulty**: Hard  
> **Frameworks Used**: C4 Model, Theory of Constraints, FMEA  

## 💬 User Prompt

> "Design a real-time chat system like Slack that supports 10 million concurrent users, message persistence, read receipts, typing indicators, and file sharing. What architecture would you use?"

---

## Phase 0: Activation Check ⚡

1. **Complex?** Yes — this involves distributed systems, real-time protocols, storage at scale, and multiple interconnected subsystems.
2. **Sufficient context?** Mostly. We'll need to make assumptions about latency requirements and consistency models.
3. **Cost of being wrong?** High — architectural mistakes at this scale are extremely expensive to fix.

---

## Phase 1: Problem Intelligence 🔍

### 1.1 Restatement
Design a horizontally scalable, fault-tolerant real-time messaging platform capable of handling 10M concurrent WebSocket connections with sub-200ms message delivery, durable storage, and rich features (read receipts, typing indicators, file uploads).

### 1.2 Objective
**Produce an architecture that delivers P99 message latency ≤ 200ms at 10M concurrent connections**, with 99.99% uptime and zero message loss.

### 1.3 Constraint Space
| Dimension | Value |
|-----------|-------|
| **Hard Constraints** | 10M concurrent users, message persistence (never lose a message), E2E latency ≤ 200ms P99 |
| **Soft Constraints** | Cost efficiency, team expertise, time to market |
| **Unknowns** | Average message size, read/write ratio, geographic distribution |
| **Stakeholders** | End users (speed), ops team (reliability), business (cost) |

### 1.4 Hidden Assumptions
- 🟢 Messages are text-first (avg ~500 bytes)
- 🟡 Read/write ratio is ~100:1 (most users read, few write at any moment)
- 🟡 Geographic distribution: primarily US/EU (2 regions)
- 🔴 "10M concurrent" means 10M live WebSocket connections, not 10M registered users

### 1.5 Back-of-Envelope Calculations
```
Concurrent users:     10,000,000
Avg messages/user/hr: 5
Messages per second:  10M × 5 / 3600 ≈ 14,000 msg/s
Peak (3x):            ~42,000 msg/s
Avg message size:     500 bytes
Bandwidth (peak):     42,000 × 500 = 21 MB/s (manageable)
Storage per day:      14,000 × 500 × 86,400 ≈ 600 GB/day
```

---

## Phase 2: Decomposition Engine 🔧

### Sub-systems
1. **Connection Layer**: Managing 10M persistent WebSocket connections
2. **Message Routing**: Delivering messages to the correct channel/user in real-time
3. **Persistence Layer**: Storing messages durably
4. **Presence System**: Online/offline status, typing indicators
5. **File Service**: Upload, storage, and CDN delivery
6. **Read Receipt Tracker**: Per-message, per-user read state

### Dependency Graph
```
Connection Layer ──→ Message Routing ──→ Persistence Layer
       │                    │
       ├──→ Presence System │
       │                    ├──→ Read Receipt Tracker
       └────────────────────└──→ File Service
```

### Complexity Assessment
| Sub-system | Difficulty | Risk if Wrong |
|---|---|---|
| Connection Layer | Hard | System crashes at scale |
| Message Routing | Hard | Messages lost or duplicated |
| Persistence | Moderate | Data loss |
| Presence | Moderate | UX degradation |
| File Service | Easy | Slow uploads (tolerable) |
| Read Receipts | Moderate | Write amplification at scale |

---

## Phase 3: Systematic Reasoning 🧮

### Sub-system 1: Connection Layer

**Problem**: A single server can handle ~100K WebSocket connections. For 10M, we need **~100 servers** minimum.

**Solution**: Horizontally scaled WebSocket gateway fleet behind a Layer 4 load balancer.

```
Clients ──→ [L4 Load Balancer (HAProxy/NLB)]
                    │
         ┌──────────┼──────────┐
         ▼          ▼          ▼
    [WS Gateway]  [WS Gateway]  [WS Gateway] × 100+
         │          │          │
         └──────────┼──────────┘
                    ▼
           [Message Bus (Kafka)]
```

**Key Design Decisions**:
- **Sticky sessions** via consistent hashing on `userId` → ensures reconnects hit the same gateway
- **Connection registry** in Redis: maps `userId → gatewayId` for cross-gateway routing
- **Heartbeat** every 30s to detect dead connections

### Sub-system 2: Message Routing

**Problem**: User A sends a message in Channel X. Users B, C, D (on different gateways) must receive it in <200ms.

**Solution**: Pub/Sub fan-out via Redis Pub/Sub for real-time + Kafka for durability.

```
Sender → Gateway A → Kafka (persist) → Consumer → DB
                  ↘ Redis Pub/Sub (fan-out)
                       ↓         ↓         ↓
                  Gateway B   Gateway C   Gateway D
                       ↓         ↓         ↓
                    User B    User C    User D
```

**Dual-write pattern**: Kafka for durability (source of truth), Redis Pub/Sub for speed (fire-and-forget).

### Sub-system 3: Persistence Layer

**Choice**: Cassandra (or ScyllaDB)

| Requirement | Why Cassandra |
|---|---|
| Write-heavy | Append-only log (perfect for chat) |
| Horizontal scaling | Linear scalability |
| Partition by channel | Natural partition key: `(channel_id, bucket)` |
| Time-ordered reads | Clustering key on `message_timestamp` |

**Schema**:
```sql
CREATE TABLE messages (
    channel_id  UUID,
    bucket      INT,        -- daily bucket for partition management
    message_id  TIMEUUID,
    sender_id   UUID,
    content     TEXT,
    attachments LIST<TEXT>,
    created_at  TIMESTAMP,
    PRIMARY KEY ((channel_id, bucket), message_id)
) WITH CLUSTERING ORDER BY (message_id DESC);
```

### Sub-system 4: Presence & Typing

**Typing indicators**: Stateless, ephemeral — broadcast via Redis Pub/Sub, never persisted. Expire client-side after 3 seconds of no signal.

**Online/offline status**: Write to Redis with a 60-second TTL. Heartbeat refreshes TTL. When TTL expires → user is offline.

### Sub-system 5: File Service

```
Client → Pre-signed URL from API → Direct upload to S3 → CDN (CloudFront)
                                         ↓
                                  Metadata stored in DB
```

No files pass through the application servers. This keeps the hot path clean.

---

## Phase 4: Multi-Path Analysis 🔀

| Criterion | Path A: Redis Pub/Sub + Kafka | Path B: Pure Kafka Consumers | Path C: gRPC Streaming |
|---|---|---|---|
| **Latency** | <50ms (Redis is in-memory) | 100-500ms (consumer lag) | <50ms |
| **Complexity** | Medium (two systems) | Low (one system) | High (custom protocol) |
| **Durability** | ✅ Kafka persists | ✅ Kafka persists | ❌ Must add separately |
| **Fan-out** | Excellent (Pub/Sub native) | Poor (each gateway polls) | Good (server push) |
| **Operational Cost** | Medium | Low | High |

**Winner: Path A** — Redis Pub/Sub for real-time fan-out + Kafka for durable persistence. This separates the "speed path" from the "safety path."

---

## Phase 5: Failure Mode Detection 🛡️

### FMEA Table

| Failure Mode | Severity | Probability | Detection | Mitigation |
|---|---|---|---|---|
| Redis Pub/Sub message lost | Medium | Low | Kafka replay | Client pulls missed messages via API on reconnect |
| Gateway crash (100K users drop) | High | Medium | Health checks | Auto-scaling + client reconnect with exponential backoff |
| Cassandra write timeout | High | Low | Metrics alert | Retry with idempotency key (message_id) |
| Kafka consumer lag spike | Medium | Medium | Lag monitor | Scale consumers, increase partitions |
| Split-brain (network partition) | Critical | Very Low | Raft consensus | Use Kafka's ISR guarantees, Cassandra's LOCAL_QUORUM |

### Key Reliability Pattern: Message Delivery Guarantee
```
1. Client sends message → gets ACK with message_id from gateway
2. Gateway writes to Kafka → guaranteed persistence
3. If Redis Pub/Sub fan-out fails → recipients pull via HTTP on reconnect
4. Client stores last_seen_message_id → can request gap-fill
```
**Result**: At-least-once delivery with client-side deduplication via `message_id`.

---

## Phase 6: Confidence Scoring 📊

| Metric | Score |
|--------|-------|
| Problem Understanding | 92 |
| Solution Correctness | 85 |
| Completeness | 80 |
| Evidence Quality | 82 |
| **Overall Confidence** | **85** |

> Confidence is 85% because several design decisions (partition strategy, exact Redis cluster sizing) require load testing to validate. The architecture is sound in theory but needs empirical tuning.

---

## Phase 7: Final Output 📋

### 📌 Executive Summary
Use a **three-tier architecture**: WebSocket Gateway Fleet → Redis Pub/Sub (real-time) + Kafka (durability) → Cassandra (persistence). This separates the speed path from the safety path, achieving <200ms P99 latency at 10M concurrent connections.

### 🎯 High-Level Architecture
```
┌─────────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                                 │
│  [Mobile App]  [Desktop App]  [Web App]                             │
│       │              │            │                                  │
│       └──────────────┼────────────┘                                  │
│                      ▼                                               │
│         [L4 Load Balancer (NLB)]                                     │
│              ┌───────┼───────┐                                       │
│              ▼       ▼       ▼                                       │
│         [WS GW]  [WS GW]  [WS GW]  × 100+                          │
│              │       │       │                                       │
├──────────────┼───────┼───────┼───────────────────────────────────────┤
│              ▼       ▼       ▼         REAL-TIME LAYER               │
│         [Redis Pub/Sub Cluster]  ←── fan-out in <10ms                │
│                                                                      │
├──────────────────────────────────────────────────────────────────────┤
│              ▼                         DURABILITY LAYER              │
│         [Kafka Cluster]  ←── source of truth                         │
│              ▼                                                       │
│         [Stream Consumers]                                           │
│              ▼                                                       │
│         [Cassandra Cluster]  ←── persistent storage                  │
│                                                                      │
├──────────────────────────────────────────────────────────────────────┤
│         [S3 + CloudFront]    ←── file storage & CDN                  │
│         [Redis TTL Store]    ←── presence & typing                   │
│         [Elasticsearch]      ←── message search                      │
└─────────────────────────────────────────────────────────────────────┘
```

### 📋 Implementation Steps
1. **Phase 1 (Week 1-2)**: Build WebSocket gateway with Redis connection registry
2. **Phase 2 (Week 3-4)**: Implement Kafka message pipeline + Cassandra persistence
3. **Phase 3 (Week 5-6)**: Add Redis Pub/Sub fan-out for real-time delivery
4. **Phase 4 (Week 7-8)**: Presence system, typing indicators, read receipts
5. **Phase 5 (Week 9-10)**: File uploads (S3 pre-signed URLs), message search (Elasticsearch)
6. **Phase 6 (Week 11-12)**: Load testing at 1M → 5M → 10M connections, tune partition strategy

### ⚠️ Caveats & Risks
- Redis Pub/Sub is fire-and-forget — clients MUST implement gap-fill on reconnect
- Cassandra partition sizing needs careful tuning (daily buckets may be too large for viral channels)
- This design assumes 2 AWS regions; global distribution adds complexity (CRDTs, conflict resolution)

### 📊 Confidence: **85/100**
The architecture follows proven patterns used by Discord, Slack, and WhatsApp. The remaining 15% uncertainty comes from partition tuning and exact infrastructure sizing, which require load testing.
