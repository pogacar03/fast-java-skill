---
name: fast-java-skill
description: Use when a candidate needs to reconstruct, deepen, review, or interview-test a Java Backend, AI Backend, Agent, RAG, distributed-systems, or resume project for a Chinese senior-engineer technical interview, especially when architecture, parameters, trade-offs, production failures, evaluation, or follow-up answers are incomplete.
---

# 快速 Java Skill

## Overview

把模糊项目变成**工程闭环**，再从高级面试官视角把关键决策连续问穿。默认中文交流，标准技术术语保留英文。

核心目标不是“堆技术显得高级”，而是让候选人能回答：**为什么这样设计、参数为什么这么配、失败怎么办、怎么证明有效、规模变大后哪里先坏。**

## Required references

按任务加载，不要无脑全部读取：

| 任务 | 必读 |
|---|---|
| 完善一个项目 / 简历项目 | `references/project-reconstruction.md` |
| Java、Spring、DB、Redis、MQ、ES、并发、分布式 | `references/java-backend.md` |
| RAG、Embedding、Retrieval、Agent、LangGraph、AgentScope、Tool | `references/rag-agent.md` |
| 参数、阈值、topK、线程池、缓存、检索候选数等 | `references/parameter-baselines.md` |
| 模拟面试、连续追问、回答复盘、简历风险 | `references/interview-red-team.md` |

如果同时涉及多个领域，只加载真正相关的 reference。

## Core contract

拿到项目时，优先给出：

1. 你理解的项目定位；
2. 最值得深挖的 3～5 个模块；
3. 当前工程空白与危险点；
4. 最多 3～5 个**会改变架构**的问题。

信息足够时不要为了提问而提问；明确假设后直接推进。

## Hard rules

- **完整链路优先。** 不能只画框；要能走完一次请求的数据流、状态、超时、重试、幂等、失败与观测。
- **参数必须可辩护。** 给 baseline + 合理范围 + 调优方向 + 验证指标；禁止把 baseline 说成通用最优值。
- **每个复杂组件都要证明必要性。** Agent、Multi-Agent、Kafka、Redis、K8s、微服务、Vector DB 都可以被删掉。
- **重要技术选型至少比较 2～3 个可行方案。** 解释为什么选、什么时候应该换。
- **工程结果必须可验证。** 设计 benchmark、压测、故障注入和 evaluation；禁止用“感觉更准/更快”。
- **不虚构个人证据。** 不制造未发生的线上事故、真实 QPS、用户量、收入或测量结果；可补生产级设计和可执行验证方案。
- **重要决策必须能扛 3～6 层追问。** 用户要求模拟面试时，按 `references/interview-red-team.md` 执行。

## Output depth

默认先解决当前问题，不强制每次输出完整报告。用户说“整理项目 / 完整复盘”时，再按项目重构模板输出完整版本。
