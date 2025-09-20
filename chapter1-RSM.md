Reliabiilty is measured by following dimensions:

example reliability matrix for a messaging app. ( ADCTFO)

| **Aspect**          | **What it means here**                         | **SLI (Metric)**                                               | **SLO (Target)**                             |
| ------------------- | ---------------------------------------------- | -------------------------------------------------------------- | -------------------------------------------- |
| **Availability**    | Service is up and accepting API calls          | % of successful API requests over 1 min                        | ≥ 99.99% monthly availability                |
| **Durability**      | Messages are never lost                        | Probability of data loss per year                              | ≤ 0.0001% annual message loss                |
| **Correctness**     | Messages delivered once and only once          | % of duplicate/undelivered messages                            | ≤ 1 in 10M messages duplicated or dropped    |
| **Timeliness**      | Messages arrive fast enough to be useful       | p99 end-to-end message delivery latency                        | ≤ 200 ms for intra-region, ≤ 1s cross-region |
| **Fault Tolerance** | System survives region/node failures           | Recovery Point Objective (RPO) / Recovery Time Objective (RTO) | RPO = 0 (no data loss), RTO ≤ 1 min failover |
| **Observability**   | Ability to detect issues before users complain | % of failures detected by monitoring vs user reports           | ≥ 95% detected by monitoring first           |

Think of “ACT-DO” → reliable systems ACT correctly and DO recover from failures.

A → Availability (is the system up?)

C → Correctness (are results accurate, e.g., no dupes/loss?)

T → Timeliness (are results delivered fast enough?)

D → Durability (is data safe and not lost?)

O → Operability/Observability (can humans monitor, detect, and fix issues easily?)


Scalability: 
| **Aspect**           | **Generic Definition**                                            | **Typical SLI (Metric)**             | **Example SLO (Target)**                      |
| -------------------- | ----------------------------------------------------------------- | ------------------------------------ | --------------------------------------------- |
| **Throughput**       | Work processed per unit time                                      | ops/sec, QPS, msgs/sec               | ≥ X ops/sec sustained                         |
| **Latency**          | Time to serve a request                                           | p95/p99 latency                      | ≤ Y ms at p99                                 |
| **Concurrency**      | Max users/clients/requests active simultaneously                  | # of concurrent sessions/connections | ≥ Z concurrent users                          |
| **Storage Growth**   | Ability to store and query growing data volume                    | Data size supported (TB/PB)          | Scale to N PB without manual intervention     |
| **Elasticity**       | Ability to handle sudden traffic spikes                           | Time to autoscale                    | Scale 2× load within T minutes                |
| **Geographic Scale** | Performance across distributed regions                            | Cross-region replication lag         | ≤ U seconds lag across regions                |
| **Hotspot Handling** | How the system deals with skewed access patterns (hot keys/nodes) | Max load handled per shard/partition | ≥ V ops/sec before rebalancing/auto-splitting |


Scalability is about SCALE-HG: Storage, Concurrency, Availability (throughput), Latency, Elasticity, Hotspots, Geography.Think of “SCALE-HG” → Systems must Cope with All Loads, Everywhere, handling Hotspots and Growth.

S → Storage Growth (data keeps piling up)

C → Concurrency (many users at once)

A → Availability under load → really Throughput (how many ops/sec)

L → Latency (speed per request)

E → Elasticity (handle sudden spikes)

H → Hotspots (skewed traffic)

G → Geographic scale (multi-region, replication lag)



Maintenabiliity:

| **Aspect**        | **What it means**                                                      | **SLI (Metric)**                                        | **SLO (Target)**                                                        |
| ----------------- | ---------------------------------------------------------------------- | ------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Operability**   | Ease of running day-to-day, monitoring, alerting, incident response    | % incidents auto-resolved / mean time to recover (MTTR) | MTTR ≤ 15 min, 80% alerts actionable                                    |
| **Simplicity**    | System complexity (code, architecture) and cognitive load              | # of components / lines of config / deps                | ≤ X services per team, minimal config duplication                       |
| **Evolvability**  | Ease of adapting to new requirements, feature addition, schema changes | Time to add new feature / schema change success rate    | Add small features in ≤ 2 weeks, 95% schema changes backward-compatible |
| **Testability**   | Ability to validate correctness via automated tests                    | % code covered / % releases with automated regression   | ≥ 80% coverage, 100% regression suite per release                       |
| **Observability** | Ability to measure system health and debug problems quickly            | % failures detected by monitoring vs. user reports      | ≥ 95% detected internally                                               |
| **Debuggability** | Ease of finding root cause in failures                                 | Median time to root cause (MTTRC)                       | ≤ 30 minutes                                                            |
| **Deployability** | Ease of safely rolling out and rolling back changes                    | % successful deploys, rollback time                     | ≥ 99% success, rollback < 5 min                                         |

👉 Quick phrase to remember:
“A well-maintained system is never ODD, it stays STEady.”
(helps tie STE-ODD into memor
