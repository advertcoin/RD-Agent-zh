---
language:
- en
size_categories:
- 10K<n<100K
license: mit
configs:
- config_name: default
  data_files:
  - split: train
    path: data/train-*
  splits:
  - name: train
    num_examples: 40315
---

# DeepScaleR 数学推理数据集

用于 DeepScaleR 的数据集：通过缩放强化学习用1.5B模型超越 O1-Preview。

> DeepScaleR-1.5B-Preview 在 AIME 2024 上达到**43.1%的 Pass@1 准确率**，比基础模型（28.8%）**提高了15%**，并且仅用1.5B参数通过分布式强化学习**超越了 OpenAI 的 O1-Preview 性能**。

## 概述

**DeepScaleR 数据集**是一个精心策划的集合，包含约**40,000个独特的数学问题-答案对**，旨在通过强化学习训练数学推理模型。该数据集涵盖从高中到奥林匹克级别的各种竞赛级数学问题，为在推理任务上扩展强化学习算法提供了坚实的基础。

DeepScaleR 证明，通过战略数据策划结合迭代上下文长度缩放（8K→16K→24K）使用 Group Relative Policy Optimization (GRPO)，可以实现复杂的数学推理。

### 数据来源

我们的训练数据集由来自著名数学竞赛和策划数据集的问题组成：

- **AIME**（美国数学邀请赛）问题（1984-2023）
- **AMC**（美国数学竞赛）问题（2023年之前）
- **Omni-MATH** 数据集
- **Still** 数据集

### 数据字段

数据集包含三个关键字段：

- `problem`: 数学问题陈述，使用 LaTeX 符号格式化
- `solution`: 问题的官方解决方案，包括 LaTeX 格式和带框的最终答案。如果没有解决方案，`solution` 字段为空字符串
- `answer`: 最终数学结果/答案，通常从解决方案中提取

## CoT 质量评估

**重要**：原始数据在训练前必须进行优化。

| 维度 | 值 |
|-----------|-------|
| baseline_quality | low |
| task_type | math |
| polish_difficulty | high |

**基线**：82% 的 `solution` 为空，18% 太短（p50=373 tokens，摘要风格）。需要为所有样本生成探索性 CoT（供参考，结构良好的 CoT 的长度通常超过模型 max_position_embeddings tokens 的 1/4）。

## 许可证

此数据集以 MIT 许可证发布。