---
title: ReAct 范式
type: concept
created: 2026-04-07
updated: 2026-04-07
sources: ["wiki/summaries/agentfold.md"]
tags: [Web Agent, 推理, Action, Reasoning]
---

# ReAct 范式

## 定义

ReAct（Synergizing Reasoning and Acting in Language Models，Yao et al., 2023）是当前 Web Agent 的主流范式，核心是"推理→行动→观察"的循环。

## 循环结构

```
Reason（推理）→ Act（行动）→ Observe（观察）→ Reason（推理）→ ...
```

每一步的推理内容、采取的行动、获得的观察结果**全部追加**到上下文中。

## 代表工作

WebThinker、WebDancer、WebSailor、WebExplorer 等主流 Web Agent 均基于 ReAct 架构。

## 致命缺陷：上下文饱和

ReAct 的 append-only 特性导致：
- 上下文随步数线性膨胀
- 早期关键信息被后期噪声淹没
- 长程任务（>50步）性能急剧下降

## AgentFold 的改进

AgentFold 在 ReAct 的循环中增加 `Fold` 步骤，将 append-only 变为**主动管理的动态工作空间**，从根本上解决上下文饱和问题。

## 参考

- 原始论文：Yao et al., "ReAct: Synergizing Reasoning and Acting in Language Models", ICLR 2023
- [[上下文饱和]]
