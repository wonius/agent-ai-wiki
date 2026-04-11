# 深度阅读报告：OLViT: Multi-Modal State Tracking via Attention-Based Embeddings for Video-Grounded Dialog Adnen Abdessaied, Manuel von Hochmeister*, Andreas Bulling University of Stuttgart, Bosch, University of Stuttgart Germany, Germany, Germany {adnen.abdessaied, andreas.bulling}@vis.uni-stuttgart.de manuel.vonhochmeister@de.bosch.com Abstract We present the Object Language Video Transformer (OLViT) – a novel model for video dialog operating over a multi-modal attention-based dialog state tracker. Existing video dialog models struggle with questions requiring both spatial and temporal localization within videos, long-term temporal reasoning, and accurate object tracking across multiple dialog turns. OLViT addresses these challenges by maintaining a global dialog state based on the output of an Object State Tracker (OST) and a Language State Tracker (LST): while the OST attends to the most important objects within the video, the LST keeps track of the most important linguistic co-references to previous dialog turns. In stark contrast to previous works, our approach is generic by nature and is therefore capable of learning continuous multi-modal dialog state representations of the most relevant objects and rounds. As a result, they can be seamlessly integrated into Large Language Models (LLMs) and offer high flexibility in dealing with different datasets and tasks. Evaluations on the challenging DVD (response classification) and SIMMC 2.1 (response generation) datasets show that OLViT achieves new state-of-the-art performance across both datasets. Keywords: Multi-Modal Learning, Video Dialog, Dialog State Tracking 1. Introduction The potential of deep learning for tackling unique challenges at the intersection of computer vision (CV) and natural language processing (NLP) has been demonstrated for a wide range of tasks (Karpathy and Fei-Fei, 2015; Antol et al., 2015; Das et al., 2017; Vo et al., 2019; Rombach et al., 2022). Among these tasks, video dialog is considered to be one of the most challenging. In contrast to visual (Antol et al., 2015) and video (Xu et al., 2016) question answering, which only require reasoning about a single question, video dialog models have to reason over the whole dialog history. Furthermore, in contrast to visual dialog (Das et al., 2017; Abdessaied et al., 2024), video dialog involves reasoning over a dynamic visual input (video) instead of a static image. While recent video dialog models have improved performance (Hori et al., 2019; Le et al., 2019, 2021a), these gains have largely been marginal, most likely due to the significant challenges posed by this novel task. Current models suffer from several specific limitations: They struggle with questions that require spatial and temporal localization within the video, they suffer from a general inability of long-term reasoning, and they fail to accurately track objects across multiple dialog turns. Moreover, they have only been evaluated on benchmarks that were not explicitly designed to minimize biases and test for higher-order reasoning capabili- * Work conducted while at the University of Stuttgart Our project web-page is accessible here DVD (Accuracy) SIMMC 2.1 (BLEU-4) Previous SOTA All Action_count Test-dev Test-std Action_query Attribute_query Compare_act_seq Compare_act_set Compare_act_freq Object_count Object_exist 43.42 28.30 25.20 21.70 21.00 45.24 52.77 45.10 61.88 61.60 51.71 45.41 67.91 67.10 43.34 40.20 71.25 69.42 39.40 38.80 54.85 51.10 Figure 1: OLViT outperforms strong baselines and achieves new state-of-the-art results on DVD (classification) and SIMMC 2.1 (generation). ties (Hori et al., 2019; Zhao et al., 2018). To address these limitations, we propose OLViT– the Object Language Video Transformer for video-grounded dialog. At the core of OLViT are two novel components: an object state tracker (OST) and a language state tracker (LST). While the former actively attends to the most relevant objects across different video frames in order to answer the question at hand, the latter encodes the most important linguistic features for a more efficient co-reference resolution (object mentions, attributes, spatial and temporal cues, etc.) across dialog turns. After each turn, both trackers compute continuous object and arXiv:2402.13146v1 [cs.CV] 20 Feb 2024

