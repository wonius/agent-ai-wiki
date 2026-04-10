# SkillRouter: Skill Routing for LLM Agents at Scale

**论文标题**: SkillRouter: Skill Routing for LLM Agents at Scale
**作者**: YanZhao Zheng, ZhenTao Zhang, Chao Ma, YuanQiang Yu, JiHuai Zhu, Yong Wu, Tianze Liu（阿里巴巴）
**arXiv**: 2603.22455 | **发表**: 2026-04-01（v4）| **状态**: Under review

---

## 一句话概括

SkillRouter 发现**技能实现体（skill body）**是技能路由的关键信号——移除 body 导致 31-44pp 的路由准确率下降——并据此提出 1.2B 参数的全文本检索-重排管道，在 80K 技能池中达到 74.0% Hit@1，同时比最强基线快 5.8 倍。

---

## 科学问题

### 核心问题

LLM Agent 的技能生态正在扩展到数万级别（Claude Code、OpenClaw 等产品都把技能作为一级能力）。暴露全部技能实现体不可行，因此需要**技能路由**：给定用户任务，从大规模技能池中检索相关技能。

关键不对称性：路由组件可以检查完整技能文本，但下游 Agent 通常只看到技能名称和描述。

### 背景与重要性

- 技能路由是整个 Agent stack 的**高杠杆瓶颈**：一旦路由组件给出了错误技能列表，下游规划和执行几乎没有恢复机会
- 现有 agent 框架隐式假设"元数据（名称+描述）足以做技能选择"，但这个假设在真实规模下未经测试
- 现有 benchmark（SkillsBench、ToolBench）研究下游工具使用，不直接评估大规模技能池的上游路由

### 现有方法不足

1. **元数据假设未验证**：现有框架假设 name+description 足够，但没在真实规模（80K 技能池）下测试
2. **缺乏全文本信号**：现有方法只用 name+description，忽略了技能实现体中的关键路由信号
3. **参数规模与效率的权衡**：更强的 encoder 需要更多参数，服务延迟更高

---

## 技术路线

### 核心发现（前置洞察）

论文在 SkillsBench-derived benchmark（约 80K 候选技能）上发现了一个关键事实：

> **移除技能 body 导致 31-44pp 的 Hit@1 下降**
> - BM25: 31.4% → 0.0%（-31.4pp）
> - Qwen3-Emb-8B: 64.0% → 25.3%（-38.7pp）
> - Qwen3-Rank-8B: 68.0% → 24.0%（-44.0pp）

这说明**技能实现体，而非元数据，是真实路由场景中的关键信号**。长度控制实验和描述质量分层排除了"只是因为 body 更长"的简单解释。

### SKILLROUTER 架构

基于这个发现，提出了 **SKILLROUTER**：一个紧凑的 1.2B 全文本检索-重排管道。

**Pipeline**：
1. **Dense Retriever**（0.6B encoder）：从 80K 技能池中召回候选技能
2. **Reranker**（0.6B reranker）：对候选技能重排，输出最终 top-K

**两个关键技术适配**（针对同质化技能池）：
1. **False-negative filtering**：处理近重复技能
2. **Listwise reranking loss**：解决细粒度候选竞争

### 关键实验设计

- **数据集**：SkillsBench-derived benchmark（75 query × 80K 技能池）
- **Easy/Hard 分层**：Hard 增加了 780 个 LLM 生成的 distractor 技能（主题相关但功能不同）
- **评估指标**：Hit@1（主指标）、MRR@10、Recall@K、FC@10

---

## 核心发现

### 主要结论

1. **全技能文本是关键路由信号**：移除 body 导致 31-44pp 下降，远超预期
2. **紧凑管道胜过大参数基线**：1.2B SKILLROUTER 达到 74.0% Hit@1，超越 16B 最强基线（68.0% Hit@1）
3. **5.8 倍加速**：1.2B 管道比最强基线快 5.8 倍
4. **路由收益可迁移到端到端**：在 4 个 coding agent 的端到端实验中，更强的路由带来更高的任务成功率

### 关键数据

| 配置 | Hit@1 | R@10 | 参数量 | 相对延迟 |
|------|-------|------|--------|---------|
| Qwen3-Rank-8B（最强基线） | 68.0% | - | 8B | 5.8x |
| **SKILLROUTER 1.2B** | **74.0%** | **70.4%** | 1.2B | 1x |
| SKILLROUTER 8B | 76.0% | - | 8B | - |

### 机制解释

技能 body 包含的信息远超名称和描述：
- **函数签名和参数**：描述该技能接受什么输入
- **实现逻辑**：揭示该技能的实际行为（而非表面描述）
- **边界条件**：描述技能的适用和不适用的场景

这些信息在技能池高度重叠时尤其关键——仅靠名称和描述无法区分功能相近但有微妙差异的技能。

---

## 创新点

1. **揭示技能 body 的关键路由价值**：首次在真实规模（80K 技能池）下量化验证了全技能文本的路由信号作用
2. **紧凑全文本检索-重排管道**：1.2B 超越 16B，13x 参数缩减 + 5.8x 加速
3. **两个训练适配**：false-negative filtering + listwise reranking loss，专门解决同质化技能池问题

---

## 与现有方法对比

| 方法 | 输入 | Hit@1 | 参数量 | 延迟 |
|------|------|-------|--------|------|
| BM25 | metadata | 31.4% | - | 低 |
| Qwen3-Emb-8B | metadata | 64.0% | 8B | 中 |
| Qwen3-Rank-8B | metadata | 68.0% | 8B | 高 |
| **SKILLROUTER 1.2B** | **全文本** | **74.0%** | **1.2B** | **低** |
| **SKILLROUTER 8B** | **全文本** | **76.0%** | **8B** | **中** |

---

## 局限性与发展方向

**局限性**：
- 技能 body 的隐私和安全问题：暴露技能实现体可能泄露内部逻辑
- 当前验证在 coding agents 场景，其他 Agent 类型待测试
- 80K 技能池规模在实际部署中可能更大

**发展方向**：
- 技能 body 的隐私保护路由
- 跨领域技能迁移
- 动态技能发现而非预定义技能库

---

## 可复现要素

- **代码**: https://github.com/zhengyanzhao1997/SkillRouter
- **模型**: 1.2B / 8B 检索-重排管道
- **数据集**: SkillsBench-derived benchmark（75 query × 80K 技能池）

---

## 对你研究的意义

1. **"元数据足够"的假设可能是错的**：在真实规模下，技能 body 包含的信号远超 name+description——这对设计 Agent 的技能系统有直接启示
2. **紧凑模型 + 正确信号 > 大模型 + 少信号**：74% Hit@1 用 1.2B 超越 68% Hit@1 用 16B，印证了 SkillRouter、AgentFold、ACON 的共同结论——找到正确信号比堆模型规模更重要
3. **路由是 Agent 的高杠杆瓶颈**：路由决策的错误会在下游被放大，这提示 Agent 系统设计者应该优先优化路由质量
4. **全文本检索的现实可行性**：1.2B + 5.8x 加速说明全文本路由在生产环境是可行的

---

*报告生成时间：2026-04-10 | 来源：SkillRouter_2603.22455.pdf（v4，Under review）*
