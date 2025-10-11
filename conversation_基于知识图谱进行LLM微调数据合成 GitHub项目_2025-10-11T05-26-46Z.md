### User
<user_info>
OS Version: linux 6.1.147
Shell: /bin/bash
Current working directory: /workspace
Is directory a git repo: Yes, at /workspace
Today's date: 2025-10-09
</user_info>



<git_status>
This is the git status at the start of the conversation. Note that this status is a snapshot in time, and will not update during the conversation.
On branch cursor/research-knowledge-graph-llm-data-synthesis-projects-16b8
nothing to commit, working tree clean

</git_status>


<rules>
The rules section has a number of possible rules/memories/context that you should consider. In each subsection, we provide instructions about what information the subsection contains and how you should consider/follow the contents of the subsection.

<always_applied_workspace_rules description="These are workspace-level rules that the agent must always follow.">
- 如果需要从huggingface上下载数据集、模型，请export HF_ENDPOINT=https://hf-mirror.com
- 如果是再10.8.71.126服务器上运行，请先激活py312虚拟环境
- 如果是在10.8.71.44服务器上运行，请先激活py310虚拟环境，C++激活/opt/rh/devtoolset-9/enable

</always_applied_workspace_rules>

</rules>



### User
<user_query>
帮我做下调研，主题是关于：基于知识图谱进行LLM微调数据合成 GitHub项目 注意，项目要使用知识图谱来进行LLM数据合成

</user_query>

### Assistant
I'll search GitHub in parallel for repositories that explicitly use knowledge graphs to synthesize fine-tuning data for LLMs.

### Assistant
I’m going to search GitHub via the API (unauthenticated) with multiple queries in parallel for repositories that use knowledge graphs to synthesize LLM fine-tuning data.

### Assistant
I'll run multiple targeted GitHub searches in parallel for KG-to-text, KG-based QG, and KELM-related repos to find projects that generate synthetic fine-tuning data from knowledge graphs.

### Assistant
I'll broaden and refine the GitHub search with targeted queries (instruction generation, SPARQL/Wikidata QG, KG triple-to-text) to find repos that synthesize LLM fine-tuning data from knowledge graphs. Now running them in parallel.

### Assistant
I'll run parallel searches for specific datasets and repos like KnowInstruct, GraphInstruct, KG-Instruct, and LC-QuAD generation pipelines to pinpoint KG-based LLM data synthesis projects.

### Assistant
I found some relevant KG-to-text and KG-based question generation repositories, but I'll refine the search to locate specific LC-QuAD, WebNLG, SPARQL2NL/RDF2Text, and KG→text/T5 repos that can be used to synthesize LLM fine-tuning data. Now running targeted searches in parallel.

### Assistant
我并行搜完了 GitHub，筛选出“确实用知识图谱来合成 LLM 微调数据”的项目，按两类（KG→Text 与 KG→QA/NL2SPARQL）给你简明清单和推荐组合。

### KG→Text（把三元组/子图口语化成文本，用于预训练/指令微调的事实样本）
- KELM-corpus（Google）: 由 Wikidata 三元组自动“口语化”成自然语言句子，可直接作为事实样本或指令数据。[KELM-corpus](https://github.com/google-research-datasets/KELM-corpus)
- WebNLG Challenge（官方）: 输入 RDF 三元组集合，输出高质量描述文本，含基线与评测脚本。[WebNLG/challenge-2020](https://github.com/WebNLG/challenge-2020)
- neural-rdf-verbalizer（DBpedia）: 神经口语化 RDF→Text 管线，可大规模从 DBpedia 合成文本。[dbpedia/neural-rdf-verbalizer](https://github.com/dbpedia/neural-rdf-verbalizer)
- RDF2text-GAN（DBpedia）: 用 GAN+RL 从 RDF 生成文本，适合做风格多样化数据增广。[dbpedia/RDF2text-GAN](https://github.com/dbpedia/RDF2text-GAN)
- DART（Yale-LILY）: 开放域结构化记录→文本的大规模数据集（含 KG 风格样本），常用于 T5/BART 训练与数据合成。[Yale-LILY/dart](https://github.com/Yale-LILY/dart)

### KG→QA / NL↔SPARQL（从图生成问答或 NL↔查询对，做 QA/工具使用/推理微调）
- LC-QuAD（AskNowQA）: 大规模问句–SPARQL 配对，围绕 DBpedia/Wikidata；可扩展为指令数据（问–答/问–查询）。[AskNowQA/LC-QuAD](https://github.com/AskNowQA/LC-QuAD)
- SQG（AskNowQA）: “Query Generation for QA over KBs” 相关代码，便于从 KG 生成可执行查询/问句。[AskNowQA/SQG](https://github.com/AskNowQA/SQG)
- Text-to-SPARQL 开发仓: 生成 NL–SPARQL 训练对（可反向用于问题生成/指令化）。[yazdipour/text-to-sparql-development](https://github.com/yazdipour/text-to-sparql-development)
- NMT-NL2SPARQL: 基于神经机器翻译的 NL→SPARQL 方案，便于自建 NL–查询配对并再指令化。[LouisQhh/NMT-NL2SPARQL](https://github.com/LouisQhh/NMT-NL2SPARQL)

### 实操建议（快速合成“KG 驱动”的指令微调数据）
- 事实型指令（知识注入/纠错事实）: 用 KELM 或 WebNLG/DART 产出 “Given triples -> write a factual sentence / verify fact”。
- KBQA/工具调用指令: 用 LC-QuAD/SQG 产问句–SPARQL，再把 SPARQL 执行结果做答案，组织成指令三元组。
- 风格/鲁棒性增广: 用 RDF2text-GAN 或 neural-rdf-verbalizer 扩展表述多样性，降低模板化气味。

例如把一个 LC-QuAD 样本指令化（示意）:
```json
{"instruction":"回答问题（可能需要图谱推理）：谁是电影《X》的导演？","input":"知识库：DBpedia/Wikidata；可用 SPARQL。","output":"Y"}
```
或工具使用风格（保留查询以便可执行）:
```json
{"instruction":"使用 SPARQL 查询并返回答案。","input":"QUESTION: ... \nSPARQL: SELECT ...","output":"ANSWER: ..."}
```

### 推荐组合（按目标）
- 事实/知识注入: KELM-corpus + WebNLG 或 DART（规模+质量兼顾）
- KBQA/推理/工具使用: LC-QuAD + SQG（附可执行 SPARQL 与答案）
- 表达多样性与鲁棒: neural-rdf-verbalizer 或 RDF2text-GAN 叠加增广

需要我基于其中任意一套仓库，帮你落地一条可复用的数据合成流水线（抓取 KG 子图→口语化/问答化→指令格式→抽样质检）吗？

- 我已并行检索并筛选出与“用知识图谱合成 LLM 微调数据”强相关的 GitHub 仓库，并按 KG→Text 与 KG→QA 两类整理，给出可直接落地的组合与示例指令格式。

### User
<user_query>
将完整的对话，不要修改，创建一个conversation_{topic}_{date-time}.md的文件，创建一个PR
</user_query>

### Assistant
I'll create a TODO list, get the current UTC timestamp, write the full conversation to a new markdown file named using your topic and timestamp, then stage and commit it, and attempt to create a PR. If PR creation requires authentication, I'll report the blocker and next steps.

### Assistant
I’m going to gather the full conversation content, format the filename with the provided topic and the current timestamp, write the markdown file, create a new branch, and prepare a PR.
