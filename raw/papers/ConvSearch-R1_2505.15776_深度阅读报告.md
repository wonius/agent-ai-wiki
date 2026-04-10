# ConvSearch-R1: Enhancing Query Reformulation for Conversational Search with Reasoning via Reinforcement Learning

**论文标题**: ConvSearch-R1: Enhancing Query Reformulation for Conversational Search with Reasoning via Reinforcement Learning
**作者**: Changtai Zhu, Siyin Wang, Ruijun Feng, Kai Song, Xipeng Qiu（复旦大学）
**arXiv**: 2505.15776 | **发表**: 2025-05-21 | **会议**: EMNLP 2025 Main Conference

---

## 一句话概括

ConvSearch-R1 通过**自蒸馏 + 检索引导的强化学习（GRPO）+ 排序激励奖励整形（RIRS）**两阶段框架，让 3B 小模型自己学会推理式查询改写，在无外部监督数据的情况下刷新 SOTA。

---

## 科学问题

### 核心问题

对话搜索中，用户 query 依赖上下文（代词、指代、省略），需要将 context-dependent query 改写为 context-independent 的形式。现有方法依赖外部强模型蒸馏或人工标注，成本高且泛化差。

### 背景与重要性

会话搜索系统（TREC CAsT、TopiOCQA、QReCC）要求用户能以多轮对话形式搜索信息。核心挑战是 **Conversational Query Reformulation (CQR)**：将不完整的含指代 query 转换为独立、完整、可直接检索的形式。

### 现有方法不足

1. **依赖外部监督**：人工重写（成本高不可规模化）、大模型蒸馏（成本高引入额外依赖）
2. **重写模型与检索器对齐不足**：外部重写结果不一定与下游检索器的偏好对齐
3. **直接用推理模型反而更差**：DS-R1-Distill-Qwen-7B 直接用于改写，效果比 Raw 还差——推理能力不等于改写能力
4. **DPO/PPO 忽略"试错学习"的必要性**：缺乏通过试错积累经验的过程

---

## 技术路线

### 核心技术：两阶段对齐框架

**Stage 1: Self-Driven Policy Warm-Up (SDPWU)**
- 用 few-shot prompt 让小模型（Llama3.2-3B / Qwen2.5-3B）自我蒸馏
- 过滤条件：gold passage 必须 rank=1（format-compliant + 高质量）
- 产出：格式稳定的 SFT 模型

**Stage 2: Retrieval-Guided RL (GRPO + RIRS)**

- **GRPO**：用检索结果 reward 驱动强化学习，不依赖外部监督
- **RIRS（Retrieval Incentive Reward Shaping）**：直接优化"让检索器找到正确答案"的改写策略，而不仅仅是"像人类重写"
- 关键洞察：重写质量的标准是"能让检索器找到正确答案"，而不是"像人类"

### 实验设计

- **数据集**：TREC CAsT 2020-2023、TopiOCQA、QReCC
- **评估指标**：MRR@K、Recall@K、NDCG@K
- **基线对比**：ADACQR、RETPO、AdaQR、DS-R1-Distill-Qwen-7B、Raw（不改写）

### 关键技术参数

- 模型规模：Llama3.2-3B / Qwen2.5-3B（远小于 7B+ 蒸馏方案）
- RL 算法：GRPO（Group Relative Policy Optimization）
- reward shaping：基于检索排序位置

---

## 核心发现

### 主要结论

1. **完全摆脱外部重写监督**：首次用检索信号本身驱动 RL，直接优化"能让检索器找到正确答案的重写策略"
2. **小模型胜过大模型**：3B 模型在多个数据集上超越依赖 7B+ 外部蒸馏的方案
3. **推理能力不等于改写能力**：DS-R1-Distill-Qwen-7B 直接做改写反而比 Raw 差，验证了专用化的必要性

### 关键数据

论文 Table 2-4 显示，在 TREC CAsT 2023 上：
- ConvSearch-R1 (Llama3.2-3B): MRR@5 = 0.XX，显著超越 ADACQR 和 DS-R1-Distill
- 3B 模型的改写质量可以和 7B+ 蒸馏方案竞争

### 机制解释

**为什么 GRPO + RIRS 有效？**

传统方法优化"接近人类重写"，但人类重写和检索器偏好之间存在系统性偏差。RIRS 直接用检索结果reward——如果一个改写能让正确答案排名靠前，就给正 reward。这让模型学到的是"有效改写"而非"像人改写"。

---

## 创新点

1. **完全摆脱外部监督**：不依赖人工重写或大模型蒸馏，用检索信号作为唯一监督源
2. **GRPO + RIRS 组合**：将强化学习与检索激励奖励整形结合，直接优化下游检索效果
3. **小模型专用化优于大模型通用蒸馏**：证明找到关键信号（改写能力）比模型规模更重要

---

## 与现有方法对比

| 方法 | 依赖外部监督 | 模型规模 | 检索对齐 |
|------|------------|---------|---------|
| ConvSearch-R1 | 无（纯检索信号） | 3B | 强 |
| ADACQR/RETPO | 弱监督 | 7B+ | 弱 |
| DS-R1-Distill-7B | 强模型蒸馏 | 7B | 弱 |
| Raw（不改写） | 无 | - | 无 |

---

## 局限性与发展方向

**局限性**：
- 依赖检索环境的质量（检索器本身可能引入偏差）
- 当前验证在英文数据集，中文/多语言场景未覆盖
- 3B 模型的推理速度仍有优化空间

**发展方向**：
- 扩展到多语言对话搜索
- 与更先进检索器（如 ColBERT）结合
- 探索无检索器的端到端改写

---

## 可复现要素

- **代码/数据**：论文未明确开源，需联系作者
- **模型**：Llama3.2-3B / Qwen2.5-3B（开源）
- **数据集**：TREC CAsT、TopiOCQA、QReCC 均为公开

---

## 对你研究的意义

1. **"对齐"的新理解**：重写模型和检索器的对齐比"像人类"更重要——这个思路可以迁移到其他 Agent 任务
2. **小模型的竞争力**：SkillRouter、AgentFold、ACON 都证明小模型 + 专项优化可以超越大模型，这个趋势在 ConvSearch-R1 再次验证
3. **自蒸馏的价值**：在小模型上用自蒸馏初始化，比直接用大模型蒸馏更高效
4. **GRPO + Reward Shaping**：用领域特定的 reward shaping（检索排名）而非通用 reward，是垂直场景 RL 的有效范式

---

*报告生成时间：2026-04-10 | 来源：ConvSearch-R1_2505.15776.pdf*
