# SkillRouter 深度阅读报告

> **论文**: SkillRouter: Skill Routing for LLM Agents at Scale  
> **作者**: YanZhao Zheng, ZhenTao Zhang, Chao Ma, YuanQiang Yu, JiHuai Zhu, WuYong Tian, Tianze Xu, Baohua Dong, Hangcheng Zhu, Ruohui Huang, Gang Yu（阿里巴巴）  
> **arXiv**: 2603.22455  
> **发表状态**: Under Review（预印本）  
> **开源**: https://github.com/zhengyanzhao1997/SkillRouter

---

## 1. 基础信息

- **任务类型**: 技能路由（Skill Routing）—— 在大规模技能池中，根据用户任务查询检索出最相关的技能集合
- **核心设定**: ∼80K 候选技能池，75 个专家验证查询（24 单技能 + 51 多技能），含 Easy/Hard 两个难度层级
- **模型规模**: 1.2B（0.6B Encoder + 0.6B Reranker）；另有 8B 扩展版本
- **核心结论**: **全技能正文（body）是关键路由信号**，移除 body 导致所有基线方法 Hit@1 下降 31–44 个百分点
- **Benchmark**: SkillsBench-derived，约 80K 技能池，含 Easy（78,361）和 Hard（79,141，含 780 个 LLM 生成干扰项）两个层级

---

## 2. 一句话概括

SkillRouter 通过**全技能正文 Retrieve-and-Rerank 流水线**，在 ∼80K 大规模技能池中实现高精度技能路由：1.2B 小模型即可达到 74.0% Hit@1，精度持平甚至超越 16B 基线模型，同时推理速度提升 5.8 倍，且路由质量可迁移到下游编码智能体的任务成功率提升。

---

## 3. 科学问题

### 核心问题
当 LLM Agent 的技能生态扩展到**数以万计的可复用技能**时，如何在推理时高效、准确地找到与用户任务相关的技能集合？

### 背景与重要性
- 技能（Skills）已成为扩展 LLM Agent 的主流抽象：Anthropic Claude Code、OpenAI Codex、OpenClaw 等产品均已暴露技能作为一等能力
- 现实部署中，将所有技能暴露给 Agent 不可行，必须进行**技能路由（Skill Routing）**
- 上游路由决策是高杠杆瓶颈：一旦错误技能短列表被推送给 Agent，下游规划和执行几乎无法恢复

### 现有方法不足
- 现有 Agent 框架（如 Tools/AgentBench/SkillsBench）通常只研究**下游工具使用或工具选择行为**，而非上游大规模技能路由
- 现有检索工作通常在**小规模候选池**、仅基于 **name + description 元数据**进行，忽略了 full skill body
- **Hidden-Body Asymmetry（隐藏正文不对称性）**：路由系统可以看到完整技能正文，但下游 Agent 通常只看到元数据——这一设计选择此前未经实测验证

---

## 4. 技术路线

### 核心技术

#### 两阶段 Retrieve-and-Rerank 流水线
1. **Bi-encoder 检索阶段**：Fine-tune Qwen3-Emb-0.6B，从 ∼80K 池中检索 Top-20 候选
2. **Cross-encoder 重排阶段**：Fine-tune Qwen3-Rank-0.6B，对 Top-20 候选进行全技能正文打分排序

#### 关键设计

**训练数据**：
- 37,979 个合成 (query, skill) 对，从 ∼80K 社区技能池分层采样
- 使用 GPT-4o-mini 基于技能元数据和正文生成真实感用户查询（不暴露技能名）
- 基准标注技能从训练监督中排除，确保学习的是可迁移路由模式

**Hard Negative Mining（4类负例混合）**：
- 语义邻居（4个）：base encoder 嵌入检索
- 词汇匹配（3个）：BM25 评分
- 分类干扰项（2个）：同技能类别的其他技能
- 随机负例（1个）：不同类别的技能

**False Negative Filtering（关键！）**：
- 三层过滤：name 去重 + body trigram Jaccard > 0.6 + 嵌入相似度 > 0.92
- 移除约 10% 的挖掘负例（因为在大型技能池中，同一功能常被不同作者独立实现）
- 贡献 **+4.0pp Hit@1**

**Listwise Reranking Loss（关键！）**：
- 训练阶段使用 listwise cross-entropy 而非 pointwise BCE
- 当检索器将池缩小到 20 个候选后，剩余技能往往**主题都相关**，reranker 必须在彼此之间比较候选，而非独立打分
- 贡献 **+30.7pp Hit@1**（pointwise 版本仅 43.3%，listwise 达 74.0%）

**全技能正文输入**：
- 两阶段均使用完整技能正文（name + description + body），而非仅元数据
- 由 Section 3 的发现（body 是关键信号）驱动

