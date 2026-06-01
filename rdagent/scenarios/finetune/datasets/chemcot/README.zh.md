---
language:
- en
license: mit
tags:
- chemistry
- chain-of-thought
- molecular-reasoning
size_categories:
- 10K<n<100K
task_categories:
- text-generation
- question-answering
---

# ChemCoT 数据集

来自 [ChemCoTBench](https://arxiv.org/abs/2505.21318) 的带有思维链注释的化学推理数据集。

**仓库**: [OpenMol/ChemCoTDataset](https://huggingface.co/datasets/OpenMol/ChemCoTDataset)

## 概述

**ChemCoTDataset** 提供约23K高质量思维链样本，用于训练化学推理模型。CoT 注释从最先进的推理模型（Gemini-2.5-pro、DeepSeek-R1、Claude-3.7-sonnet-thinking）中提取，并由13名化学博士候选人验证，准确率超过90%。

### 数据集规模

| 类别 | 子任务 | 样本数 |
|----------|----------|---------|
| mol_und | fg_count, ring_count, ring_system_scaffold, Murcko_scaffold | 6,319 |
| mol_edit | add, delete, sub | 4,497 |
| mol_opt | drd, gsk, jnk, qed, solubility, logp | 5,587 |
| rxn | fs_by_product, fs_major_product, rcr | 6,820 |
| **总计** | **16个子任务** | **23,223** |

## 任务

### 1. 分子理解 (mol_und)

| 子任务 | 描述 |
|---------|-------------|
| `fg_count` | 官能团计数 |
| `ring_count` | 环计数 |
| `Murcko_scaffold` | Murcko 骨架提取 |
| `ring_system_scaffold` | 环系骨架提取 |

**指标**：计数任务使用 MAE，骨架提取使用 Tanimoto 相似度

### 2. 分子编辑 (mol_edit)

| 子任务 | 描述 |
|---------|-------------|
| `add` | 向分子添加官能团 |
| `delete` | 从分子中删除官能团 |
| `sub` | 替换分子中的官能团 |

**指标**：Pass@1（有效性和指令匹配）

### 3. 分子优化 (mol_opt)

| 子任务 | 描述 |
|---------|-------------|
| `logp` | LogP（脂溶性）优化 |
| `solubility` | 水溶性优化 |
| `qed` | QED（药物相似性）优化 |
| `drd` | DRD2 结合亲和力优化 |
| `gsk` | GSK3-beta 结合亲和力优化 |
| `jnk` | JNK3 结合亲和力优化 |

**指标**：平均改善率、成功率

### 4. 反应预测 (rxn)

| 子任务 | 描述 |
|---------|-------------|
| `fs` | 正向合成（主要产物 + 副产物预测） |
| `rcr` | 反应条件推荐（催化剂预测） |

**指标**：Top-1 准确率、指纹相似度

## 数据格式

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `id` | string | 唯一样本标识符 |
| `query` | string | 化学问题 |
| `task` | string | 任务类别（mol_und, mol_edit, mol_opt, rxn） |
| `subtask` | string | 特定子任务名称 |
| `struct_cot` | string | 结构化思维链推理 |
| `raw_cot` | string | 原始思维链注释 |
| `meta` | object | 附加元数据 |

## CoT 质量评估

**重要**：提取的 CoT 可能需要领域细化。

| 维度 | 值 |
|-----------|-------|
| baseline_quality | medium-high |
| task_type | chemistry |
| polish_difficulty | medium |

**基线**：CoT 从 Gemini-2.5-pro/DeepSeek-R1/Claude 提取，由13名化学博士候选人验证（>90%准确率）。论文指出："提取策略在化学领域效果不佳"——考虑专家细化以获得最佳结果。

## 许可证

MIT 许可证