# 深度阅读报告：Meaning Beyond Truth Conditions: Evaluating Discourse Level Understanding via Anaphora Accessibility Xiaomeng Zhu∗ Zhenghao Zhou∗ Simon Charlow Robert Frank Department of Linguistics Yale University {miranda.zhu, herbert.zhou, simon.charlow, robert.frank}@yale.edu Abstract We present a hierarchy of natural language understanding abilities and argue for the importance of moving beyond assessments of understanding at the lexical and sentence levels to the discourse level. We propose the task of anaphora accessibility as a diagnostic for assessing discourse understanding, and to this end, present an evaluation dataset inspired by theoretical research in dynamic semantics. We evaluate human and LLM performance on our dataset and find that LLMs and humans align on some tasks and diverge on others. Such divergence can be explained by LLMs’ reliance on specific lexical items during language comprehension, in contrast to human sensitivity to structural abstractions. 1 Introduction The success of modern large language models (LLMs) depends on their capacity for natural language understanding (NLU), i.e., the ability to extract the semantic information contained in a text. Systematic assessment of NLU abilities has been carried out using a diverse set of evaluation tasks, but few of them target whether LLMs accurately represent and update states of natural language discourse. Successful interpretation of discourse requires the ability to use pronominal expressions to refer to entities that have been introduced in a text. The felicity of pronominal anaphora, i.e., using pronouns to refer back to discourse referents introduced earlier, is influenced by the semantic scope of the antecedent: (1) {A, #Every} farmer worked in his field. He dreamed of the harvest. Example (1) shows that an entity introduced by an existential quantifier is accessible in the same sentence, as well as in subsequent sentences. In *Equal contribution. A farmer worked in his ﬁeld. He dreamed of the harvest. Every farmer worked in his ﬁeld. He dreamed of the harvest. # farmer farmer He He Quantiﬁer scope Discourse entity Anaphora Infelicitous because outside of scope his his Figure 1: Quantifier scope and its impact on anaphora. contrast, entities introduced by universal quantifiers are only accessible to pronouns in the same sentence; anaphora is infelicitous otherwise. This is illustrated in Figure 1: the discourse referent is subordinated to the universal quantifier — that is, inaccessible outside its scope, which extends to the end of the first sentence in the sequence. This makes subsequent reference to he in the second sentence infelicitous. The process of introducing discourse referents is formalized in ‘dynamic’ variants of formal semantics (e.g., Heim, 1983; Groenendijk and Stokhof, 1991; Kamp et al., 2010). In dynamic semantics, utterances precipitate changes in the discourse state, for example by introducing discourse referents. This gives rise to notions of discourse or textual scope which differentiate (e.g.) existential and universal quantifiers, in line with Figure 1. Here, we focus on one aspect of discourse-level semantic knowledge, namely the fine-grained interactions between semantic scope and referent accessibility. We investigate whether LLMs demonstrate knowledge of the semantic scope properties of various quantifiers and logical connectives, and whether this knowledge is used to generate and update representations of discourse states in humanlike ways. Contribution We make the following contributions: • In Section 2, we propose a hierarchy of levels of semantic understanding abilities, which 1 arXiv:2502.14119v1 [cs.CL] 19 Feb 2025

**生成时间**: 2026-04-11 11:20:40  
**源文件**: anaphora_2502.14119.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | Meaning Beyond Truth Conditions: Evaluating Discourse Level Understanding via Anaphora Accessibility Xiaomeng Zhu∗ Zhenghao Zhou∗ Simon Charlow Robert Frank Department of Linguistics Yale University {miranda.zhu, herbert.zhou, simon.charlow, robert.frank}@yale.edu Abstract We present a hierarchy of natural language understanding abilities and argue for the importance of moving beyond assessments of understanding at the lexical and sentence levels to the discourse level. We propose the task of anaphora accessibility as a diagnostic for assessing discourse understanding, and to this end, present an evaluation dataset inspired by theoretical research in dynamic semantics. We evaluate human and LLM performance on our dataset and find that LLMs and humans align on some tasks and diverge on others. Such divergence can be explained by LLMs’ reliance on specific lexical items during language comprehension, in contrast to human sensitivity to structural abstractions. 1 Introduction The success of modern large language models (LLMs) depends on their capacity for natural language understanding (NLU), i.e., the ability to extract the semantic information contained in a text. Systematic assessment of NLU abilities has been carried out using a diverse set of evaluation tasks, but few of them target whether LLMs accurately represent and update states of natural language discourse. Successful interpretation of discourse requires the ability to use pronominal expressions to refer to entities that have been introduced in a text. The felicity of pronominal anaphora, i.e., using pronouns to refer back to discourse referents introduced earlier, is influenced by the semantic scope of the antecedent: (1) {A, #Every} farmer worked in his field. He dreamed of the harvest. Example (1) shows that an entity introduced by an existential quantifier is accessible in the same sentence, as well as in subsequent sentences. In *Equal contribution. A farmer worked in his ﬁeld. He dreamed of the harvest. Every farmer worked in his ﬁeld. He dreamed of the harvest. # farmer farmer He He Quantiﬁer scope Discourse entity Anaphora Infelicitous because outside of scope his his Figure 1: Quantifier scope and its impact on anaphora. contrast, entities introduced by universal quantifiers are only accessible to pronouns in the same sentence; anaphora is infelicitous otherwise. This is illustrated in Figure 1: the discourse referent is subordinated to the universal quantifier — that is, inaccessible outside its scope, which extends to the end of the first sentence in the sequence. This makes subsequent reference to he in the second sentence infelicitous. The process of introducing discourse referents is formalized in ‘dynamic’ variants of formal semantics (e.g., Heim, 1983; Groenendijk and Stokhof, 1991; Kamp et al., 2010). In dynamic semantics, utterances precipitate changes in the discourse state, for example by introducing discourse referents. This gives rise to notions of discourse or textual scope which differentiate (e.g.) existential and universal quantifiers, in line with Figure 1. Here, we focus on one aspect of discourse-level semantic knowledge, namely the fine-grained interactions between semantic scope and referent accessibility. We investigate whether LLMs demonstrate knowledge of the semantic scope properties of various quantifiers and logical connectives, and whether this knowledge is used to generate and update representations of discourse states in humanlike ways. Contribution We make the following contributions: • In Section 2, we propose a hierarchy of levels of semantic understanding abilities, which 1 arXiv:2502.14119v1 [cs.CL] 19 Feb 2025 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 19 Feb 2025 |
| **发表日期** | 未知 |


**百分比数据:** 90, 81.73, 75

**浓度数据:** 10M, 2m, 1.00m

**温度条件:** 022°C

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

*报告生成时间：2026-04-11 11:20:40*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
