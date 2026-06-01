---
license: cc-by-4.0
configs:
- config_name: PQA
	data_files:
		- split: train
			path: PQA.json
		- split: test
			path: PQA_test.json
- config_name: ERR
	data_files:
		- split: train
			path: ERR.json
		- split: test
			path: ERR_test.json
- config_name: ORD
	data_files:
		- split: train
			path: ORD.json
		- split: test
			path: ORD_test.json
- config_name: GEN
	data_files:
		- split: train
			path: GEN.json
		- split: test
			path: GEN_test.json
---

# BioProBench 数据集（用于 LLM 微调）

BioProBench 是一个大规模、多任务基准测试，专注于大型语言模型（LLMs）的生物协议理解和推理。它包含四个微调任务：协议问答（PQA）、步骤排序（ORD）、错误纠正（ERR）和协议生成（GEN）。

该数据集基于约27K生物协议的原始语料库构建，在各个任务中提供超过550K结构化实例，每个任务有1000个示例的保留测试集。有关完整详细信息，请参阅原始基准测试：
- 代码：https://github.com/YuyangSunshine/bioprotocolbench/
- 数据集中心：https://huggingface.co/BioProBench
- 许可证：CC BY 4.0

## 数据文件

每个任务（训练/测试）的 JSON 文件按任务组织。如果您的微调管道需要本地文件，请将它们放在此 README 旁边或相应更新路径。

- PQA: [bioprobench/PQA.json](bioprobench/PQA.json), [bioprobench/PQA_test.json](bioprobench/PQA_test.json)
- ERR: [bioprobench/ERR.json](bioprobench/ERR.json), [bioprobench/ERR_test.json](bioprobench/ERR_test.json)
- ORD: [bioprobench/ORD.json](bioprobench/ORD.json), [bioprobench/ORD_test.json](bioprobench/ORD_test.json)
- GEN: [bioprobench/GEN.json](bioprobench/GEN.json), [bioprobench/GEN_test.json](bioprobench/GEN_test.json)

## 任务定义和字段

### PQA — 协议问答
基于协议内容的多项选择题问答。
- 字段：
	- `question`: 问题字符串
	- `choices`: 候选答案列表
	- `answer`: 正确答案
	- `type`: 问题类别（例如，参数、试剂、操作）
	- `id`: 唯一标识符

### ORD — 步骤排序
正确排序协议步骤（顶层或子步骤序列）。
- 字段：
	- `question`: 描述步骤列表和上下文/标题的提示
	- `wrong_steps`: 以打乱或错误顺序排列的步骤列表
	- `correct_steps`: 按正确时间顺序排列的步骤
	- `type`: 序列粒度（例如，`top`、`child`）
	- `id`: 唯一标识符

### ERR — 错误纠正
检测并纠正协议文本中的错误（包含局部上下文）。
- 字段：
	- `context`: 包含 `purpose`、`prior_step`、`next_step` 的对象
	- `corrupted_text`: 错误文本（正确情况下可能为 `null`）
	- `corrected_text`: 文本的更正版本
	- `is_correct`: 指示提供的文本是否已经正确的布尔值
	- `type`: 类别（例如，参数、试剂、操作或 `correct`）
	- `error_description`: 更正的简要理由
	- `id`: 唯一标识符

### GEN — 协议生成
根据提示生成简洁的单级编号协议步骤。
- 字段：
	- `system_prompt`: 角色/系统指令
	- `instruction`: 格式和样式约束
	- `input`: 任务描述或查询
	- `output`: 编号步骤列表（扁平的 1., 2., 3. ...）
	- `id`: 唯一标识符
	- `type`: 难度标签（例如，`easy`）

## 数据划分
- 训练：使用每个任务的非 `_test.json` 文件。
- 测试：每个任务提供1000个示例的保留集。

## 训练数据指南（微调关键）

### ERR — 错误纠正

**关键：答案语义**

基准测试提示说："如果发现任何错误，回答 False。"

| 条件 | `is_correct` 字段 | 正确的训练输出 |
|-----------|-------------------|------------------------|
| 协议步骤正确 | `True` | `[ANSWER_START]True[ANSWER_END]` |
| 协议步骤有错误 | `False` | `[ANSWER_START]False[ANSWER_END]` |

**重要**：训练数据生成脚本必须使用此逻辑：
```python
def gold_answer_from_is_correct(is_correct: bool) -> str:
    # True = 步骤正确, False = 步骤有错误
    return ANSWER_TRUE if is_correct else ANSWER_FALSE
```

不要反转此逻辑 - 基准测试评估器直接将模型输出与 `is_correct` 字段进行比较。

### ORD — 步骤排序

**输出格式（关键）**
- 答案必须是有效的 Python 列表：`[0, 2, 1, 3]`
- 不是空格分隔：`0 2 1 3`（错误）
- 不是无括号的逗号分隔：`0, 2, 1, 3`（错误）

**训练数据格式**
- 可以包含简短推理（1-2句）
- 最终答案必须格式为：`[ANSWER_START][list][ANSWER_END]`
- 示例：`[ANSWER_START][2, 0, 1, 3][ANSWER_END]`

### GEN — 协议生成

**输出格式**
- 分步协议包装在 `[ANSWER_START]...[ANSWER_END]` 中
- CoT（思维链）格式对此任务是可接受的

### 通用注意事项
- 所有任务支持 `<think>...</think>` 标签用于 CoT 推理（评估器将剥离它们）
- 答案必须包装在 `[ANSWER_START]` 和 `[ANSWER_END]` 标签中

## 许可证
- CC BY 4.0 — 参见 https://creativecommons.org/licenses/by/4.0/

## 注意事项
- 任务涵盖协议问答、排序、纠正和生成（REA 是更广泛基准测试的一部分，但不包含在上述文件中）。
- 数据涵盖多种生物领域和存储库；有关详细信息，请参阅原始基准测试。