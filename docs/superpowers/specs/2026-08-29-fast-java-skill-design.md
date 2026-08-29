# Fast Java Skill Design

## Goal

Build `fast-java-skill` as a maintainable, public Agent Skill for Chinese-language senior Java Backend + Agent/RAG interview preparation. It should reconstruct vague project ideas into production-grade engineering designs, then red-team them with multi-layer technical interview questions.

## Product Positioning

The repository is not a one-shot prompt dump. It is a reusable skill package with:

- a short discoverable `SKILL.md` entrypoint;
- deeper domain references loaded only when needed;
- realistic examples;
- pressure-test scenarios for skill maintenance;
- a polished README that works as the public project homepage.

Primary audience: candidates targeting Java Backend, AI Backend, Agent Engineer, and RAG/LLM application roles in Chinese technical interviews.

## Core Design Principles

1. **Engineering depth over buzzwords.** Every technology must justify why it exists.
2. **Concrete parameters with evidence.** Provide baselines, ranges, tuning direction, and evaluation methods.
3. **End-to-end request flow.** Architecture must include inputs, outputs, timeout, retry, idempotency, cache, consistency, observability, and failure behavior.
4. **Interview red-team depth.** Important decisions should support 3–6 layers of follow-up questioning.
5. **No fabricated personal evidence.** The skill may construct production-grade designs, PoCs, benchmarks, failure simulations, and evaluation plans, but must not invent unverifiable personal incidents, QPS, revenue, or measured business outcomes.
6. **Progressive disclosure.** Keep `SKILL.md` concise; move heavy domain material to `references/` so agents do not consume the entire repository on every invocation.

## Repository Structure

```text
fast-java-skill/
├── README.md
├── SKILL.md
├── references/
│   ├── project-reconstruction.md
│   ├── java-backend.md
│   ├── rag-agent.md
│   ├── interview-red-team.md
│   └── parameter-baselines.md
├── examples/
│   └── sports-commerce-rag.md
├── tests/
│   └── skill-evals.md
├── docs/
│   ├── maintainer-guide.md
│   └── superpowers/
│       └── specs/
├── CHANGELOG.md
└── CONTRIBUTING.md
```

## SKILL.md Contract

`SKILL.md` must contain valid YAML frontmatter with:

- `name`: `fast-java-skill`
- `description`: trigger-only wording beginning with `Use when...`; it must describe when an agent should load the skill rather than summarizing the workflow.

The body should remain concise and point to domain references based on the task:

- project reconstruction → `references/project-reconstruction.md`
- Java / distributed systems → `references/java-backend.md`
- RAG / Agent → `references/rag-agent.md`
- interview simulation → `references/interview-red-team.md`
- parameter questions → `references/parameter-baselines.md`

## Interaction Contract

When a user gives a project, the skill should first return:

1. the interpreted project positioning;
2. the 3–5 modules most worth deep-diving;
3. current engineering gaps;
4. at most 3–5 questions whose answers materially change architecture.

If enough information already exists, it should state explicit assumptions and proceed without asking questions for their own sake.

The reconstruction output should cover, where applicable:

- business problem and scope;
- architecture and complete request flow;
- data model and interfaces;
- technology choices and rejected alternatives;
- parameters and tuning rationale;
- performance and scale;
- reliability and consistency;
- security and Agent tool safety;
- observability;
- evaluation;
- common production failures and troubleshooting;
- trade-offs;
- 30-second, 2-minute, and deep-dive interview answers;
- resume-risk review.

## README Information Architecture

The README should look like a mature open-source project homepage without unnecessary decoration. It should include:

1. centered project title and concise positioning;
2. lightweight badges;
3. the problem it solves;
4. a Mermaid workflow diagram;
5. capability matrix;
6. installation for `~/.agents/skills/`, Claude-style skill directories, and generic prompt-based environments;
7. quick-start example;
8. design principles;
9. repository structure;
10. common use cases using collapsible sections;
11. maintenance guidance;
12. roadmap and contribution section.

## Domain Reference Boundaries

### project-reconstruction.md
Owns discovery, architecture reconstruction, request-flow analysis, production failure analysis, observability, evaluation, and final project output format.

### java-backend.md
Owns JVM, concurrency, Spring transactions, MySQL, Redis, Kafka/RocketMQ, Elasticsearch infrastructure, idempotency, distributed consistency, scaling, and Java runtime questions.

### rag-agent.md
Owns ingestion, chunking, embeddings, retrieval, hybrid search, reranking, RAG evaluation, Agent routing, state, graph/checkpoint, tools, retries, prompt injection, and Human-in-the-loop.

### interview-red-team.md
Owns interviewer mode, 3–6 layer questioning, answer grading, follow-up generation, 30-second/2-minute/deep-dive answer packaging, and resume danger checks.

### parameter-baselines.md
Owns clearly labeled starting baselines and tuning heuristics. It must repeatedly state that baselines require project-specific evaluation rather than presenting them as universal best practices.

## Skill Evaluation Strategy

`tests/skill-evals.md` will contain pressure scenarios intended to catch common regressions, including:

- accepting technology choices without asking why;
- giving vague parameter advice;
- turning baselines into fake universal truths;
- producing architecture boxes without request flow;
- overusing Multi-Agent / Kafka / microservices;
- inventing personal production metrics;
- asking too many discovery questions;
- failing to design evaluation and observability;
- stopping after one-level interview questions.

For each scenario the file should define expected behavior and failure signals so future maintainers can manually or agentically re-run the scenarios.

## Non-Goals

- It is not a complete Java interview encyclopedia.
- It is not a replacement for implementing and benchmarking a real system.
- It does not fabricate personal work history.
- It should not force every project to use Agent, RAG, MQ, Redis, Kubernetes, or microservices.

## Success Criteria

The first release is successful when:

- the repository can be cloned directly into a common skills directory;
- an agent can discover the skill from its description;
- a vague Java/Agent project is transformed into a coherent engineering design;
- outputs contain concrete parameter baselines plus validation methods;
- architecture decisions are challenged with credible alternatives;
- major project claims can survive at least 3 layers of follow-up questioning;
- README makes installation and use obvious in under two minutes.
