# FT-Agent: 自主 LLM 微调

此目录包含 ICML 2026 论文 [FT-Dojo: Towards Autonomous LLM Fine-Tuning with Language Agents](https://arxiv.org/abs/2603.01712) 中 **FT-Agent** 使用的 RD-Agent LLM 微调场景。

FT-Agent 自动化基准驱动的微调循环：

1. 检查目标基准和可用的原始数据集；
2. 生成数据处理代码和 LLaMA-Factory 训练配置；
3. 在完整训练前运行快速失败验证；
4. 微调目标模型；
5. 使用 OpenCompass 进行评估，并将反馈用于下一次迭代。

该实现以研究为导向。完整运行可能会下载大型数据集、构建训练/评估环境、调用 LLM API 进行数据处理，并消耗 GPU 小时数。

## 支持的基准测试

该场景目前包括 FT-Dojo 任务和几个相关扩展的基准适配器。

| 领域 | 基准测试 | 主要原始数据集 |
| --- | --- | --- |
| 数学 | `aime24`, `aime25` | `deepscaler` |
| 专利 | `panorama_par4pc`, `panorama_pi4pc`, `panorama_noc4pc` | `panorama` |
| 化学 | `chemcotbench_mol_und`, `chemcotbench_mol_edit`, `chemcotbench_mol_opt`, `chemcotbench_reaction` | `chemcot` |
| 表格问答 | `tablebench_data_analysis`, `tablebench_fact_checking`, `tablebench_numerical_reasoning`, `tablebench_visualization` | `tableinstruct` |
| 金融 | `FinanceIQ_gen` | `financeiq` |
| 生物学 | `bioprobench_gen`, `bioprobench_ord`, `bioprobench_err`, `bioprobench_pqa` | `bioprobench` |

数据集注册位于 `rdagent/scenarios/finetune/datasets/__init__.py`。基准适配器位于 `rdagent/scenarios/finetune/benchmark/data/adaptor.py`。

当前数据集行为：场景启动时在 `FT_FILE_PATH` 下准备注册的数据集资源。因此第一次运行可能很大且缓慢。重用相同的 `FT_FILE_PATH` 可以让后续运行重用已下载的资产。`--dataset` 选项在准备后限制代理选择的数据集；它不会改变当前的准备步骤。

## 前提条件

- Linux。
- 当前用户无需 `sudo` 即可使用 Docker，或兼容的 conda 设置。
- NVIDIA GPU 用于实际的微调运行。
- LLM API 访问权限用于 RD-Agent 规划器和可选的数据处理调用。
- Hugging Face 访问权限用于目标模型和数据集。某些数据集可能需要接受上游许可证或设置 `HF_TOKEN`。

使用此场景时从源代码安装 RD-Agent：

```bash
git clone https://github.com/microsoft/RD-Agent
cd RD-Agent
make dev
```

## 最小 `.env`

在仓库根目录创建 `.env`。根据您的提供商调整模型名称和 API 设置。

```bash
# LLM 后端
BACKEND=rdagent.oai.backend.LiteLLMAPIBackend
CHAT_MODEL=gpt-4o
CHAT_TEMPERATURE=1
CHAT_STREAM=True
OPENAI_API_KEY=<your_api_key>
# OPENAI_API_BASE=<your_api_base_if_needed>

# RD-Agent 基础设施使用的嵌入模型
EMBEDDING_MODEL=text-embedding-3-small

# 微调工作区。保持稳定以重用下载的模型/数据集。
FT_FILE_PATH=/absolute/path/to/finetune_files

# 运行时环境：docker 是默认路径；conda 可用于本地设置。
FT_Coder_CoSTEER_env_type=docker

# 目标任务。您也可以通过 CLI 参数传递这些。
FT_TARGET_BENCHMARK=aime25
FT_BENCHMARK_DESCRIPTION="AIME 2025 math competition problems. Each answer is an integer from 0 to 999. Expected Output Format: put the final answer within \\boxed{}, for example \\boxed{42}."

# 目标模型和数据处理设置
FT_BASE_MODEL=Qwen/Qwen2.5-7B-Instruct
FT_UPPER_DATA_SIZE_LIMIT=2000
FT_API_MAX_WORKERS=8
FT_STRONG_MODELS='["gpt-4o"]'
FT_WEAK_MODELS='["gpt-4o-mini"]'

# Hugging Face token（如果模型或数据集需要）。
# HF_TOKEN=<your_hf_token>
```

`FT_API_MAX_WORKERS` 默认值为 `8`，以避免公共用户出现意外的速率限制和成本激增。如果您有高吞吐量的内部端点，请在 `.env` 中显式增加它。

## 单任务运行

使用 CLI 入口点：

```bash
rdagent llm_finetune \
  --benchmark aime25 \
  --benchmark-description "AIME 2025 math competition problems. Each answer is an integer from 0 to 999. Expected Output Format: put the final answer within \\boxed{}, for example \\boxed{42}." \
  --base-model Qwen/Qwen2.5-7B-Instruct \
  --loop-n 3 \
  --timeout 12h
```

等效的直接 Python 入口点：

```bash
dotenv run -- python rdagent/app/finetune/llm/loop.py \
  --benchmark aime25 \
  --benchmark-description "AIME 2025 math competition problems. Each answer is an integer from 0 to 999. Expected Output Format: put the final answer within \\boxed{}, for example \\boxed{42}." \
  --base-model Qwen/Qwen2.5-7B-Instruct \
  --loop-n 3 \
  --timeout 12h
```

有用的参数：

| 参数 | 含义 |
| --- | --- |
| `--base-model` | 要微调的 Hugging Face 模型 ID。除非由 `FT_BASE_MODEL` 设置，否则是必需的。 |
| `--benchmark` | 目标基准键，例如 `aime25` 或 `chemcotbench_mol_edit`。 |
| `--benchmark-description` | 自然语言任务和输出格式描述。除非在 `.env` 中设置，否则是必需的。 |
| `--dataset` | 数据集准备后要为代理选择的数据集名称。 |
| `--upper-data-size-limit` | 一个实验使用的最大训练示例数。 |
| `--loop-n` | RD-Agent 循环的最大次数。 |
| `--timeout` | 总体挂钟预算，例如 `12h`。 |

## 批量运行

对于多个基准/模型运行，请使用此目录中的作业辅助工具：

```bash
cp rdagent/app/finetune/llm/job/tasks.json.example rdagent/app/finetune/llm/job/tasks.json
cp .env rdagent/app/finetune/llm/job/.env
bash rdagent/app/finetune/llm/job/run_ft_job.sh rdagent/app/finetune/llm/job/tasks.json
```

当任务未直接提供 `benchmark_description` 时，作业运行器从 `rdagent/app/finetune/llm/job/scenarios.json` 读取基准描述。

## 日志和 UI

运行在配置的日志目录下写入 RD-Agent 轨迹。对于 Streamlit FT UI：

```bash
streamlit run rdagent/app/finetune/llm/ui/app.py
```

对于通用 RD-Agent 日志 UI：

```bash
rdagent ui --port 19899 --log-dir <your_log_folder>
```

## 注意事项

- 第一次运行预计会很慢，因为它可能下载模型、数据集、LLaMA-Factory 资产和 OpenCompass 资产。
- Docker 模式将模型和数据集从 `FT_FILE_PATH` 挂载到训练/评估环境中。
- 生成的训练数据必须使用 Alpaca 风格的 `instruction`、`input` 和 `output` 字段；验证器在完整训练前检查这一点。
- 评估使用验证反馈进行代理迭代，并保留测试集用于最终报告/前端显示。