# Parameter Baselines & Tuning

This file provides **starting points for discussion and experiments**, not universal best practices. Every value must be validated against the project's data distribution, latency SLO, dependency capacity, hardware, model, and traffic pattern.

Use this structure whenever giving a parameter:

```text
Baseline → reasonable exploration range → why → metric → tuning direction → stop condition
```

## RAG ingestion

### Chunk size

A practical first experiment for product/support documents:

```text
target: 200–400 tokens
min:    ~80–120 tokens
max:    ~500–700 tokens
overlap: 30–60 tokens or about one sentence
```

Use shorter chunks for atomic FAQ/spec facts and longer chunks for procedures that lose meaning when split.

Tune with:

- Recall@K / MRR;
- duplicate-result rate;
- context token usage;
- answer faithfulness;
- boundary-error examples.

If retrieval returns the right section but insufficient context, increase semantic completeness or use parent-child retrieval before blindly increasing all chunks.

### Semantic breakpoint

Avoid claiming a universal cosine threshold. Prefer dataset-relative methods such as lower-similarity percentiles or change-point heuristics, constrained by min/max chunk size.

A first experiment might compare:

```text
fixed structural chunking
vs bottom 10% similarity breakpoints
vs bottom 20% similarity breakpoints
```

Evaluate on labeled queries rather than choosing the prettiest chunks manually.

## Retrieval

### Candidate stages

A useful initial experiment for medium-sized customer-service RAG:

```text
lexical/vector candidates per channel: 20–50
fusion result considered for rerank: 10–30
final context sent to LLM: 3–8 chunks
```

Increase candidates when Recall@K is weak; decrease when latency/cost rises without recall gains. If the correct chunk appears before reranking but disappears afterward, fix reranker/context-selection behavior rather than increasing ANN candidates.

### Vector ANN

For HNSW-style search, distinguish:

- index-build parameters affecting graph quality/storage/build cost;
- query candidate breadth affecting recall/latency;
- final `k` affecting returned result count.

Do not copy `m`, `ef_construction`, or `num_candidates` from another system without measuring recall/latency on the actual index. Start from the engine/model defaults where sensible, then benchmark.

### Hybrid fusion

Do not start with arbitrary score weights unless score distributions are understood. Rank-based fusion such as RRF is often easier to reason about across BM25 and vector scores; its constants still require evaluation.

Validate with query slices such as:

- exact SKU/model/error codes;
- semantic paraphrases;
- mixed exact + semantic questions;
- hard metadata constraints.

## Reranking

A reasonable first design is:

```text
retrieve/fuse 20–50
rerank 10–30
keep 3–8
```

Reranker latency must be measured separately. If P95 becomes unacceptable, reduce candidate count, batch where supported, choose a lighter reranker, or skip rerank on obvious exact-match queries.

## Agent execution budgets

For open-ended Agent loops, establish explicit limits. Example starting experiment:

```text
max reasoning/tool steps: 6–12
per-tool retry: normally 0–2 depending on idempotency/error class
overall execution deadline: derive from user-facing SLO
```

High-risk side effects should not gain more freedom because the step budget is larger. Deterministic validation gates remain mandatory.

Tune from traces:

- successful task step distribution;
- repeated-call rate;
- timeout rate;
- token cost;
- user-visible success rate.

If most successful tasks finish in ≤3 steps, a 20-step default is probably wasteful and risky.

## Timeout and retry

Never choose each timeout independently. Start from an end-to-end latency budget and allocate it across stages.

Example reasoning for an interactive request:

```text
end-to-end budget
├── routing / validation
├── retrieval
├── rerank
├── model
└── tools
```

Retries consume the same budget. Retry only transient, classified failures and add exponential backoff/jitter when appropriate. Writes with ambiguous outcomes require idempotency or status lookup before retry.

## Java executors

Do not use a universal thread-pool formula as truth.

For CPU-bound work, concurrency is usually near available CPU capacity after measurement. For I/O-bound work, concurrency can be higher but must be capped by downstream connection pools, rate limits, memory, and queueing delay.

Always define:

```text
core/max or virtual-thread concurrency gate
queue type/capacity
rejection policy
per-task timeout
bulkhead boundary
```

Tune from CPU utilization, runnable threads, queue wait, task duration, connection-pool saturation, GC/allocation pressure, and dependency P95/P99.

## HTTP / DB / Redis pools

Connection pool size is a concurrency budget, not a performance knob to maximize.

Start from measured concurrent in-flight demand and dependency capacity. Check:

- active/idle/waiting connections;
- acquisition wait time;
- downstream saturation;
- timeout rate;
- request queueing.

A larger pool can worsen an overloaded database or service.

## Redis TTL

TTL should derive from business staleness tolerance and invalidation strategy. For many ordinary caches, introduce randomized jitter to avoid synchronized expiry; the actual TTL may range from seconds to hours depending on the data.

Always discuss:

- freshness requirement;
- miss cost;
- write/update frequency;
- hot-key risk;
- invalidation event availability;
- stale-data tolerance.

## Kafka partitions

Do not say “X partitions is standard”. Estimate from:

```text
required throughput
÷ sustainable throughput per partition/consumer path
+ headroom
```

Then account for ordering key distribution, consumer parallelism, broker topology, storage/network, and future growth. Too many partitions also have metadata, file, recovery, and rebalance costs.

## Elasticsearch shards

Never derive shard count only from document count.

Collect:

- projected primary index size;
- node count and memory;
- query concurrency;
- ingestion rate;
- vector-index memory/disk behavior;
- recovery/rebalance target;
- growth horizon.

Choose a test topology, load representative data, benchmark, then revise. Use aliases/reindex strategies so topology can evolve.

## Evaluation sample size

For an early project PoC, a few hundred carefully labeled queries often provide more value than thousands of unreviewed synthetic questions. Expand toward larger representative sets as the system stabilizes.

Always stratify by failure-prone categories rather than reporting only one aggregate number.

## The interview rule

When an interviewer asks “为什么这个参数是 X？”, the strongest answer structure is:

```text
X 不是通用最优值
→ 它是我们的初始 baseline
→ 当时主要考虑 A/B constraints
→ 我们会在 range R 内做实验
→ 用 metric M 判断
→ 如果 symptom S 出现就往 direction D 调
```

That answer demonstrates engineering judgment instead of memorization.
