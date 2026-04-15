# 深度阅读报告：ConvSearch-R1: Enhancing Query Reformulation for Conversational Search with Reasoning via Reinforcement Learning Changtai Zhu1*, Siyin Wang1, Ruijun Feng3, Kai Song2, Xipeng Qiu1† 1Fudan University, 2ByteDance Inc, 3University of New South Wales ctzhu23@m.fudan.edu.cn, {siyinwang20, xpqiu}@fudan.edu.cn, songkai.sk.batman@bytedance.com, ruijun.feng@unsw.edu.au Abstract Conversational search systems require effective handling of context-dependent queries that often contain ambiguity, omission, and coreference. Conversational Query Reformulation (CQR) addresses this challenge by transforming these queries into self-contained forms suitable for off-the-shelf retrievers. However, existing CQR approaches suffer from two critical constraints: high dependency on costly external supervision from human annotations or large language models, and insufficient alignment between the rewriting model and downstream retrievers. We present ConvSearch-R1, the first self-driven framework that completely eliminates dependency on external rewrite supervision by leveraging reinforcement learning to optimize reformulation directly through retrieval signals. Our novel two-stage approach combines Self-Driven Policy Warm-Up to address the cold-start problem through retrievalguided self-distillation, followed by RetrievalGuided Reinforcement Learning with a specially designed rank-incentive reward shaping mechanism that addresses the sparsity issue in conventional retrieval metrics. Extensive experiments on TopiOCQA and QReCC datasets demonstrate that ConvSearch-R1 significantly outperforms previous state-of-the-art methods, achieving over 10% improvement on the challenging TopiOCQA dataset while using smaller 3B parameter models without any external supervision. 1 Introduction Conversational search aims to fulfill users’ information needs through multi-turn interactions, unlike traditional information retrieval systems that only consider single-turn, keyword-based queries (Joshi et al., 2017; Kwiatkowski et al., 2019). In these multi-turn scenarios, conversational queries often contain ambiguity, omission, and coreference, *Work done during internship at ByteDance Inc. †Corresponding author. Conversation Who is the current president of the United States? Donald Trump is the 47th and current president of the United States. Does Trump have his company in New York? In New York, Trump owns a building called Trump Tower, designed by Der Scutt. Rewriter Off-the-shelf Retriever Context & Query ConvSearch-R1 <think> ......... </think> <rewrite> ......... </rewrite> Rewritten Query rt Donald Trump's Trump Tower in New York was designed by Der Scutt. What other buildings did Der Scutt design? 1 2 3 Retriever Collection 4 Which other buildings were designed by the designer of his company? Der Scutt also designed One Astor Plaza and 520 Madison Avenue. Reference Passage Der Scutt worked on Trump Tower next to the Tiffany & Co. flagship store on Fifth Avenue, New York City, developed by Donald Trump. His other major buildings include One Astor Plaza, 520 Madison Avenue, ... He was the design consultant for the Grand Hyatt New York. 5 Retrieve Figure 1: Illustration of the CQR task. Given a query and its context, the rewriter aims to reformulate the query into a stand-alone form, which facilitates the offthe-shelf retriever in finding the most relevant passage. making it difficult for existing retrieval methods to accurately capture user intent (Anantha et al., 2020; Qu et al., 2020; Gao et al., 2022). Given the challenges and computational costs of training multiturn retrievers, Conversational Query Reformulation (CQR) (Elgohary et al., 2019; Vakulenko et al., 2020; Yu et al., 2020) has emerged as a practical solution, transforming context-dependent queries into self-contained forms that can be effectively processed by off-the-shelf retrievers (As shown in Figure 1). Existing CQR approaches have explored various strategies to address conversational search challenges. Some methods rely on explicit rewrites as supervision, obtained through either human annotations (Lin et al., 2020; Vakulenko et al., 2020; Del Tredici et al., 2021; Vakulenko et al., 2021) or knowledge distillation (Mao et al., 2023b; Mo 1 arXiv:2505.15776v2 [cs.CL] 14 Sep 2025

