# 基于知识图谱进行LLM微调数据合成 - GitHub项目调研报告（完整版）

**调研日期**: 2025-10-09  
**调研主题**: 基于知识图谱进行LLM微调数据合成的GitHub项目及技术方案  
**更新说明**: 包含所有发现的相关项目，分类更详细

---

## 📋 目录
1. [调研概述](#调研概述)
2. [项目分类总览](#项目分类总览)
3. [核心数据合成项目](#核心数据合成项目)
4. [知识图谱构建项目](#知识图谱构建项目)
5. [KG-LLM集成框架](#kg-llm集成框架)
6. [论文与资源集合](#论文与资源集合)
7. [技术方案深度分析](#技术方案深度分析)
8. [实施路线图](#实施路线图)
9. [总结与建议](#总结与建议)

---

## 🎯 调研概述

经过全面搜索，发现GitHub上专门针对"基于知识图谱进行LLM微调数据合成"的项目**较为有限**，但存在多个相关方向的优秀项目。这些项目可以分为以下几类：

1. **直接数据合成项目** - 使用知识图谱生成训练数据
2. **知识图谱构建项目** - 用LLM构建和提取知识图谱
3. **KG-LLM集成项目** - 将知识图谱与LLM结合（主要是RAG）
4. **论文与资源集合** - 相关研究和论文整理

---

## 📊 项目分类总览

### 核心统计

| 类别 | 项目数量 | 代表项目 |
|------|---------|---------|
| **直接数据合成** | 4个 | GraphGen, Knowledge2Data, EasyInstruct, Synthetic Data Kit |
| **知识图谱构建** | 5个 | DeepKE, AutoKG, fusion-jena/auto-KG, kg2text |
| **KG-LLM集成/RAG** | 10+个 | HippoRAG, graph-rag-agent, KG_RAG |
| **论文资源** | 3个 | Awesome-LLM-KG, KG-LLM-Papers |

---

## 🎯 核心数据合成项目

### 1. **GraphGen** ⭐⭐⭐⭐⭐ [最直接相关]

- **项目地址**: https://github.com/open-sciencelab/GraphGen
- **Stars**: 381
- **最后更新**: 2025-09-30
- **组织**: open-sciencelab

#### 核心价值
这是**最直接针对主题**的项目 - 使用知识图谱指导合成数据生成来增强LLM的监督微调（SFT）。

#### 核心特性
- ✅ **知识驱动的数据合成**：基于知识图谱结构生成高质量训练数据
- ✅ **专为SFT设计**：直接用于LLM监督微调
- ✅ **AI4Science应用**：专注科研领域
- ✅ **框架集成**：支持Llama Factory、XTuner、Qwen
- ✅ **多种数据格式**：QA对、预训练数据、SFT数据
- ✅ **在线Demo**：https://g-app-center-120612-6433-jpdvmvp.openxlab.space

#### 技术栈
```python
知识图谱 → 数据生成器 → SFT数据集 → 微调框架
```

#### 适用场景
- 科学研究领域的专业知识微调
- 需要结构化推理的任务
- 领域专业化的LLM训练

---

### 2. **Knowledge2Data (zjunlp)** ⭐⭐⭐⭐

- **项目地址**: https://github.com/zjunlp/Knowledge2Data
- **Stars**: 3
- **论文**: https://arxiv.org/abs/2505.22633
- **数据集**: https://huggingface.co/datasets/zjunlp/Knowledge2Data
- **组织**: 浙江大学NLP实验室

#### 核心价值
**空间知识图谱指导的多模态合成** - 将知识图谱应用于多模态数据生成。

#### 核心特性
- ✅ **空间知识图谱（SKG）**：构建对象及其空间关系的图谱
- ✅ **多模态合成**：生成图像+文本配对数据
- ✅ **场景生成**：自动生成复杂场景描述
- ✅ **图像生成**：基于SKG生成对应图像
- ✅ **HuggingFace数据集**：提供训练和测试数据

#### 技术流程
```
定义对象和关系 → 生成空间KG → 场景描述生成 → 多模态数据生成
```

#### 模型依赖
- Diffusers-generation-text-box
- Stable-diffusion-xl-refiner
- Sam-vit-base

#### 创新点
- 利用空间关系知识指导生成
- 多模态数据一致性保证
- 可控的场景复杂度

---

### 3. **EasyInstruct (zjunlp)** ⭐⭐⭐⭐⭐

- **项目地址**: https://github.com/zjunlp/EasyInstruct
- **Stars**: 405
- **会议**: ACL 2024 System Demonstration
- **论文**: https://arxiv.org/abs/2402.03049
- **Demo**: https://huggingface.co/spaces/zjunlp/EasyInstruct

#### 核心价值
**易用的指令数据生成和选择框架** - 模块化的指令数据处理pipeline，包含**KG2Instruct方法**。

#### 核心特性

**Generators（生成器）**:
- ✅ **Self-Instruct**: 自我指令生成
- ✅ **Evol-Instruct**: 进化式指令生成
- ✅ **Backtranslation**: 反向翻译生成
- ✅ **KG2Instruct**: 📌 **从知识图谱生成指令数据**

**Selectors（选择器）**:
- ✅ Deduplicator: 去重
- ✅ LengthSelector: 长度筛选
- ✅ RougeSelector: ROUGE评分筛选
- ✅ GPTScoreSelector: GPT评分筛选
- ✅ PPLSelector: 困惑度筛选
- ✅ MTLDSelector: 文本词汇多样性筛选

#### KG2Instruct 方法
```python
from easyinstruct import KG2InstructGenerator

generator = KG2InstructGenerator(
    num_instructions_to_generate=100
)
generator.generate()
```

#### 支持的LLM
- OpenAI (GPT-3.5, GPT-4)
- Anthropic (Claude, Claude-Instant)
- Cohere (Command, Command-Light)

#### 技术亮点
- 模块化设计，易于扩展
- 完整的数据质量控制
- 多种生成和选择策略组合
- 友好的配置文件系统

---

### 4. **Synthetic Data Kit (Meta Llama)** ⭐⭐⭐⭐⭐

- **项目地址**: https://github.com/meta-llama/synthetic-data-kit
- **开发者**: Meta
- **PyPI**: synthetic-data-kit

#### 核心价值
Meta官方的**通用合成数据工具包** - 虽不直接使用知识图谱，但提供完整的数据合成最佳实践。

#### 4阶段Pipeline
```
Ingest → Create → Curate → Save-as
```

1. **Ingest**: 多格式文档解析（PDF/HTML/DOCX/PPT/YouTube等）
2. **Create**: 生成QA对、CoT推理、摘要等
3. **Curate**: LLM-as-Judge质量评估
4. **Save-as**: 保存为多种微调格式

#### 核心特性
- ✅ 批量目录处理
- ✅ 多模态支持
- ✅ 质量阈值筛选
- ✅ 多种输出格式（Alpaca, ChatML, JSONL等）
- ✅ 可定制提示词模板

#### 集成的方法论
基于顶级论文的最佳实践：
- **LIMA**: 质量>数量
- **Self-Alignment**: 指令反向翻译
- **Distilling Step-by-Step**: CoT提取
- **TinyStories**: 渐进式学习
- **Textbooks**: 高质量结构化数据
- **ToolLLM**: 工具使用数据

---

## 🔧 知识图谱构建项目

### 5. **DeepKE (zjunlp)** ⭐⭐⭐⭐⭐

- **项目地址**: https://github.com/zjunlp/DeepKE
- **Stars**: 4,135
- **会议**: EMNLP 2022
- **组织**: 浙江大学NLP实验室

#### 核心价值
**知识图谱提取和构建的开源工具包** - 从文本中提取实体和关系，构建知识图谱。

#### 核心功能
- 命名实体识别（NER）
- 关系提取（RE）
- 属性抽取（AE）
- 事件抽取（EE）
- 多模态知识提取

#### 应用价值
可以作为数据合成的前置步骤，先构建知识图谱，再基于图谱生成训练数据。

---

### 6. **AutoKG (zjunlp)** ⭐⭐⭐⭐

- **项目地址**: https://github.com/zjunlp/AutoKG
- **Stars**: 451
- **论文**: WWWJ 2024

#### 核心价值
**LLM驱动的知识图谱构建和推理** - 研究如何使用大语言模型自动构建和推理知识图谱。

#### 覆盖内容
- LLM for KG Construction
- LLM for KG Reasoning
- LLM for KG-enhanced Tasks

---

### 7. **fusion-jena/automatic-KG-creation-with-LLM** ⭐⭐⭐⭐

- **项目地址**: https://github.com/fusion-jena/automatic-KG-creation-with-LLM
- **Stars**: 289

#### 核心价值
使用LLM自动创建本体（Ontology）和知识图谱。

#### 特点
- 自动本体构建
- 知识图谱自动化
- LLM驱动的知识抽取

---

### 8. **UKPLab/kg2text** ⭐⭐⭐⭐

- **项目地址**: https://github.com/UKPLab/kg2text
- **Stars**: 94
- **会议**: TACL 2020
- **论文**: "Modeling Global and Local Node Contexts for Text Generation from Knowledge Graphs"

#### 核心价值
**知识图谱到文本生成** - 从知识图谱生成自然语言描述。

#### 应用
可用于将结构化知识图谱转换为自然语言训练数据。

---

### 9. **Few-Shot-KG2Text** ⭐⭐⭐

- **项目地址**: https://github.com/turboLJY/Few-Shot-KG2Text
- **Stars**: 19
- **会议**: ACL 2021 Findings
- **论文**: "Few-shot Knowledge Graph-to-Text Generation with Pretrained Language Models"

#### 核心价值
**少样本知识图谱到文本** - 在少样本场景下从KG生成文本。

---

## 🤝 KG-LLM集成框架

### 10. **HippoRAG** ⭐⭐⭐⭐⭐ [RAG框架，非数据合成]

- **项目地址**: https://github.com/OSU-NLP-Group/HippoRAG
- **Stars**: 2,846
- **会议**: NeurIPS 2024
- **论文**: https://arxiv.org/abs/2405.14831

#### ⚠️ 重要说明
**HippoRAG是RAG检索框架，不是数据合成工具**。

#### 功能定位
- ✅ 知识图谱增强的检索
- ✅ PageRank检索算法
- ✅ 持续学习能力
- ❌ 不生成微调训练数据
- ❌ 不用于数据合成

#### 可借鉴之处
- 知识图谱构建方法（OpenIE）
- 图与向量的融合检索

---

### 11. **graph-rag-agent** ⭐⭐⭐⭐

- **项目地址**: https://github.com/1517005260/graph-rag-agent
- **Stars**: 1,280

#### 核心价值
融合GraphRAG、LightRAG的**知识图谱RAG系统**。

#### 功能
- 知识图谱构建pipeline
- 多种搜索策略
- 评估框架

---

### 12. **KG_RAG (BaranziniLab)** ⭐⭐⭐⭐

- **项目地址**: https://github.com/BaranziniLab/KG_RAG
- **Stars**: 890

#### 核心价值
**生物医学领域的KG-RAG应用**。

---

## 📚 论文与资源集合

### 13. **Awesome-LLM-KG** ⭐⭐⭐⭐⭐

- **项目地址**: https://github.com/RManLuo/Awesome-LLM-KG
- **Stars**: 2,477

#### 核心价值
**统一LLM和KG的优秀论文集合**。

#### 覆盖主题
- KG-enhanced LLMs
- LLM-augmented KGs
- Synergized LLMs + KGs

---

### 14. **KG-LLM-Papers** ⭐⭐⭐⭐⭐

- **项目地址**: https://github.com/zjukg/KG-LLM-Papers
- **Stars**: 2,052

#### 核心价值
整合知识图谱和大语言模型的**论文列表**。

---

### 15. **其他zjunlp项目**

浙江大学NLP实验室还有多个相关项目：

| 项目 | Stars | 描述 |
|------|-------|------|
| **KnowLM** | 1,349 | 知识增强的大语言模型框架 |
| **EasyEdit** | 2,572 | LLM知识编辑框架 |
| **KnowledgeEditingPapers** | 1,167 | 知识编辑论文集合 |
| **KnowAgent** | 244 | 知识增强的LLM Agent |
| **KnowPrompt** | 205 | 知识感知的提示学习 |

---

## 🔬 技术方案深度分析

### 方案A: 知识图谱 → 指令数据生成

**代表项目**: EasyInstruct (KG2Instruct)

```
知识图谱三元组 → 模板/LLM → 指令-回答对 → 质量筛选 → 微调数据集
```

**流程**:
1. 从知识图谱提取三元组: `(实体1, 关系, 实体2)`
2. 设计提示词模板将三元组转换为问答
3. 使用LLM生成自然语言指令和回答
4. 质量评估和筛选
5. 格式化为微调数据

**优点**:
- 知识准确性高（基于已验证的KG）
- 可控性强
- 逻辑一致性好

**挑战**:
- 需要高质量知识图谱
- 生成的多样性可能受限
- 模板设计需要专业知识

---

### 方案B: 空间知识图谱 → 多模态数据

**代表项目**: Knowledge2Data

```
定义场景元素 → 构建空间KG → 生成场景描述 → 图像生成 → 多模态数据集
```

**流程**:
1. 定义对象及其空间关系
2. 构建空间知识图谱（SKG）
3. 基于SKG生成场景自然语言描述
4. 使用扩散模型生成对应图像
5. 生成图像-文本配对数据

**优点**:
- 多模态一致性
- 空间关系明确
- 适合视觉推理任务

**适用场景**:
- 视觉问答（VQA）
- 图像描述生成
- 空间推理任务

---

### 方案C: 知识驱动的合成数据生成

**代表项目**: GraphGen

```
领域知识图谱 → 知识采样 → LLM生成 → 数据合成 → SFT数据集
```

**流程**:
1. 构建或使用领域知识图谱
2. 从图谱中采样知识单元
3. 使用LLM基于知识生成训练样本
4. 质量控制和多样性保证
5. 生成面向特定任务的SFT数据

**优点**:
- 领域专业化
- 知识覆盖全面
- 支持科研应用

---

### 方案D: 文档 → KG → 数据合成

**综合方案**:

```
原始文档 → 知识抽取(DeepKE) → 知识图谱 → 数据生成(EasyInstruct) → 训练数据
```

**流程**:
1. 收集领域文档
2. 使用DeepKE等工具提取实体和关系
3. 构建领域知识图谱
4. 基于KG生成指令数据
5. 质量筛选和格式转换

**优点**:
- 端到端自动化
- 适合缺少现成KG的场景
- 可定制化

---

## 🛠️ 实施路线图

### 路线1: 快速启动（使用现有工具）

**适合**: 希望快速验证想法的团队

```bash
# Step 1: 安装EasyInstruct
pip install easyinstruct

# Step 2: 准备知识图谱数据
# 格式: (实体1, 关系, 实体2)

# Step 3: 使用KG2Instruct生成数据
from easyinstruct import KG2InstructGenerator
generator = KG2InstructGenerator(
    num_instructions_to_generate=1000
)
generator.generate()

# Step 4: 质量筛选
from easyinstruct import GPTScoreSelector
selector = GPTScoreSelector(threshold=8.0)
selector.process()

# Step 5: 使用Llama Factory进行微调
```

**时间**: 1-2周  
**成本**: 低（主要是API调用）

---

### 路线2: 完整定制（从零构建）

**适合**: 有特定需求的研究团队

```bash
# Phase 1: 知识图谱构建 (2-4周)
1. 收集领域文档和数据
2. 使用DeepKE进行知识抽取
3. 知识融合和图谱构建
4. 图谱质量验证

# Phase 2: 数据生成策略设计 (1-2周)
1. 设计生成提示词模板
2. 确定采样策略
3. 设计质量评估指标

# Phase 3: 数据生成 (2-3周)
1. 从KG采样知识单元
2. LLM生成训练样本
3. 多轮迭代优化

# Phase 4: 质量控制 (1-2周)
1. 自动评估（困惑度、多样性等）
2. LLM评分
3. 人工抽样验证

# Phase 5: 微调和评估 (2-3周)
1. 格式转换
2. 模型微调
3. 下游任务评估
```

**时间**: 2-3个月  
**成本**: 中-高

---

### 路线3: 多模态扩展

**适合**: 需要视觉-语言数据的团队

```bash
# 基于Knowledge2Data方法

# Step 1: 定义场景元素和关系
# Step 2: 构建空间知识图谱
# Step 3: 生成场景描述
# Step 4: 图像生成
# Step 5: 多模态数据配对
```

---

## 💡 技术选型建议

### 场景1: 科研领域专业知识微调

**推荐方案**:
- 主工具: **GraphGen**
- 辅助: DeepKE（知识抽取）
- 微调: Llama Factory

**理由**: GraphGen专为科研设计，直接支持SFT数据生成

---

### 场景2: 通用领域指令微调

**推荐方案**:
- 主工具: **EasyInstruct**
- 方法: KG2Instruct
- 质量控制: GPTScoreSelector + RougeSelector

**理由**: 成熟的框架，模块化设计，易于定制

---

### 场景3: 多模态数据生成

**推荐方案**:
- 主工具: **Knowledge2Data**
- 图像生成: Stable Diffusion XL
- 文本生成: GPT-4/Claude

**理由**: 专门设计用于多模态场景

---

### 场景4: 从文档开始（无现成KG）

**推荐方案**:
```
文档 → DeepKE (抽取) → Neo4j (存储) → EasyInstruct (生成)
```

**理由**: 端到端pipeline，适合从零开始

---

## 📈 技术栈推荐

### 知识图谱构建
- **提取**: DeepKE, SpaCy, Stanford CoreNLP
- **存储**: Neo4j, ArangoDB, NetworkX
- **可视化**: Gephi, Cytoscape

### LLM推理
- **API**: OpenAI GPT-4, Anthropic Claude
- **开源**: vLLM + Llama 3.3, Qwen
- **本地**: Ollama

### 数据生成
- **框架**: EasyInstruct, GraphGen
- **通用**: Synthetic Data Kit
- **多模态**: Knowledge2Data

### 微调框架
- **主流**: Llama Factory, XTuner, Axolotl
- **轻量**: PEFT, LoRA
- **分布式**: DeepSpeed, FSDP

### 质量评估
- **自动**: Perplexity, ROUGE, BLEU
- **LLM评分**: GPT-4-as-Judge
- **人工**: 抽样标注

---

## 🎓 学习资源

### 论文阅读路线

**基础**:
1. Self-Instruct (2022)
2. Evol-Instruct / WizardLM (2023)
3. Instruction Backtranslation (2023)

**知识图谱相关**:
1. KG2Instruct / InstructIE (2023)
2. GraphGen (2025)
3. Knowledge2Data (2025)

**质量控制**:
1. LIMA (2023)
2. Textbooks Are All You Need (2023)
3. Distilling Step-by-Step (2023)

### GitHub资源

**论文集合**:
- Awesome-LLM-KG: https://github.com/RManLuo/Awesome-LLM-KG
- KG-LLM-Papers: https://github.com/zjukg/KG-LLM-Papers

**zjunlp系列**:
- DeepKE: 知识抽取
- EasyInstruct: 指令生成
- AutoKG: KG构建
- KnowLM: 知识LLM

---

## ⚠️ 常见问题与注意事项

### Q1: 为什么专门的项目不多？

**答**: 
1. **领域较新**: KG+LLM数据合成是新兴方向
2. **工具通用化**: 很多团队使用通用工具组合
3. **商业闭源**: 部分优秀方案未开源
4. **学术周期**: 新论文对应代码发布需要时间

### Q2: HippoRAG不能用于数据合成吗？

**答**: 
- ❌ HippoRAG是**RAG检索框架**，用于推理时检索
- ❌ 不生成微调训练数据
- ✅ 但其知识图谱构建方法可以借鉴

### Q3: GraphRAG系列都是RAG吗？

**答**:
- GraphRAG (Microsoft): RAG框架
- LightRAG: RAG框架
- graph-rag-agent: RAG系统
- **GraphGen**: ✅ 数据合成工具（不是RAG）

### Q4: 如何选择合适的项目？

**决策树**:
```
是否有现成知识图谱？
├─ 是 → 使用EasyInstruct (KG2Instruct)
└─ 否 → 是否有领域文档？
    ├─ 是 → DeepKE提取 + EasyInstruct生成
    └─ 否 → 使用Synthetic Data Kit从零开始

是否需要多模态？
└─ 是 → Knowledge2Data

是否是科研领域？
└─ 是 → GraphGen
```

---

## 🔮 未来趋势

### 1. **知识图谱自动化**
- LLM驱动的端到端KG构建
- 零样本知识抽取
- 多模态知识融合

### 2. **质量与多样性平衡**
- 更智能的采样策略
- 知识覆盖度评估
- 自适应生成

### 3. **垂直领域专业化**
- 医学、法律、金融等专业KG
- 领域特定的生成策略
- 专家知识注入

### 4. **多模态扩展**
- 图像-文本-知识三元融合
- 视频理解与生成
- 跨模态推理

### 5. **可解释性增强**
- 生成过程可追溯
- 知识来源标注
- 推理路径可视化

---

## 📊 完整项目列表

### 数据合成类（4个）

| 项目 | Stars | 描述 | 推荐度 |
|------|-------|------|--------|
| GraphGen | 381 | 知识驱动的SFT数据生成 | ⭐⭐⭐⭐⭐ |
| Knowledge2Data | 3 | 空间KG多模态合成 | ⭐⭐⭐⭐ |
| EasyInstruct | 405 | 指令数据生成框架 | ⭐⭐⭐⭐⭐ |
| Synthetic Data Kit | - | Meta通用合成工具 | ⭐⭐⭐⭐⭐ |

### 知识图谱构建类（5个）

| 项目 | Stars | 描述 | 推荐度 |
|------|-------|------|--------|
| DeepKE | 4,135 | KG提取工具包 | ⭐⭐⭐⭐⭐ |
| AutoKG | 451 | LLM for KG | ⭐⭐⭐⭐ |
| fusion-jena/auto-KG | 289 | 自动KG创建 | ⭐⭐⭐⭐ |
| kg2text | 94 | KG到文本生成 | ⭐⭐⭐⭐ |
| Few-Shot-KG2Text | 19 | 少样本KG2Text | ⭐⭐⭐ |

### zjunlp其他相关项目（10+个）

| 项目 | Stars | 描述 |
|------|-------|------|
| KnowLM | 1,349 | 知识LLM框架 |
| EasyEdit | 2,572 | 知识编辑 |
| KnowAgent | 244 | 知识Agent |
| OneKE | 118 | KG提取系统 |
| ... | ... | ... |

### 论文资源类（3个）

| 项目 | Stars | 描述 | 推荐度 |
|------|-------|------|--------|
| Awesome-LLM-KG | 2,477 | LLM+KG论文集 | ⭐⭐⭐⭐⭐ |
| KG-LLM-Papers | 2,052 | KG-LLM论文 | ⭐⭐⭐⭐⭐ |
| KnowledgeEditingPapers | 1,167 | 知识编辑论文 | ⭐⭐⭐⭐ |

---

## 🎯 总结与建议

### 核心发现

1. **专门项目确实有限** - GitHub上直接针对"KG → LLM微调数据"的项目约4-5个
2. **zjunlp是主力军** - 浙江大学NLP实验室贡献了多个高质量项目
3. **工具组合是常态** - 实践中多使用工具组合而非单一项目
4. **RAG vs 数据合成要区分** - 很多KG+LLM项目是RAG，不是数据合成

### 实施建议

**入门推荐**:
1. 先用**EasyInstruct**快速验证
2. 如有知识图谱，直接用**KG2Instruct**
3. 无KG则从**Synthetic Data Kit**开始

**深度定制**:
1. 使用**DeepKE**构建领域KG
2. 参考**GraphGen**设计生成策略
3. 借鉴**Knowledge2Data**的多模态方法

**质量保证**:
1. 采用多种质量筛选指标
2. LLM-as-Judge评分
3. 人工抽样验证
4. 下游任务评估

### 关键洞察

✅ **质量 > 数量** (LIMA原则仍然适用)  
✅ **知识图谱提供结构化保证**  
✅ **领域专业化是趋势**  
✅ **工具组合优于单一工具**  
✅ **评估体系至关重要**

### 未来方向

🔮 **更智能的KG构建** - LLM自动化提取  
🔮 **端到端Pipeline** - 从文档到微调数据  
🔮 **多模态融合** - 图像+文本+知识  
🔮 **可解释性** - 知识来源可追溯  
🔮 **持续学习** - 知识图谱动态更新

---

## 📧 联系与贡献

**报告维护**: Cursor AI Assistant  
**建议更新周期**: 每季度  
**欢迎补充**: 如发现新项目请告知

**有用的链接**:
- zjunlp组织: https://github.com/zjunlp
- Awesome-LLM-KG: https://github.com/RManLuo/Awesome-LLM-KG
- Meta Llama: https://github.com/meta-llama

---

**最后更新**: 2025-10-09  
**版本**: v2.0  
**项目总数**: 25+

