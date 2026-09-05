# Project Owner Skill — MVP Design

Date: 2026-09-06

## Goal

Add a reusable `project-owner-skill` under `fast-java-skill` that converts resume bullets, project notes, or an existing interview answer into an **Owner-style, top-down project narrative**.

The skill must optimize for interview defensibility rather than rhetorical polish: start from business problem and constraints, surface architecture decisions, compare credible alternatives, state trade-offs, and keep real experience separate from production-grade extrapolation.

## Relationship to fast-java-skill

Two-layer model:

```text
project-owner-skill
  -> project framing / owner narrative / decision trade-offs
  -> output defensible interview story

fast-java-skill
  -> consumes that story
  -> deepens Java / Redis / MySQL / MQ / concurrency / Agent / RAG details
  -> red-team follow-up
```

`project-owner-skill` is domain-agnostic. `fast-java-skill` remains the technical specialization layer.

## MVP Inputs

Primary:

1. Resume project bullets.
2. Existing project introduction / interview answer.

Supplementary:

3. Longer project notes, architecture fragments, or implementation details.

The skill should not require a full project dump before it can start.

## Core Workflow

### 1. Project Framing

Extract:

- business/user problem;
- desired outcome;
- key constraints;
- candidate's ownership boundary.

### 2. Owner Decision Extraction

Select only the 3-5 decisions that best demonstrate ownership and engineering judgment. Do not narrate every technology used.

### 3. Owner Decision Cards

For each decision produce:

1. Business Problem
2. Constraints
3. Options (2-3 credible alternatives)
4. Decision
5. Why this option fits the constraints
6. Why alternatives were rejected
7. Trade-off accepted
8. Switching Condition
9. Evidence / validation status

Internal score: 10 points total — problem definition, constraints, option comparison, trade-off, evidence/switching condition; 2 points each. Decisions below 7/10 require targeted clarification before becoming a primary talking point.

### 4. Gap Detection

Detect especially:

- bottom-up technology-stack narration;
- choices without alternatives;
- claims without evidence;
- unclear ownership boundary;
- slogans such as “high concurrency/high availability” without mechanism;
- unjustified complexity;
- likely follow-up holes.

### 5. Targeted Interview

Ask at most 3-5 questions whose answers materially improve the story. Do not send a generic questionnaire.

### 6. Production Completion

The skill may propose missing production-grade mechanisms such as idempotency, retry, degradation, observability, consistency handling, and scale evolution, but every such statement must be labeled as one of:

- `[真实做过]`
- `[合理设计]`
- `[待验证]`

Never convert a design suggestion into claimed personal experience.

### 7. Story Packaging

Generate:

- one-line project positioning;
- 30-second introduction;
- 2-minute Owner introduction;
- Deep Dive structure;
- 3-5 Owner Decision Cards;
- likely follow-up entry points.

## Hard Rules

1. **Top-down first.** The project introduction begins with the problem, goal, or constraint — not Redis, Kafka, MySQL, Agent, RAG, or framework names.
2. **Decision over technology.** Technologies are evidence of a decision, not the narrative spine.
3. **Alternatives are mandatory for major decisions.** Compare at least two credible options and explain the switching condition.
4. **Trade-offs are mandatory.** A decision is incomplete until the downside is explicit.
5. **No fake ownership.** Separate real work, extrapolated design, and items requiring validation.
6. **Evidence over adjectives.** Prefer a measurable validation method; never invent production numbers.
7. **Selective depth.** Promote only the highest-value 3-5 decisions; keep the rest as follow-up material.

## MVP Folder Layout

```text
project-owner-skill/
  SKILL.md
  tests/
    pressure-scenarios.md
```

Keep the MVP self-contained. Do not add extra references until the main skill becomes too large or repeated patterns justify extraction.

## MVP Test Scenarios

The skill should be tested against at least these behaviors:

1. A resume bullet that starts with Redis/Kafka should be reframed around the business problem first.
2. A user-provided architecture choice without alternatives should trigger credible A/B comparison rather than generic praise.
3. An unsupported QPS/latency claim should be marked as needing evidence rather than repeated as fact.
4. A partially implemented project should preserve `[真实做过] / [合理设计] / [待验证]` boundaries.
5. A project with 10+ technologies should select only 3-5 high-value decisions instead of producing a stack dump.
6. A major decision should include a switching condition, not a universal “X is better than Y” conclusion.

## fast-java-skill Integration

After the MVP exists, update the root `SKILL.md` so that requests about project introductions, project storytelling, resume project narration, or Owner-style project defense first use the `project-owner-skill` framing, then load Java/AI backend references only for the relevant technical deep dive.

Do not duplicate the entire Owner workflow inside root `SKILL.md`; keep the root integration as a routing rule.

## Out of Scope for MVP

- UI or web app;
- automatic resume parsing from arbitrary file formats;
- domain-specific knowledge packs beyond existing fast-java references;
- generating fabricated metrics or production incidents;
- a large multi-file framework.

## Success Criteria

The MVP is successful when a weak bottom-up answer such as:

> “I used Redis + Lua and Kafka for inventory deduction.”

is transformed into a concise story that explains:

- what problem existed;
- what constraints mattered;
- what alternatives were considered;
- why the selected design fit better;
- what cost it introduced;
- how the choice would be validated;
- when another design would become preferable;
- which parts were actually implemented by the candidate.
