# Java Backend Deep Dive

Use this reference when a project depends materially on Java runtime behavior, Spring, databases, cache, messaging, Elasticsearch infrastructure, concurrency, or distributed-system guarantees.

## Core questioning pattern

For every backend component, do not stop at “用了什么”. Ask in order:

1. **Why does it exist?**
2. **What is the hot path?**
3. **What state does it own?**
4. **What are its failure semantics?**
5. **What parameter becomes limiting first?**
6. **How is the bottleneck observed and proven?**
7. **What changes at 10x / 100x scale?**

## Java runtime and concurrency

When relevant, cover:

- JVM heap sizing assumptions and allocation pressure;
- GC choice only when workload evidence makes it relevant;
- thread model: platform threads, Virtual Threads, Reactor, or bounded executor;
- blocking vs non-blocking dependencies;
- thread-pool queue type, core/max size, rejection strategy, and task isolation;
- `CompletableFuture` fan-out/fan-in, timeout, cancellation, exception propagation;
- HTTP client / DB / Redis connection-pool saturation;
- context propagation for trace/user/request metadata;
- CPU-bound vs I/O-bound behavior.

Do not claim “Virtual Threads make it faster” generically. Explain what blocking wait they simplify, what downstream capacity still limits throughput, and how concurrency is capped.

## Spring

Deep-dive topics may include:

- AOP proxy boundary and self-invocation;
- `@Transactional` rollback rules and propagation;
- transaction scope vs remote calls;
- Spring bean lifecycle only where it affects the project;
- MVC vs WebFlux choice;
- resilience at client boundaries;
- validation, exception mapping, and request idempotency.

For transactions, always identify the exact local DB transaction boundary. Do not imply a Spring transaction automatically covers Redis, Kafka, or remote RPC.

## MySQL / PostgreSQL

For important queries specify:

- access pattern;
- expected selectivity;
- index design and index order;
- execution-plan risks;
- transaction isolation requirement;
- lock scope;
- pagination strategy;
- write conflict strategy;
- schema evolution / online migration concerns.

Common follow-ups:

- why this composite index order;
- when it becomes a covering index;
- why offset pagination degrades;
- optimistic vs pessimistic locking;
- unique constraints as idempotency backstop;
- deadlock investigation and retry boundary.

## Redis

Treat Redis as a system with failure and consistency trade-offs, not a magic acceleration layer.

For a cache path define:

```text
key
value shape
ttl + jitter
read/write pattern
source of truth
miss path
invalidations
consistency tolerance
hot-key risk
big-key risk
```

Question cache penetration, breakdown, avalanche, hot key, cluster slot distribution, serialization size, and stale-data windows.

For distributed locks define:

- lock key granularity;
- lease/watchdog behavior;
- ownership token;
- unlock safety;
- critical-section duration;
- retry / wait policy;
- whether a database uniqueness constraint or CAS is still required.

## Kafka / RocketMQ

Never add MQ only to make an architecture look distributed.

If MQ is justified, define:

- producer event semantics;
- topic and partitioning key;
- ordering requirement;
- consumer group;
- offset/ack behavior;
- duplicate delivery handling;
- idempotency key;
- retry and dead-letter policy;
- backlog metric and capacity estimate;
- schema evolution;
- reconciliation for partial failure.

Ask whether async messaging is solving latency decoupling, traffic smoothing, fan-out, durability, or cross-service eventual consistency. If none applies, synchronous execution may be simpler.

## Elasticsearch

Separate Elasticsearch's roles:

1. lexical/full-text retrieval;
2. vector retrieval;
3. hybrid retrieval;
4. analytics/log search if present.

For application search discuss where relevant:

- mapping and field types;
- analyzer / tokenizer;
- exact keyword fields vs analyzed text;
- `dense_vector` dimension and similarity;
- HNSW ANN trade-offs;
- `k` and `num_candidates`;
- metadata filters;
- shard/replica decisions based on data volume and workload;
- refresh interval and ingestion latency;
- bulk indexing;
- reindex / alias migration;
- segment merge and disk/memory pressure;
- slow-query diagnosis.

Never give a shard count solely from document count; require approximate index size, query concurrency, node topology, and growth assumptions.

## Idempotency and distributed consistency

For every write command ask:

```text
Can the client retry?
Can the gateway retry?
Can MQ redeliver?
Can the Tool/Agent call twice?
Can a timeout hide a successful write?
```

Use appropriate layers such as:

- client request id / idempotency key;
- unique DB constraint;
- compare-and-set / version column;
- persisted operation state;
- transactional outbox;
- deduplication table;
- reconciliation job.

Do not rely on a distributed lock alone as the only correctness guarantee when durable uniqueness can be enforced at the source of truth.

## Scale review

For every reconstructed backend project, be able to answer:

- What is the first bottleneck at 10x traffic?
- Which pool/queue/index/partition saturates?
- What metric exposes saturation?
- What is the safe degradation behavior?
- What needs horizontal scaling vs redesign?

Prefer evidence-driven capacity reasoning over arbitrary “high concurrency” claims.
