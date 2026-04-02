# 50 Grinding System Design Interview Questions

## Storage Internals & Data Structures

### 1. If B-Trees provide O(log n) lookups and are proven in production databases, why do modern systems like Cassandra and RocksDB prefer LSM Trees despite their read amplification problem?

**What I'm looking for:**
- Write amplification vs. read amplification trade-offs
- Sequential vs. random I/O patterns on SSDs and HDDs
- Compaction strategies and their impact on tail latencies
- Write-heavy vs. read-heavy workload characteristics
- Page cache behavior and OS-level optimizations

### 2. If columnar storage (Parquet, ORC) is so efficient for analytics, why don't we use it for OLTP databases?

**What I'm looking for:**
- Row-oriented vs. column-oriented access patterns
- CPU cache locality for transactional workloads
- Write amplification in columnar formats
- Tuple reconstruction overhead
- SIMD vectorization benefits vs. random access penalties

### 3. If memory-mapped files give you "free" caching via the OS page cache, why do databases like PostgreSQL and MySQL maintain their own buffer pools?

**What I'm looking for:**
- Control over eviction policies (LRU vs. Clock vs. 2Q)
- Page replacement algorithms and workload-specific tuning
- Double buffering problem
- fsync() and durability guarantees
- NUMA awareness and memory locality

### 4. If hash indexes provide O(1) lookups, why do most databases default to B-Tree indexes?

**What I'm looking for:**
- Range query support
- Ordered iteration requirements
- Hash collision handling overhead
- Memory vs. disk trade-offs
- Index scan vs. index seek patterns

### 5. If append-only logs are so efficient (Kafka, WAL), why don't we build all databases as append-only systems?

**What I'm looking for:**
- Space amplification and garbage collection
- Compaction overhead and I/O amplification
- Point-in-time recovery vs. current state queries
- Tombstone handling and deletion semantics
- Read performance degradation over time

## Concurrency & Hardware

### 6. If multi-threading makes systems faster, why does single-threaded Redis outperform most multi-threaded databases?

**What I'm looking for:**
- Context switching overhead
- Lock contention and cache line bouncing
- CPU cache coherency protocols (MESI)
- Memory barriers and fence instructions
- Event loop efficiency vs. thread pool overhead

### 7. If lock-free data structures eliminate contention, why aren't all concurrent systems lock-free?

**What I'm looking for:**
- ABA problem and memory reclamation
- CAS loop retry storms under high contention
- Memory ordering and happens-before relationships
- Complexity vs. maintainability trade-offs
- Hardware support (LL/SC vs. CAS)

### 8. If CPU caches are so fast, why do distributed systems often outperform single-machine systems with massive RAM?

**What I'm looking for:**
- Cache line size (64 bytes) and false sharing
- L1/L2/L3 cache hierarchy and latency numbers
- Memory bandwidth saturation
- NUMA effects on multi-socket systems
- Parallelism vs. cache locality trade-offs

### 9. If async I/O (io_uring, epoll) is more efficient, why do many high-performance systems still use thread-per-connection models?

**What I'm looking for:**
- State machine complexity vs. thread stack simplicity
- Callback hell and debugging difficulty
- CPU-bound vs. I/O-bound workload characteristics
- Thread pool sizing and work stealing
- Proactor vs. Reactor patterns

### 10. If NUMA-aware allocation improves performance, why don't all databases implement it by default?

**What I'm looking for:**
- Cross-socket memory access penalties
- Load balancing vs. locality trade-offs
- Migration overhead when threads move
- Complexity of NUMA-aware algorithms
- Workload predictability requirements

## Consistency Models & Distributed Systems

### 11. If eventual consistency is so scalable, why do banks still use strongly consistent databases?

**What I'm looking for:**
- Anomalies: lost updates, dirty reads, write skew
- Compensation logic complexity
- Audit and compliance requirements
- CAP theorem practical implications
- Business logic invariants

