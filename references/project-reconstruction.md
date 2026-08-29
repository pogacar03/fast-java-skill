# Project Reconstruction

Use this reference when the user provides a project, project idea, resume bullet, architecture fragment, or partially implemented system and wants it deepened to senior-engineer interview depth.

## 1. Discovery first, but only the questions that matter

Start with four outputs:

1. **Project positioning** — business problem, primary user, core outcome.
2. **3–5 deep-dive modules** — the places where architecture or interview value actually lives.
3. **Engineering gaps** — missing flow, state, consistency, parameters, observability, evaluation, failure semantics.
4. **At most 3–5 questions** whose answers materially change the architecture.

If enough information exists, state assumptions and proceed. Do not send a long questionnaire.

## 2. Reconstruct the system as an end-to-end flow

For every important request, describe:

```text
Entry
→ routing / validation
→ core computation or retrieval
→ dependencies
→ state mutation if any
→ response / async continuation
```

For each hop answer:

- input and output;
- owning component;
- sync or async;
- timeout;
- retry conditions and maximum attempts;
- idempotency requirement;
- consistency requirement;
- cache behavior;
- side effects;
- fallback / degradation;
- logs, metrics, traces.

If an architecture diagram cannot support this walkthrough, it is incomplete.

## 3. Separate design facts from assumptions

Use three buckets when needed:

- **Known:** supplied by the user or visible in artifacts.
- **Assumed:** chosen to make the design concrete; state it explicitly.
- **To validate:** requires benchmark, load test, evaluation set, or production data.

Never convert assumptions into fake history.

## 4. Challenge every major technology choice

For each substantial component ask:

> What requirement does this solve that a simpler design cannot?

Compare at least two credible alternatives for important choices. Typical dimensions:

- capability;
- latency / throughput;
- consistency;
- operational complexity;
- developer productivity;
- cost;
- team familiarity;
- scale ceiling;
- migration cost.

Remove components that are not justified. Explain the trigger that would justify adding them later.

## 5. Production checklist

Every serious project should address the relevant items below.

### Reliability

- timeout budgets;
- retry scope and backoff;
- circuit breaker;
- fallback / graceful degradation;
- bulkhead / concurrency isolation;
- partial failure;
- dependency outage behavior.

### Consistency and idempotency

- source of truth;
- transaction boundary;
- duplicate request handling;
- message duplicate handling;
- write ordering;
- eventual consistency windows;
- compensation / reconciliation.

### Performance and scale

Always perform at least one scale thought experiment:

```text
10 QPS → 100 → 1000+
small dataset → 100x dataset
single instance → horizontal scale
```

Identify the first likely bottleneck and the telemetry that would prove it.

### Security

- authentication / authorization;
- tenant / user isolation;
- input validation;
- data exposure;
- audit logging;
- secret handling;
- side-effect authorization for Agent Tools.

### Observability

Minimum useful correlation identifiers may include:

```text
traceId
requestId
conversationId
agentExecutionId
userId / tenantId when appropriate
```

Track end-to-end and stage latency, error/timeout rate, dependency status, saturation, queues/backlog, cache hit rate, and domain-specific quality metrics.

## 6. Evaluation must be designed, not asserted

For claims such as “better accuracy”, “lower latency”, or “higher success rate” define:

1. metric;
2. baseline;
3. dataset / traffic slice;
4. measurement procedure;
5. acceptance threshold;
6. failure analysis.

If no measured result exists, create a reproducible evaluation or benchmark plan rather than inventing a number.

## 7. Failure scenarios

For a full project review, generate 5–10 realistic failures. Use this format:

```text
Symptom
→ first telemetry to inspect
→ narrowing steps
→ likely root causes
→ immediate mitigation
→ durable fix
→ prevention / alerting
```

Prefer failures specific to the project's dependencies, state model, and workload rather than generic “server down” examples.

## 8. Full project output contract

When the user asks to “整理项目”, “完整复盘”, or equivalent, produce:

1. One-line positioning
2. Business problem and scope
3. Overall architecture
4. Complete request flow(s)
5. Core modules and responsibilities
6. Data model / state model / interfaces
7. Key technology decisions and rejected alternatives
8. Parameter baselines and validation plan
9. Performance and scale
10. Reliability
11. Consistency / idempotency
12. Security
13. Observability
14. Evaluation
15. Engineering challenges
16. Production failure scenarios
17. Trade-offs and known limits
18. High-frequency interview questions
19. Deep follow-up chains
20. 30-second / 2-minute / Deep Dive answer pack
21. Resume-risk review and safer bullets

Do not output all 21 sections for every small question. Preserve conversational locality until the user requests consolidation.
