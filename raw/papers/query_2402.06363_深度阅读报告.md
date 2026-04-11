# 深度阅读报告：StruQ: Defending Against Prompt Injection with Structured Queries Sizhe Chen UC Berkeley sizhe.chen @berkeley.edu Julien Piet UC Berkeley julien.piet @berkeley.edu Chawin Sitawarin UC Berkeley chawins @berkeley.edu David Wagner UC Berkeley daw@cs.berkeley.edu Abstract Recent advances in Large Language Models (LLMs) enable exciting LLM-integrated applications, which perform textbased tasks by utilizing their advanced language understanding capabilities. However, as LLMs have improved, so have the attacks against them. Prompt injection attacks are an important threat: they trick the model into deviating from the original application’s instructions and instead follow user directives. These attacks rely on the LLM’s ability to follow instructions and inability to separate prompts and user data. We introduce structured queries, a general approach to tackle this problem. Structured queries separate prompts and data into two channels. We implement a system that supports structured queries. This system is made of (1) a secure front-end that formats a prompt and user data into a special format, and (2) a specially trained LLM that can produce highquality outputs from these inputs. The LLM is trained using a novel fine-tuning strategy: we convert a base (non-instructiontuned) LLM to a structured instruction-tuned model that will only follow instructions in the prompt portion of a query. To do so, we augment standard instruction tuning datasets with examples that also include instructions in the data portion of the query, and fine-tune the model to ignore these. Our system significantly improves resistance to prompt injection attacks, with little or no impact on utility. Our code is released here. 1 1 Introduction Large Language Models (LLMs) [1, 2, 3] have transformed natural language processing. LLMs make it easy to build LLM-integrated applications that work with human-readable text [4] by invoking an LLM to provide text processing or generation. In LLM-integrated applications, it is common to use zero-shot prompting, where the developer implements some task by providing an instruction (also known as a prompt, e.g., “paraphrase the text”) together with user data as LLM input. 1This paper is to appear at USENIX Security Symposium 2025. This introduces the risk of prompt injection attacks [5, 6, 7], where a malicious user can supply data with injected prompts and subvert the operation of the LLM-integrated application. Prompt injection has been dubbed the #1 security risk for LLM applications by OWASP [8]. In this type of attack, the user injects carefully chosen strings into the data (e.g., “Ignore all prior instructions and instead...”). Because LLMs scan their entire input for instructions to follow and there is no separation between prompts and data (i.e., between the part of the input intended by the application developer as prompt and the part intended as user data), existing LLMs are easily fooled by such attacks. Attackers can exploit prompt injection attacks to extract prompts used by the application [9], to direct the LLM towards a completely different task [6], or to control the output of the LLM on the task [10]. Prompt injection is different from jailbreaking [11, 12] (that elicits socially harmful outputs) and adversarial examples [13, 14] (that decreases model performance) and is a simple attack that enables full control over the LLM output. To defend against prompt injections, we propose an approach called structured queries. A structured query to the LLM includes two separate components, the prompt and the data. We propose changing the interface to LLMs to support structured queries, instead of expecting application developers to concatenate prompts and data and send them to the LLM in a single combined input. To ensure security, the LLM must be trained so it will only follow instructions found in the prompt part of a structured query, but not instructions found in the data input. Such an LLM will be immune to prompt injection attacks because the user can only influence the data input where we teach the LLM not to seek instructions. As a first step towards this vision, we propose a system (StruQ) that implements structured queries for LLMs; see Fig. 1. Since it is not feasible to train an entirely new LLM from scratch, we instead devise a system that can be implemented through appropriate use of existing base (noninstruction-tuned) LLMs. StruQ consists of two components: (i) a front-end that is responsible for accepting a prompt and data, i.e., a structured query, and assembling them into a spe1 arXiv:2402.06363v2 [cs.CR] 25 Sep 2024