### 12. If Paxos/Raft provide strong consistency, why does Cassandra choose eventual consistency with tunable consistency levels?

**What I'm looking for:**
- Latency vs. consistency trade-offs
- Quorum reads/writes and coordination overhead
- Network partition handling
- Write availability during failures
- Read repair and anti-entropy mechanisms

### 13. If distributed transactions (2PC) guarantee ACID, why do microservices prefer Saga patterns?

**What I'm looking for:**
- Blocking coordinator problem
- Timeout and failure handling complexity
- Lock duration and throughput impact
- Partial failure and compensation
- Operational complexity of XA transactions

### 14. If linearizability is the strongest consistency model, why does Spanner use external consistency instead?

**What I'm looking for:**
- TrueTime API and clock synchronization
- Commit wait and uncertainty intervals
- Global ordering vs. per-key ordering
- Read-only transaction optimization
- Latency implications of waiting

### 15. If CRDTs provide conflict-free replication, why aren't they used everywhere?

**What I'm looking for:**
- Metadata overhead and tombstone accumulation
- Limited operation types (commutative/idempotent)
- Garbage collection challenges
- Semantic complexity for application developers
- State size growth over time

## Network & Protocols

### 16. If gRPC is faster than REST, why do most public APIs still use REST?

**What I'm looking for:**
- HTTP/2 multiplexing vs. HTTP/1.1 head-of-line blocking
- Binary vs. text protocol debugging
- Browser support and ecosystem maturity
- Schema evolution and backward compatibility
- Load balancer and proxy support

### 17. If UDP avoids TCP's overhead, why isn't it used for all latency-sensitive applications?

**What I'm looking for:**
- Packet loss and reordering handling
- Congestion control and fairness
- Application-level reliability implementation
- NAT traversal challenges
- Kernel bypass (DPDK) considerations

### 18. If HTTP/3 (QUIC) solves head-of-line blocking, why hasn't it completely replaced HTTP/2?

**What I'm looking for:**
- UDP blocking in corporate firewalls
- CPU overhead of userspace crypto
- Connection migration benefits
- Middlebox compatibility
- 0-RTT security trade-offs

### 19. If connection pooling reduces overhead, why do some systems prefer short-lived connections?

**What I'm looking for:**
- Connection state and memory overhead
- Load balancing and failover challenges
- Stale connection detection
- Connection leak risks
- DNS-based service discovery

### 20. If multiplexing (HTTP/2, QUIC) allows multiple streams, why do we still need connection pools?

**What I'm looking for:**
- Per-connection limits and flow control
- Head-of-line blocking at connection level
- Error isolation between streams
- Load distribution across servers
- Connection establishment cost amortization

## Caching & Performance

### 21. If CDNs cache content globally, why do we still need application-level caching (Redis, Memcached)?

**What I'm looking for:**
- Dynamic vs. static content
- Personalization and user-specific data
- Cache invalidation strategies
- Geographic distribution vs. data center locality
- Cost of cache misses and origin load

### 22. If write-through caching guarantees consistency, why do most systems use write-back caching?

**What I'm looking for:**
- Write latency and user experience
- Batching and write coalescing
- Durability vs. performance trade-offs
- Cache coherency protocols
- Failure handling and data loss risks

### 23. If LRU is the standard eviction policy, why do databases use Clock or 2Q algorithms?

**What I'm looking for:**
- Sequential scan pollution
- Implementation complexity and overhead
- Lock contention in concurrent environments
- Workload-specific patterns (80/20 rule)
- Approximate LRU benefits

### 24. If client-side caching reduces server load, why is it considered dangerous for consistency?

**What I'm looking for:**
- Stale data and invalidation challenges
- Cache coherency across clients
- TTL vs. event-based invalidation
- Network partition scenarios
- Thundering herd on expiration

### 25. If pre-warming caches improves performance, why don't all systems do it?

**What I'm looking for:**
- Cold start problem and prediction accuracy
- Memory pressure and eviction of useful data
- Time-to-first-byte vs. steady-state performance
- Workload predictability requirements
- Cost of warming vs. lazy loading

