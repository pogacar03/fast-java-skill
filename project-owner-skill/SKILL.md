---
name: project-owner-skill
description: Use when preparing project introductions, resume-project narration, architecture-choice explanations, or project-defense answers for interviews.
---

# Project Owner Skill

## Overview

把“我用了什么技术”改造成“我为什么这样做决策”的 **Owner 视角项目讲述**。

核心原则：**先问题，后架构；先决策，后技术；先约束，后优劣。** 技术栈只能证明实现方式，不能充当项目主线。

## Core contract

拿到简历 bullet、项目笔记或已有口述后，先输出：

1. **Project Framing**：业务/用户问题、目标、关键约束、候选人的 Ownership 边界；
2. **3–5 个最高价值决策**：只选最能体现判断力的点，不做技术栈巡礼；
3. **信息缺口**：只标记会改变主线或选型结论的缺口；
4. 信息足够则直接继续；不足时最多问 3–5 个关键问题。

## Workflow

主线固定为：

```text
为什么做 / 解决什么问题
→ 当时最重要的约束
→ 我如何拆问题
→ 3–5 个关键决策
→ 为什么选 A、不选 B/C
→ 我主动接受了什么代价
→ 如何验证
→ 什么条件变化后会换方案
```

不要按 `Redis → Kafka → MySQL → K8s` 的顺序介绍项目。

## Owner Decision Card

每个主决策都整理为：

```text
【Decision】
Business Problem:
Constraints:
Options: A / B / C
Decision:
Why this fits:
Rejected Alternatives:
Trade-off:
Switching Condition:
Evidence / Validation:
```

比较的是**当前约束下的适配度**，不是抽象地判断“哪个技术更高级”。替代方案必须真实可行；不要为了凑 A/B/C 制造稻草人方案。

## Evidence boundary

所有内容按事实边界标记：

- `[真实做过]`：用户明确做过、实现过或有材料支持；
- `[合理设计]`：为了生产化完整性给出的架构补充或演进方案；
- `[待验证]`：QPS、延迟、准确率、成本收益、容量上限等尚缺证据的结论。

可以补幂等、重试、补偿、降级、监控、扩容等生产级设计，但**绝不能把 `[合理设计]` 改写成“我线上做过”**。没有测量证据时，给验证方案或更安全的表述，不编数字、事故和结果。

## Story packaging

完成决策卡后，再生成口述稿：

### 一句话定位

```text
为了解决 <问题>，我负责 <Ownership>，核心是通过 <1–2 个关键决策> 达成 <目标>。
```

### 30 秒

```text
业务问题 → 我的职责 → 1–2 个关键决策 → 最值得深挖的难点
```

### 2 分钟 Owner 版

```text
背景与约束
→ 我如何拆问题
→ 2–3 个关键决策及方案对比
→ 一个重要 Trade-off / 工程难点
→ 验证方式或结果边界
→ 架构演进 / Switching Condition
```

### Deep Dive

围绕 Decision Card 展开，不重复背整套项目。优先准备架构、选型、失败、规模、证据五类分支。

## Clarification rule

只有当缺失信息会改变以下内容时才追问：

- 项目目标或 Ownership；
- 主架构；
- 关键方案选择；
- 证据可信度。

一次最多 3–5 个问题。能用明确假设继续时，不为了“了解更多”发送长问卷。

## Hard rules

- **Top-down first**：最终项目介绍的开头不能是 Redis、Kafka、MySQL、Agent、RAG 或框架列表。
- **Decision over technology**：技术只作为决策的实现载体。
- **Selective depth**：主讲最多 3–5 个决策，其余留给追问。
- **Alternatives required**：重要选型至少有一个可信替代方案，并解释拒绝原因。
- **Trade-off required**：没有代价的“最优方案”视为不完整。
- **Switching condition required**：禁止无条件说“X 比 Y 好”。
- **Evidence over adjectives**：不用“高并发、高可用、效果很好”代替机制和证据。
- **No fake ownership**：不扩大用户真实承担的职责范围。

## Quality gate

每张主 Decision Card 按 5 项各 0–2 分：

| 项目 | 2 分标准 |
|---|---|
| Problem | 问题与目标具体 |
| Constraints | 约束能解释为什么需要做决策 |
| Alternatives | 至少一个可信替代方案并有比较 |
| Trade-off | 明确当前方案牺牲了什么 |
| Evidence + Switch | 有验证方法/证据边界，且知道何时换方案 |

**低于 7/10 的卡不能直接成为主讲点。** 先定向追问补齐；若用户没有更多事实，则降级为 supporting point，并明确假设或 `[待验证]`。
