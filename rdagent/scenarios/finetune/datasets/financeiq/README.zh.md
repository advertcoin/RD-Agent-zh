---
language:
- zh
license: cc-by-nc-sa-4.0
tags:
- finance
- chinese
- multiple-choice
- professional-certification
size_categories:
- 1K<n<10K
task_categories:
- question-answering
- text-generation
---

# FinanceIQ 数据集

涵盖10个主要金融领域的中文金融专业认证考试题目。

**仓库**: [LlamaFactory/FinanceIQ](https://huggingface.co/datasets/LlamaFactory/FinanceIQ)

## 概述

**FinanceIQ 数据集**是一个包含约**6,179道多项选择题**的综合集合，来自中国金融专业认证考试。它涵盖10个不同的金融领域，为评估中文语言模型的金融推理能力提供了强大的基准测试。

### 数据集规模

| 类别 | 中文名称 | 样本数 |
|----------|--------------|---------|
| Insurance (CICE) | 保险从业资格CICE | 596 |
| Fund Practitioner | 基金从业资格 | 772 |
| Futures Practitioner | 期货从业资格 | 333 |
| CPA | 注册会计师（CPA） | 1,211 |
| Financial Planner | 理财规划师 | 195 |
| Tax Advisor | 税务师 | 388 |
| Actuary (Financial Math) | 精算师-金融数学 | 44 |
| Economist | 经济师 | 420 |
| Securities Practitioner | 证券从业资格 | 1,076 |
| Banking Practitioner | 银行从业资格 | 1,144 |
| **总计** | **10个类别** | **6,179** |

## 任务

### 单选题问答

**任务**：为每个金融问题从四个选项中选择正确答案（A/B/C/D）。

**评估**：LLM Judge（将模型的答案选择与真实答案进行比较）

**指标**：每个类别的准确率、平均准确率

## 数据格式（CSV 字段）

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `Question` | string | 中文问题文本 |
| `A` | string | 选项 A 文本 |
| `B` | string | 选项 B 文本 |
| `C` | string | 选项 C 文本 |
| `D` | string | 选项 D 文本 |
| `Answer` | string | 正确答案（A、B、C 或 D） |

### 示例数据

```csv
Question,A,B,C,D,Answer
关于生命价值理论的理解，以下哪一项表述是不正确的？,补偿生命经济价值可能受到的损失...,个人预期收入的货币价值...,任何触及个人收入能力的事件...,早逝、残疾、退休或失业可能导致...,B
```

## 数据划分策略

数据集使用基于末尾的划分策略：

- **测试集**：从每个类别的末尾取（每个类别最多50个样本，如果少于50个则取50%）
- **训练集**：取开头剩余的样本

这确保了所有类别之间一致的训练/测试分离。

## 类别分布分析

**重要**：样本分布高度不平衡：

| 类别 | 样本数 | 占总数的百分比 | 备注 |
|----------|---------|------------|------|
| CPA | 1,211 | 19.6% | 最大 |
| Banking | 1,144 | 18.5% | |
| Securities | 1,076 | 17.4% | |
| Fund | 772 | 12.5% | |
| Insurance | 596 | 9.6% | |
| Economist | 420 | 6.8% | |
| Tax | 388 | 6.3% | |
| Futures | 333 | 5.4% | |
| Financial Planner | 195 | 3.2% | 小 |
| **Actuary** | **44** | **0.7%** | **非常小** |

**建议**：生成训练数据时，确保跨类别平衡采样，特别是精算师（精算师）只有44个样本。

## CoT 质量评估

**重要**：原始数据仅包含问答对，没有推理链。

| 维度 | 值 |
|-----------|-------|
| baseline_quality | N/A (无 CoT) |
| task_type | finance reasoning |
| polish_difficulty | medium |

**基线**：问题是多项选择题格式，没有解释。**训练样本必须生成 CoT（思维链推理）**才能获得良好结果。

## 基线性能

| 模型 | 准确率 |
|-------|----------|
| Qwen2.5-7B-Instruct（零样本） | ~65% |

**注意**：精算师（精算师-金融数学）类别特别具有挑战性，基线准确率约为27-36%。

## 许可证

CC-BY-NC-SA-4.0 许可证