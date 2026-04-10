# AgentFold 深度阅读报告

**源文件**: AgentFold_2510.24699.pdf  
**arXiv ID**: 2510.24699  
**发表日期**: 2025-10-29  
**机构**: Tongyi Lab, Alibaba Group  
**模型基座**: Qwen3-30B-A3B（监督微调，无持续预训练或 RL）  
**报告生成**: 2026-04-09

---

## 1. 基础信息

| 字段 | 内容 |
|:---|:---|
| **标题** | AgentFold: Long-Horizon Web Agents with Proactive Context Management |
| **中文标题** | AgentFold：基于主动上下文管理的长期 Web Agent |
| **arXiv** | 2510.24699 |
| **发表日期** | 2025-10-29 |
| **作者** | RuiYe, Zhongwang Zhang, Kuan Li, Huifeng Yin（中电52所/上交），以及其他阿里巴巴同门 |
| **通讯作者** | Huifeng Yin (yr991129@sjtu.edu.cn), Yong Jiang (yongjiang.jy@alibaba-inc.com) |
| **机构** | Tongyi Lab, Alibaba Group |
| **开源代码** | https://github.com/Alibaba-NLP/DeepResearch |
| **博客** | https://tongyi-agent.github.io/blog |

**核心模型**: AgentFold-30B-A3B（30B 推理塔 + 3B 动作塔，通过 SFT 训练）

---

## 2. 一句话概括

AgentFold 发明了一种**主动上下文折叠（Proactive Context Folding）**机制，让 Web Agent 在长程任务中动态管理自己的记忆——每步可以选择保留细节（粒度凝缩）或抽象掉整块子任务（深度整合），从而在 100 步后仅用约 7k tokens 的上下文达到甚至超越 671B 大模型的效果。

---

## 3. 科学问题

### 核心矛盾：上下文完整性与简洁性的根本两难

LLM Web Agent 在长程任务中面临一个本质张力：

| 方案 | 代表工作 | 优势 | 致命缺陷 |
|:---|:---|:---|:---|
| **ReAct append-only** | WebThinker, WebDancer, WebSailor | 信息完整性好 | 上下文饱和，100步后关键信息存活率仅 **36.6%** |
| **固定全量摘要** | 同期相关工作 | 上下文始终干净 | 每步不可逆压缩，500步后存活率仅 **0.66%** |

论文的 critical insight 是：**人类认知并非每步全量摘要，而是在关键节点进行回顾性整合（retrospective consolidation）**——丢弃无关步骤、提炼中间发现、抽象关键洞察。AgentFold 要在 Agent 中复现这一机制。

---

## 4. 技术路线

### 4.1 核心架构：动态认知工作空间

AgentFold 将 Agent 上下文重新设计为一个**动态认知工作空间**，由两部分组成：

```
[State Summary 1] [State Summary 2] ... [State Summary N] | [Latest Interaction]
     （多尺度状态摘要，有序序列）                      （最新一步完整记录）
```

**Latest Interaction**：上一轮完整的思考-行动-观察，保留原始细节供短期决策参考。

**Multi-Scale State Summaries**：由若干摘要块组成的有序序列，每个摘要块可以是：
- **粒度凝缩**（granular condensation）：s(x_{t-1})，单步压缩，保留关键结论
- **深度整合**（deep consolidation）：s(x_i, x_j)，跨越多步的抽象结论，清除已完成子任务的噪声

### 4.2 每步四元响应

每步 AgentFold 深度推理后同时输出四个部分：

```
[Thinking] + [Folding Directive] + [Explanation] + [Tool Call]
 思考       折叠指令               解释            工具调用
```

**Folding Directive（折叠指令）** 的双尺度性质：
1. **作为粒度凝缩**：将 Latest Interaction 结晶为新状态摘要，追加到序列
2. **作为深度整合**：将 Latest Interaction 与若干先前摘要融合，替换为更粗粒度的抽象

### 4.3 折叠操作示例

假设一个"调查AI新闻"的 5 步任务：