### 实验设计
- **主 Benchmark**：75 核心查询，Easy/Hard 平均
- **辅助 Benchmark**：SkillBench-Supp（3个技能源，256查询，30个 GT 技能显式 held-out）
- **下游 End-to-End 评估**：4 个编码 Agent（Kimi-K2.5、glm-5、Claude Sonnet 4.6、Claude Opus 4.6），1200s 超时，Claude Code harness

### 关键技术参数
- Encoder: Qwen3-Emb-0.6B，within-batch InfoNCE 优化
- Reranker: Qwen3-Rank-0.6B，listwise cross-entropy
- 训练: 单 GPU，~32K candidate lists for reranker
- 推理延迟: 495.8ms 中位数（1.2B），vs. 16B 基线 5.8× 更慢

---

## 5. 核心发现

### 主要结论

1. **全技能正文是关键路由信号**：在 ∼80K 技能池上，移除 body 导致所有基线 Hit@1 下降 **31–44pp**
   - BM25: 31.4% → 0.0%
   - Qwen3-Emb-8B: 64.0% → 25.3%
   - Qwen3-Emb-8B×Qwen3-Rank-8B: 68.0% → 24.0%

2. **长度控制注意力分析排除简单长度解释**：Body 占 96.5% token，但在 Layer 19 时 name field 达到 26.3% attention 峰值（仅占 3.0% tokens），最终层回到 98.1% body attention。最终层 body attention 超过 token share（69/75 查询），与 body 绝对长度几乎无相关（r=0.04）

3. **描述质量分层排除质量混淆**：即使对最长描述的技能四分位数，nd→full 差距仍 ≥ 26pp

4. **Fine-tuning 比 Scale 更有效**：SR-Emb-0.6B（0.6B）超越 Qwen3-Emb-8B（8B）13× 参数差距，证明**任务特定训练数据 + 负采样策略可以弥补规模差距**

5. **Compact pipeline 精度超越最强基线**：
   - 1.2B SKILLRouter: **74.0% Hit@1**（平均 Easy/Hard）
   - 16B 基线（Qwen3-Emb-8B×Qwen3-Rank-8B）: 68.0% Hit@1
   - +10.0pp 超越同尺度基线配置

6. **路由增益可迁移到下游 Agent 执行**：SKILLRouter 相比最强基线路由器，4 个编码 Agent 平均任务成功率 +1.78pp（top-1）和 +2.33pp（top-10），更强 Agent 受益更明显（+3.22pp）

7. **Serving 效率显著提升**：1.2B 相比 16B 基线，GPU 内存减少 15.8%，推理速度提升 5.8×，达 495.8ms 中位数延迟

### 关键数据

| 配置 | 参数 | E-Hit@1 | H-Hit@1 | A-Hit@1 |
|------|------|---------|---------|---------|
| BM25 | - | 34.7% | 28.0% | 31.4% |
| Qwen3-Emb-8B | 8B | 65.3% | 62.7% | 64.0% |
| Qwen3-Emb-8B×Qwen3-Rank-8B | 16B | 68.0% | 68.0% | 68.0% |
| **SR-Emb-0.6B×SR-Rank-0.6B** | **1.2B** | **76.0%** | **72.0%** | **74.0%** |
| SR-Emb-8B×SR-Rank-8B | 8B | 78.7% | 73.3% | 76.0% |

---

## 6. 创新点

### 创新点 1：全技能正文作为关键路由信号（实证发现）
- 首次在 ∼80K 真实规模技能池上系统验证：现有框架隐藏技能正文的设计选择导致 **31–44pp 精度损失**
- 长度控制注意力诊断 + 描述质量分层，排除简单替代解释
- 这一发现具有广泛意义：**可能扩展到 API 路由、插件选择等其他结构化检索场景**

### 创新点 2：针对同质化技能池的 False Negative Filtering
- 在大型技能池中，同一功能常被不同作者独立实现，挖掘负例中不可避免包含功能等价的"假负例"
- 三层过滤（name 去重 + trigram Jaccard + 嵌入相似度）移除约 10% 假负例，贡献 **+4.0pp Hit@1**

### 创新点 3：Listwise Reranking Loss for Fine-grained Candidate Competition
- 当 Top-20 候选都已主题相关时，pointwise BCE 训练策略完全失效（43.3% Hit@1）
- Listwise cross-entropy 强制 reranker 在彼此之间比较候选，实现 **+30.7pp Hit@1**

---

## 7. 与现有方法对比

| 方法 | 特点 | 局限性 |
|------|------|--------|
| BM25 / Sparse Retrieval | 仅用全文，无训练 | 精度低（0.0% without body） |
| E5-Large-v2 / GTE-Large-v1.5 / BGE-Large-v1.5 | 传统 bi-encoder | 规模较大，精度不如 task-specific tuned |
| Qwen3-Emb-8B / NV-Embed-v2 | Decoder-based encoder | 无 task-specific 训练时不如 0.6B tuned |
| Proprietary API (OpenAI, Gemini) | 即用型 | 精度中等，不支持 full-text 本地部署 |
| LLM-as-Judge (GPT-4o-mini) | GPT-4o-mini 评分 | 仅 67.3% Hit@1，不提供完整 reranking |
| **SKILLRouter（ours）** | Full-text + task-specific + listwise | — |