## Scaling & Architecture

### 26. If horizontal scaling is unlimited, why do we still optimize for vertical scaling?

**What I'm looking for:**
- Network overhead and serialization costs
- Coordination and consensus latency
- Data locality and cross-shard queries
- Operational complexity and failure modes
- Cost efficiency at different scales

### 27. If microservices improve scalability, why do they often have worse performance than monoliths?

**What I'm looking for:**
- Network latency vs. function call overhead
- Serialization/deserialization costs
- Distributed tracing and debugging complexity
- Transaction boundaries and consistency
- Service mesh overhead

### 28. If auto-scaling handles traffic spikes, why is it considered dangerous for stateful services?

**What I'm looking for:**
- State migration and rebalancing overhead
- Connection draining and graceful shutdown
- Quorum and consensus reconfiguration
- Cold start and warm-up time
- Thundering herd during scale-up

### 29. If sharding distributes load, why does it often make queries slower?

**What I'm looking for:**
- Scatter-gather pattern overhead
- Cross-shard joins and transactions
- Hot shard and skewed distribution
- Rebalancing and data movement
- Query planning complexity

### 30. If read replicas improve read throughput, why do they complicate the system?

**What I'm looking for:**
- Replication lag and stale reads
- Failover and split-brain scenarios
- Write scaling limitations
- Consistency guarantees
- Operational overhead of managing replicas

## Failure Handling & Resilience

### 31. If circuit breakers prevent cascading failures, why do they sometimes make things worse?

**What I'm looking for:**
- False positives and unnecessary blocking
- Thundering herd on circuit close
- Timeout tuning and failure detection
- Partial degradation vs. complete failure
- Backpressure propagation

### 32. If retries improve reliability, why do they often cause outages?

**What I'm looking for:**
- Retry storms and exponential backoff
- Idempotency requirements
- Timeout budget exhaustion
- Amplification of load during recovery
- Jitter and randomization

### 33. If timeouts prevent hanging requests, why are they so hard to configure correctly?

**What I'm looking for:**
- P99 vs. P50 latency considerations
- Cascading timeout chains
- Network vs. processing time
- Adaptive timeout strategies
- False positives and premature termination

### 34. If bulkheads isolate failures, why don't we isolate everything?

**What I'm looking for:**
- Resource utilization and waste
- Shared dependencies and bottlenecks
- Complexity of managing multiple pools
- Failure domain granularity
- Cost of over-provisioning

### 35. If health checks detect failures, why do they sometimes cause failures?

**What I'm looking for:**
- Health check overhead and resource consumption
- False negatives during startup
- Cascading failures from aggressive checks
- Deep vs. shallow health checks
- Load balancer behavior during checks

## Message Queues & Event Systems

### 36. If Kafka provides ordering guarantees, why is it so hard to maintain order in distributed consumers?

**What I'm looking for:**
- Partition-level vs. global ordering
- Consumer group rebalancing
- Parallel processing trade-offs
- Retry and dead letter queue handling
- Exactly-once semantics complexity

### 37. If message queues decouple systems, why do they often become the bottleneck?

**What I'm looking for:**
- Broker capacity and partition limits
- Consumer lag and backpressure
- Message size and serialization overhead
- Persistence and durability costs
- Operational complexity

### 38. If at-least-once delivery is easier, why do systems struggle with idempotency?

**What I'm looking for:**
- Duplicate detection and deduplication windows
- State management and side effects
- Distributed transaction coordination
- Performance overhead of idempotency checks
- Business logic complexity

### 39. If pub-sub scales better than point-to-point, why do we still use queues?

**What I'm looking for:**
- Work distribution vs. broadcast semantics
- Consumer group coordination
- Message retention and replay
- Backpressure and flow control
- Ordering guarantees

### 40. If event sourcing provides complete audit trails, why isn't it used everywhere?

