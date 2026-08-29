# Fast Java Skill v1 Implementation Plan

> **For agentic workers:** implement this plan task-by-task; keep the Skill entrypoint concise and move depth into references.

**Goal:** Ship a professional first release of `fast-java-skill` that reconstructs vague Java Backend / Agent / RAG projects into production-grade designs and red-teams them for Chinese senior-engineer interviews.

**Architecture:** `SKILL.md` is a small discovery/router layer. Domain depth lives in focused files under `references/`. README is a public product page, not the runtime prompt. Evaluation scenarios are written before the Skill body so maintenance is driven by explicit failure cases.

**Tech Stack:** Markdown, Agent Skills-compatible YAML frontmatter, Mermaid, GitHub README conventions.

**Spec:** `docs/superpowers/specs/2026-08-29-fast-java-skill-design.md`

## Global Constraints

- Skill name is `fast-java-skill`.
- Default interview language is Chinese; keep standard English technical terms.
- Important decisions must withstand 3–6 layers of follow-up questions.
- Give parameter baselines, ranges, tuning direction, and validation methods; never present baselines as universal best practices.
- Do not invent unverifiable personal incidents, traffic, revenue, or measured business outcomes.
- Do not force Agent, RAG, Kafka, Redis, Kubernetes, Multi-Agent, or microservices when a simpler design is better.
- Keep `SKILL.md` concise and load heavy references only when relevant.

---

### Task 1: Define pressure tests first

**Files:**
- Create: `tests/skill-evals.md`

- [ ] Define scenarios for vague parameters, buzzword acceptance, missing request flow, overengineering, fabricated evidence, excessive discovery questions, missing evaluation/observability, and shallow interview follow-ups.
- [ ] For each scenario specify expected behavior and failure signals.
- [ ] Use these scenarios as the review checklist for the rest of the implementation.

### Task 2: Create the Skill entrypoint

**Files:**
- Create: `SKILL.md`

- [ ] Add valid YAML frontmatter with trigger-only `description` beginning with `Use when...`.
- [ ] Define the short core contract: discover → reconstruct → validate → red-team.
- [ ] Route domain work to the correct reference file.
- [ ] Define hard rules around evidence, parameter claims, questioning, and overengineering.

### Task 3: Implement domain references

**Files:**
- Create: `references/project-reconstruction.md`
- Create: `references/java-backend.md`
- Create: `references/rag-agent.md`
- Create: `references/interview-red-team.md`
- Create: `references/parameter-baselines.md`

- [ ] Project reconstruction owns discovery, end-to-end request flow, production failure analysis, observability, evaluation, trade-offs, and final output contract.
- [ ] Java reference owns JVM/concurrency/Spring/MySQL/Redis/MQ/ES infrastructure/distributed consistency and scale.
- [ ] RAG/Agent reference owns ingestion, chunking, embeddings, retrieval, reranking, evaluation, routing, graph/state/checkpoint, Tool safety, and prompt injection.
- [ ] Interview reference owns multi-layer follow-ups, answer grading, answer packaging, and resume-risk review.
- [ ] Parameter reference owns labeled starting baselines plus tuning and validation guidance.

### Task 4: Add a worked example

**Files:**
- Create: `examples/sports-commerce-rag.md`

- [ ] Start from a deliberately vague sports-commerce customer-service project.
- [ ] Show how the Skill identifies gaps and reconstructs architecture.
- [ ] Include realistic baseline parameters without claiming measured production results.
- [ ] Include at least one 4-layer interview follow-up chain.

### Task 5: Build the public README

**Files:**
- Modify: `README.md`

- [ ] Create centered title, positioning, badges, and concise value proposition.
- [ ] Explain the problem with a “surface question → deep follow-up” table.
- [ ] Add Mermaid workflow, capability matrix, installation, quick start, principles, repository structure, use cases, maintenance model, roadmap, and contribution links.
- [ ] Make installation wording careful about differences across Agent runtimes.

### Task 6: Add maintenance metadata

**Files:**
- Create: `docs/maintainer-guide.md`
- Create: `CONTRIBUTING.md`
- Create: `CHANGELOG.md`

- [ ] Explain progressive disclosure and how to add references without bloating `SKILL.md`.
- [ ] Require new behavior changes to add/update a pressure test first.
- [ ] Document v0.1.0 scope and future roadmap.

### Task 7: Verification

- [ ] Re-read `SKILL.md` and confirm the description contains triggers only, not workflow summary.
- [ ] Confirm every README path exists.
- [ ] Confirm parameter reference labels values as baselines, not guarantees.
- [ ] Confirm interview protocol requires 3–6 follow-up layers for core decisions.
- [ ] Confirm no file encourages fabricated personal evidence.
- [ ] Confirm no `TODO` / `TBD` placeholders remain in release files.
- [ ] Re-read repository homepage for install/use clarity.
