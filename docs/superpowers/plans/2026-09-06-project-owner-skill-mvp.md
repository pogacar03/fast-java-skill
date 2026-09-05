# Project Owner Skill MVP Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a minimal, reusable `project-owner-skill` inside `fast-java-skill` that turns bottom-up project descriptions into defensible Owner-style interview narratives.

**Architecture:** Keep the MVP self-contained in `project-owner-skill/`. The nested skill owns framing, decision extraction, evidence boundaries, and story packaging; the root `fast-java-skill/SKILL.md` only routes project-storytelling requests into it before Java/AI technical deep dives. Behavior is guarded by pressure scenarios rather than executable code tests because this repository is a prompt/skill package.

**Tech Stack:** Markdown skill files, GitHub repository, pressure-evaluation scenarios.

**Spec:** `docs/superpowers/specs/2026-09-06-project-owner-skill-design.md`

## Global Constraints

- Project introductions must be top-down: problem / goal / constraints before implementation technologies.
- Promote only 3–5 high-value decisions; do not narrate the full technology stack.
- Every major decision must contain credible alternatives, an explicit trade-off, and a switching condition.
- Distinguish `[真实做过]`, `[合理设计]`, and `[待验证]`; never convert extrapolation into claimed personal experience.
- Do not invent QPS, latency, accuracy, incidents, or other production evidence.
- Ask at most 3–5 clarification questions, and only when the missing answers materially change the story.
- Keep the MVP self-contained; do not add extra reference files unless required by repeated complexity.

---

### Task 1: Add pressure scenarios before implementation

**Files:**
- Create: `project-owner-skill/tests/pressure-scenarios.md`

**Interfaces:**
- Consumes: MVP behavior defined by the design spec.
- Produces: regression scenarios that define the expected output shape for the nested skill.

- [ ] **Step 1: Write the RED scenarios**

Create six scenarios covering: technology-first narration, missing alternatives, unsupported metrics, partial implementation boundaries, technology-stack dumping, and universal technology claims without switching conditions.

Each scenario must contain:

```markdown
## Pn — <behavior>

**Prompt**
> <candidate input>

**Expected behavior**
- <observable requirement>

**Failure signals**
- <observable failure>
```

- [ ] **Step 2: Verify the scenarios fail against the pre-MVP capability boundary**

Review the current root `SKILL.md` and existing references. Record the baseline gap at the top of the test file: the repository has technical reconstruction and interview red-team rules, but no dedicated contract that requires Owner framing, 3–5 Owner Decision Cards, or the three evidence labels as one coherent story-packaging workflow.

Expected result: **RED by missing dedicated behavior contract**, not by a syntax error.

- [ ] **Step 3: Commit**

```bash
git add project-owner-skill/tests/pressure-scenarios.md
git commit -m "test: add project owner skill pressure scenarios"
```

---

### Task 2: Implement the minimal nested skill

**Files:**
- Create: `project-owner-skill/SKILL.md`
- Test: `project-owner-skill/tests/pressure-scenarios.md`

**Interfaces:**
- Consumes: resume bullets, project notes, or an existing interview answer.
- Produces: project framing, 3–5 Owner Decision Cards, targeted clarifications when needed, evidence labels, and a one-line / 30-second / 2-minute / Deep-Dive answer pack.

- [ ] **Step 1: Implement the smallest skill that satisfies the RED scenarios**

Use valid skill frontmatter:

```yaml
---
name: project-owner-skill
description: Use when a candidate needs to explain a resume project, project introduction, architecture choice, or interview project answer from an Owner perspective rather than as a bottom-up technology list.
---
```

The body must define these sections in this order:

1. `Overview`
2. `Core contract`
3. `Workflow`
4. `Owner Decision Card`
5. `Evidence boundary`
6. `Story packaging`
7. `Clarification rule`
8. `Hard rules`
9. `Quality gate`

The Owner Decision Card must contain exactly these semantic fields:

```text
Business Problem
Constraints
Options
Decision
Why this fits
Rejected Alternatives
Trade-off
Switching Condition
Evidence / Validation
```

The quality gate scores five dimensions at 0–2 points each: problem definition, constraints, alternatives, trade-off, evidence/switching condition. Cards below 7/10 must not become primary talking points until clarified or explicitly downgraded to a weaker supporting point.

- [ ] **Step 2: Check the skill against all six pressure scenarios**

For every scenario, verify the skill contains an explicit instruction that makes the expected behavior more likely and the listed failure signal less likely. In particular verify:

```text
technology-first -> top-down framing rule
missing alternatives -> Options + Rejected Alternatives
unsupported metric -> [待验证] + no invented evidence
partial implementation -> three evidence labels
stack dump -> only 3–5 decisions
universal claim -> Switching Condition
```

Expected result: all six scenarios are covered by an explicit structural rule, not vague prose.

- [ ] **Step 3: Refactor for compactness**

Remove duplicate explanations already owned by `references/project-reconstruction.md` or `references/interview-red-team.md`. Keep this skill focused on narrative ownership and decision packaging.

- [ ] **Step 4: Commit**

```bash
git add project-owner-skill/SKILL.md
git commit -m "feat: add project owner storytelling skill"
```

---

### Task 3: Route fast-java-skill through the Owner layer and verify integration

**Files:**
- Modify: `SKILL.md`
- Test: `project-owner-skill/tests/pressure-scenarios.md`

**Interfaces:**
- Consumes: root requests about resume projects, project introductions, project storytelling, architecture explanation, and project defense.
- Produces: routing into `project-owner-skill` first, then only the relevant Java/AI technical references for deeper follow-up.

- [ ] **Step 1: Add a routing rule to root `SKILL.md`**

Add `project-owner-skill/SKILL.md` to the task/reference routing table for project narration requests. Add one concise integration paragraph saying:

```text
When the user asks how to introduce, narrate, defend, or reframe a project, first apply project-owner-skill to establish the top-down Owner story. Then load only the Java/AI references needed to deepen the selected decisions. Do not duplicate the Owner workflow in the root skill.
```

Do not rewrite the existing reconstruction or red-team references.

- [ ] **Step 2: Verify integration boundaries**

Check that:

```text
project-owner-skill = framing / ownership / decision story
project-reconstruction = technical system reconstruction
interview-red-team = follow-up attack / grading
```

Expected result: no duplicated large workflow block in root `SKILL.md`.

- [ ] **Step 3: Verify file structure and frontmatter**

Confirm these paths exist:

```text
project-owner-skill/SKILL.md
project-owner-skill/tests/pressure-scenarios.md
```

Confirm `project-owner-skill/SKILL.md` frontmatter contains only a valid hyphenated `name` and a trigger-focused `description`.

- [ ] **Step 4: Final content review**

Run a manual regression review against all six pressure scenarios plus one combined scenario containing 10+ technologies, an unsupported 40% improvement claim, and a request to “包装高级一点”. The expected answer contract must still force selective decisions, evidence boundaries, and top-down narration.

- [ ] **Step 5: Commit**

```bash
git add SKILL.md project-owner-skill/
git commit -m "feat: integrate project owner skill with fast java skill"
```