**What I'm looking for:**
- Event store size growth
- Query complexity and projections
- Schema evolution challenges
- Snapshot strategies
- Eventual consistency implications

## Database Internals

### 41. If MVCC eliminates read locks, why do databases still have lock contention?

**What I'm looking for:**
- Write-write conflicts and row-level locks
- Version chain length and garbage collection
- Snapshot isolation anomalies
- Index locking and phantom reads
- Transaction ID wraparound

### 42. If denormalization improves read performance, why do we still normalize?

**What I'm looking for:**
- Update anomalies and consistency
- Storage overhead and write amplification
- Query flexibility and ad-hoc analysis
- Maintenance complexity
- Data integrity enforcement

### 43. If materialized views speed up queries, why aren't they used more?

**What I'm looking for:**
- Refresh overhead and staleness
- Storage cost and maintenance
- Incremental vs. full refresh
- Query optimizer complexity
- Write performance impact

### 44. If database triggers automate logic, why are they considered harmful?

**What I'm looking for:**
- Hidden business logic and debugging
- Performance unpredictability
- Cascading trigger chains
- Transaction scope complications
- Testing and version control challenges

### 45. If stored procedures reduce network round-trips, why do ORMs avoid them?

**What I'm looking for:**
- Vendor lock-in and portability
- Version control and deployment
- Testing and debugging complexity
- Language limitations
- Separation of concerns

## Observability & Operations

### 46. If distributed tracing shows bottlenecks, why is it so expensive to run in production?

**What I'm looking for:**
- Sampling strategies and accuracy
- Storage and indexing costs
- Performance overhead of instrumentation
- Context propagation complexity
- Cardinality explosion

### 47. If metrics tell you what's wrong, why do you still need logs?

**What I'm looking for:**
- Aggregation loss of detail
- Debugging specific requests
- Cardinality limitations
- Correlation and causation
- Cost and retention trade-offs

### 48. If chaos engineering finds weaknesses, why don't all companies practice it?

**What I'm looking for:**
- Risk of production impact
- Organizational maturity requirements
- Blast radius control
- Hypothesis-driven testing
- Cost of building resilience

### 49. If blue-green deployments eliminate downtime, why do we still use rolling deployments?

**What I'm looking for:**
- Resource cost of double infrastructure
- Database migration challenges
- Rollback complexity
- Traffic shifting strategies
- Stateful service considerations

### 50. If feature flags enable safe rollouts, why do they become technical debt?

**What I'm looking for:**
- Code complexity and branching logic
- Testing combinatorial explosion
- Flag cleanup and lifecycle management
- Performance overhead of evaluation
- Configuration management challenges

---

## How to Use These Questions

These questions are designed to:
1. **Challenge assumptions** - Force candidates to think beyond surface-level knowledge
2. **Explore trade-offs** - Every technology choice has costs and benefits
3. **Test depth** - Require understanding of internals, not just API usage
4. **Encourage discussion** - No single "right" answer; looking for reasoning

## Interview Tips

- **Don't expect perfect answers** - Look for thought process and depth
- **Probe deeper** - Ask "why" and "what if" follow-ups
- **Connect to experience** - Ask about real-world scenarios they've faced
- **Evaluate trade-off thinking** - Can they articulate costs and benefits?
- **Check for curiosity** - Do they want to understand the "why" behind things?

## Key Technical Concepts to Listen For

- **I/O Patterns**: Sequential vs. random, read vs. write amplification
- **Memory Hierarchy**: CPU cache, RAM, SSD, HDD latencies
- **Concurrency Primitives**: Locks, atomics, memory barriers, cache coherency
- **Consistency Models**: Linearizability, serializability, eventual consistency
- **Network Fundamentals**: TCP vs. UDP, HTTP versions, multiplexing
- **Failure Modes**: Partial failures, cascading failures, split-brain
- **Performance Metrics**: Latency (P50, P99, P999), throughput, utilization
- **Operational Concerns**: Observability, deployment strategies, cost
