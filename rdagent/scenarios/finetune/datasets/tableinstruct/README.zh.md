---
language:
- en
size_categories:
- 1K<n<10K
license: mit
configs:
- config_name: test
  data_files:
  - split: test
    path: data/test-*
  splits:
  - name: test
    num_examples: 886
- config_name: train
  data_files:
  - split: train
    path: data/train-*
  splits:
  - name: train
    num_examples: ~10K
---

# TableBench：表格问答数据集

用于 TableBench 的数据集：一个全面且复杂的表格问答基准测试。

> TableBench 是一个**全面**且**复杂**的基准测试，旨在评估表格问答（TableQA）能力，涵盖**4大类别**中的**18个问题类别**，包含**886**个精心策划的测试用例。

## 概述

**TableBench 数据集**包含两个主要组件：

1. **TableBench（测试）**：886个高质量测试用例，涵盖4大推理类别
2. **TableInstruct（训练）**：大规模训练数据集，包含多样化的表格问答示例

TableBench 大大推动了大型语言模型在复杂 TableQA 场景中的边界，与现实世界表格问答应用中的"问题推理复杂性"维度紧密对齐。

### 任务类别

基准测试涵盖**4大类别**，包含**18个子任务**：

1. **事实核查**：验证表数据中的事实陈述
   - 简单事实验证、跨表验证、时间一致性

2. **数值推理**：数学计算和比较
   - 算术运算、聚合、比较分析

3. **数据分析**：复杂分析推理
   - 影响分析、相关性分析、趋势预测、统计分析

4. **可视化**：图表生成和解释
   - 条形图、折线图、饼图、散点图

### 数据来源

