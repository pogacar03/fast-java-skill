# Interview Red Team

Use this reference when the user asks for mock interviews, likely questions, follow-up questions, answer review, project defense, or resume-risk analysis.

## Mode selection

Use one of three modes based on the user's request.

### Interviewer Mode

Ask one question at a time. Do **not** reveal the ideal answer before the user responds.

After the answer:

1. identify the strongest part;
2. identify the biggest technical hole;
3. ask the next question that naturally attacks that hole;
4. continue 3–6 layers for core decisions before switching topics.

### Answer Pack Mode

Provide prepared answers at three depths:

- **30 seconds** — conclusion + 1–2 reasons;
- **2 minutes** — architecture/decision + parameter/evidence + trade-off;
- **Deep Dive** — mechanism, failure mode, scale, alternatives, evaluation.

### Review Mode

Take the user's existing answer and grade it, then rewrite only after explaining the missing engineering logic.

## Follow-up chain design

A good chain is causal, not a random question list.

Example:

```text
Why Elasticsearch instead of Pinecone?
→ Why does this project need lexical + semantic retrieval?
→ How do BM25 and vector results get fused?
→ Why RRF rather than weighted score fusion?
→ What candidate counts / fusion parameters would you start with?
→ How do you prove the setting is better?
→ What changes at 10M documents or during an ES latency spike?
```

Each answer should create the next question.

## Attack surfaces

For every project, search for questions in these categories:

### Architecture

- Why this component?
- Why not a simpler design?
- What owns the source of truth?
- What is sync vs async?
- Where is the transaction boundary?

### Parameters

- Exact baseline?
- Why this range?
- What metric is sensitive to it?
- What happens when it is too high/low?
- How was it evaluated?

### Failure

- What if the dependency times out?
- Can a retry duplicate side effects?
- What if the request succeeds remotely but the client times out?
- What happens after process/node restart?

### Scale

- First bottleneck at 10x?
- What saturates: CPU, memory, pool, shard, partition, connection, queue, LLM quota?
- How would you know before users complain?

### Trade-offs

- What did this design make worse?
- Which assumption would make you choose the alternative?
- What complexity would you remove in a smaller system?

### Evidence

- How is “better” measured?
- Baseline and dataset?
- Offline vs online metric?
- How do you distinguish retrieval failure from generation failure?

## Answer grading rubric

Score important answers across five dimensions, each 0–2:

| Dimension | 0 | 1 | 2 |
|---|---|---|---|
| Correctness | wrong / confused | mostly correct | technically sound |
| Specificity | slogans | some detail | concrete flow/parameters |
| Engineering | happy path only | mentions failures | clear failure/consistency/ops reasoning |
| Evidence | no validation | vague testing | explicit metric/benchmark/eval |
| Trade-off | no alternatives | names alternative | explains switching condition |

Then give:

```text
Score: X/10
Best point:
Biggest hole:
Likely next question:
How to upgrade the answer:
```

Do not over-penalize a short first answer if it is strategically concise; judge whether it provides a strong hook for deeper explanation.

## Resume danger review

Flag claims with high interview risk:

- percentages without metric definition;
- QPS/latency without test conditions;
- “高并发/高可用/强一致” without mechanism;
- “引入 Kafka/Redis/Agent 提升性能” with unclear causality;
- “解决幻觉” as an absolute claim;
- claiming ownership of a system the candidate cannot walk end to end;
- named technologies that cannot be justified.

For quantitative claims ask:

```text
Metric definition?
Baseline?
Sample size / traffic?
Test environment?
Measurement period?
What changed besides your intervention?
Can it be reproduced?
```

If evidence is incomplete, produce a safer statement focused on implemented mechanism and validation method rather than preserving an unsupported number.

## Project introduction packaging

### 30-second project intro

Structure:

```text
业务问题
→ 我的核心职责/模块
→ 关键技术决策
→ 最值得深挖的一个难点
```

Do not list the whole technology stack.

### 2-minute project intro

Structure:

```text
背景与约束
→ architecture/request flow
→ 2–3 key decisions
→ one engineering challenge
→ evaluation / reliability
→ trade-off or next step
```

### Deep Dive

Prepare branches for:

- architecture;
- parameter choices;
- consistency/idempotency;
- performance;
- failure troubleshooting;
- technology alternatives;
- evaluation.

## Interview honesty without losing depth

If a project is a design, PoC, or partially implemented system, keep the answer technically strong without inventing personal production history.

Prefer precise language such as:

- “这个模块我的方案是……”
- “PoC 里我验证的是……”
- “生产化时最大的风险是……”
- “这个参数我会先以 X 为 baseline，再通过 Y 指标调。”

The goal is to be defensible under follow-up, not to sound impressive for one sentence.
