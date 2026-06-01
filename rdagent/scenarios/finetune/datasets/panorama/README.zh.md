---
language:
- en
license: cc-by-nc-4.0
tags:
- patent
- legal
- retrieval
- classification
size_categories:
- 100K<n<1M
task_categories:
- text-classification
- question-answering
---

# PANORAMA 数据集

捕获专利审查决策轨迹和理由的基准测试，来自 [PANORAMA](https://huggingface.co/datasets/LG-AI-Research/PANORAMA)。

**仓库**: [LG-AI-Research/PANORAMA](https://huggingface.co/datasets/LG-AI-Research/PANORAMA)

## 任务

### 1. PAR4PC: 专利权利要求的现有技术检索

**任务**：多标签分类 - 从8个候选中选择相关的现有技术文档。

**训练样本**：54,028

**指标**：精确匹配准确率、自定义分数（部分得分）

### 2. PI4PC: 专利权利要求的段落识别

**任务**：单选题 - 识别现有技术文档中最相关的段落。

**训练样本**：64,210

**指标**：精确匹配准确率

### 3. NOC4PC: 新颖性和非显而易见性分类

**任务**：三分类 - 确定权利要求应被 ALLOW、102 驳回或 103 驳回。

**训练样本**：136,211

**指标**：Macro F1-score、每类准确率

## 法律背景

- **35 U.S.C. §102（新颖性）**：如果被单个现有技术参考文献预期，权利要求被驳回
- **35 U.S.C. §103（非显而易见性）**：如果从组合现有技术来看是显而易见的，权利要求被驳回

## 数据格式（Parquet 字段）

### PAR4PC / PI4PC 格式

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `application_number` | str | 专利申请标识符 |
| `claim_number` | int64 | 正在评估的特定权利要求编号 |
| `context` | dict | 专利上下文：`{abstract: str, claims: list[str], title: str}` |
| `options` | dict | 8个候选文档：`{A: {abstract, claims, patent_id, title}, B: {...}, ...}` |
| `gold_answers` | ndarray | 正确答案标签，例如 `array(['G'])` 或 `array(['A', 'C'])` |
| `silver_answers` | ndarray | 部分正确答案 |
| `negative_answers` | ndarray | 错误选项 |

**注意**：PI4PC 有一个额外的 `prior_art_specification` 字段，包含相关现有技术文档文本。

### NOC4PC 格式

| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `application_number` | str | 专利申请标识符 |
| `claim_number` | int64 | 正在评估的特定权利要求编号 |
| `context` | dict | 专利上下文：`{abstract: str, claims: list[str], title: str}` |
| `prior_art_specifications` | list | 现有技术文档规范 |
| `answer` | str | 分类标签：`ALLOW`、`102` 或 `103` |

**重要**：数组字段（`gold_answers`、`silver_answers`、`negative_answers`）是 `numpy.ndarray` 类型。在处理前使用 `.tolist()` 转换为 Python 列表。

### 示例数据

```python
{
    "application_number": 14281639,
    "claim_number": 1,
    "context": {
        "abstract": "In an endodontic procedure...",
        "claims": ["claim 1 text", "claim 2 text", ...],
        "title": "Method for irrigating root canals"
    },
    "options": {
        "A": {"abstract": "...", "claims": [...], "patent_id": "US1234567", "title": "..."},
        "B": {"abstract": "...", "claims": [...], "patent_id": "US2345678", "title": "..."},
        # ... G, H
    },
    "gold_answers": array(['G'], dtype=object),  # numpy.ndarray, 使用 .tolist() -> ['G']
    "negative_answers": array(['A', 'B', 'C', 'D', 'E', 'F', 'H'], dtype=object)
}
```

## CoT 质量评估

**重要**：此数据集不包含 CoT 注释。

| 维度 | 值 |
|-----------|-------|
| baseline_quality | N/A (无 CoT) |
| task_type | legal reasoning |
| polish_difficulty | high |

**基线**：原始数据包含驳回理由，但**没有**分步推理链。论文明确指出"缺乏真实 CoT"。**训练前必须生成 CoT**。

## 基线性能（CoT 提示）

| 任务 | 最佳模型 | 分数 |
|------|-----------|-------|
| PAR4PC | Gemma-3-12B | 77.30% |
| PI4PC | GPT-4o | 62.62% |
| NOC4PC | Claude-3.7-Sonnet | 45.40% |

## 引用

```bibtex
@article{panorama2024,
  title={PANORAMA: A Dataset and Benchmarks Capturing Decision Trails and Rationales in Patent Examination},
  author={LG AI Research and KAIST},
  year={2024},
  url={https://huggingface.co/datasets/LG-AI-Research/PANORAMA}
}
```

## 许可证

CC-BY-NC-4.0 许可证