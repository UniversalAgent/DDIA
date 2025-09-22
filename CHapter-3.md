# 📘 DDIA Chapter 3 – Storage & Retrieval (Interview Cheat Sheet)

---

## 1️⃣ Storage Engine Families

| Storage Engine | How It Works | Strengths | Weaknesses | Example Systems |
|----------------|-------------|-----------|------------|-----------------|
| **B-Trees** | In-place updates, sorted pages, WAL for durability | Balanced reads/writes, great for range queries | Random I/O → slower under heavy writes | MySQL InnoDB, Postgres |
| **LSM-Trees** | WAL → Memtable → SSTable → Compaction | High write throughput (sequential I/O), low write latency | Reads slower (SSTable lookup + compaction overhead) | Cassandra, RocksDB, HBase |
| **Hash Index** | Direct mapping key → location | O(1) point lookups | No range queries, collisions | Redis, Dynamo |
| **SSTables** | Immutable, sorted files on disk | Sequential writes, efficient compaction | Read amplification if many files | LevelDB, RocksDB |

**Mnemonic:** **BLoHS → B-Trees, LSM, Hash, SSTables**

---

## 2️⃣ Write Optimization Techniques

| Technique | Purpose | Used In | Trade-offs |
|-----------|---------|---------|------------|
| **WAL (Write-Ahead Log)** | Durability & crash recovery | Both B-Tree & LSM DBs | Adds latency (extra disk write) |
| **Memtable** | Buffer in-memory writes before flush | LSM-Tree | Data loss risk if WAL not written |
| **Compaction** | Merge SSTables, reclaim space | LSM-Tree | Expensive background process |
| **Buffer Pool / Page Cache** | Cache B-Tree pages in memory | B-Tree | Uses RAM heavily |

---

## 3️⃣ Index Types

| Index | Strength | Weakness | Example |
|-------|----------|----------|---------|
| **Primary** | Fast key lookups | Only 1 per table | Postgres PK |
| **Secondary** | Flexible queries | Slows writes | MongoDB secondary indexes |
| **Composite** | Multi-field queries | Order-dependent | MySQL composite index |

---

## 4️⃣ Trade-offs Summary

| Trade-off | B-Trees | LSM-Trees |
|-----------|---------|-----------|
| **Writes** | Random I/O, slower under heavy write load | Sequential, very fast |
| **Reads** | Efficient (logarithmic traversal) | Slower (may check many SSTables) |
| **Range Queries** | Excellent | Harder (need merged SSTables) |
| **Storage Efficiency** | Moderate, fragmentation possible | Better, compaction reclaims space |
| **Use Case** | OLTP, relational workloads | Logging, time-series, analytics |

---

## 5️⃣ Real-World Examples

| System | Storage Engine | Why |
|--------|----------------|-----|
| **Banking / Payments** | B-Tree (Postgres/MySQL) | ACID, range queries (balances, transactions) |
| **Messaging / Logs (Kafka)** | LSM-style log segments | High write throughput, sequential I/O |
| **Time-Series DB (InfluxDB, Cassandra)** | LSM-Tree | Write-heavy, append-only workload |
| **Caching (Redis)** | Hash / Key-Value | Fast lookups, no need for range queries |

---

## 6️⃣ Mnemonics

- **B = Balanced, L = Log-heavy**
- **W → M → S → C** = WAL → Memtable → SSTable → Compaction
- **BLoHS** = B-Tree, LSM, Hash, SSTable

---

✅ **Interview Tip:**  
When asked *“Which storage engine would you pick?”* →  
Answer using **QPSC framework** (Query Pattern, Scale, Consistency, Flexibility):  
- B-Tree if you need **range queries, balanced workload**  
- LSM if you need **high write throughput**  
- Hash if you need **fast point lookups**

nemonic: “BRaLS”

👉 B-Tree = Random
👉 LSM = Sequential

Just remember “BRaLS” → B = Random, L = Sequential.
(Pronounce it like “Brace” — as in brace yourself for choosing the right DB engine)

| **Theme**                     | **Sample Question**                                                                          | **What They’re Testing (Detailed)**                                                                                                                                                                                                                                           |
| ----------------------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Write Path Tradeoffs**      | *Why do LSM trees achieve faster writes than B-Trees?*                                       | ✅ Do you understand **sequential vs random I/O** at the storage layer? LSM → append-only, minimal seeks. B-Tree → in-place updates cause page splits and random writes. They want to see if you can reason from **disk mechanics → DB design → system tradeoffs**.            |
| **Read Amplification**        | *If LSM writes are so fast, why doesn’t every DB use LSM?*                                   | ✅ They’re checking if you see the **other side of the coin**: LSM trades write speed for **slower reads (SSTable fan-out, tombstones)**. Bloom filters + compaction mitigate but don’t eliminate. They want to see if you balance pros/cons, not blindly say “LSM is faster.” |
| **Consistency vs Durability** | *When is a write considered “committed” in LSM vs B-Tree engines?*                           | ✅ Testing if you know **durability mechanics**. B-Tree → committed once written in-place + WAL. LSM → committed once in WAL (memtable flush is deferred). This probes if you understand **when a DB can acknowledge success to the client**.                                  |
| **Workload Fit**              | *You’re designing a high-throughput time-series DB. Which storage engine is better and why?* | ✅ Can you **map access patterns to engines**? Append-only time-series data → perfect for LSM (fast sequential writes, range scans). If you said B-Tree, they’ll ask why → you should recognize it’s bad for hot writes. They’re testing your **pattern-to-engine intuition**. |
| **Space Amplification**       | *Why can LSM trees use more disk than B-Trees?*                                              | ✅ They want you to recall **compaction mechanics**: duplicate keys + tombstones exist until merged. B-Trees don’t have this overhead since they overwrite in place. Tests if you know **storage cost tradeoffs**.                                                             |
| **Latency Spikes**            | *Why do some LSM-based DBs (like Cassandra) occasionally see high read latency?*             | ✅ Testing if you connect **background tasks (compaction, GC, merges)** to **P99 latency**. Shows if you know why tail latencies spike, and how ops teams mitigate (tuning compaction, tiered storage).                                                                        |
| **Hybrid Approaches**         | *Can you imagine a DB that mixes both B-Tree and LSM techniques?*                            | ✅ They want to see if you can think beyond textbook categories → e.g., RocksDB (LSM + Bloom filters), WiredTiger (Mongo hybrid), PostgreSQL heap tables (B-Tree + WAL). Probes if you can **reason about real-world DB design evolution**.                                    |
| **Failure Recovery**          | *Why is WAL required even in LSM engines? Isn’t SSTable append-only?*                        | ✅ They’re checking if you see the **gap between theory & reality**: SSTables are immutable but memtables live in memory → crash = data loss. WAL ensures durability. They want to see if you grasp **the durability gap**.                                                    |

e
