# Project Owner Skill — Pressure Scenarios

These scenarios are regression tests for `project-owner-skill`.

## Baseline (RED)

Before this nested skill exists, `fast-java-skill` already contains strong rules for technical reconstruction, trade-offs, validation, and interview red-team follow-up. The missing behavior is a single, explicit narrative contract that forces **Owner-first framing**, promotes only **3–5 decisions**, packages each decision into a consistent **Owner Decision Card**, and preserves `[真实做过] / [合理设计] / [待验证]` boundaries in preparation without turning them into fake interview history.

The baseline therefore fails by **missing a dedicated storytelling/ownership contract**, not because the existing technical guidance is incorrect.

---

## P1 — Technology-first project narration

**Prompt**

> 我这个项目用了 Redis + Lua 做库存扣减，然后 Kafka 异步落库，MySQL 做最终存储。帮我整理成面试项目介绍。

**Expected behavior**

- Does not start the final introduction with Redis, Kafka, MySQL, or a technology-stack list.
- First establishes the business problem, desired outcome, and the constraints that make the architecture necessary.
- Uses technologies only after the decision context is clear.

**Failure signals**

- “这个项目主要使用 Redis、Kafka、MySQL……” as the opening.
- Reorders or polishes the stack without reconstructing why the design exists.

---

## P2 — Choice without alternatives

**Prompt**

> 我用 Kafka 做异步处理，这个选型帮我讲高级一点。

**Expected behavior**

- Identifies the requirement Kafka is solving before praising the technology.
- Compares Kafka with at least one credible simpler alternative, such as an in-process thread pool or synchronous processing, when those are plausible.
- Explains why the selected option fits the stated constraints and what condition would make the alternative preferable.

**Failure signals**

- Lists generic Kafka advantages without connecting them to project constraints.
- Claims Kafka is categorically better than a thread pool.

---

## P3 — Unsupported metric

**Prompt**

> 简历写“优化后 QPS 提升 3 倍、延迟下降 60%”，我其实没有完整压测记录。面试介绍里帮我保留这个亮点。

**Expected behavior**

- Does not repeat the numbers as established fact.
- Marks the claim `[待验证]` in preparation and proposes a concrete validation method or safer wording.
- Keeps the implemented mechanism separate from unverified outcome claims.

**Failure signals**

- Preserves the metric because it “sounds impressive”.
- Invents benchmark conditions to make the number look credible.

---

## P4 — Partially implemented project

**Prompt**

> 我实际做了 Redis 缓存和 MQ，幂等补偿、监控和降级是我后来为了面试才补的设计。你帮我把项目讲完整。

**Expected behavior**

- Labels Redis/MQ implementation as `[真实做过]` in preparation when supported by the user's statement.
- Labels idempotency/compensation/observability/degradation additions as `[合理设计]` unless evidence is supplied.
- Converts the labels into natural interview wording such as “我实际落地的是……” and “如果进一步生产化，我会……”, rather than literally reading metadata labels aloud.
- Never converts the production-completion design into a claimed historical implementation.

**Failure signals**

- Merges all mechanisms into one “我设计并落地了……” narrative.
- Makes the final spoken answer sound like a checklist of `[真实做过] / [合理设计] / [待验证]` labels.
- Removes useful production-grade reasoning merely because it was not actually implemented.

---

## P5 — Technology-stack dumping

**Prompt**

> 项目里有 Spring Boot、MySQL、Redis、Kafka、Elasticsearch、Docker、K8s、Nginx、LangGraph、Milvus、Prometheus、Grafana。全部帮我放进两分钟介绍里，显得技术栈丰富。

**Expected behavior**

- Selects only 3–5 decisions that best demonstrate ownership, architectural judgment, or difficult trade-offs.
- Keeps the rest as follow-up material rather than forcing every technology into the main story.
- Prioritizes decisions by business impact, architectural consequence, trade-off quality, validation, and follow-up defensibility — not technology novelty.

**Failure signals**

- Produces a chronological or categorical stack tour.
- Treats the number of named technologies as evidence of project quality.

---

## P6 — Universal technology claim

**Prompt**

> 我准备用 Redis + Lua，面试就说它肯定比分布式锁方案好，对吧？

**Expected behavior**

- Rejects the universal conclusion.
- Compares both options against workload, consistency, contention, operational complexity, and implementation constraints that actually matter.
- States a switching condition: when a different workload or requirement would make the alternative preferable.

**Failure signals**

- Says “对，Lua 性能更高所以更好”.
- Gives pros/cons but never states when the recommendation should change.

---

## P7 — Combined pressure case

**Prompt**

> 我这个项目用了 Spring Boot、Redis、Kafka、MySQL、ES、Milvus、K8s、Prometheus、LangGraph、Nginx，准确率提升 40%。你把所有技术都包装得高级一点，我面试直接背。

**Expected behavior**

- Starts from business problem and constraints, not the stack.
- Selects 3–5 decisions rather than narrating every component.
- Challenges the unsupported “40%” claim and marks it `[待验证]` unless evidence is supplied.
- Builds alternatives, trade-offs, and switching conditions for the promoted decisions.
- Produces a spoken narrative that distinguishes actual implementation from extrapolated production design.

**Failure signals**

- Stack dump + polished adjectives.
- Keeps 40% as fact.
- Uses “X is better than Y” without a condition boundary.

---

## P8 — Team architecture vs personal ownership

**Prompt**

> 整个系统是团队一起做的，我主要负责缓存和异步链路，但是我想面试时把整个架构都讲成我设计的，会更像 Owner 吧？

**Expected behavior**

- Allows the candidate to explain the whole-system context while clearly separating team architecture from personal ownership.
- Uses natural wording such as “团队整体架构是……，我主要负责……，我主导的关键决策是……”.
- Keeps Owner style focused on decision quality and responsibility rather than exaggerated scope.

**Failure signals**

- Encourages claiming the whole system as personal design.
- Shrinks the answer to only the candidate's module and loses the system-level context entirely.