**关键差距**：现有方法要么仅用 metadata（name+description）进行路由，要么即使使用全文也未针对同质化技能池进行特定训练优化。SKILLRouter 的核心优势在于：
1. Full-text 两阶段流水线
2. False negative filtering 处理近重复技能
3. Listwise loss 处理细粒度候选竞争
4. 小 13× 参数仍超越更大模型

---

## 8. 局限性与发展方向

### 局限性

1. **Benchmark 来源有限**：仅来自 SkillsBench 和少量技能源，结论主要适用于"大规模、高重叠"的技能注册场景；小规模目录可能 metadata-only 路由就足够

2. **下游评估范围有限**：
   - 仅 4 个编码 Agent，单一执行预算（1200s）
   - FC@10（完整 GT 集合覆盖率）和 end-to-end top-10 成功衡量的是**不同量**（穷举 gold-set 恢复 vs. 有界包效用），不应互换

3. **多技能查询的严格覆盖率仍有差距**：最强基线在 FC@10 上略优（.382 vs .353），SKILLRouter 的优势主要在 Top-1 路由

4. **模型架构**：未引入新的 encoder/reranker 架构，使用的是标准 Qwen3-Emb-0.6B 和 Qwen3-Rank-0.6B

### 发展方向

1. **扩展到其他结构化检索场景**：API 路由、插件选择、Agent 工具包路由
2. **多模态技能路由**：技能可能包含代码、文档、API schema 等多模态内容
3. **动态技能注册**：新增技能的在线纳入，无需全量重训练
4. **更大规模评估**：更多技能源、更多 Agent 类型（非仅编码）、更长执行预算
5. **与其他 LLM backbone 结合**：测试 GPT-4o、Claude 等作为 backbone 的效果

---

## 9. 可复现要素

### 数据
- **技能池**：∼80K 社区技能池，来源：Claude Skill Registry Core (Majiayu000, 2026)，51 个类别
- **训练查询**：GPT-4o-mini 基于技能 metadata + body 生成（37,979 对）；测试技能排除
- **Reranker 训练集**：32,283 candidate lists from SR-Emb-0.6B，每 list 20 skills，二元相关标签

### 核心超参数
- Encoder: Qwen3-Emb-0.6B fine-tune，within-batch InfoNCE
- Reranker: Qwen3-Rank-0.6B fine-tune，listwise cross-entropy
- Candidate list size: top-20
- Hard negatives: 10 per query（4 semantic + 3 BM25 + 2 taxonomy + 1 random）
- False negative filter: trigram Jaccard > 0.6, embedding similarity > 0.92

### 代码
- GitHub: https://github.com/zhengyanzhao1997/SkillRouter
- 依赖：Qwen3-Emb-0.6B / Qwen3-Rank-0.6B（HuggingFace）

### 硬件
- 训练：单 GPU
- 推理延迟基准：GPU serving benchmark over 80 timed queries

---

## 10. 对你的研究的意义

### 直接相关
1. **SkillRouter 路由 → OpenClaw 技能系统**：OpenClaw 本身有技能注册机制（skill ecosystem），本论文直接研究大规模技能注册场景下的路由问题，与 OpenClaw 实际部署高度相关

2. **全技能正文路由**：OpenClaw 的 skill 实现细节（body）包含工具定义、执行逻辑、API schema 等，是技能匹配的关键信号——SkillRouter 证明这些信息比仅用 name/description 重要得多

3. **Compact 高效 pipeline**：1.2B 模型在精度上超越 16B 模型，且推理快 5.8×——这对本地部署和延迟敏感的 Agent 系统非常有价值

### 可借鉴的设计
1. **False Negative Filtering**：处理 OpenClaw 技能池中近重复技能（不同命名但功能相同的技能）有直接价值
2. **Listwise Reranking**：当技能短列表（top-10/top-20）已经主题相关时，reranker 需要比较能力而非独立打分
3. **两阶段 Retrieve-and-Rerank**：Encoder 做召回（top-20），Cross-encoder reranker 做精排——可移植到 OpenClaw 的技能路由实现

### 待探索
1. **OpenClaw 技能池的规模**：是否达到 SkillsBench 的 ∼80K 规模？如果规模较小，metadata-only 路由可能足够
2. **多模态技能**：OpenClaw 技能可能包含文件、代码片段等，body 的形式更丰富
3. **Agent 消费形式**：OpenClaw Agent 如何消费路由后的技能？是否只看到 name+description 还是也暴露 body？

---

*深度阅读报告生成时间：2026-04-09*
*Paper slug: skillrouter*