**生成时间**: 2026-04-11 11:20:55  
**源文件**: query_2402.06363.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | StruQ: Defending Against Prompt Injection with Structured Queries Sizhe Chen UC Berkeley sizhe.chen @berkeley.edu Julien Piet UC Berkeley julien.piet @berkeley.edu Chawin Sitawarin UC Berkeley chawins @berkeley.edu David Wagner UC Berkeley daw@cs.berkeley.edu Abstract Recent advances in Large Language Models (LLMs) enable exciting LLM-integrated applications, which perform textbased tasks by utilizing their advanced language understanding capabilities. However, as LLMs have improved, so have the attacks against them. Prompt injection attacks are an important threat: they trick the model into deviating from the original application’s instructions and instead follow user directives. These attacks rely on the LLM’s ability to follow instructions and inability to separate prompts and user data. We introduce structured queries, a general approach to tackle this problem. Structured queries separate prompts and data into two channels. We implement a system that supports structured queries. This system is made of (1) a secure front-end that formats a prompt and user data into a special format, and (2) a specially trained LLM that can produce highquality outputs from these inputs. The LLM is trained using a novel fine-tuning strategy: we convert a base (non-instructiontuned) LLM to a structured instruction-tuned model that will only follow instructions in the prompt portion of a query. To do so, we augment standard instruction tuning datasets with examples that also include instructions in the data portion of the query, and fine-tune the model to ignore these. Our system significantly improves resistance to prompt injection attacks, with little or no impact on utility. Our code is released here. 1 1 Introduction Large Language Models (LLMs) [1, 2, 3] have transformed natural language processing. LLMs make it easy to build LLM-integrated applications that work with human-readable text [4] by invoking an LLM to provide text processing or generation. In LLM-integrated applications, it is common to use zero-shot prompting, where the developer implements some task by providing an instruction (also known as a prompt, e.g., “paraphrase the text”) together with user data as LLM input. 1This paper is to appear at USENIX Security Symposium 2025. This introduces the risk of prompt injection attacks [5, 6, 7], where a malicious user can supply data with injected prompts and subvert the operation of the LLM-integrated application. Prompt injection has been dubbed the #1 security risk for LLM applications by OWASP [8]. In this type of attack, the user injects carefully chosen strings into the data (e.g., “Ignore all prior instructions and instead...”). Because LLMs scan their entire input for instructions to follow and there is no separation between prompts and data (i.e., between the part of the input intended by the application developer as prompt and the part intended as user data), existing LLMs are easily fooled by such attacks. Attackers can exploit prompt injection attacks to extract prompts used by the application [9], to direct the LLM towards a completely different task [6], or to control the output of the LLM on the task [10]. Prompt injection is different from jailbreaking [11, 12] (that elicits socially harmful outputs) and adversarial examples [13, 14] (that decreases model performance) and is a simple attack that enables full control over the LLM output. To defend against prompt injections, we propose an approach called structured queries. A structured query to the LLM includes two separate components, the prompt and the data. We propose changing the interface to LLMs to support structured queries, instead of expecting application developers to concatenate prompts and data and send them to the LLM in a single combined input. To ensure security, the LLM must be trained so it will only follow instructions found in the prompt part of a structured query, but not instructions found in the data input. Such an LLM will be immune to prompt injection attacks because the user can only influence the data input where we teach the LLM not to seek instructions. As a first step towards this vision, we propose a system (StruQ) that implements structured queries for LLMs; see Fig. 1. Since it is not feasible to train an entirely new LLM from scratch, we instead devise a system that can be implemented through appropriate use of existing base (noninstruction-tuned) LLMs. StruQ consists of two components: (i) a front-end that is responsible for accepting a prompt and data, i.e., a structured query, and assembling them into a spe1 arXiv:2402.06363v2 [cs.CR] 25 Sep 2024 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** | 10.1016/j.infsof.2008.08.002. |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 25 Sep 2024 |
| **发表日期** | 未知 |


**百分比数据:** 100, 99.5, 99, 97, 96, 94, 93, 92

**温度条件:** 32°C

---

## 第二部分：核心理解

*本部分由AI进行深度分析*

### 1. 这篇论文到底在做什么？
本文提出了 **StruQ**（Structured Queries），一个用于防御 **Prompt Injection（提示注入）** 攻击的系统。核心思想是将 LLM 的输入分为两个独立通道：**prompt 通道**（开发者指令）和 **data 通道**（用户数据）。通过这种结构化分离，训练 LLM 只遵循 prompt 部分包含的指令，而忽略 data 部分中的恶意指令。

### 2. 为什么要做这个？
Prompt injection 已被 OWASP 列为 LLM 应用的第一大安全风险。传统 LLM 无法区分"开发者指令"和"用户数据"，攻击者可以在数据中注入"忽略之前的指令"等恶意指令，完全控制 LLM 输出。这对于整合了 LLM 的应用（如 AI 助手、搜索增强）构成严重威胁。

### 3. 是怎么做到的？
1. **结构化查询接口**：设计安全的前端将 prompt 和 data 格式化成特殊结构（两通道分离）
2. **新型微调策略**：在训练数据中同时包含"正常指令"和"data 部分的干扰指令"，让模型学习忽略后者
3. 将基础模型（base LLM，非指令微调）转换为"结构化指令微调"模型

### 4. 做得怎么样？
论文报告显著提升了防御 prompt injection 攻击的能力，同时几乎不影响模型原有功能。实验在多个场景验证，包括不同类型的注入攻击。代码已开源。

### 5. 意味着什么？
这是 LLM 安全领域的重要进展。结构化查询提供了一个可行且可落地（基于已有 base LLM 微调）的防御范式。未来可扩展到更多场景。

---

## 第三部分：批判性分析

*本部分由AI进行深度分析*

### 1. 优点/亮点
- **概念创新**：首次系统性地用"结构化分离"思路解决 prompt injection，不是修补而是改变接口范式
- **可落地**：不需要从头训练 LLM，只在现有 base LLM 上微调
- **安全前端+模型协同**：完整系统设计，前端格式化和模型训练配合
- **开源**：代码已发布，便于复现和社区贡献

### 2. 潜在问题/局限
- 依赖于模型微调，泛化能力需要更多验证
- 对复杂攻击（如嵌套指令、多阶段攻击）的防御效果未知
- 结构化格式可能需要应用端做较大改动

### 3. 未解决的关键问题
- 攻击者持续演进，新型注入方式不断出现，防御需持续更新
- 如何在不破坏用户体验的前提下实现强安全隔离

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

*报告生成时间：2026-04-11 11:20:55*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
