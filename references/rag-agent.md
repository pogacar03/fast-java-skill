# RAG & Agent Deep Dive

Use this reference when the project contains RAG, embeddings, vector search, hybrid retrieval, reranking, LLM orchestration, Agent routing, LangGraph, AgentScope, MCP, Tools, or multi-step reasoning.

## RAG: reconstruct both ingestion and query paths

A RAG design is incomplete unless both paths are clear.

### Ingestion

```text
Source document
→ parse / OCR only if needed
→ clean / normalize
→ structure detection
→ chunking
→ metadata enrichment
→ embedding
→ index / upsert
→ versioning / reindex strategy
```

For each stage define failure behavior and reprocessing/idempotency semantics.

### Query

```text
User query
→ intent / filter extraction if needed
→ query rewrite / normalization if justified
→ lexical + vector retrieval
→ fusion
→ dedup / diversity control
→ rerank
→ context selection
→ prompt assembly
→ LLM answer
→ citation / grounding check
```

Do not jump directly from “用户问题” to “Vector DB”.

## Chunking

Prefer business/structural boundaries before arbitrary token boundaries when documents have reliable structure.

Possible hierarchy:

```text
heading / section
→ paragraph
→ sentence
→ token-length fallback
```

For semantic chunking, explain the actual mechanism: sentence/window embeddings, adjacent similarity or breakpoint scoring, minimum/maximum size constraints, and boundary selection.

Always question:

- what one chunk should be able to answer;
- whether a chunk remains understandable when retrieved alone;
- whether product/document identity is retained;
- whether overlap creates duplicate retrieval noise;
- whether parent-child retrieval is better than making every chunk large.

## Embedding

For the selected model define:

- model name/version;
- output dimension;
- language/domain fit;
- maximum input length;
- normalization requirements;
- similarity metric compatibility;
- embedding cost/latency if externally hosted;
- migration/re-embedding plan when the model changes.

Do not equate larger dimension with automatically better retrieval.

## Retrieval

Distinguish responsibilities:

- **BM25 / lexical:** exact terms, SKU, model number, error code, product names.
- **dense vector:** semantic paraphrases and natural-language intent.
- **metadata filter:** hard constraints such as product/category/tenant/version/access control.
- **hybrid fusion:** combine complementary retrieval channels.
- **reranker:** spend more compute on a smaller candidate set for better final ordering.

For hybrid retrieval explain why fusion is needed and compare simple weighted score fusion with rank-based methods such as RRF where relevant.

## Retrieval evaluation

Create a labeled query set with expected relevant document/chunk(s). Use metrics appropriate to the retrieval objective:

- Recall@K / Hit Rate;
- Precision@K when noise matters;
- MRR for first-good-result quality;
- NDCG for graded relevance;
- latency and candidate cost.

Do not optimize generation prompt before checking whether the correct evidence is actually retrieved.

## Generation evaluation

Potential dimensions:

- faithfulness / groundedness;
- answer relevance;
- context relevance;
- citation correctness;
- unsupported-claim rate;
- refusal / escalation correctness;
- task-specific success rate.

Separate retrieval failures from generation failures during error analysis.

---

# Agent design

## First question: why Agent?

Before designing Agent orchestration, classify the task:

| Pattern | Use when |
|---|---|
| Deterministic Workflow | steps and branches are known ahead of time |
| State Machine | explicit state transitions and recovery dominate |
| ReAct-style Agent | next tool/action genuinely depends on intermediate observations |
| Plan-and-Execute | task benefits from an explicit multi-step plan and execution/replanning |
| Multi-Agent | independent roles/context/tool boundaries justify separate agents |

Do not use Multi-Agent just because multiple modules exist.

## Router: ReAct vs Plan-and-Execute

A reasonable routing policy can consider:

- number of expected steps;
- dependency between steps;
- whether the task can be solved with one/few tool calls;
- need for replanning;
- cost/latency budget;
- side-effect risk.

Example principle:

```text
simple lookup / short tool loop → direct or ReAct
multi-stage task with explicit dependencies → Plan-and-Execute
high-risk deterministic operation → workflow/state machine, not free-form Agent
```

The exact threshold should be evaluated from traces rather than treated as a fixed universal rule.

## State

For graph/stateful Agent systems, define the minimum durable state needed, such as:

```text
conversationId
user/tenant identity
intent
task/plan
current node
retrieved context
tool results
execution history
retry counters
permissions / confirmation state
artifact references
```

Do not store everything forever. Separate conversational memory, execution checkpoint state, and business source-of-truth data.

## LangGraph / AgentScope orchestration

Be able to explain:

- node responsibility;
- node input/output;
- edge / conditional-edge decision;
- checkpoint boundary;
- what can be replayed safely;
- interrupt / resume semantics;
- retry scope;
- where deterministic business logic lives vs LLM reasoning.

A checkpoint is not a distributed transaction. External Tools may need idempotent replay, persisted operation status, or reconciliation.

## Tool / MCP safety

Tool schema validation is not authorization.

For side-effecting operations, insert deterministic gates:

```text
LLM proposal
→ schema validation
→ identity / authorization
→ reference validation
→ business-rule validation
→ idempotency / version check
→ confirmation or policy gate when required
→ execution
→ durable result / audit log
```

For every Tool define:

- input/output schema;
- timeout;
- retry eligibility;
- idempotency key;
- permission scope;
- rate limit;
- audit fields;
- error taxonomy;
- result size limit.

Never blindly retry a non-idempotent write after an ambiguous timeout.

## Agent failure modes

At minimum consider:

- infinite/repeated tool loop;
- hallucinated tool or invalid parameters;
- duplicated side effects;
- slow/unavailable tool;
- partial plan execution;
- model timeout / rate limit;
- context or token explosion;
- stale checkpoint;
- prompt injection from retrieved/tool content;
- privilege escalation through tool arguments;
- model/provider degradation.

Controls can include max steps, per-tool budgets, typed errors, bounded retries, circuit breakers, state-machine gates, model fallback, content isolation, tool allowlists, checkpointing, and human confirmation.

## Prompt injection / untrusted context

Treat retrieved documents and Tool outputs as untrusted data, not system instructions. Keep policy/instructions separated from evidence, minimize tool permissions, and validate all side-effect parameters outside the LLM.

## Observability

Trace an Agent turn as nested spans where possible:

```text
request
├── router
├── retrieval
│   ├── lexical
│   ├── vector
│   └── rerank
├── model
└── tool(s)
```

Useful metrics include:

- total and per-stage P50/P95/P99;
- token input/output and estimated cost;
- model error/fallback rate;
- tool success/timeout/retry rate;
- loop step count;
- retrieval hit quality;
- checkpoint/resume count;
- user-visible task success.

## Agent evaluation

Build scenario-based test sets that label expected:

- route;
- tool(s);
- tool arguments / constraints;
- required evidence;
- expected final outcome;
- unsafe actions that must not occur.

Evaluate not only final text quality but execution correctness, tool selection, argument correctness, side-effect safety, latency, token cost, and recovery behavior.