```
Step 1: 搜索 AI 新闻 → State Summary 1: "搜索到10条AI新闻"
Step 2: 打开第1条 → State Summary 2: "第1条是关于GPT-5的报道"
Step 3: 阅读详情 → （深度整合）→ 合并 Step 1-3 → State Summary: "GPT-5发布，性能提升30%"
Step 4: 对比竞品 → 新 State Summary
Step 5: 写报告 → Latest Interaction
```

### 4.4 训练方式：Fold-Generator 数据收集 + SFT

**核心挑战**：现有数据集中没有展示"何时折叠、折叠多少"的轨迹。

**解法**：Fold-Generator 流水线
1. 用 LLM 生成带有折叠动作的轨迹
2. 加入拒绝采样（rejection sampling）确保多尺度折叠质量
3. 在 Qwen3-30B-A3B 上做 SFT（纯监督微调，无 RL）

---

## 5. 核心发现

### 5.1 主要 Benchmark 结果

| Benchmark | AgentFold-30B-A3B | DeepSeek-V3.1-671B-A37B | GPT-4o | o4-mini |
|:---|:---:|:---:|:---:|:---:|
| **BrowseComp** | **36.2%** | 30.0% | — | 34.0% |
| **BrowseComp-ZH** | **47.3%** | — | — | — |
| **WideSearch** | **62.1%** | 54.5% | 49.9% | 58.9% |
| **GAIA** | **67.0%** | — | — | — |

> AgentFold-30B-A3B 在所有基准上均超越或匹配规模大 20 倍的开源模型（DeepSeek-V3.1-671B）和领先的闭源 Agent（o4-mini）。

### 5.2 上下文效率

- **100 步后上下文仅约 7k tokens**（上限 128k 的 5%）
- **可扩展至 500 步**，而 ReAct baseline 在 200 步时已几乎失效
- 关键信息存活率：**100 步 92%** vs ReAct 的 36.6%

### 5.3 关键发现总结

1. **上下文管理本身就是一种能力**：让 30B 模型在特定任务上超越 671B 模型
2. **Acting 与 Reflecting 互相强化**：制定行动和制定折叠指令形成自我调节循环
3. **多尺度折叠优于单一尺度**：既能保护细粒度细节，又能抽象掉已完成子任务
4. **纯 SFT 足够，无需 RL**：只要训练数据质量够好，监督微调即可习得折叠能力

---

## 6. 创新点

### 创新点 1：主动上下文折叠范式（Proactive Context Folding）

首次在 Agent 中引入**认知科学启发的回顾性整合机制**，将上下文视为需要主动管理的动态工作空间，而非被动填满的日志。

### 创新点 2：双尺度折叠操作

提出**粒度凝缩 + 深度整合**的双尺度折叠，使 Agent 可以在同一框架下：
- 保护细粒度关键细节不被后续压缩影响
- 抽象掉已完成子任务的全部噪声

### 创新点 3：四元响应设计

每步同时输出 Thinking + Folding + Explanation + Action，将"何时折叠、折叠多少"的决策内化到模型能力中，而非依赖外部规则。

### 创新点 4：Fold-Generator 数据流水线

无需人工标注，通过 LLM + 拒绝采样自动生成展示多尺度折叠的轨迹数据。

---

## 7. 与现有方法对比

### 7.1 与 ReAct 范式对比

| 维度 | ReAct | AgentFold |
|:---|:---|:---|
| 上下文管理 | Append-only，饱和后降级 | 主动折叠，按需压缩 |
| 关键信息存活（100步） | 36.6% | 92% |
| 长程任务表现 | 随步数增加急剧下降 | 稳定维持，可达 500 步 |
| 每步输出 | Thought + Action | Thought + Fold + Explanation + Action |

### 7.2 与固定摘要方法对比

| 维度 | 固定全量摘要 | AgentFold |
|:---|:---|:---|
| 信息丢失风险 | 每步不可逆丢失（500步后 0.66%） | 多尺度选择，最小化关键信息丢失 |
| 灵活性 | 固定策略，无法自适应 | 自适应选择折叠尺度 |
| 子任务抽象能力 | 无 | 深度整合可抽象完整子任务 |

### 7.3 与其他 Long-Horizon Web Agent 对比

| 方法 | Context 管理 | 核心问题 |
|:---|:---|:---|
| WebThinker | 探索式，未解决饱和 | 长步数性能下降 |
| WebDancer | 类似 ReAct | 同上 |
| AgentFold | 多尺度主动折叠 | 从根本上解决饱和问题 |

