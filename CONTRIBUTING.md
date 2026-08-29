# Contributing

Thanks for improving **快速 Java Skill**.

The project values **defensible engineering depth** over longer prompts or larger technology lists.

## Good contributions

- A follow-up question that exposes a real architecture weakness.
- A production failure scenario with a concrete diagnostic path.
- A parameter-tuning method with measurable validation.
- A new Java / RAG / Agent pattern that appears repeatedly in interviews.
- A pressure-test scenario that catches shallow or unsafe behavior.
- A worked example that demonstrates a reusable reasoning pattern.

## Before changing behavior

Please update `tests/skill-evals.md` first when possible. Describe:

1. the prompt that exposes the weakness;
2. the expected behavior;
3. the failure signals.

Then change the smallest relevant reference.

## File ownership

| Area | File |
|---|---|
| project reconstruction | `references/project-reconstruction.md` |
| Java backend / distributed systems | `references/java-backend.md` |
| RAG / Agent | `references/rag-agent.md` |
| parameter baselines | `references/parameter-baselines.md` |
| interview red-team | `references/interview-red-team.md` |
| trigger / global behavior | `SKILL.md` |

Keep the entrypoint short. Do not duplicate a reference into `SKILL.md`.

## Evidence policy

Do not contribute fabricated personal metrics, outages, traffic, revenue, or production outcomes. Hypothetical examples must be labeled as assumptions, baselines, simulations, or benchmark plans.

## Parameter policy

Avoid statements like:

> “topK=20 is optimal.”

Prefer:

> “Start with candidate range X–Y under assumptions A/B, observe metrics M/N, and increase/decrease when symptom S appears.”

Version-specific library defaults should be checked against current official documentation before inclusion.

## Style

- Chinese explanations are preferred for interview-facing content.
- Keep standard technical terms in English where that improves precision.
- Prefer tables, short decision rules, and flow diagrams over long narrative prose.
- Explain trade-offs and switching conditions.
- Do not add technologies solely for prestige.

## Pull request checklist

- [ ] Behavior change has an eval scenario when appropriate.
- [ ] No unsupported universal parameter claims.
- [ ] No fabricated personal evidence.
- [ ] README links remain valid.
- [ ] `CHANGELOG.md` updated for user-visible behavior changes.
