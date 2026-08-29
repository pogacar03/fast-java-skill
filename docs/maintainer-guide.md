# Maintainer Guide

This repository is designed as a **progressively disclosed Agent Skill**, not a single giant prompt.

## Mental model

```text
SKILL.md
  ├── decides when the skill applies
  ├── defines hard behavioral rules
  └── routes to references

references/
  ├── project reconstruction
  ├── Java backend
  ├── RAG / Agent
  ├── parameter baselines
  └── interview red-team

examples/
  └── demonstrate the behavior without becoming rules

tests/
  └── describe pressure scenarios that catch regressions
```

## Before changing behavior

1. Identify the failure mode.
2. Add or update a pressure scenario in `tests/skill-evals.md`.
3. Verify which reference should own the rule.
4. Change the smallest relevant file.
5. Re-read `SKILL.md` and ensure it did not grow into a domain encyclopedia.

## What belongs in SKILL.md

Keep only:

- trigger-oriented YAML frontmatter;
- project-wide invariants;
- routing to deeper references;
- short interaction contract.

Do **not** move long Java, RAG, Agent, or parameter explanations into `SKILL.md` just because they are useful. They should be loaded only when relevant.

## Description rule

The YAML `description` must answer:

> “When should an agent read this skill?”

It must not summarize the full workflow. Trigger-rich descriptions improve discovery; workflow-heavy descriptions can cause agents to shortcut the Skill body.

## Adding a new technology area

Prefer extending an existing reference when the concept belongs to an existing domain.

Create a new reference only if:

- the topic is repeatedly used;
- the existing reference would become difficult to scan;
- the topic has its own coherent questioning/evaluation model.

If adding `references/<new-domain>.md`, also update the routing table in `SKILL.md` and repository structure in `README.md`.

## Parameter maintenance

Parameter guidance must always include:

```text
baseline
reasonable exploration range
assumptions
metric(s)
tuning direction
validation method
```

Never add a value only because “everyone uses it”. If a parameter is tied to a specific library/version/default, verify current official documentation before documenting it.

## Examples vs evidence

Examples may contain hypothetical values clearly labeled as baselines or assumptions. They must not imply the repository author personally observed production incidents or measured business improvements unless such evidence is actually supplied and documented.

## Interview behavior maintenance

A new interview rule should improve at least one of:

- correctness;
- specificity;
- engineering failure reasoning;
- evidence/measurement;
- trade-off clarity;
- follow-up depth.

Do not turn `interview-red-team.md` into a static question bank. Questions should follow causally from the previous answer.

## Release checklist

Before a release:

- [ ] `SKILL.md` frontmatter parses visually and uses `name: fast-java-skill`.
- [ ] Description begins with `Use when...` and contains triggers only.
- [ ] All README links point to existing files.
- [ ] No new universal parameter claims were introduced.
- [ ] Core decisions still require 3–6 layers of follow-up.
- [ ] No fabricated personal evidence appears in examples or guidance.
- [ ] No `TODO` / `TBD` placeholders remain in release content.
- [ ] `CHANGELOG.md` records behavior changes.

## Suggested versioning

Use small semantic-style releases:

- patch: wording/clarity/example fixes without changing behavior;
- minor: new domain reference, new interview behavior, new evaluation scenario;
- major: changes to the Skill's core interaction contract or evidence policy.