---

## 8. 局限性与发展方向

### 8.1 主要局限性

1. **折叠决策的可解释性有限**：模型学会折叠，但折叠决策的内在逻辑难以解释和调试
2. **Fold-Generator 的质量依赖 LLM 能力**：底层 LLM 不够强时，拒绝采样开销大
3. **仅验证了 Web Agent 场景**：在其他模态（代码生成、机器人控制）的泛化性未知
4. **无 RL 强化**：SFT 学到的折叠策略可能不是全局最优，有待在更大搜索空间探索
5. **评估基准有限**：主要在 BrowseComp/WideSearch/GAIA，缺乏真实复杂任务的更多验证

### 8.2 未来发展方向

1. **自适应折叠阈值**：模型自动学习何时该折叠，而非依赖手工触发
2. **跨模态泛化**：将折叠机制扩展到多模态 Agent（视觉+语言+代码）
3. **RL 增强**：用 PPO/GRPO 等方法进一步优化折叠策略
4. **折叠可视化工具**：帮助开发者理解和调试 Agent 的折叠决策
5. **与其他 Agent 架构结合**：如与 Memory Bank、Plan-and-Execute 范式结合

---

## 9. 可复现要素

### 9.1 关键组件

| 组件 | 描述 | 是否开源 |
|:---|:---|:---|
| **AgentFold 代码** | GitHub: Alibaba-NLP/DeepResearch | ✅ |
| **Fold-Generator 流水线** | 自动生成折叠轨迹数据 | ✅ |
| **模型权重** | AgentFold-30B-A3B | 部分开源 |
| **训练数据** | Fold-Generator 生成 | 待发布 |
| **Benchmark 数据** | BrowseComp, BrowseComp-ZH, WideSearch, GAIA | 公开可用 |

### 9.2 复现关键点

1. **基座模型**：Qwen3-30B-A3B（需申请或使用开源版本）
2. **SFT 配置**：标准 causal LM SFT，无特殊 RL
3. **折叠决策的监督信号**：通过 Fold-Generator 的拒绝采样生成
4. **Benchmark**：BrowseComp/WideSearch 为公开基准，GAIA 在 HuggingFace 可获取

### 9.3 实验环境参考

- 预训练框架：未详细说明（估计用 Megatron-LM 或类似）
- 上下文长度：上限 128k tokens
- 训练步数：未公开具体数字

---

## 10. 对你的研究的意义

### 10.1 与 Agent 工程的关联

1. **上下文管理是 Agent 工程的核心瓶颈**：本文用实验证明，在信息密集型任务中，上下文管理能力可以直接转化为性能提升。对构建生产级 Agent 系统的工程实践有直接指导意义。

2. **小模型+好的上下文管理 > 大模型+差的上下文管理**：这一结论对 Agent 系统架构选型有重要影响——与其追求更大的推理模型，不如投资更好的上下文管理机制。

3. **多尺度抽象是处理长程任务的必由之路**：无论在哪个领域（网页、代码、文档），长程任务都需要层次化的信息管理。AgentFold 的双尺度折叠提供了一个可借鉴的框架。

### 10.2 可借鉴的具体思路

1. **在你的 Web Agent 或 RAG 系统中引入多尺度状态摘要机制**：不是简单截断或全量摘要，而是让 Agent 自己决定何时摘要、摘要多少。

2. **将 Fold-Generator 的拒绝采样思路迁移到其他 Agent 数据生成场景**：自动生成带有"元动作"（如折叠）的轨迹数据，是提升 Agent 能力的有效途径。

3. **在你的 Agent 评估体系中加入长程任务基准**：BrowseComp、GAIA 等基准可以客观衡量上下文管理能力。

### 10.3 待进一步研究的问题

1. **折叠决策的规则化**：能否将 AgentFold 的折叠策略提取为可解释的规则，降低对模型能力的依赖？
2. **折叠与其他 Agent 能力的组合**：将折叠与 Planning、Self-Correction 等能力结合，是否有协同效应？
3. **端到端强化学习**：用 RL 而非 SFT 训练折叠策略，是否能达到更好的效果？