**生成时间**: 2026-04-11 11:20:59  
**源文件**: ConvSearch-R1_2505.15776.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | ConvSearch-R1: Enhancing Query Reformulation for Conversational Search with Reasoning via Reinforcement Learning Changtai Zhu1*, Siyin Wang1, Ruijun Feng3, Kai Song2, Xipeng Qiu1† 1Fudan University, 2ByteDance Inc, 3University of New South Wales ctzhu23@m.fudan.edu.cn, {siyinwang20, xpqiu}@fudan.edu.cn, songkai.sk.batman@bytedance.com, ruijun.feng@unsw.edu.au Abstract Conversational search systems require effective handling of context-dependent queries that often contain ambiguity, omission, and coreference. Conversational Query Reformulation (CQR) addresses this challenge by transforming these queries into self-contained forms suitable for off-the-shelf retrievers. However, existing CQR approaches suffer from two critical constraints: high dependency on costly external supervision from human annotations or large language models, and insufficient alignment between the rewriting model and downstream retrievers. We present ConvSearch-R1, the first self-driven framework that completely eliminates dependency on external rewrite supervision by leveraging reinforcement learning to optimize reformulation directly through retrieval signals. Our novel two-stage approach combines Self-Driven Policy Warm-Up to address the cold-start problem through retrievalguided self-distillation, followed by RetrievalGuided Reinforcement Learning with a specially designed rank-incentive reward shaping mechanism that addresses the sparsity issue in conventional retrieval metrics. Extensive experiments on TopiOCQA and QReCC datasets demonstrate that ConvSearch-R1 significantly outperforms previous state-of-the-art methods, achieving over 10% improvement on the challenging TopiOCQA dataset while using smaller 3B parameter models without any external supervision. 1 Introduction Conversational search aims to fulfill users’ information needs through multi-turn interactions, unlike traditional information retrieval systems that only consider single-turn, keyword-based queries (Joshi et al., 2017; Kwiatkowski et al., 2019). In these multi-turn scenarios, conversational queries often contain ambiguity, omission, and coreference, *Work done during internship at ByteDance Inc. †Corresponding author. Conversation Who is the current president of the United States? Donald Trump is the 47th and current president of the United States. Does Trump have his company in New York? In New York, Trump owns a building called Trump Tower, designed by Der Scutt. Rewriter Off-the-shelf Retriever Context & Query ConvSearch-R1 <think> ......... </think> <rewrite> ......... </rewrite> Rewritten Query rt Donald Trump's Trump Tower in New York was designed by Der Scutt. What other buildings did Der Scutt design? 1 2 3 Retriever Collection 4 Which other buildings were designed by the designer of his company? Der Scutt also designed One Astor Plaza and 520 Madison Avenue. Reference Passage Der Scutt worked on Trump Tower next to the Tiffany & Co. flagship store on Fifth Avenue, New York City, developed by Donald Trump. His other major buildings include One Astor Plaza, 520 Madison Avenue, ... He was the design consultant for the Grand Hyatt New York. 5 Retrieve Figure 1: Illustration of the CQR task. Given a query and its context, the rewriter aims to reformulate the query into a stand-alone form, which facilitates the offthe-shelf retriever in finding the most relevant passage. making it difficult for existing retrieval methods to accurately capture user intent (Anantha et al., 2020; Qu et al., 2020; Gao et al., 2022). Given the challenges and computational costs of training multiturn retrievers, Conversational Query Reformulation (CQR) (Elgohary et al., 2019; Vakulenko et al., 2020; Yu et al., 2020) has emerged as a practical solution, transforming context-dependent queries into self-contained forms that can be effectively processed by off-the-shelf retrievers (As shown in Figure 1). Existing CQR approaches have explored various strategies to address conversational search challenges. Some methods rely on explicit rewrites as supervision, obtained through either human annotations (Lin et al., 2020; Vakulenko et al., 2020; Del Tredici et al., 2021; Vakulenko et al., 2021) or knowledge distillation (Mao et al., 2023b; Mo 1 arXiv:2505.15776v2 [cs.CL] 14 Sep 2025 |
| **中文标题** | [待翻译] |
| **期刊** | Science |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 14 Sep 2025 |
| **发表日期** | 3 June 1937 |


**百分比数据:** 10.7, 10.3, 10

**浓度数据:** 2M, 25M, 5.1M, 54M, 3M, 16M

**温度条件:** 024, 022, 021°C

---

## 第二部分：核心理解

*本部分由AI进行深度分析*

### 1. 这篇论文到底在做什么？
[AI分析中...]

### 2. 为什么要做这个？
[AI分析中...]

### 3. 是怎么做到的？
[AI分析中...]

### 4. 做得怎么样？
[AI分析中...]

### 5. 意味着什么？
[AI分析中...]

---

## 第三部分：批判性分析

*本部分由AI进行深度分析*

### 1. 优点/亮点
[AI分析中...]

### 2. 潜在问题/局限
[AI分析中...]

### 3. 未解决的关键问题
[AI分析中...]

---

## 第四部分：用户研究的关联

*本部分由用户补充*

### 1. 相关度评估
- [ ] 高：直接相关，可借鉴
- [ ] 中：间接相关，有参考价值
- [ ] 低：领域较远，仅作了解

**说明**：[由用户填写]

### 2. 可借鉴之处
- 技术方法：[由用户填写]
- 分析思路：[由用户填写]
- 实验设计：[由用户填写]

### 3. 可能的应用场景
- 研究方向：[由用户填写]
- 实际应用：[由用户填写]

### 4. 补充笔记
[由用户填写]

---

*报告生成时间：2026-04-11 11:20:59*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
