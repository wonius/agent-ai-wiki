# 深度阅读报告：The Drama Machine: Simulating Character Development with LLM Agents Liam Magee, Vanicka Arora, Gus Gollings, Norma Lam-Saw September 4, 2024 Contents 1 Introduction 2 2 Literature 3 2.1 ‘Character-building’ with AI . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3 2.2 Society of Mind . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4 2.3 Dramaturgy and Personality . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 5 2.4 The Drama of the Subject . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6 2.5 Presentations of a Divided Subject . . . . . . . . . . . . . . . . . . . . . . . . . . . 7 3 Method: Designing the ‘Drama Machine’ 8 3.1 Scripting Plots: The Bildungsroman of a Model . . . . . . . . . . . . . . . . . . . . 9 3.2 Theatrical Simulations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 10 3.2.1 Model and parameter variants . . . . . . . . . . . . . . . . . . . . . . . . . 10 3.2.2 Fine-tuning the Ego: Strategies for the Superego . . . . . . . . . . . . . . . 11 4 Results: Two Short Plays 11 4.1 First Scenario: The Interview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 4.1.1 Without Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 4.1.2 With Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 13 4.1.3 Commentary . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15 4.2 Second Scenario: The Plot-Driven Drama . . . . . . . . . . . . . . . . . . . . . . . 15 4.2.1 Without Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 16 4.2.2 With Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 16 4.2.3 Commentary . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 17 5 Discussion 18 5.1 A ‘Critic’s’ View . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 18 5.2 Performativity, or How to Build an Automated Identity . . . . . . . . . . . . . . . 19 5.3 From Playing Games to Playing Roles . . . . . . . . . . . . . . . . . . . . . . . . . 20 6 Conclusion 21 A Appendix A: Drama Machine 25 B Appendix B: Prompts for the Interview 25 B.1 Prompt for Jenny . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 25 B.2 Prompt for Cleo . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 26 B.3 Prompt for Sasha . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27 C Appendix C: Prompts for the Plot-Driven Drama 27 C.1 Prompt for Ashley . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27 C.2 Prompt for Timothy . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27 C.3 Prompt for Ben . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 29 C.4 Prompt for Sasha . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 29 1 arXiv:2408.01725v2 [cs.CY] 31 Aug 2024

**生成时间**: 2026-04-11 11:20:50  
**源文件**: multiturn_2408.01725.pdf

---

## 第一部分：基础信息（自动提取）

| 字段 | 内容 |
|:---|:---|
| **标题** | The Drama Machine: Simulating Character Development with LLM Agents Liam Magee, Vanicka Arora, Gus Gollings, Norma Lam-Saw September 4, 2024 Contents 1 Introduction 2 2 Literature 3 2.1 ‘Character-building’ with AI . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3 2.2 Society of Mind . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4 2.3 Dramaturgy and Personality . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 5 2.4 The Drama of the Subject . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 6 2.5 Presentations of a Divided Subject . . . . . . . . . . . . . . . . . . . . . . . . . . . 7 3 Method: Designing the ‘Drama Machine’ 8 3.1 Scripting Plots: The Bildungsroman of a Model . . . . . . . . . . . . . . . . . . . . 9 3.2 Theatrical Simulations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 10 3.2.1 Model and parameter variants . . . . . . . . . . . . . . . . . . . . . . . . . 10 3.2.2 Fine-tuning the Ego: Strategies for the Superego . . . . . . . . . . . . . . . 11 4 Results: Two Short Plays 11 4.1 First Scenario: The Interview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 4.1.1 Without Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 12 4.1.2 With Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 13 4.1.3 Commentary . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 15 4.2 Second Scenario: The Plot-Driven Drama . . . . . . . . . . . . . . . . . . . . . . . 15 4.2.1 Without Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 16 4.2.2 With Superego . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 16 4.2.3 Commentary . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 17 5 Discussion 18 5.1 A ‘Critic’s’ View . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 18 5.2 Performativity, or How to Build an Automated Identity . . . . . . . . . . . . . . . 19 5.3 From Playing Games to Playing Roles . . . . . . . . . . . . . . . . . . . . . . . . . 20 6 Conclusion 21 A Appendix A: Drama Machine 25 B Appendix B: Prompts for the Interview 25 B.1 Prompt for Jenny . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 25 B.2 Prompt for Cleo . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 26 B.3 Prompt for Sasha . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27 C Appendix C: Prompts for the Plot-Driven Drama 27 C.1 Prompt for Ashley . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27 C.2 Prompt for Timothy . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27 C.3 Prompt for Ben . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 29 C.4 Prompt for Sasha . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 29 1 arXiv:2408.01725v2 [cs.CY] 31 Aug 2024 |
| **中文标题** | [待翻译] |
| **期刊** | Nature |
| **DOI** |  |
| **作者** | 未找到作者 |
| **通讯作者** | 未找到 |
| **接收日期** | 31 Aug 2024 |
| **发表日期** | 20 June 2024 |



**浓度数据:** 3M, 2.1M, 6M, 13M, 18m, 3m

**温度条件:** 29, 28, 27, 024°C

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

*报告生成时间：2026-04-11 11:20:50*

*💡 提示：请查看同目录下的完整分析版本（带AI深度分析内容）*