**生成时间**: 2026-04-11 11:20:49  
**源文件**: multiturn_2402.13146.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | OLViT: Multi-Modal State Tracking via Attention-Based Embeddings for Video-Grounded Dialog Adnen Abdessaied, Manuel von Hochmeister*, Andreas Bulling University of Stuttgart, Bosch, University of Stuttgart Germany, Germany, Germany {adnen.abdessaied, andreas.bulling}@vis.uni-stuttgart.de manuel.vonhochmeister@de.bosch.com Abstract We present the Object Language Video Transformer (OLViT) – a novel model for video dialog operating over a multi-modal attention-based dialog state tracker. Existing video dialog models struggle with questions requiring both spatial and temporal localization within videos, long-term temporal reasoning, and accurate object tracking across multiple dialog turns. OLViT addresses these challenges by maintaining a global dialog state based on the output of an Object State Tracker (OST) and a Language State Tracker (LST): while the OST attends to the most important objects within the video, the LST keeps track of the most important linguistic co-references to previous dialog turns. In stark contrast to previous works, our approach is generic by nature and is therefore capable of learning continuous multi-modal dialog state representations of the most relevant objects and rounds. As a result, they can be seamlessly integrated into Large Language Models (LLMs) and offer high flexibility in dealing with different datasets and tasks. Evaluations on the challenging DVD (response classification) and SIMMC 2.1 (response generation) datasets show that OLViT achieves new state-of-the-art performance across both datasets. Keywords: Multi-Modal Learning, Video Dialog, Dialog State Tracking 1. Introduction The potential of deep learning for tackling unique challenges at the intersection of computer vision (CV) and natural language processing (NLP) has been demonstrated for a wide range of tasks (Karpathy and Fei-Fei, 2015; Antol et al., 2015; Das et al., 2017; Vo et al., 2019; Rombach et al., 2022). Among these tasks, video dialog is considered to be one of the most challenging. In contrast to visual (Antol et al., 2015) and video (Xu et al., 2016) question answering, which only require reasoning about a single question, video dialog models have to reason over the whole dialog history. Furthermore, in contrast to visual dialog (Das et al., 2017; Abdessaied et al., 2024), video dialog involves reasoning over a dynamic visual input (video) instead of a static image. While recent video dialog models have improved performance (Hori et al., 2019; Le et al., 2019, 2021a), these gains have largely been marginal, most likely due to the significant challenges posed by this novel task. Current models suffer from several specific limitations: They struggle with questions that require spatial and temporal localization within the video, they suffer from a general inability of long-term reasoning, and they fail to accurately track objects across multiple dialog turns. Moreover, they have only been evaluated on benchmarks that were not explicitly designed to minimize biases and test for higher-order reasoning capabili- * Work conducted while at the University of Stuttgart Our project web-page is accessible here DVD (Accuracy) SIMMC 2.1 (BLEU-4) Previous SOTA All Action_count Test-dev Test-std Action_query Attribute_query Compare_act_seq Compare_act_set Compare_act_freq Object_count Object_exist 43.42 28.30 25.20 21.70 21.00 45.24 52.77 45.10 61.88 61.60 51.71 45.41 67.91 67.10 43.34 40.20 71.25 69.42 39.40 38.80 54.85 51.10 Figure 1: OLViT outperforms strong baselines and achieves new state-of-the-art results on DVD (classification) and SIMMC 2.1 (generation). ties (Hori et al., 2019; Zhao et al., 2018). To address these limitations, we propose OLViT– the Object Language Video Transformer for video-grounded dialog. At the core of OLViT are two novel components: an object state tracker (OST) and a language state tracker (LST). While the former actively attends to the most relevant objects across different video frames in order to answer the question at hand, the latter encodes the most important linguistic features for a more efficient co-reference resolution (object mentions, attributes, spatial and temporal cues, etc.) across dialog turns. After each turn, both trackers compute continuous object and arXiv:2402.13146v1 [cs.CV] 20 Feb 2024 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 20 Feb 2024 |
| **发表日期** | 未知 |


**百分比数据:** 74.83, 61.28, 55.41, 55.39, 55.10, 54.94, 54.86, 54.85

**浓度数据:** 354M, 1M, 81.5M

**温度条件:** 40, 30, 28°C

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

*报告生成时间：2026-04-11 11:20:49*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