**测试数据（TableBench）**：
- 仓库：[Multilingual-Multimodal-NLP/TableBench](https://huggingface.co/datasets/Multilingual-Multimodal-NLP/TableBench)
- 886个精心策划和验证的测试用例
- 2025年4月发布增强版，包含错误修正

**训练数据（TableInstruct）**：
- 仓库：[Multilingual-Multimodal-NLP/TableInstruct](https://huggingface.co/datasets/Multilingual-Multimodal-NLP/TableInstruct)
- 用于表格问答的大规模指令微调数据集
- 多样化的问题类型和推理模式

### 数据字段

数据集包含以下关键字段：

TableInstruct 数据集包含以下字段：

- `id` (string): 每个样本的唯一标识符
- `qtype` (string): 主要任务类别（4个值）
  - `FactChecking`, `NumericalReasoning`, `DataAnalysis`, `Visualization`
- `qsubtype` (string): 特定子任务类型（18个值）
  - 示例：`Counting`, `Aggregation`, `Comparison`, `CorrelationAnalysis` 等
- `instruction` (string): 完整的指令模板，包含任务指南
  - 包含定义任务方法的完整提示模板
  - 包括角色定义、指南、代码格式要求
  - 通常根据指令类型为800-15,000字符
- `instruction_type` (string): 推理策略类型（4个值）
  - `DP`（直接提示）、`TCoT`（文本思维链）
  - `PoT`（思维程序）、`SCoT`（结构化思维链）
- `table` (string): JSON 格式的表格数据
  - 结构：`{"columns": [...], "data": [[...], [...], ...]}`
- `question` (string): 关于表格的特定问题
- `response` (string): 模型的答案，包括推理过程

**TableBench 测试数据集字段**：

- `question`: 表格问题或任务描述
- `table`: 表格数据（JSON 格式）
- `answer`: 真实答案
- `category`: 主要类别
- `subcategory`: 特定子任务类型

### 指令类型和推理策略

Tablebench 训练数据（TableInstruct）支持多种指令类型内容，定义了模型如何进行推理和生成答案。理解这些类型对于数据集过滤和微调策略选择至关重要。

### 可用的指令类型
**1. 直接提示（DP）**
**特点**：
- 直接提供解决方案，无需中间推理步骤
- 最简单的指令格式，专注于即时答案生成
- 最适合直接的事实核查和简单查询
**指令模板模式**：
  您是一名表格分析师。您的任务是根据表格内容回答问题。
  阅读下面的 JSON 格式表格：[TABLE]
  问题：[QUESTION]
  直接回答。
  **响应格式**：
  [直接答案]

**2. 文本思维链（TCoT）**
**特点**：
- LLMs 通过文本推理逐步推导中间步骤
- 每个推理步骤的自然语言解释
- 适用于需要逻辑推理的复杂推理

**指令模板模式**：
  您是一名表格分析师。您的任务是根据表格内容回答问题。
  [逐步推理指南]
  逐步思考
  展示您的推理过程
  提供最终答案
  ***响应格式**：
  让我们逐步分析：
  [第一步推理]
  [第二步推理]
  ...
  最终答案：[答案]

#### 3. 思维程序（PoT）

**特点**：
- 将问题分解为可执行的 Python 代码
- 使用编程将计算与推理分离
- 最适合数值推理和计算任务
- TableInstruct 中分析任务最常见的类型

**指令模板模式**（实际来自数据集）：
  您是一名精通 Python 的数据分析师。您的任务是编写可执行的 Python
  代码来分析表格然后回答问题。
  [指南]
  1. 根据问题，写出您的分析方法，然后编写 Python 代码
  2. 代码需要简洁易懂
  3. 代码块需要严格以
  '''
  import pandas as pd
  df = pd.read_csv('table.csv')
  ...
  print(f'Final Answer: {answer}')
  '''
  4.您的分析必须完全基于上述数据
  5.生成可执行代码并使用 print 函数输出结果
  6.确保使用以下方式加载表格：df = pd.read_csv('table.csv')

#### 4. 符号思维链（SCoT）

**特点**：
- 一种利用基于 Python 的指令促进逻辑推理的方法
- 将符号推理与可执行代码验证相结合
- 重复三个主要步骤，直到得出明确结论
- 通过强调迭代分析-生成-模拟周期与 PoT 区分

**三步过程**：
- **步骤1**：分析可用信息以确定下一步行动
- **步骤2**：使用 Python 编程语言命令生成指令
- **步骤3**：通过执行指令并分析结果来模拟结果

**指令模板模式**：
  您是一名表格分析师。使用符号推理和迭代 Python 命令。
  过程：
  步骤1：分析可用信息以确定下一步行动
  步骤2：生成 Python 编程语言命令
  步骤3：通过执行指令并分析结果来模拟结果
  重复这三个步骤，直到得出明确结论

### 评估指标

根据任务类型使用不同的指标：

| 任务类型 | 指标 | 描述 |
|-----------|--------|-------------|
| 事实核查 | 精确匹配（EM） | 预测陈述的精确匹配 |
| 数值推理 | 精确匹配（EM） | 数值输出的正确性 |
| 影响分析 | 精确匹配（EM） | 影响因素的精确匹配 |
| 相关性/趋势/统计 | EM_with_error_10 | ±10% 数值误差范围 |
| 其他数据分析 | ROUGE-L | 用于开放式文本响应 |
| 可视化 | Pass@1 | 首次尝试生成正确图表 |

## CoT 质量评估

**重要**：考虑在训练准备期间增强推理链。

| 维度 | 值 |
|-----------|-------|
| baseline_quality | medium-high |
| task_type | table_qa |
| polish_difficulty | medium |

**基线**：训练数据（TableInstruct）包含推理解析，但测试数据侧重于最终答案。对于复杂推理任务（数据分析、数值推理），生成详细的逐步 CoT 可以显著提高模型性能。

**建议**：对于数据分析和数值推理类别，扩展推理链以包含：
- 表格理解和模式识别
- 逐步计算或逻辑推理
- 中间结果和验证
- 带有置信度指标的最终答案

## 示例

### 事实核查
```json
{
  "question": "Based on the table, verify if the statement is true: 'Company A had higher revenue than Company B in Q4 2023'",
  "table": "| Company | Q4 2023 Revenue |\n|---------|----------------|\n| A       | $2.5M          |\n| B       | $3.1M          |",
  "answer": "False",
  "category": "Fact Checking",
  "subcategory": "simple_fact_verification"
}
```

### 数值推理
```json
{
  "question": "What is the total revenue across all quarters for Product X?",
  "table": "| Quarter | Product X Revenue |\n|---------|------------------|\n| Q1      | 150              |\n| Q2      | 200              |\n| Q3      | 175              |\n| Q4      | 225              |",
  "answer": "750",
  "category": "Numerical Reasoning",
  "subcategory": "aggregation"
}
```

### 数据分析
```json
{
  "question": "Analyze the correlation between marketing spend and sales growth. What is the correlation coefficient?",
  "table": "| Month | Marketing ($K) | Sales Growth (%) |\n|-------|----------------|------------------|\n| Jan   | 50             | 12               |\n| Feb   | 75             | 18               |\n| Mar   | 60             | 15               |",
  "answer": "0.95",
  "category": "Data Analysis",
  "subcategory": "correlation_analysis"
}
```

## 许可证

此数据集以 MIT 许可证发布。